## ✨ Qwen3：思考与速度兼得的新一代大模型家族

Qwen3 是阿里通义实验室推出的开源大模型家族，最大的特点是**混合推理模式**——模型会根据任务复杂度自动决定是否进行深度思考。简单问题直接给答案，复杂问题先在内部走一遍推理链再输出结果，兼顾了响应速度和推理质量。

模型家族同时覆盖了 **Dense 密集架构** 和 **MoE 混合专家架构** 两条路线：从 0.6B 的轻量化版本到 235B 的旗舰规格，从消费级显卡到企业级多卡集群，都能找到合适的部署方案。桌面客户端、移动端 App、云端 API、本地部署四种使用形态全部打通，开发者和普通用户都能找到自己的入口。

安全方面，家族还包含了专门的 **Qwen3Guard 护栏模型**，支持流式实时安全检测，可以在模型生成有害内容时逐 token 截断，是生产环境部署的重要配套。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://qwen3.hhdc.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 端可在各应用市场搜索 Qwen；iOS 端 App Store 搜索 Qwen；桌面端从官网下载 Windows / macOS 原生安装包

---

## 🔧 多平台安装指南

Qwen3 的使用方式分为两类：**普通用户**直接用官方桌面版或移动端 App；**开发者**则需要在本地部署模型或接入云端 API。两条路线的安装方式完全不同，这里分别说明。

### 桌面客户端安装

官方提供 Windows 和 macOS 原生应用，无需配置 API Key 或部署模型，登录通义账号即可使用。

**macOS（Apple Silicon + Intel 双架构支持）：**

1. 下载 `.dmg` 安装文件
2. 双击打开，把 Qwen 图标拖到 Applications 文件夹
3. 首次打开如果 Gatekeeper 提示安全警告，右键 → 打开 → 确认即可

**Windows（支持 Win10 / Win11）：**

1. 下载 `.exe` 安装程序
2. 双击运行，按向导提示选择安装路径
3. 安装完成后从开始菜单启动

> [!TIP] 首次启动需要登录
> 桌面版支持通义账号、支付宝扫码、阿里云账号三种登录方式，任选其一即可进入对话界面。如果之前在网页版有历史对话，登录后会自动同步。

### 本地模型部署（开发者）

如果想用自己的 GPU 跑模型，推荐用 Transformers 库加载。

**环境准备：**

1. 安装 Python 3.10+ 和 PyTorch（根据显卡型号选择 CUDA 版本）
2. 安装 Transformers、Accelerate 等依赖：`pip install transformers accelerate`
3. 首次加载模型时会从 Hugging Face 自动下载权重，建议提前配置镜像源加速

**加载 Dense 模型示例：**

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

