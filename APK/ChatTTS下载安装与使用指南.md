## ✨ ChatTTS 是什么

ChatTTS 是 2noise 团队开源的一款语音合成模型，特点是**能够同时控制音色和韵律**——不像传统 TTS 只能念得平平板板，ChatTTS 生成的语音带有接近真人的自然停顿、重音和情绪起伏。模型采用说话人嵌入（speaker embedding）机制，你可以先采样一个随机音色，再在后续所有推理中复用同一个嵌入，实现角色配音的一致性。

核心能力包括：随机采样说话人、零样本说话人克隆（提供几秒参考音频）、流式推理输出。模型文件走 HuggingFace 分发，代码部分采用 AGPLv3+ 协议发布，权重另有 CC BY-NC 协议约束。整体定位是给开发者和个人研究者用的技术工具——它不是开箱即用的 App，需要 Python 环境和 PyTorch 依赖，对有代码基础的用户更友好。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://chattts.usea.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：ChatTTS 是代码形式分发的开源项目，不走应用商店渠道，需要从官方仓库拉取源码或通过 pip 安装

---

## 🔧 安装步骤

ChatTTS 本质是 Python 包，安装分两步：先装 PyTorch，再装 ChatTTS 本身。PyTorch 的版本选择取决于你有没有 GPU——有 NVIDIA 显卡就装带 CUDA 的版本，没有就装 CPU 版。

**第一步：安装 PyTorch**。到 PyTorch 官网的安装页面，按你的系统（Windows/Linux/Mac）、CUDA 版本（如果用 GPU）复制对应的 pip 命令。以 Windows + CUDA 12.1 为例：

```bash
pip install torch==2.3.0 torchaudio==2.3.0 --index-url https://download.pytorch.org/whl/cu121
```

> [!WARNING]
> Windows 用户注意：PyTorch 在某些版本中对 Windows 平台的 torch.compile 支持不完整，会在 ChatTTS 首次加载时报 `RuntimeError: Windows not yet supported for torch.compile`。这不是 ChatTTS 的问题，是 PyTorch 的平台限制——解决方法见"使用入门"章节。

**第二步：安装 ChatTTS**。推荐用 pip 直接从 GitHub 安装最新版：

```bash
pip install git+https://github.com/2noise/ChatTTS.git
```

或者克隆仓库后本地安装：

```bash
git clone https://github.com/2noise/ChatTTS.git
cd ChatTTS
pip install -e .
```

**第三步：验证安装**。进入 Python 交互环境，尝试导入：

```python
import ChatTTS
print("OK")
```

能正常打印 OK 就是安装成功了。ChatTTS 首次调用 `chat.load()` 时会自动从 HuggingFace 下载模型权重（约几百 MB），国内访问 HuggingFace 可能很慢甚至超时，加速方案见下一节。

---

## 📖 使用入门

### 首次加载与模型下载

ChatTTS 的核心类是 `Chat`。基本的加载和推理流程：

```python
import ChatTTS
import torch

chat = ChatTTS.Chat()
chat.load(compile=False)  # compile=False 对 Windows 更友好
```

首次 `chat.load()` 会触发模型下载。默认从 HuggingFace 拉取 `2Noise/ChatTTS`，国内访问速度经常只有几 KB/s 甚至直接超时。有四种加速方案：

**方案一，ModelScope 源**——最简单，一行改参数：

```python
chat.load(compile=False, source='modelscope')
```

ModelScope 是阿里的开源模型平台，ChatTTS 内置了对它的支持，国内下载速度通常比 HuggingFace 快一个数量级。

**方案二，预下载 + 本地缓存**。先用 huggingface-cli 把模型拉到本地，ChatTTS 加载时会自动从缓存读取：

```bash
# PowerShell 设置镜像（Linux/Mac 用 export）
$env:HF_ENDPOINT = "https://hf-mirror.com"
huggingface-cli download 2Noise/ChatTTS
```

**方案三，指定自定义目录**。不想用默认缓存路径的话，可以手动指定：

```bash
$env:HF_ENDPOINT = "https://hf-mirror.com"
huggingface-cli download 2Noise/ChatTTS --local-dir ./my_chattts_model
```

代码里对应改成：

```python
chat.load(compile=False, source='custom', custom_path='./my_chattts_model')
```

**方案四，代理**。有可用代理的话设置 HTTPS_PROXY 环境变量即可。

下载完成后，模型权重默认缓存到 `~/.cache/huggingface/hub/models--2Noise--ChatTTS/`（Windows 下对应 `C:\Users\你的用户名\.cache\huggingface\hub\models--2Noise--ChatTTS\`），目录里应该有 `.safetensors` 或 `.bin` 权重文件和配置文件。

### 生成第一段语音

加载好模型后，用 `infer()` 方法生成语音：

```python
# 随机采样一个说话人
rand_spk = chat.sample_random_speaker()

