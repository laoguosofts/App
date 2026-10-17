## ✨ Adobe Firefly 是什么

Adobe Firefly 是 Adobe 推出的生成式 AI 创意工具集合，覆盖图像生成、视频生成、音频生成、3D 材质生成等多个创作领域。它的核心定位是"商用安全"——所有训练数据都经过授权筛选，输出自动附带 Content Credentials（内容凭证），适合设计师、视频创作者、营销团队直接商用而不必担心版权纠纷。

Firefly 目前提供两种主要使用形态：一是独立的网页端工作台（firefly.adobe.com），二是集成在 Creative Cloud 应用里的生成式功能面板——Photoshop 的生成式填充、生成式扩展，Premiere Pro 的文本生成视频、音频修饰，Illustrator 的生成式重新着色等。网页端适合从零创作，应用内集成适合基于已有素材做局部修改。

适合用 Firefly 的场景包括：概念草图快速探索、产品图背景替换、视频分镜预览、社交媒体素材批量生成、网页 Banner 设计、音乐背景片段生成等。如果你已经在用 Adobe 全家桶，Firefly 的生成式功能可以直接嵌入现有工作流，不必切换到第三方 AI 工具。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://adobe-firefly.qjxz.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Adobe Creative Cloud 通过官网分发，桌面端访问 Adobe 官网下载 Creative Cloud 客户端，登录后在应用列表中找到 Firefly 或直接使用 Photoshop / Premiere Pro 等应用内的生成式功能；移动端可通过 App Store 或 Google Play 搜索 Adobe Express 获取部分 Firefly 功能

---

## 🔧 安装步骤

Adobe Firefly 没有独立的安装包，它以两种方式交付：Creative Cloud 订阅内的集成功能，以及网页端直接访问。

### 桌面端安装（Creative Cloud 路线）

1. **注册 Adobe 账号**——访问 Adobe 官网注册免费账号，完成邮箱验证。
2. **下载 Creative Cloud 客户端**——官网下载页面选择 Windows 或 macOS 版本，运行安装程序。
3. **选择订阅方案**——登录后在客户端内选择 Firefly 单独订阅（Firefly Premium）或全家桶订阅（All Apps），按月或按年付费。
4. **安装应用**——在 Creative Cloud 客户端的应用列表中，安装 Photoshop（2024 及以上版本）、Premiere Pro（2024 及以上版本）或直接安装 Firefly 独立应用。安装路径可以自定义，建议保留默认路径避免权限问题。
5. **激活生成式功能**——打开 Photoshop 后，菜单栏 Edit → Generative Fill 即可使用生成式填充；或在右侧属性面板找到 Generative Expand、Generative Recolor 等功能入口。

### 网页端直接使用

不需要安装任何软件，直接浏览器访问 firefly.adobe.com，用 Adobe 账号登录后即可进入工作台。网页端的好处是跨设备、零环境配置，适合临时快速出图或在非主力设备上工作。

> [!WARNING] 版本核对
> Adobe 生成式功能需要 Photoshop 2024（版本 25.0）或更高版本，旧版本不支持 Generative Fill / Generative Expand。如果你的 PS 里找不到生成式功能入口，先检查版本号——Help → About Photoshop。

### credits 与套餐

Firefly 的生成资源以 credits（点数）形式计量，不同模型和不同分辨率消耗不同数量的 credits。快速模式出低分辨率变体消耗较少，高精度直接出 2K+ 消耗较多。套餐从免费试用到 Premium 不等，免费试用通常包含有限的 credits 额度供体验。

---

## 📖 Adobe Firefly 使用入门

### 网页端工作台：快速模式 vs 高精度生成

网页端工作台是从零创作的主力界面，最常用的模型选择是快速模式和高精度生成的搭配。

快速模式开启后，Firefly 会先用 512×512 分辨率出四组变体。它最适合三种场景：简单主题和背景、大多数插图和人物特写、需要快速探索大量方向的概念草稿阶段。如果你脑子里只有模糊方向，快速模式的价值巨大——出四组变体只需几秒，你可以不断改提示词、切纵横比、换参考图，在一分钟内跑出十几组方向，比一次等 2K 生成快得多。

快速模式的标准工作流：

1. 在模型部分选 Firefly Image 3 或 Image 4（注意：Image 5 不支持快速模式，直接走高精度）
2. 开启快速模式开关，定好纵横比、内容类型
3. 粗写提示词先跑一版
4. 看四组变体里哪组接近你要的方向，点击"用作构图参考"或"用作风格参考"
5. 基于这个参考微调提示词再跑一次——通常 2-3 轮就能锁定方向
6. 锁定后关快速模式，换 Firefly Image 5 走高精度生成

