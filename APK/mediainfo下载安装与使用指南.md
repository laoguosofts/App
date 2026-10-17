## ✨ MediaInfo 是什么——媒体文件的听诊器

你有没有过这样的经历：下载了一个 4K HDR 电影文件，资源管理器只告诉你"这是个 MKV，12GB"——但你真正想知道的是它用的 HEVC Profile 是 Main 10 还是 Main Still Picture？到底是不是真 HDR（PQ 曲线还是 HDR10+）？码率平均多少、峰值多少？内建了几路音轨，哪些是 DTS-X 哪些是普通 AAC？字幕有几条？是 PGS 还是 SRT？

所有这些信息都实实在在地写在文件容器的内部结构里，但 Windows/macOS 的资源管理器、大多数播放器界面、甚至压缩软件都不会把它们暴露出来。MediaInfo 就是为"把这些暗箱里的数据完整摊开来"而生的工具——它分析、展示、导出文件的技术参数和标签信息，却不修改、不重编码、不转封装，纯粹是只读性质的听诊器。

MediaInfo 由 MediaArea 团队开发，核心维护者是法国工程师 Jérôme Martinez。项目始于 2007 年，最初只是一个命令行小工具，后来逐渐发展出图形界面、多平台版本、开发者库、以及在线版。项目以 BSD 2-Clause 协议开源发布，免费使用、免费获取源码，允许商业软件整合，只需要保留版权声明，没有传染性。

它不做什么也值得强调——MediaInfo 不编辑标签、不转码、不切封装、不提供文件预览或播放功能。它的定位非常清晰：先看清文件现状，再用其他工具动手。这个定位让它在影视收藏、视频创作、流媒体工程、音乐管理、甚至国家级数字化保存机构（美国国会图书馆、英国电影学院、欧洲广播联盟都在其工具链中引用 MediaInfo）的场景里都扮演着不可替代的角色。

MediaInfo 能把解析到的数据按文件内部结构分成五个区段——General（通用）、Video（视频流）、Audio（音频流）、Text（字幕流）、Chapters（章节），每个区段独立展示、独立可展开。除了技术参数，它还能识别并展示容器或流上附着的元数据标签，包括 ID3v1/ID3v2、Vorbis Comment、APE Tag、容器级 Tags 等。这些能力让它既能给人看，也能给机器读。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://mediainfo.bpfl.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 平台 Google Play 搜索 "MediaInfo"；F-Droid 上也有同名包，功能一致

---

## 🔧 各平台安装详解

MediaInfo 跨平台覆盖范围广——Windows、macOS、Linux、Android 都有原生版本。桌面端提供图形界面（GUI）和命令行（CLI）两套程序，移动端目前只有 Android 有官方版，iOS 暂未推出。

### Windows 桌面版（GUI + CLI）

