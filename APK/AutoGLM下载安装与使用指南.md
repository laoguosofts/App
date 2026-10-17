## ✨ AutoGLM 是什么——让大模型真正会用手机的开源 GUI Agent 框架

AutoGLM 是一个面向移动端的 GUI Agent 开源框架，核心目标是让大语言模型像人一样"看"手机屏幕、"理解"界面布局、"执行"触摸操作。不同于传统自动化依赖固定坐标或控件树，AutoGLM 走的是视觉理解路线——模型接收屏幕截图，直接输出结构化的动作指令，整个链路从截图到动作执行全自动完成。

框架的核心组件包括一个多模态大模型（AutoGLM-Phone-9B）和一个 Phone Agent 运行时。模型负责"想"——根据当前截图推理下一步该做什么；运行时负责"做"——把模型输出的动作 JSON 翻译成对手机的实际操控（ADB/HDCD/WDA）。两者解耦意味着你可以自由选择推理引擎（vLLM / SGLang / 智谱云端 API），也可以切换不同的设备控制通道。

适合谁用呢？如果你在做移动端自动化测试、批量操作脚本、或者想探索"让 AI 替你玩手机"这个方向，AutoGLM 提供了一套完整的参考实现和可扩展的接口设计。它不是一个开箱即用的 App，而是一套需要自己部署模型、连接设备、写指令的技术栈。门槛不低，但跑通之后的成就感也很实在——你用自然语言说一句话，手机自己走完从打开 App 到点击下单的完整链路。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://autoglm.boop.uno/download.html) —— 下载页整理了当前可用的版本和安装说明
> - HuggingFace / ModelScope：搜索 `zai-org/AutoGLM-Phone-9B` 下载模型权重
> - GitHub：Clone 项目仓库获取 Phone Agent 框架源码

---

## 🔧 模型部署与环境搭建

AutoGLM 的运行分为两层：模型推理服务和 Phone Agent 运行时。先把模型服务跑起来，再启动 Agent 连接服务控制设备。

### 环境前置

Python 3.10+ 是基本要求，GPU 方面分两档：纯 CPU 跑量化模型也行但速度很慢，有 NVIDIA GPU 会顺畅很多。模型本身对显存的需求取决于你用的量化等级——FP16 完整权重需要约 18GB，Q8_0 约 9GB，Q5_K_M 约 7GB，Q4_K_M 约 5GB。没有 GPU 也可以直接用智谱云端 API，零 GPU 起步。

设备控制通道取决于你的目标平台：Android 走 ADB，鸿蒙走 HDCD，iOS 走 WDA。三者的设备发现、截图、注入操作原理类似，框架内部做了统一封装，你只需要在启动时指定 `--device-type` 参数。

### 推理引擎选择与安装

AutoGLM 模型兼容 vLLM 和 SGLang 两种主流推理引擎。选型很简单：第一次部署优先选 vLLM，中文资料多、坑少；如果 vLLM 在视觉处理上遇到问题，或者你需要多视觉任务高并发，可以试试 SGLang。

安装 vLLM：

```bash
pip install vllm
```

安装 SGLang：

```bash
pip install sglang
# Docker 方式
docker pull lmsysorg/sglang:v0.5.6.post1
```

两种引擎的 Docker 镜像都已经预编译好 CUDA 依赖，拉下来就能用。容器内如果遇到 cuDNN 版本问题，补装对应版本的 `nvidia-cudnn-cu12` 即可。

### 下载模型与启动服务

从 HuggingFace 或 ModelScope 拉取 `zai-org/AutoGLM-Phone-9B` 权重。权重文件大约 18GB（FP16），建议用支持断点续传的下载工具。

vLLM 启动命令：

```bash
python -m vllm.entrypoints.openai.api_server \
  --model-path zai-org/AutoGLM-Phone-9B \
  --served-model-name autoglm-phone-9b \
  --max-model-len 25480 \
  --mm_processor_kwargs '{"max_pixels":5000000}' \
  --mm-encoder-tp-mode data \
  --port 8000
```

SGLang 启动命令参数略有不同：

```bash
python -m sglang.launch_server \
  --model-path zai-org/AutoGLM-Phone-9B \
  --served-model-name autoglm-phone-9b \
  --context-length 25480 \
  --mm-enable-dp-encoder \
  --mm-process-config '{"image":{"max_pixels":5000000}}' \
  --port 8000
```

> [!TIP] 参数对照：vLLM 的 `--max-model-len` 对应 SGLang 的 `--context-length`，`--mm_processor_kwargs` 对应 `--mm-process-config`，`--mm-encoder-tp-mode data` 对应 `--mm-enable-dp-encoder`。两边虽然名字不同，但含义是一一对应的，迁移时直接替换就行。

### 部署验证

服务启动后，先跑官方验证脚本确认模型正常工作，不要急着连设备：

```bash
python scripts/check_deployment_cn.py \
  --base-url http://localhost:8000/v1 \
  --model autoglm-phone-9b
```

脚本会发送一条测试请求，检查返回内容里是否包含 `<think>...</think>` 思维链标签和 `<answer>do(action="...")</answer>` 结构化动作 JSON。这两个元素是 AutoGLM 模型输出的标准格式，缺任何一个都说明部署有问题。

> [!WARNING] 常见的部署失败信号：思维链过短（模型只推理了一两句话）、输出乱码（中文变问号）、JSON 格式不合法。出现这些问题时优先检查启动参数是否完整，尤其是上下文长度和视觉处理配置项——漏掉任意一个都会导致模型推理降级或输出格式错乱。

验证脚本**只测模型服务本身**，不涉及设备控制。完整链路验证需要跑一条最小 Agent 命令：

