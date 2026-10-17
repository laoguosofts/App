## ✨ EZCast 是什么

EZCast 是一套让手机、平板、笔记本把画面和声音投到大屏电视或投影仪的软硬件解决方案。硬件是一枚比 U 盘大不了多少的同屏器，插在电视的 HDMI 口上就能接收来自各种设备的投屏信号；配套的 EZCast App 在 Android、iOS、Windows、macOS 上都有，负责发现同屏器、建立连接、控制各种功能。

EZCast 的一个核心特点是**支持多种投屏协议**：自家的 EZCast Mirror 协议、行业通用的 Miracast（EZMirror）、Apple 的 AirPlay（EZAir）、以及 DLNA——这意味着不管你手里是安卓手机、iPhone、Windows 笔记本还是 Mac，基本都能找到一条投屏路径，而且很多场景下不需要装 App 就能直接用系统原生协议。

产品线从两百元出头的入门款一直延伸到上千元的 Pro 企业系列。入门款足够家庭追剧看视频，Pro 系列支持四分屏同时投射、Host 主持权、文档批注这些办公场景的硬需求。App 本身也分普通版和 Pro 版，Pro App 要搭配 Pro 硬件才能解锁全部功能。

从用户画像来看，EZCast 主要覆盖四类人群：**家庭影音档**想把手机视频投到客厅电视、**办公演示档**需要会议室里多人同时投屏、**Apple 生态档**不想装额外 App 直接走 AirPlay、**硬件折腾档**喜欢刷固件调设置。硬件型号决定了 App 里哪些菜单项会出现，挑硬件之前先对号入座自己是哪一档用户。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://ezcast.hfdk.space/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play / App Store 搜 EZCast；Windows / macOS 桌面版可在官网下载

---

## 🔧 各平台安装要点

### iOS / iPadOS

EZCast 在 App Store 可直接搜索下载，开发者是 WinnerWave。首次启动时系统会请求"连接本地网络"和"访问相册"权限，**全部允许**——不授权的话同屏器发现不了。另外 App Store 里还有一个独立的 EZCast Pro App，两者功能不重叠，Pro App 需要搭配 Pro 系列硬件。

> [!TIP]
> 绝大多数 iOS 用户**不需要装 EZCast App**。如果只是想镜像画面或投视频，同屏器切到 EZAir 模式后，iOS 控制中心的"屏幕镜像"就能直接发现同屏器，全程走系统原生 AirPlay，不装任何 App 也能完成 95% 的投屏场景。只有需要固件升级、修改同屏器设置、用 EZChannel 视频聚合或文档演示批注时才必须装 App。

### Android

Google Play 搜索 EZCast 即可安装。Android 上需要额外注意"未知来源"授权——如果从官网下载 APK 安装包，需要在系统设置里允许安装未知来源应用。首次启动同样要授权"位置信息"和"附近设备"权限，因为 Android 12+ 把 Wi-Fi 扫描和附近设备发现拆成了独立权限。

### Windows / macOS

桌面版从官网下载安装包。Windows 版本首次连接同屏器时可能被 Defender 或其他杀毒软件误报——这是因为 EZCast Mirror 协议需要建立本地 Wi-Fi 直连，被某些安全软件识别为可疑网络行为。允许后后续就正常了。macOS 版本在最新系统上需要在"系统设置 → 隐私与安全性"里允许屏幕录制权限，否则 Mirror 功能抓不到画面帧。

> [!WARNING]
> **安装后核对两件事**：一是 App 打开后能否自动扫描到同屏器（列表里出现设备名且右上角有绿色连接标识），二是 Settings 里显示的固件版本号——如果固件太旧，建议先手动升级一次再开启自动升级。

---

## 📖 从镜像到固件升级的完整使用指南

### 画面镜像：最快的投屏方式

EZCast App 右上角那个带方框的 ✱ 图标是**自家 Mirror 协议的入口**，点它会把手机或电脑画面实时编码后通过同屏器的 Wi-Fi 投到电视上。操作很简单：打开 App 确认同屏器已连接 → 点右上角 ✱ → 手机弹出"开始截取屏幕"确认 → 电视上 1~2 秒内出现画面。桌面版点 Mirror 按钮同屏，点 Extend 按钮当扩展屏用。

关于 Mirror 有一个很多人踩的坑：**Android 上 Mirror 按钮只有画面没有声音**。这不是 bug，是 Android 系统从 Android 10 起 MediaProjection API 的硬性限制——App 抓不到音频输出。要声音就得跳过 Mirror 按钮，Android 走 EZMirror（Miracast），iPhone 直接走 EZAir（AirPlay），或者只是看视频的话用 Video 菜单的 DLNA 方式（视频和音频同时投）。iOS 上的 Mirror 按钮走的是系统 AirPlay 栈的子集，是有声音的，不存在这个限制。

Mirror 的画质和延迟可以在 Settings 里切换：High Quality 模式用高码率 H.264 编码，画面锐利但延迟稍高，适合演示和幻灯片；Low Quality 模式降低分辨率和码率，延迟更低适合互动操作但画面会模糊一点。停止镜像时**不要直接关 App**——App 关了投屏进程可能残留。手机下拉通知栏点"停止投射"，桌面版在 App 里点 Stop 或回到 Idle 才是正确姿势。