> [!TIP] credits 消耗技巧
> 快速模式本身消耗的 credits 比高精度少。"全部放大"（一次性升级四组变体到 2K）会额外消耗一个 credit，但比你单独对每张做放大要划算。如果提示词已经很精确了，直接走高精度更省——避免先跑快速模式再升级的双重消耗。

### Photoshop 集成：生成式填充与扩展

如果你已经有一张照片，需要局部修改或扩展画布，直接在 Photoshop 里用 Firefly 的生成式功能。

**生成式填充**是在图像内部选一个区域编辑——用套索工具框选要修改的部分，然后在上下文任务栏选 Generative Fill，写描述（比如"换成海滩背景""加一只猫"），选模型点生成，几秒钟出 3-4 组变体供你挑选。

**生成式扩展**是它的兄弟功能，专门用于扩画布：用裁剪工具拉到图像外，选 Generative Expand，写描述或留空让 AI 自动续画。常见场景包括：把竖屏照片裁成横屏做封面、原照构图太紧想让 AI 在边缘扩展更多背景、把窄产品图扩展成宽屏 Banner。这些场景用生成式扩展比手动克隆图章快 10 倍，接缝处也自然得多。

两者共享同一套模型路由和 credits 消耗，但扩展的模型针对"边缘续画"做了专门优化，效果比用填充硬扩好很多。如果扩展出来的边缘和原图接缝明显，可以换 Firefly Image 5 模型试试，或者在提示词里加上"保持与原图相同的光照和纹理"。

### 视频生成：合作伙伴模型选择

Firefly 的视频生成功能集成了多家合作伙伴模型，各有专长：

| 模型系列 | 出品方 | 核心优势 | 适用场景 |
| --- | --- | --- | --- |
| Kling | 快手 | 中文语义理解最强 | 提示词含成语、网络用语、中国文化意象 |
| Ray | Luma | 电影质感、光影细腻 | 商业广告、影视项目 |
| Veo | Google DeepMind | 运动一致性好、不闪烁 | 人物运动场景、需要主体稳定 |
| Runway Gen-4.5 | Runway | 可控性强、长视频 | 精细 storyboard、镜头运动控制 |
| Gemini Omni Flash | Google | 速度最快 | 批量概念探索 |

Kling 系列里，Kling 2.5 Turbo 速度最快适合快速探索，Kling 3.0 物理模拟准确人物动作自然，Kling 3.0 Omni 支持更长视频时长。Ray 系列的 3.14 / 3.14 HDR 是最新版，商业广告和影视项目首选。Veo 3.1 综合能力强，Veo 3.1 Fast 速度快 2-3 倍适合迭代。

> [!NOTE] 商业安全提醒
> Firefly 平台上部分合作伙伴模型的输出也会自动附加 Content Credentials，但版权归属和可商用性可能和 Adobe 自研模型不同。正式商用前建议核对最新的产品条款。

### 音乐生成：低成本配乐方案

Firefly Music 是视频、播客、社交媒体等场景的低成本配乐方案，可以生成约 1-2 分钟的短片段背景音乐，避免版权问题。

基本用法：在 Audio 面板下拉到 Generate Music，描述你想要的音乐风格和情绪（比如"轻快的民谣吉他伴奏，适合旅行 Vlog""深夜电子氛围音乐，放松冥想"），选时长、BPM 范围、情绪倾向，点 Generate 返回 2-3 段音乐片段。试听后下载 WAV，或直接导入 Premiere / Audition 继续编辑。

适合用音乐生成的场景：社交媒体短视频背景音乐（15 秒-1 分钟）、播客片头片尾过渡音乐、演示文稿 / 产品发布视频的轻配乐、游戏背景氛围层。但它不适合替代专业作曲——生成的音乐结构简单、和弦循环为主，没有完整的歌曲结构；目前也不支持人声演唱。

套餐配额方面，Firefly Standard 每月 100 分钟，Pro 200 分钟，Pro Plus 500 分钟，Premium 2500 分钟。

---

## 🔗 相关资源

- [Adobe Firefly官网](https://adobe-firefly.qjxz.skin)
- [Adobe Firefly文档](https://adobe-firefly.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/Adobe%20Firefly)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/Adobe%20Firefly)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/Adobe%20Firefly)

---

## 📚 相关文章

- [Adobe Firefly 新手入门：0基础也能5分钟跑出AI创作](https://laoguosofts.github.io/posts/adobe-firefly.html)
- [Adobe Firefly 从零上手——五分钟跑通第一次 AI 生图](https://alipymanbu.github.io/posts/adobe-firefly.html)
