## ✨ Wan2.2 是什么：开源视频生成模型的新标杆

Wan2.2 是阿里巴巴集团 Wan 团队发布的开源视频生成模型家族，在 Wan2.1 的基础上引入了**混合专家（MoE）架构**和更大规模的训练数据，在视频生成质量、运动连贯性和细节还原上都有显著提升。整个家族覆盖了不同硬件档位的需求：从消费级显卡就能跑的 TI2V-5B，到需要 80GB 显存的 A14B MoE 旗舰版本，再到专门做角色动画的 Animate-14B。

Wan2.2 采用 Apache 2.0 许可发布，模型权重和代码都开源，允许商业使用、二次开发和自由分发——这对想要把视频生成能力集成到自己产品里的开发者来说非常友好。

和市面上的闭源视频生成服务不同，Wan2.2 可以完全本地化部署，不需要把素材上传到任何云端服务器，数据隐私可控。对于有 GPU 算力的创作者和开发者来说，这是一个性价比很高的选择。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://wan2-2.uerg.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - HuggingFace：搜索 Wan-AI 组织下的 Wan2.2 系列仓库，按需下载对应模型权重
> - GitHub：Wan-AI/Wan2.2 仓库提供完整的推理代码和安装脚本

---

## 🔧 环境搭建与模型安装

### 硬件需求速查

Wan2.2 家族不同模型对硬件的要求差异很大，先选对模型再准备机器：

| 模型 | 最低显存 | 推荐显存 | 定位 |
|---|---|---|---|
| TI2V-5B | 16 GB | 24 GB（RTX 4090 / 3090） | 消费级唯一选择，文生/图生 720P |
| A14B MoE | 80 GB（A100/H100） | 80 GB | 旗舰质量，文生/图生 1080P |
| S2V-14B | 80 GB | 80 GB | 视频编辑专用 |
| Animate-14B | 80 GB | 80 GB | 角色动画/动作迁移 |

> [!TIP]
> 如果你只有消费级显卡（24GB 及以下），直接选 **TI2V-5B**。A14B 和 S2V 系列在消费级硬件上跑不动，不要浪费时间尝试。

### 基础环境安装

安装 Wan2.2 推理代码之前，先把 Python 和 PyTorch 环境准备好：

1. **Python 3.10+**：推荐用 conda 创建独立环境
2. **PyTorch**：按 CUDA 版本安装对应命令，CUDA 12.1 示例：

```
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

3. **Diffusers 及依赖**（如果走 Pipeline 路线）：

```
pip install diffusers --upgrade
pip install accelerate transformers safetensors sentencepiece
```

4. **克隆推理仓库**：

```
git clone https://github.com/Wan-Video/Wan2.2.git
cd Wan2.2
```

### 模型权重下载

权重文件比较大，TI2V-5B 约 10GB，A14B MoE 约 27GB。从 HuggingFace 下载到本地目录，比如 `./Wan2.2-TI2V-5B/`，然后在运行命令里通过 `--ckpt_dir` 指定路径。

> [!WARNING]
> 下载权重时核对文件大小是否与官方仓库一致。权重不完整会导致推理时出现形状不匹配的报错，而且这种错误不会直接提示权重缺失，排查起来比较费时间。

---

## 📖 使用入门：从命令行到代码集成

### TI2V-5B 单卡 4090 跑通 720P

TI2V-5B 是整个家族里唯一能在消费级显卡上流畅运行的模型。下面是一条可以直接跑的文生视频命令：

```
python generate.py \
  --task ti2v-5B \
  --size 1280*704 \
  --ckpt_dir ./Wan2.2-TI2V-5B \
  --offload_model True \
  --convert_model_dtype \
  --t5_cpu \
  --prompt "Two anthropomorphic cats in boxing gear fight on a spotlighted stage."
```

几个关键参数解释清楚，调参时不会踩坑：

- **分辨率用 `1280*704` 而不是 `1280*720`**——因为 Wan2.2 的 VAE 压缩比是 4×32×32，高宽必须是 32 的倍数，720 不是，704 是最接近 720P 的合法值
- **`--offload_model True`**：把部分模型组件卸载到 CPU，单卡显存不够整个模型常驻 GPU，这个参数必须开
- **`--convert_model_dtype`**：把权重从 float32 转成 bfloat16，显存占用直接减半
- **`--t5_cpu`**：把 T5 文本编码器放 CPU 上跑，这是单卡能跑起来的关键——T5 本身不吃显存但占计算，放 CPU 不影响 GPU 推理效率

图生视频和文生几乎一样，只多一个 `--image` 参数指定参考图片路径，prompt 里可以描述对图中主体的动作预期。

### 实际性能参考

官方和社区在 RTX 4090 上的实测数据：

| 硬件 | 分辨率 | 时长 | 总耗时 | 峰值显存 |
|---|---|---|---|---|
| RTX 4090 24GB | 720P（1280×704） | 5 秒 | ~9 分钟 | ~22 GB |
| RTX 4090 24GB | 480P | 5 秒 | ~5 分钟 | ~14 GB |
| A100 80GB | 720P | 5 秒 | ~2 分钟 | 约 40GB（可关 offload） |
| H100 80GB | 720P | 5 秒 | ~1.5 分钟 | 同上 |

### Diffusers Pipeline：几行代码集成到项目

如果你想把 Wan2.2 集成到自己的 Python 项目里，用 HuggingFace Diffusers 的 Pipeline API 是最简洁的方式：

```python
import torch
from diffusers import WanPipeline
from diffusers.utils.export_utils import export_to_video