```bash
python main.py --base-url http://localhost:8000/v1 --model autoglm-phone-9b "打开设置应用"
```

如果这一条能正常启动设置应用，说明模型服务、Agent 框架、设备连接三层全部打通了。

---

## 📖 从一句自然语言到一次完整操作：Agent 主循环解析

部署完成后，你就可以通过自然语言指令驱动手机了。框架的核心是一个循环：截图 → 模型推理 → 执行动作 → 判断是否完成，直到模型认为任务结束或达到最大步数上限。一条"帮我在美团点一杯瑞幸咖啡的标准美式，半糖"指令，Agent 大致会走 15~25 步循环。

### 两种运行模式

**单次任务模式**：一条指令执行完自动退出，适合脚本化调用和自动化场景。

**交互模式**：启动后进入提示符，连续执行多条指令，适合调试和多轮任务。启动方式就是 `python main.py --base-url ... --model ...`，不带任务指令参数。交互模式下每输一条指令回车执行，完成后继续等待下一条，按 `Ctrl+C` 退出。

> [!TIP] 强烈建议调试时加上 `--verbose` 参数。开启后每一步都会打印模型的思考过程和执行动作 JSON，清楚地看到"它在想什么"和"它要做什么"。失败排查时这个输出会告诉你到底是模型想歪了还是执行层出了问题。

### 关键启动参数

| 参数 | 说明 | 默认值 |
|---|---|---|
| `--base-url` | 模型服务地址 | `http://localhost:8000/v1` |
| `--model` | 模型名称（与部署时的 `--served-model-name` 一致） | `autoglm-phone-9b` |
| `--device-type` | 设备控制通道：`adb` / `hdcd` / `wda` | `adb` |
| `--device-id` | 多设备时指定目标设备 | 自动检测 |
| `--max-steps` | 单任务最大循环步数 | 100 |
| `--lang` | 系统提示词语言：`cn` / `en` | `cn` |

### 原子动作体系

模型输出的结构化动作 JSON 对应 10 种原子操作，框架内部做了统一封装。从截图坐标到设备注入，每种动作都有完整的边界条件检查：

- **Tap**：点击屏幕坐标
- **Swipe**：滑动（支持方向和距离参数）
- **Type**：输入文本
- **Launch**：启动 App
- **Press**：按键（返回/主页/最近任务）
- **Wait**：等待页面加载

模型每次推理输出一个动作，Agent 执行完后立即截图送回模型，形成闭环。这个设计让 Agent 能应对 App 界面变化——不依赖固定坐标，而是靠视觉理解当前屏幕状态决定下一步。

### 外卖下单场景的完整链路

跑美团外卖下单这条命令，verbose 输出会展示 Agent 的完整思考链：

```
[启动美团 → 点搜索框 → 输"瑞幸咖啡" → 点结果 → 选商品 → 选规格 → 选糖度 → 加购 → 选地址 → 点提交订单]
```

几个值得注意的设计细节：

**App 加载慢**——Agent 不会傻等，有专门的 Wait 操作。模型看截图发现"页面还在加载中"就主动等待几秒再继续推理。

**搜索结果多页**——Agent 会 Swipe 滑动浏览，根据任务描述里的"评分最高""离得近"等条件筛选目标店铺。

**授权弹窗**——首次启动 App 经常弹出权限请求，Agent 能识别到弹窗上的"允许"按钮并点击通过。

**支付环节**——到了支付确认时框架会触发 Take_over 回调，请求人工接管。这是安全设计，避免自动支付带来风险。

### 云端 API 零 GPU 起步

如果本地没有 GPU 资源，可以直接用智谱云端的 AutoGLM-Phone API，不需要自己部署模型。只需要把 `--base-url` 换成 `https://open.bigmodel.cn/api/paas/v4`，`--model` 换成 `autoglm-phone`，`--apikey` 填上 API Key 就行。推理引擎和部署细节都省了，直接进 Agent 调试阶段。

### 部署常见问题排查

模型部署失败时按以下顺序排查：

1. **HTTP 400/500**——先确认模型服务是否真的启动成功，base-url 和 model 名称是否匹配
2. **思维链过短**——检查 `--max-model-len` / `--context-length` 是否设置了足够的值（25480 是模型训练时的长度）
3. **中文乱码**——检查 vLLM 的 `--chat-template-content-format string` 或 SGLang 的上下文长度参数
4. **JSON 格式错误**——确认用的是 `zai-org/AutoGLM-Phone-9B` 而不是其他变体模型，输出格式依赖模型特定的 chat template

### 实战建议

1. **先用交互模式跑通**：交互模式下可以快速验证单条指令，出问题立即看到 verbose 输出，比写脚本调试效率高
2. **从小场景开始**：先让 Agent 做些简单的——打开设置、搜索联系人、浏览网页——跑通后再挑战外卖下单、电商购物这类多步骤场景
3. **记录 verbose 日志**：把每次成功执行的 verbose 输出存下来，遇到模型"想歪"时对照日志调整指令措辞
4. **注意 Take_over**：涉及支付、发送消息等敏感操作时框架会触发人工接管回调，这不是 bug 是正常的安全设计

---

## 🔗 相关资源

- [AutoGLM官网](https://autoglm.boop.uno)
- [AutoGLM文档](https://autoglm-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/AutoGLM)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/AutoGLM)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/AutoGLM)

---

## 📚 相关文章

- [AutoGLM 深度盘点：让大模型直接操控手机的 GUI Agent 技术栈能做什么](https://laoguosofts.github.io/posts/autoglm-docs.html)
- [AutoGLM 深度盘点：让大模型真正会用手机的开源框架到底能做什么](https://alipymanbu.github.io/posts/autoglm-docs.html)