model_name = "Qwen/Qwen3-8B"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype="auto",
    device_map="auto"
)
```

> [!WARNING] MoE 模型显存门槛
> MoE 模型的总参数虽然远大于激活参数，但总参数量仍然需要全部加载进显存。30B-A3B 需要约 60GB 显存，235B-A22B 则需要 470GB 以上，消费级单卡无法承载。本地单卡用户优先选择 Dense 8B 或 14B 量化版本。

### Ollama 一行部署

如果不想写代码，可以用 Ollama：

```bash
ollama run qwen3:8b
```

Ollama 会自动下载模型并启动对话，适合快速测试和日常轻量使用。

---

## 📖 使用入门与核心功能

### 混合推理：模型自己决定要不要思考

Qwen3 默认开启思考模式——收到用户输入后，模型会先生成一段内部思考链，再基于思考链输出最终答案。在数学推理、代码调试、复杂逻辑分析这类任务上，思考模式显著提升准确率。

但思考模式也有代价：**推理慢、token 多、成本高、输出啰嗦**。有时候用户只想快速得到一个简单答案，思考过程反而是负担。

#### 关闭思考的三种方法

| 方法 | 强度 | 适用场景 | 缺点 |
| --- | --- | --- | --- |
| 提示词 `/no_think` | 软开关 | 通用、快速测试 | 不保证 100% 关闭 |
| Transformers `enable_thinking=False` | 硬开关 | 纯 Python 调用 | 只适用于 Transformers |
| SGLang / vLLM API 参数 | 硬开关 | 推理服务部署 | 框架特定配置 |

最通用的做法是在系统提示词或用户消息里加 `/no_think`，所有调用方式都支持。如果需要彻底关闭（比如生产环境要求输出格式严格可控），就在 tokenizer 层面设 `enable_thinking=False`，模型完全不生成思考链。

> [!NOTE] 何时必须用硬开关
> 需要格式化输出（JSON、XML、代码）、低延迟实时对话、生产环境 API、token 预算紧张这四类场景，不要只靠 prompt，直接用硬开关保证行为可预测。

### Dense 还是 MoE？怎么选

Qwen3 同时开源了两种架构，理解它们的差异才能做出正确的选型。

**Dense 密集架构**：每一层的所有参数在每次推理时都参与计算。参数量 = 推理计算量，推理稳定可预测，主流推理框架（TensorRT、vLLM、SGLang）都做了充分优化，LoRA 微调直接适用。缺点是扩展受限于显存——24GB 显存的消费级 GPU 很难跑得动 70B 级别。

**MoE 混合专家架构**：每一层有 128 个专家子网络，但每次推理只激活其中 8 个。总参数远大于激活参数，计算量只和激活参数相关。Qwen3 的 30B-A3B 总参数 300 亿但激活参数只有 30 亿，计算量接近 Dense 8B；235B-A22B 总参数 2350 亿，激活参数约 220 亿，计算量接近 Dense 32B。

| 场景 | 推荐 | 理由 |
| --- | --- | --- |
| 本地单卡（≤24GB） | Dense 8B / 14B 量化版 | MoE 总参数太大装不下 |
| 本地多卡工作站 | MoE 30B-A3B | 激活参数小，计算量可控 |
| 云端 API 调用 | MoE 235B-A22B | 旗舰能力，云端托管 |
| 需要微调 | Dense 14B / 32B | 微调生态成熟 |
| 需要低延迟推理 | Dense 8B 量化版 | 计算路径固定，优化充分 |

### 桌面版的独门功能

桌面客户端相比网页版多了两个实用功能：

**截图理解**：点击截图按钮或按快捷键，框选屏幕任意区域，Qwen 自动分析截图内容——识别文字、理解 UI 布局、解答界面相关的问题都能做。适合从网页上复制一段文字让它翻译，或者让它帮你读一张报错截图。

**文件分析**：支持上传 PDF、Word、TXT、Markdown 等文本文件以及图片，自动做 OCR + 内容理解。长文档让它帮你总结、提取要点、回答具体问题都很方便。

桌面版和网页版的核心差异：

| 方面 | 桌面版 | 网页版 |
| --- | --- | --- |
| 截图 | ✅ | ❌ |
| 文件分析 | ✅ | 有限 |
| 快捷键唤起 | ✅ | ❌ |
| 离线使用 | ❌ | ❌ |

### Qwen3Guard：给生产环境加一道保险

如果打算把模型接入自己的产品或服务，Qwen3Guard 值得关注——它是家族中首款专为安全防护设计的护栏模型。

家族提供两种版本：

- **Qwen3Guard-Gen**：离线安全标注，对完整输入输出做一次性分类，适合训练数据标注、批量审核、RLHF 奖励模型
- **Qwen3Guard-Stream**：在线实时拦截，在模型生成过程中**逐 token 检测**，一旦发现有害内容立即截断，大幅提升安全响应效率

两个版本都开源了 0.6B / 4B / 8B 三种规格，轻量部署到边缘设备或企业级高精度检测都能覆盖。如果你的服务需要严格的安全控制，可以把 Qwen3Guard-Stream 串在主模型后面做实时拦截。

> [!TIP] 普通用户不需要 Guard
> Qwen3 系列模型本身已经内置了基础的安全对齐，日常使用不会触发有害输出。Qwen3Guard 主要面向需要更严格安全保障的开发者和企业用户。

---

## 🔗 相关资源

- [qwen3官网](https://qwen3.hhdc.tech)
- [qwen3文档](https://qwen3-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%AD%A3%E7%89%88/qwen3)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%AD%A3%E7%89%88/qwen3)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%AD%A3%E7%89%88/qwen3)

## 📚 相关文章

- [Qwen3 到底值不值得掏钱？一份冷静的付费决策账本](https://laoguosofts.github.io/posts/qwen3-docs.html)
- [Qwen3 到底值不值得掏钱？一篇把本地部署和云端 API 的账算到骨头里](https://alipymanbu.github.io/posts/qwen3-docs.html)