从下载页下载 MediaInfo GUI 安装包（通常是 NSIS 安装器，体积约 20MB）。双击运行，一路下一步即可，安装路径默认在 `C:\Program Files\MediaInfo\`，可以自定义。GUI 安装包会自动附带 CLI 可执行文件 `mediainfo.exe`，同时注册右键菜单——以后任何媒体文件上右键都能直接选 "MediaInfo" 打开，无需先启动程序。

> [!WARNING]
> 安装时如果有杀毒软件拦截，属于正常现象——MediaInfo 要注册文件关联和右键菜单，被误报的概率不低。在杀毒里放行即可，或暂时关闭实时防护。

CLI 独立包也可以单独下载（便携版 zip），解压后直接用 `mediainfo.exe`，不需要安装，适合服务器或不想装 GUI 的场景。

### macOS

macOS 版本以 dmg 镜像分发。双击 dmg，把 MediaInfo.app 拖进 Applications 文件夹即可首次启动。首次打开如果提示"来自未知开发者"，在系统设置 → 隐私与安全里点"仍要打开"就行。

CLI 版可以通过 Homebrew 安装：

```bash
brew install mediainfo
```

### Android

Android 版的官方渠道是 Google Play，搜索 "MediaInfo" 即可找到。F-Droid 上也有同名包，功能一致。

> [!NOTE]
> Android 版不需要特殊权限——它通过系统文件选择器读取媒体文件，不请求存储权限，不扫描全盘，隐私层面非常干净。

### 核对安装是否正确

装好之后，拿一个你手头的视频文件拖进 MediaInfo 窗口，如果能显示容器格式、时长、视频编码这些信息，说明安装成功。CLI 版本跑 `mediainfo --version` 能打印出版本号就是正常的。

---

## 📖 核心能力与使用入门

MediaInfo 把媒体文件内部的技术参数完整解析出来，按五个区段分层展示。这是它最核心的价值——下面逐层拆解它能告诉你什么。

### 五大信息区段详解

**General（通用区段）** 是整个文件容器层面的汇总，包括容器格式与配置（Matroska / MP4 / MXF / ...）、文件时长（精确到毫秒级）、文件大小与总体码率、编码写入程序（比如 "Lavc57.25.100" 表示用的 FFmpeg 2.5）、编码库名称与版本。这些信息告诉你"这个文件用什么容器装的、谁写出来的、总体规格是啥"。

**Video（视频流区段）** 是每一路视频流的完整技术参数——编码格式与编码 ID（H.264 / H.265 / AV1 / ProRes / VP9 ...）、编码 Profile / Level（比如 Main / High / Main 10 / High 4:4:4 ...）、分辨率与宽高比、帧率（标称值 / 实际值 / VFR 标记）、色彩空间（BT.601 / BT.709 / BT.2020）、色度采样（4:2:0 / 4:2:2 / 4:4:4）、位深（8 bit / 10 bit / 12 bit）、码率与码率模式（CBR / VBR / ABR）、HDR 元数据（色彩原色、白点、Mastering Display 亮度、Content Light Level）、扫描类型（逐行 / 隔行 + 场序）。

> [!TIP]
> 判断"是不是真 HDR"就看 Video 区段有没有 `transfer_characteristics` 和 `matrix_coefficients` 两个字段——如果 transfer 里显示 PQ 曲线，matrix 显示 BT.2020，那就是真 HDR（HDR10）。如果有 `mastering_display_*` 那一组字段，那就是 HDR10+。

**Audio（音频流区段）** 涵盖每一路音频流的编码格式（AAC / MP3 / DTS / Dolby Atmos / FLAC / Opus / ...）、通道数与通道布局（2.0 / 5.1 / 7.1.4 / Object-based ...）、采样率、位深、码率、语言标记（zh / en / ja / ...）、动态范围压缩标记、Dialogue Normalization 等。

**Text（字幕流区段）** 列出文件里每一条字幕流的格式（SRT / SSA / ASS / PGS / DVB Subtitle / Teletext / CEA-608 / ...）、编码 ID、语言标记、Forced 标记（强制字幕）。

**Chapters（章节区段）** 显示章节总数、每章的时间点与名称。

### GUI 六种查看模式

MediaInfo GUI 提供六种查看模式，适应不同场景：

| 模式 | 适用场景 |
|------|----------|
| 基础视图 | 默认视图，最重要的字段优先展示 |
| 表格视图 | 多文件并排对比最方便 |
| 树形视图 | 按容器结构层级缩进，适合技术分析 |
| HTML 报告 | 导出后在浏览器里看最漂亮，带编码设置分组 |
| 文本视图 | 完整原始数据，适合复制粘贴到工单 |
| 自定义视图 | 用 MediaInfo 字段语法拼接你自己的输出格式 |

切换方式：顶部工具栏的 "View" 菜单或 Ctrl+1~6 快捷键。

### 批量文件处理

打开一个文件夹，MediaInfo 可以一次性加载全部媒体文件，左侧列表逐文件切换查看。CLI 版本的批量能力更强——可以递归扫描目录，把数百个文件的参数汇总成一份 CSV 表格，方便你批量做编码参数合规检查。比如流媒体工程师上传到 CDN 之前，用 CLI 批量扫描一遍编码参数、码率、分辨率，在进入发布流程之前就把不合规的文件挡下来。

### 输出格式多样性

MediaInfo 不只是在界面上展示——它能把解析结果导出成多种格式：纯文本（人类可读）、HTML 报告（美观、带编码设置分组）、CSV 表格（可导入 Excel 做数据分析）、JSON / XML（结构化数据，方便程序解析）、自定义模板（用 MediaInfo 的字段语法拼接你自己的输出格式）。GUI 版在详情页点右上角三点 → 导出，CLI 版用 `--Output=` 参数指定格式。

### 码率曲线图插件（高级功能）

MediaInfo 的主程序显示码率的**平均值**，但一个视频文件的码率在时间轴上是不断变化的——VBR 编码的视频在快速动作场景码率会飙升、在静止场景会压低。Graph Plugin（码率曲线图插件）就是把这个变化画成一张图，X 轴是时间轴，Y 轴是码率，蓝色曲线是视频码率，红色曲线是音频码率。

**安装方式**：GUI 版打开 MediaInfo → Options → Preferences → Graph → 点 "Download graph plugin" 按钮，会自动下载安装。如果网络问题自动安装失败，从官方下载页下载 Graph Plugin 安装包手动双击即可。CLI 版的 Graph Plugin 是 zip 包，解压后把 DLL 放到 CLI 的安装目录下。

**实战价值**：码率曲线图能帮你快速判断编码质量。VBR 编码的曲线上下起伏明显，CBR 编码几乎是一条水平线。曲线的最高点是码率峰值——如果峰值远高于平均值，说明编码时给了较大的峰值预算；如果峰值和平均值差不多，说明编码时约束了峰值。

> [!NOTE]
> Graph Plugin 版本号和主程序版本号不同步——这是正常的，插件版本号独立维护。新版本（v24.11+）已经把 Graphviz 静态编进去了，不需要单独装 Graphviz。

### 典型使用场景

影视收藏爱好者管理 4K 片库时，MediaInfo 是判断一个文件值不值得留的最快方式——码率够不够、编码 Profile 是什么、有没有 HDR 元数据、音频轨有哪些规格。视频创作者编码完了再验一次输出，确保设置真的生效。流媒体工程师批量扫描编码参数做合规检查。音乐爱好者看 FLAC 的采样率、位深、Vorbis Comment 标签和封面嵌入情况。

---

## 🔗 相关资源

- [mediainfo官网](https://mediainfo.bpfl.quest)
- [mediainfo文档](https://mediainfo-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/mediainfo)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/mediainfo)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/mediainfo)

---

## 📚 相关文章

- [MediaInfo 到底值不值得掏钱——一份免费软件的"付费决策账本"](https://laoguosofts.github.io/posts/mediainfo-docs.html)
- [MediaInfo 到底值不值得掏钱？一笔账算明白](https://alipymanbu.github.io/posts/mediainfo-docs.html)
