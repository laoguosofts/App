## ✨ CodeFormer 是什么

CodeFormer 是一个基于 Transformer 架构的人脸修复模型，核心思路是把低清人脸编码成 Codebook token 序列，再从视觉原子组合中重建出高清人脸。与传统 GAN 方案不同，CodeFormer 的重建路径和原图特征之间有一条可控的残差连接——通过一个 `w` 参数调节两条路径的融合比例，用户可以在"画质优先"和"身份保真"之间自由滑动。

这个模型的典型应用场景包括：老照片修复、AI 生图后人脸优化、视频逐帧增强、压缩伪影去除等。如果你手头有模糊的长辈照片、或者需要批量处理人像素材，CodeFormer 是目前效果最均衡的开源方案之一。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://codeformer.laze.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：CodeFormer 是 Python 开源项目，没有独立的移动端 App，本地使用需通过 pip 安装源码

---

## 🔧 本地环境搭建

CodeFormer 本质上是一个 Python 推理脚本，需要 PyTorch 1.8+ 和 CUDA 支持才能跑 GPU 推理。先确认你的机器是否装了 NVIDIA 显卡驱动，然后按以下步骤走。

### 基础依赖

1. **安装 Anaconda 或 Miniconda**（推荐 conda 管理 Python 版本，避免 PyTorch 依赖冲突）
2. **创建虚拟环境**：`conda create -n codeformer python=3.8 -y && conda activate codeformer`
3. **安装 PyTorch**：到 PyTorch 官网选对应 CUDA 版本的命令，比如 CUDA 11.7 用 `pip install torch==1.13.1 torchvision==0.14.1 --index-url https://download.pytorch.org/whl/cu117`
4. **安装 CodeFormer 依赖**：`pip install basicsr facexlib realesrgan opencv-python`

### 拉取源码和预训练权重

```bash
git clone https://github.com/sczhou/CodeFormer.git
cd CodeFormer
pip install -r requirements.txt
```

预训练权重需要单独下载，放在 `weights/CodeFormer/` 目录下。README 里给了 Google Drive 和百度网盘两个渠道。

> [!TIP]
> 显存门槛：整图 + 背景增强需要至少 12GB 显存。只有 6GB 显存的显卡可以关掉背景增强参数跑单张人脸修复，速度会慢一些但能出结果。

### 常见安装坑

- **Linux 无界面服务器 libGL 缺失**：Docker 容器或 AutoDL 实例上跑 `import cv2` 报 `libGL.so.1: cannot open shared object file`，一行命令解决：`sudo apt-get update && sudo apt-get install -y libgl1`。CentOS 系统用 `sudo yum install -y mesa-libGL`。
- **ffmpeg 未装（视频处理需要）**：`sudo apt-get install ffmpeg` 或 Windows 到官网下载二进制文件加到 PATH。
- **CUDA 版本不匹配**：PyTorch 版本必须和你的 CUDA 工具链对应，否则 `torch.cuda.is_available()` 返回 False，脚本会退到 CPU 推理（非常慢）。

---

## 📖 使用入门

搭好环境后，核心命令就一条：`python inference_codeformer.py`。下面按使用频率从高到低介绍各个场景的实战要点。

### 单张图片修复

最常用的命令，修复一张低清人脸：

```bash
python inference_codeformer.py -w 0.5 --input_path face.jpg
```

输出自动存到 `results/CodeFormer_w0.5/face.jpg`，`-w` 参数决定融合比例（后面专门讲）。

### 视频逐帧增强

视频处理的底层逻辑是：ffmpeg 抽帧 → CodeFormer 逐帧修复 → ffmpeg 合成并保留原音频。命令行和图片几乎一样，只是 `--input_path` 换成视频文件：

```bash
python inference_codeformer.py --bg_upsampler realesrgan --face_upsample -w 1.0 --input_path video.mp4
```

> [!WARNING]
> 视频场景必须用 **w=1.0**。如果 w 值偏小，同一人的相邻帧可能长相微变（身份漂移导致闪烁），时间一致性会出问题。

视频格式支持 mp4、mov、avi、mkv 等 ffmpeg 能处理的容器。不支持实时视频流和 USB 摄像头直接输入，先录屏保存成文件再处理。

耗时参考（单张人脸修复约 2-3 秒，视频是逐帧线性累加）：

| 视频规格 | RTX 3060 | RTX 3080 | T4 云 GPU |
| --- | --- | --- | --- |
| 10s 720p | ~8 分钟 | ~5 分钟 | ~12 分钟 |
| 10s 1080p | ~15 分钟 | ~9 分钟 | ~22 分钟 |
| 60s 1080p | ~1.5 小时 | ~50 分钟 | ~2.5 小时 |

### w 参数调优指南

这是 CodeFormer 最核心的调参点。模型推理时有两条信息流：Transformer + Codebook token 路径（从视觉原子重建"理想人脸"）和残差连接（把原图特征喂给解码器）。`w` 值就是残差连接的流量开关，控制两条路径的融合比例。

| w 值 | 效果 | 适用场景 |
| --- | --- | --- |
| 0.0 | 纯 Codebook 重建，皮肤光滑细腻但可能改掉原图独特特征（痣、疤痕） | AI 生图后处理、只想要好看头像 |
| 0.5 | 两条路径等权融合，两边都过得去但两边都不极端 | 默认值，大多数日常场景 |
| 1.0 | 最大身份保真，原图一切细节保留（包括噪点和模糊） | 视频逐帧、警方档案、身份证照片 |

**区域敏感度差异**：瞳孔/虹膜对 w 最敏感——w 小会改写颜色和反光点；皮肤纹理次之；头发的 Codebook 建模较弱，w 大小时差异不大。

> [!NOTE]
> w 是标量，0 到 1 的变化是**线性插值**。w=0.6 ≈ 60% w=1 特征 + 40% w=0 特征，可以连续微调而不是只有几个档位。

### Replicate 在线 API

不想搭本地环境的话，Replicate 平台上有官方端点 `sczhou/codeformer`，网页 UI 直接上传图片或视频就能跑，也支持 CLI 和 Python 代码调用：

```python
import replicate

client = replicate.Client(api_token="r8_xxx")
output = client.run(
    "sczhou/codeformer",
    input={
        "image": open("photo.jpg", "rb"),
        "w": 0.7,
        "upscale": 2,
        "bg_upsampler": "realesrgan",
        "face_upsample": True,
    }
)
```

费用按运行时长秒数计：单张图片约 $0.001-0.005，10 秒视频约 $0.05-0.15。小批量处理比自建 GPU 划算，大批量（几千张以上）则反过来。

### 显存紧张的应对策略

显存不够 12GB 时关掉背景增强（去掉 `--bg_upsampler` 参数），代价是人脸修好了但背景可能有"分辨率差"感。4K 视频逐帧处理在 12GB 显存上很容易 OOM，建议先用 ffmpeg 缩到 1080p 再处理。

---

## 🔗 相关资源

- [codeformer官网](https://codeformer.laze.skin)
- [codeformer文档](https://codeformer-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD/codeformer)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD/codeformer)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD/codeformer)

---

## 📚 相关文章

- [CodeFormer 全链路盘点：从模型原理到生产部署，人脸修复能覆盖多少场景](https://laoguosofts.github.io/posts/codeformer-docs.html)
- [CodeFormer 深度盘点：从老照片修复到 AI 生图后处理，一个模型能扛多少活](https://alipymanbu.github.io/posts/codeformer-docs.html)
