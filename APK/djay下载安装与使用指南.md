## ✨ djay 是什么

djay 是由 Algoriddim 公司开发的跨平台 DJ 打碟软件，覆盖 macOS、Windows、iOS、Android、visionOS 乃至 Meta Quest VR 六大平台，是目前移动端 DJ 生态里份额最大的数字打碟工具。它的定位很明确——**让普通人也能像专业 DJ 一样打碟**，但同时也为进阶用户留足了空间。

核心功能集中在三个方面：一是**双甲板混音**，模拟传统 DJ 的"打碟"工作流，两个独立的播放通道可以同时放歌、各自调速、交叉淡入淡出；二是 **Neural Mix Stem 分离**，这是 djay 最突出的 AI 能力——把一首歌实时拆成人声、鼓组、贝斯、其他四个音轨，可以单独静音或单独调整音量，做现场 remix 和 acappella 提取都靠它；三是**流媒体直连**，目前完整接入了 Apple Music、Spotify、TIDAL、SoundCloud、Beatport LINK、Beatsource LINK 等 7 个曲库，总曲目超过 1 亿首，打开软件就能直接搜歌打碟，不需要提前下载到本地。

djay 适合三类人：**入门小白**——界面直觉，有转盘视图可以像玩真机器一样搓盘，Automix 自动混音功能甚至能让软件帮你完成过渡；**进阶爱好者**——波形甲板视图配合 Neural Mix，可以做精细的 cue 点、Loop、Stem 分离；**移动端 DJ**——手机/iPad + 蓝牙 DJ 控制器就能出门跑场，不用扛笔记本电脑。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://djay.uerg.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：iOS / iPadOS 搜索 djay 直接在 App Store 安装；Android 在 Google Play 搜 djay；macOS 在 Mac App Store 可下载；Windows 版本在官网分发渠道获取

---

## 🔧 安装步骤（Mac / Windows / 手机全覆盖）

### 桌面端（Mac / Windows）

**macOS 用户**：两种渠道——Mac App Store 直接搜索 djay 一键安装，或者从官网下载 `.dmg` 镜像。双击 `.dmg` 后把 djay 图标拖进「应用程序」文件夹即可。首次打开如果系统提示"已损坏"，右键 → 打开 → 确认，是 macOS 的 Gatekeeper 保护机制，不是软件有问题。

