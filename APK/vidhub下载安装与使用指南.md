## ✨ VidHub 是什么 —— 智能影视媒体播放器

VidHub 是一款面向多设备的本地 + 云盘影视播放器，核心设计理念是「**一个应用管遍你的所有影视资源**」——不管文件存在手机本地、NAS、还是阿里云盘/115/百度网盘里，VidHub 都能直接访问并播放，无需把文件下载到设备上。

它的优势不只是能播，而是播得舒服：手势操作为主的交互体系让单手控制成为可能，在线字幕搜索解决了找字幕的痛点，4K HDR + Dolby Vision 的完整支持覆盖了高清观影需求，画中画和投屏功能则把屏幕利用率拉满。支持的平台覆盖了 iPhone / iPad / Android / Apple TV / macOS，跨设备观影体验统一。

对于 NAS 用户来说，VidHub 是 Emby/Jellyfin 之外更轻量的选择——不需要搭服务器、不需要刮削，绑定 WebDAV 就能直接播。对于追剧党来说，云盘直播省去了反复下载的麻烦，在线字幕一键匹配补全了音画之外最关键的一环。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://vidhub.fwen.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：iPhone/iPad 用户可在 App Store 搜索 VidHub 获取最新版；Android 用户在 Google Play 或各应用市场搜索即可；Apple TV 在 tvOS App Store 下载原生版；macOS 在 Mac App Store 或官网获取

---

## 🔧 安装步骤

### iOS / iPadOS 安装

iPhone 和 iPad 用户最简单，直接打开 App Store 搜索「VidHub」，点获取就能安装。首次打开时会请求通知权限和媒体库访问权限，按需授权即可。

> [!TIP]
> 如果 App Store 搜不到（地区限制），可以换一个非大陆 Apple ID 登录后再搜，或者用 TestFlight 渠道。

### Android 安装

Android 用户有几种途径：Google Play 直接装、应用市场搜包、或者从官网下载 APK 安装。安装时如果跳出「未知来源」提示，去系统设置里临时允许浏览器/文件管理器的安装权限，装完记得关掉。

### Apple TV 安装

在 tvOS App Store 搜「VidHub」，直接安装到 Apple TV。首次启动需要用手机端 VidHub 扫码配对登录，之后就能在电视上直接操作了。

### macOS 安装

Mac 用户可以在 Mac App Store 搜，或者从官网下载 dmg 包。打开 dmg 后把 VidHub 拖进 Applications 文件夹即可。首次启动如果 macOS 弹出安全提示，去「系统设置 → 隐私与安全性」里点「仍要打开」。

### 安装后核对

装完打开应用，进「关于」页面确认版本号是最新版（下载页有当前最新版本号），避免装到旧版本。如果之前装过，升级时应用会自动迁移数据，不用担心本地片库丢失。

---

## 📖 使用入门

### 播放器手势操作 —— 单手就能控制

VidHub 移动端的播放控制以手势为核心，习惯后效率极高，大部分操作不需要去点屏幕上的按钮：

| 手势 | 区域 | 功能 |
|------|------|------|
| 单指双击 | 画面左半侧 | 快退（默认 15 秒） |
| 单指双击 | 画面右半侧 | 快进（默认 15 秒） |
| 单指双击 | 画面中间 | 暂停 / 继续 |
| 单指滑动 | 画面左侧上下 | 调亮度 |
| 单指滑动 | 画面右侧上下 | 调音量 |
| 双指捏合 / 张开 | 画面 | 缩小 / 放大画面 |

快进间隔可以在「设置 → 播放 → 快进间隔」里自定义，常用回看字幕的话建议设为 5 秒或 10 秒。iOS 上关闭双击快进也可以，关掉后双击只负责暂停/继续。

桌面端（macOS）以键盘快捷键为主：空格暂停、⌘+←/→ 快进退、↑↓ 调音量、F 全屏、C 切字幕，还有 VLC 风格的 J/K/L（后退/暂停/前进）。

### 在线字幕搜索 —— 一键找字幕

VidHub 内置了 SubHD 字幕源集成，播放时如果视频自带音轨却没有字幕，可以直接让 VidHub 搜：

1. 播放中 → 右上角三个点 → 字幕
2. 选「在线搜索字幕」
3. VidHub 用视频文件名自动搜索，返回匹配的字幕列表
4. 每条结果显示语言、格式（SRT/ASS 等）、上传者，选一个点下载
5. 下载完自动加载到当前视频，字幕文件也会存到视频所在目录下次自动匹配