### 四类用户各有主力功能

| 用户类型 | 主力功能组合 | 推荐硬件 |
| --- | --- | --- |
| 家庭影音档 | Mirror 临时镜像 + Video/Photo/Music 本地投射 + DLNA NAS 直投 + EZChannel 自动播放 | EZCast 2 / Ultra / Opus |
| 办公演示档 | Pro App 四分屏 + EZNote 批注 + Document 文档演示 + Cast Code 房间密码 | Pro Box / Pro Dongle II |
| Apple 生态档 | EZAir 直接 AirPlay，偶尔用 EZChannel | 全系列均可 |
| 硬件折腾档 | 全套 Settings + OTA 升级 + 工厂重置 + Pro Host Control | Pro Box + Pocket 双持 |

多人会议频繁的选 Pro Box，便携出差选 Pro Dongle II。只是想把手机视频投客厅电视追剧的话，入门款 Pocket Plus 就够用。

### iOS 专属选择：AirPlay 还是装 App

iOS 用户面对的是一个选择题——AirPlay 全原生不用 App，EZCast App 功能更全但需要额外安装。具体对比如下：

| 场景 | AirPlay（不装 App） | EZCast App |
| --- | --- | --- |
| 手机画面镜像 | ✅ | ✅ |
| 视频投射 | ✅ | ✅ |
| 照片轮播 | ❌ | ✅ |
| 文档演示 + 批注 | ❌ | ✅ |
| EZChannel 聚合 | ❌ | ✅ |
| 固件升级 | ❌ | ✅ |
| 修改同屏器设置 | ❌ | ✅ |

EZAir 模式下 AirPlay 投屏的路径是：同屏器通电、电视显示待机画面 → iOS 控制中心点"屏幕镜像"→ 列表里出现同屏器设备名 → 点一下 1 秒内画面就出来了。停止时再次点"屏幕镜像"选"停止镜像"。Mac 上在菜单栏 AirPlay 图标里选同屏器即可。

> [!NOTE]
> iOS 版 App 没有 EZMirror 菜单是产品设计不是 bug。iOS 原生不支持 Miracast 协议，EZCast App 也不能强行开启。iOS 用户需要传声音的镜像直接走 EZAir 就好。另外 iOS 的 EZCast App 演示 Office/PDF 文档需要通过 Finder 或 iTunes 文件共享把文件同步进去——macOS Catalina 以后用 Finder 的"设备 → 文件 → EZCast"拖文件，或者"文件"App 的 iCloud Drive，邮件附件也能直接打开。

### 固件升级：自动 vs 手动

EZCast 同屏器支持自动升级——Settings → Upgrade 里有一个 Auto Upgrade 开关，开启后同屏器每天后台检查新版本、下载完在凌晨 2:00~6:00 空闲时段自动刷进去。但这个开关默认**可能没开**，需要手动确认一下。

自动升级生效需要三个条件同时满足：同屏器保持通电（如果只在投屏时插 USB 供电投完就拔，永远触发不了）、已绑定路由器 Wi-Fi（Settings → WiFi router）、Wi-Fi 信号足够强（固件文件几 MB 到十几 MB，弱信号容易超时或损坏）。

| 维度 | 自动升级 | 手动升级 |
| --- | --- | --- |
| 需要做什么 | 打开开关然后忘了它 | 定期手动检查 + 点升级 |
| 触发时机 | 每天凌晨自动 | 你想起来时 |
| 可控 | ❌ 自动就刷不能推迟 | ✅ 弹通知可晚点刷 |
| 适合谁 | 普通家庭用户、懒人 | 企业 IT 管理员、想先看更新日志的 |

有几种情况建议关掉自动升级：公司 IT 环境有管控（IT 可能统一管固件版本）、用的是老旧稳定固件（某些老固件 Bug 少新固件反而引入新问题）、担心升级变砖（手动在白天升级更保险）、同屏器只在投屏时通电（开了也白开）。

自动升级完成后同屏器正常重启，待机画面不会弹任何提示——这是因为升级是后台悄悄刷的。验证是否成功的方法：进 Settings → Upgrade 看固件版本号，记一下过几天再打开看数字有没有变；或者 Pro 型号的同屏器可以在 Settings → System Info 里看系统日志，里面会有 "Auto upgrade completed" 的记录。

如果反复自动升级失败，手动进 Settings → Upgrade 点"检查更新"可以立即触发一次手动升级，App 会显示详细的错误日志，比自动模式的黑盒诊断更容易定位问题。同时检查一下 Wi-Fi 信号够不够强、USB 供电是不是稳定 5V/1.5A（换原装电源试试），实在不行恢复出厂设置后再开自动升级。

---

## 🔗 相关资源

- [EZCast官网](https://ezcast.hfdk.space)
- [EZCast文档](https://ezcast.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/EZCast)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/EZCast)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/EZCast)

---

## 📚 相关文章

- [EZCast用了仨月踩的五个坑——以为是坏了，其实全是设置问题](https://laoguosofts.github.io/posts/ezcast.html)

- [EZCast 同屏器避坑指南——用了两年踩过的那些要命的坑](https://alipymanbu.github.io/posts/ezcast.html)