**Windows 用户**：从官网下载 `.exe` 安装包，双击运行安装向导。默认安装在 `C:\Program Files\djay\`，建议保持默认路径避免后续权限问题。安装过程中如果被 Windows Defender 拦截（误报是 DJ 软件的常见情况），点「更多信息」→「仍要运行」即可。

> [!WARNING]
> 安装完成后务必在应用内检查版本号（设置 → 关于），确认是最新版。djay 的 Spotify 集成、Neural Mix 等核心功能都有明确的最低版本门槛，老版本可能缺失关键特性。

### 移动端（iOS / Android）

**iOS / iPadOS**：直接在 App Store 搜索 djay，点「获取」即可。iOS 没有侧载渠道，所有版本都通过 App Store 分发。iPad 版和 iPhone 版是同一个 App，会根据屏幕尺寸自动适配界面。

**Android**：在 Google Play 搜索 djay 安装。如果你的手机没有 Google Play（部分国内 ROM），可以从官网下载 `.apk` 侧载。侧载前需要在系统设置里允许"未知来源"应用安装：设置 → 安全 → 打开"允许安装未知来源应用"。

> [!NOTE]
> Android 10+ 对 USB MIDI 和蓝牙 MIDI 原生支持较好，老版本 Android（8 / 9）可能需要额外权限或兼容层。

### 装完核对

安装完成后建议做三件事确认一切正常：

1. **启动软件 → 检查版本号**：桌面端设置 → 关于；移动端设置 → 通用 → 关于
2. **试播放一首歌**：随便找一首本地音乐或连一下 Apple Music / Spotify，确认能正常出声音
3. **检查 Neural Mix 是否可用**：加载一首本地文件到甲板，看波形区域有没有出现彩色分离波形（需要 PRO 订阅或试用版）

---

## 📖 djay 快速上手指南

### 界面两种模式：转盘 vs 波形甲板

djay 桌面端提供**两种完全不同的界面布局**，对应两种 DJ 工作流：

**转盘视图（Turntable View）** 模拟传统 DJ 的经典布局——左右两个旋转的黑胶转盘 + 中间混音器。转盘会跟着播放实时转动，外圈有节拍光点跟着 beat 闪烁，唱片中央显示歌曲封面、曲目名和 BPM。支持用鼠标或手指直接在唱片上搓盘（Scratch 手势），Jog Wheel 可以点按做精准 cue、旋转调 pitch bend。

**波形甲板视图（Waveform Deck View）** 把高分辨率波形并排展示在中央，混音器在下方，媒体库在侧边。每个波形是彩色的——开启 Neural Mix 后会显示人声、鼓组、贝斯三条叠加的彩色波形，每个鼓点、drop 位置都清晰可见。两个波形并排，直接对比两首歌的结构就能找到最佳过渡点，波形下方还有迷你缩略条可以快速跳转。

| 你的背景 | 推荐视图 | 原因 |
|---|---|---|
| 纯新手，第一次碰 DJ 软件 | 转盘视图 | 直觉上"黑胶 → 黑胶"，像玩真实机器 |
| 用过 rekordbox / Serato | 波形甲板 | 熟悉并排波形的工作流 |
| 想练 Scratch / 搓盘 | 转盘视图 | 转盘有完整的 scratch 物理反馈 |
| 想用 Neural Mix 彩色波形 | 波形甲板 | AI 彩色波形在波形视图下最清晰 |
| 手机 / iPad 打碟 | 转盘视图 | 触屏上推唱片比拖波形更顺手 |

切换方式：桌面端顶部菜单栏 → 视图 → 切换视图模式（快捷键 `⌘+T` / `Ctrl+T`）；移动端左右三指滑动切换。

### 本地音乐文件导入

即使接了 1 亿+ 曲库的 Apple Music / Spotify，你仍然需要维护自己的本地音乐库——**Neural Mix Stem 分离、现场录音、USB 歌单导出这三个核心功能只认本地文件**，流媒体曲目做不了。

**支持的音频格式**：

| 格式 | 扩展名 | 说明 |
|---|---|---|
| MP3 | .mp3 | 最通用 |
| WAV | .wav | 无损，最推荐 |
| FLAC | .flac | 无损压缩，体积比 WAV 小 |
| AAC | .aac / .m4a | Apple 生态格式 |
| AIFF | .aiff / .aif | 无损，苹果/专业音乐制作 |
| OGG Vorbis | .ogg | 开源格式 |

> [!TIP]
> 推荐用 **WAV / FLAC** 格式——无损音质 + djay 的 Fluid Beatgrid 分析引擎对无损格式的 BPM / 节拍网格精度最高。

**桌面端导入最简单的方法是直接拖拽**：从 Finder（Mac）或 Explorer（Win）把一个文件夹直接拖到 djay 媒体库里，软件会自动扫描并分析 BPM、调式、节拍网格。也可以在设置 → 媒体库里点"添加文件夹"来指定路径，支持内置硬盘、USB 外置硬盘、NAS 网络存储。外置硬盘建议在启动 djay **之前**插上，软件启动时会自动扫描；启动后才插上的话需要手动在设置里刷新扫描。

**iOS 导入**：打开"文件" App → 进入 djay 文件夹 → `Music` 子文件夹，把音乐文件复制或拖放进去。Mac 用户可以用 Finder → iPhone → 文件 → djay → Music 直接传输；Windows 用户用 iTunes（或 Finder for Windows）的文件共享功能操作。iPhone 15+ 有 USB-C，直接插闪存盘也能通过文件 App 存到 djay 目录。

**Android 导入**：djay 首次启动会自动创建 `内部存储/DJayMusic/` 目录，把音乐文件复制进去，或者直接放到系统的 `内部存储/Music/` 目录，重新打开 djay 就会自动扫描。Android 还支持 USB OTG——插上外置硬盘或闪存盘后在设置 → 媒体库 → "添加 USB 存储"即可。

扫描速度取决于文件数量和设备性能：桌面端 1000 首歌大约 5-15 分钟，移动端 20-40 分钟。扫描期间可以继续用软件打碟，新扫描完的歌会自动出现在媒体库。

### Spotify 接入详解

djay 从 v5.4.3 开始完整支持 Spotify 集成，桌面端和移动端都能连，但有几个硬性门槛需要注意：

| 平台 | 最低 djay 版本 | 最低系统要求 |
|---|---|---|
| macOS | **djay 5.4.3+** | macOS 10.15 Catalina+ |
| Windows | **djay 5.4.3+** | Windows 10 21H1+ |
| iOS | **djay 5.5+** | iOS 15+ |
| Android | **djay 5.5+** | Android 10+ |

> [!WARNING]
> Spotify 集成**只支持 Premium 订阅**——Spotify Free（免费版）的 API 不支持 DJ 软件集成，连不上。Spotify Family / Student 订阅也可以。

连接步骤很简单：打开 djay → 设置 → 音乐源 → Spotify → "连接"，浏览器会跳转到 Spotify 登录页，输入账号密码授权后回到 djay，媒体库就会出现 Spotify 标签。移动端可以选择"用 Spotify App 登录"直接跳转授权，或者用浏览器手动输入账号。

**Spotify 曲目在 djay 里的功能限制**和 Apple Music 类似，都是流媒体版权锁的问题：

| 功能 | Spotify 曲目 | 本地文件 |
|---|---|---|
| 打碟 / 混音 | ✅ | ✅ |
| Neural Mix Stem 分离 | ❌ | ✅ |
| Recording 录音 | ❌ | ✅ |
| USB 歌单导出 | ❌ | ✅ |
| Loop / Cue 点 | ✅ 基础 | ✅ 完整 |
| Automix | ✅ | ✅ |

Spotify 和 Apple Music 最大的区别是：Spotify **没有离线下载的本地文件**可以让 djay 扫描——Spotify 的缓存文件是加密的、不对外暴露，所以 Stem 分离、录音这些功能在 Spotify 曲目中**永远不可用**，没有"下载到本地再解锁"这条路。而 Apple Music 下载的 AAC 文件是真实存在磁盘上的，djay 可以扫描到（虽然 DRM 保护有时会影响 Neural Mix 分析）。

### MIDI 控制器连接

虽然鼠标或触屏也能打碟，但 DJ 控制器带来的实物操作体验是软件模拟没法替代的——两只手同时操作、触觉反馈、肌肉记忆的速度优势，以及物理 jog wheel 的搓盘手感，都是触屏做不到的。

djay 对控制器的支持分三层：

**第一层是自动识别**：Algoriddim 和 Pioneer DJ、Reloop、Numark、Hercules、Denon DJ 等主流厂牌合作，以下控制器插上后 djay 自动配置——不用装驱动、不用手动映射：

| 厂牌 | 代表型号 | 特点 |
|---|---|---|
| Pioneer DJ | DDJ-FLX4、DDJ-FLX2、DDJ-200、DDJ-400 | 蓝牙 / USB 双连接；DDJ-200 / FLX4 支持 iOS 蓝牙 |
| Reloop | Buddy、Mixtour Pro、Beatpad 2 | Reloop Buddy 有 Neural Mix 专属控制区 |
| Numark | MixTrack Go、Mixon 8 Pro、Mixstream Pro Go | 便宜入门款 MixTrack Go 蓝牙直连 |
| Hercules | DJControl Mix Ultra（CES 2025 Innovation Award） | 全无线、10 小时电池、超低延迟蓝牙 MIDI |
| Pioneer CDJ | CDJ-3000X、CDJ-2000NXS2 等 | 俱乐部级原生集成 |

**第二层是 MIDI Learn 自定义**：不在自动识别列表里的控制器也能用。打开 djay → 设置 → 硬件 → MIDI Learn → 选一个目标功能（比如"Deck 1 Loop In"）→ 在控制器上按下想绑定的按钮 → djay 自动识别 MIDI 信号 → 保存映射。可映射的功能覆盖播放控制、节拍控制、混音器 EQ、FX 效果器、Neural Mix Stem、Loop、Automix、Looper、Sequencer 等数千个。

**第三层是 class-compliant 免驱标准**：遵循标准 USB Audio / MIDI 协议的设备不需要厂商驱动就能被操作系统识别。macOS 和 iOS / iPadOS 原生支持，Windows 10 1903+ 和 Android 10+ 也原生支持 USB Audio Class。老版本 Windows 或非 class-compliant 控制器需要去厂商官网下载驱动。

**蓝牙控制器**是移动端 DJ 的福音——iPhone / Android 手机没有 USB 接口（或 USB-C 不够用），蓝牙解决了线材问题。支持蓝牙的 DJ 控制器包括 Pioneer DDJ-FLX4 / DDJ-200、Reloop Buddy、Numark MixTrack Go、Hercules DJControl Mix Ultra（纯蓝牙、10 小时电池）。好的蓝牙控制器延迟 < 10ms，和 USB 几乎没区别；便宜的蓝牙控制器可能有 20-50ms 延迟，不太适合打碟但可以用 Loop / Cue 等非实时操作。

蓝牙连接步骤：打开控制器电源 → 按住 Bluetooth 按钮 3 秒进入配对模式 → 手机系统设置 → 蓝牙 → 配对控制器 → 打开 djay → 设置 → 硬件，应该能自动检测到。如果没看到，重启 djay 和控制器后重新配对即可。

> [!NOTE]
> iOS 上用 USB-C 转接头插 DJ 控制器时，务必使用 Apple 官方的 Lightning to USB 3 Camera Adapter 或第三方兼容转接头。便宜转接头可能不带供电——DJ 控制器需要额外供电（某些型号需要插电源适配器）。

---

## 🔗 相关资源

- [djay官网](https://djay.uerg.quest)
- [djay文档](https://djay-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/djay)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/djay)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/djay)

## 📚 相关文章

- [手机电脑都能打碟？djay 小白五分钟上手指南](https://laoguosofts.github.io/posts/djay-docs.html)
- [第一次碰打碟软件？djay 五分钟带你从开门到出活](https://alipymanbu.github.io/posts/djay-docs.html)