如果搜不到，换关键词试试——用英文原名比中文译名命中率更高（比如搜「Interstellar」比「星际穿越」准）。也可以手动去字幕网站下好 SRT/ASS 文件，用「本地加载」选进来。

> [!NOTE]
> 在线字幕来自第三方网站，VidHub 只是提供便捷的搜索下载入口，字幕版权归原翻译者和网站所有。

### 投屏到大屏 —— Apple TV / Chromecast / AirPlay to Mac

小屏幕看累了，想投到大屏上看，VidHub 支持多种投屏方案：

**AirPlay 到 Apple TV**：手机和 Apple TV 在同一 Wi-Fi，播放时点右侧面板最上方的投屏图标，选你的 Apple TV 就行。投屏后手机可以锁屏或切其他 App，视频继续在电视上全屏播放，手机手势还能控制快进快退和音量。

**Google Cast 到 Chromecast / Android TV**：同样需要同一 Wi-Fi，点投屏图标，VidHub 会自动发现附近的 Chromecast 和 Android TV 设备。注意 Chromecast 对 DRM 保护内容可能有限制，但本地视频和自建 NAS 的视频通常没问题，只是 4K HDR 投屏时可能降级。

**AirPlay to Mac / iPad**：macOS Monterey+ 支持作为 AirPlay 接收器，在「系统设置 → 通用 → AirPlay 与接力」里开启 AirPlay 接收器后，iOS 上就能把视频镜像到 Mac 屏幕。

| 投屏方式 | 前提条件 | 推荐场景 |
|---------|---------|---------|
| AirPlay → Apple TV | 同一 Wi-Fi，tvOS 11+ | Apple TV 用户首选，体验最好 |
| Cast → Chromecast | 同一 Wi-Fi，Google Cast 开启 | 非 Apple TV 大屏设备 |
| AirPlay → Mac/iPad | macOS Monterey+ | 少见但实用的多任务场景 |

### 画中画模式 —— 边看边刷

iOS 14+ 和 iPadOS 14+ 用户可以用画中画功能：切换到其他 App 时，视频会缩小成一个悬浮小窗口继续播放，大小可调、可以拖到屏幕四角。iPad 上还能配合 Split View 同时开两个 App，比如一边 Safari 查资料一边 VidHub 看教程。

### 高清格式支持 —— 4K HDR 全链路

VidHub 对高清格式的支持非常完整：

**4K UHD**：iPhone 12 Pro+、iPad Pro 2021+、Apple TV 4K 2021+、Mac M1+、Android 旗舰（骁龙 8 Gen 2+）都能完整解码 4K，配合支持 HDR 的屏幕或电视效果最佳。

**HDR 格式**：原生支持 HDR10（最常见）、Dolby Vision（动态元数据，每个场景独立优化色调）、HDR10+。注意需要播放设备和显示设备两端都支持，Apple TV 4K + Dolby Vision 电视是目前的最佳搭配。

**编码格式**：HEVC/H.265（4K/HDR 主流编码）硬件解码原生支持，AV1（新一代开放编码）在 M1+ 芯片和 iPhone 15 Pro+ 上有硬件加速，其他编码用双引擎回退机制保证能播。

**容器格式**覆盖了 MP4、MKV、MOV、AVI、WebM、TS/M2TS、VOB 等几乎所有主流格式，音频格式支持 FLAC、MP3、AAC、OPUS、WAV 等。

> [!WARNING]
> 云盘视频播放时务必选「原画」，用转码的话 4K 会被降成 1080p，HDR 也会丢失。高清格式播放必须打开硬件加速，关闭的话全靠 CPU 软件解码，大概率卡顿。

---

## 🔗 相关资源

- [vidhub官网](https://vidhub.fwen.ren)
- [vidhub文档](https://vidhub-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/vidhub)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/vidhub)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/vidhub)

---

## 📚 相关文章

- [VidHub 快问快答：NAS 党和追剧党都该知道的播放器](https://laoguosofts.github.io/posts/vidhub-docs.html)
- [VidHub 快问快答：多设备影视播放器 9 个最关心的问题一次说清](https://alipymanbu.github.io/posts/vidhub-docs.html)