wavs = chat.infer(
    "你好，这是 ChatTTS 生成的第一段语音。",
    params_infer_code=ChatTTS.Chat.InferCodeParams(spk_emb=rand_spk)
)
```

`infer()` 返回的是 numpy 数组，可以用 soundfile 或 scipy 写成 wav 文件：

```python
import soundfile as sf
sf.write("hello.wav", wavs[0], 24000)  # ChatTTS 输出采样率 24kHz
```

### 音色复用与持久化

随机采样出的音色可能恰好很好听、很适合某个角色，但下次跑脚本又会采样出完全不同的音色——之前的好结果就丢了。解决方法是把说话人嵌入保存到文件，下次加载直接复用：

```python
# 第一次：采样并保存
import json
rand_spk = chat.sample_random_speaker()
with open('speaker.json', 'w', encoding='utf-8') as f:
    json.dump({'speaker': rand_spk}, f)

# 后续：直接加载，不再采样
with open('speaker.json', 'r', encoding='utf-8') as f:
    rand_spk = json.load(f)['speaker']
```

`sample_random_speaker()` 返回的是说话人嵌入向量的 JSON 序列化字符串，大约几百字节，存起来毫无压力。加载后直接喂给 `InferCodeParams` 的 `spk_emb` 参数就行，不需要再"采样"。

典型场景包括：**角色配音的一致性**——第一次为角色 Alice 采样并保存到 `characters/alice.json`，后续所有 Alice 的台词都复用这个嵌入；**批量生产复用**——第一次运行时采样一个默认音色保存，后续运行直接读取，节省随机采样的不确定性。

> [!TIP]
> 注意跨版本兼容性：不同 ChatTTS 版本之间，嵌入字符串的内部格式可能有差异。如果升级了 ChatTTS 版本，之前保存的字符串可能不再兼容——此时重新采样即可。另外，嵌入是在 `sample_random_speaker()` 所在进程的模型上采样的，但跨设备复用没问题（比如在 A 电脑采样，把文件拷到 B 电脑上加载，效果一致）。

### Windows 平台的 torch.compile 问题

某些 PyTorch 版本在 Windows 上调用 `torch.compile` 会抛出 `RuntimeError: Windows not yet supported for torch.compile`，这不是 ChatTTS 的 bug。解决方法有三种：

**最直接的方案**：确保 `chat.load(compile=False)`。关掉 compile 不会影响功能，只是推理速度稍慢——单次推理可能慢几十秒，但日常个人使用几乎察觉不到差异。

**次选方案**：升级或降级 PyTorch 版本，试试 2.3.0 之类较新的版本。

**最彻底的方案**：换到 Linux 环境（包括 WSL2）。torch.compile 在 Linux 上支持最完整，追求极致速度的批量生产场景本来也不会用 Windows 跑 GPU 推理。

关掉 compile 的影响总结如下：

| 方面 | compile=True | compile=False |
| --- | --- | --- |
| 首次加载速度 | 慢（编译计算图） | 快 |
| 后续推理速度 | 快（图优化） | 约慢 1.3~2 倍 |
| Windows 兼容性 | 部分版本报错 | 全部可用 |
| 个人使用感知 | 无 | 无明显差异 |

### 授权协议要点

ChatTTS 代码部分采用 **AGPLv3+** 协议发布，权重文件另有 **CC BY-NC** 协议。理解 AGPLv3+ 的关键区别：它比 GPLv3 多了一条——**通过网络提供服务（WebUI、API、在线 TTS 网站）也触发协议**。

简单来说，你可以自由使用、复制、分发、修改代码，甚至商业使用，但前提是如果修改了 ChatTTS 代码并对外提供服务（无论分发二进制包还是跑在线服务），都必须公开你的修改后的源码。不修改代码、只调用权重做本地离线推理则不受协议约束。

| 场景 | 是否触发 AGPLv3+ |
| --- | --- |
| 本地自己玩 | 不触发 |
| 内部团队工具 | 一般不触发 |
| 做在线 TTS SaaS | **触发，必须公开源码** |
| 修改后打包分发 exe | **触发，必须公开修改** |
| 论文引用 | 不触发 |

如果想基于 ChatTTS 做商业化产品但不想公开源码，有两个方向：联系 2noise 谈商业许可，或者只调用模型权重不修改代码（AGPLv3+ 管的是代码不是权重）。

---

## 🔗 相关资源

- [ChatTTS官网](https://chattts.usea.quest)
- [ChatTTS文档](https://chattts-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/ChatTTS)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/ChatTTS)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/ChatTTS)

---

## 📚 相关文章

- [ChatTTS 踩过的五个坑——从显存爆掉到音色丢失，全记录](https://laoguosofts.github.io/posts/chattts-docs.html)
- [ChatTTS 踩过六个坑，才敢说它真能给 LLM 当嘴](https://alipymanbu.github.io/posts/chattts-docs.html)