pipe = WanPipeline.from_pretrained(
    "Wan-AI/Wan2.2-TI2V-5B-Diffusers",
    torch_dtype=torch.bfloat16
).to("cuda")

video = pipe(prompt="一只戴墨镜的猫坐在冲浪板上", num_inference_steps=50).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

同一个 Pipeline 也支持图生视频，传 `image` 参数就行：

```python
from diffusers import WanImageToVideoPipeline
from diffusers.utils import load_image

pipe = WanImageToVideoPipeline.from_pretrained(
    "Wan-AI/Wan2.2-TI2V-5B-Diffusers",
    torch_dtype=torch.bfloat16
).to("cuda")

image = load_image("参考图.jpg")
video = pipe(
    prompt="海浪轻轻摇晃着它",
    image=image,
    num_inference_steps=50
).frames[0]
```

A14B MoE 版本在 Diffusers 里会自动处理两份专家权重的加载和 SNR 切换，你只需要加载带 `-Diffusers` 后缀的对应仓库即可。Animate-14B 需要用专门的 `WanAnimatePipeline`，支持 Animation 模式（角色图 + 动作参考视频 → 新角色复现动作）。

### 显存管理实用技巧

显存不够时，这几个方法叠加使用效果明显：

- **`enable_model_cpu_offload()`**：把不活跃的组件卸载到 CPU，显存紧张时必开
- **`enable_vae_slicing()`**：VAE 解码时分块处理，降低峰值显存
- **把 `num_inference_steps` 降到 20~25**：质量损失不大，生成速度能翻倍
- **RTX 4060 Ti 16GB 用户**：把分辨率降到 480P，再不行就等社区的 FP8 量化版本

### MoE 架构：为什么 Wan2.2 比 2.1 好

Wan2.2 相对 2.1 的最大架构变化是引入了**双专家混合专家（MoE）**设计，验证损失对比实验很能说明问题：

| 模型配置 | 高噪专家 | 低噪专家 | 验证损失 |
|---|---|---|---|
| Wan2.1（基线） | 单专家 | 单专家 | 较高 |
| Wan2.1 + Wan2.2 高噪专家 | Wan2.2 新训练 | 复用 Wan2.1 | 较高 |
| Wan2.2 高噪专家 + Wan2.1 | 复用 Wan2.1 | Wan2.2 新训练 | 较高 |
| **Wan2.2 完整 MoE** | Wan2.2 新训练 | Wan2.2 新训练 | **最低** |

两个专家**必须联合训练**才能发挥效果，单独拿来其中一个专家复用旧权重效果都显著变差。分工也很清晰：高噪专家负责视频生成的前期阶段，处理整体布局；低噪专家负责后期阶段，打磨细节。总参数 27B 但每步只激活 14B，这就是 MoE 的核心优势——参数翻倍但计算量基本不变。

除了架构创新，训练数据也比 Wan2.1 大幅扩充：图像 +65.6%、视频 +83.2%。数据和架构两个因素叠加，才让 2.2 相对于 2.1 有了质的飞跃。

### 许可证与二次开发空间

Wan2.2 采用 Apache 2.0 许可，模型权重和代码都可以自由使用：

- ✅ 商业使用模型生成的内容
- ✅ 二次开发、微调模型
- ✅ 把模型集成到自己的产品里
- ✅ 分发、再许可、甚至卖出修改后的版本
- ❌ 去掉许可证声明
- ❌ 用 Wan-AI / Alibaba 的品牌为你的产品背书
- ❌ 直接复制模型权重本身作为产品出售（可以卖集成了它的服务）

> [!NOTE]
> Apache 2.0 是"代码作者不负责"的免责条款——你对你用 Wan2.2 生成的内容负全部责任。如果生成内容涉及真人素材，优先用自己拥有或授权的素材，不要生成政治敏感、暴力、色情等场景。百炼 API 云端有安全过滤，本地部署没有这个保护。

社区已经有不少基于 Wan2.2 的二次开发项目：Helios 做分钟级长视频、LightX2V 做量化加速、HuMo 以人为中心做多模态生成、TalkVerse 基于 TI2V-5B 做分钟级音频驱动。可以 fork 官方仓库自由修改，甚至发布自己的改进版本——只要保留原作者的 License Header 即可。

### TI2V-5B 与 A14B 的质量差距

50 亿参数的 TI2V-5B 和 270 亿参数的 A14B MoE 在质量上确实有差距，主要体现在三个方面：

- **复杂运动的连贯性**——A14B 明显更好，TI2V-5B 在多主体互动场景下偶尔会出现运动断裂
- **运镜的精准度**——A14B 支持显式运镜控制，TI2V-5B 完全靠 prompt 描述
- **极端场景的稳定性**——密集人群、复杂光影下 A14B 更稳

但对于日常创意素材、电商产品动态展示、短视频开头封面等场景，TI2V-5B 的效果已经足够可用——毕竟它是目前唯一能在本地消费级显卡上跑的 720P@24fps 开源视频模型。

---

## 🔗 相关资源

- [Wan2.2官网](https://wan2-2.uerg.quest)
- [Wan2.2文档](https://wan2-2-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Wan2.2)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Wan2.2)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Wan2.2)

## 📚 相关文章

- [我把 Wan2.2 从跑不起来到跑通 720P，踩了这六个坑](https://laoguosofts.github.io/posts/wan2-2-docs.html)

- [Wan2.2 本地部署避坑指南：从 OOM 到 FlashAttention 编译失败，我踩过的八个坑](https://alipymanbu.github.io/posts/wan2-2-docs.html)
