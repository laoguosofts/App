## ✨ AudioRelay 是什么

AudioRelay 是一款跨平台的音频中继工具——简单说就是把你的手机变成电脑的扬声器、麦克风，或者反过来把手机的系统音频转发到电脑上。它走局域网通信，不需要互联网连接，延迟可以做到很低，适合打游戏、看电影、远程语音等场景。

核心能力有三块：把 PC/Mac 的声音推到手机播放（扬声器模式）、把手机物理麦克风接到 PC 当输入设备（麦克风模式）、把 Android 上其他 App 正在播的声音转发到别的设备（Apps source，iOS 版受系统限制不可用）。桌面端跑在 Windows 和 macOS 上，移动端覆盖 Android 和 iOS，支持多设备同时连接。

AudioRelay 走的是标准 mDNS 自动发现机制——同一局域网内打开两端，手机播放器 Tab 会自动扫到 PC 服务器。但它对网络环境比较敏感，路由器的 AP 隔离、访客网络、VPN 全隧道模式这些常见配置都会让自动发现直接挂掉，需要手动 IP 连接兜底。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://audiorelay.xnyx.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 端在 Google Play 搜 AudioRelay；iOS 版处于 Early Access 阶段，通过社区论坛 TestFlight 邀请分发；桌面端从官网下载安装包

---

## 🔧 安装步骤

### Android 端

从 Google Play 直接搜索安装是最省事的方式——商店版本已经过签名校验，不需要额外处理。如果是侧载 APK，需要在系统设置里允许「未知来源」安装，装完后第一次启动会弹出麦克风权限和通知权限请求，全部允许。

> [!WARNING]
> 有些国产 ROM（MIUI、ColorOS、OneUI 等）会在后台自动杀掉 AudioRelay 的前台服务。装完后去系统设置里把 AudioRelay 加到自启动白名单和电池优化白名单，否则手机锁屏后连接会断。

### iOS 端

iOS 版目前还在 1.0.0-alpha Early Access 阶段，只能通过 Apple TestFlight 安装。流程是：打开社区论坛 Announcements 板块找最新 TestFlight 邀请帖，点里面的 TestFlight 链接（会打开 App Store 的 TestFlight 应用），接受邀请后自动下载。TestFlight 有热门应用名额上限，满了就只能等扩容或新版本。

需要注意版本互兼容：iOS 1.0.0-alpha 只能连接 1.0.0+ 的对端，和当前 0.x.x 桌面端不互通。电脑上跑 0.27.x 的话 iOS 版连不上，要么升级桌面端到 1.0.0+，要么用 Android 设备。

### 桌面端（Windows / macOS）

官网下载对应系统的安装包运行即可。**安装向导里一定要勾选防火墙规则**——这是最容易漏掉的一步。不勾的话 Windows Defender 会默认屏蔽 AudioRelay 的端口，导致手机完全发现不到 PC。

如果已经装完才发现没勾，手动补放行：Windows 安全中心 → 防火墙和网络保护 → 允许应用通过防火墙 → 勾选 AudioRelay 的专用网络（Private）权限。

> [!TIP]
> 装完后确认 AudioRelay 主界面顶部显示了 `IP地址:59100` 格式的地址——比如 `192.168.1.50:59100`。如果 IP 显示 `127.0.0.1` 或完全空白，说明桌面端还没正确监听局域网接口，先检查防火墙和网络类型。

### 版本核对

不同端的版本号需要大致对齐才能稳定互通。建议桌面端和移动端都用最新的稳定版，iOS 版单独看 TestFlight alpha 版本是否和桌面端版本号匹配。

---

## 📖 使用入门

### 基本操作流程

打开两端 AudioRelay，确认手机和 PC 连在同一个 Wi-Fi（同 SSID 就行，2.4G/5G 同名也没问题）。手机端进播放器 Tab，等几秒服务器列表应该会自动出现你的 PC 名称，点一下就连上了。

连上之后有三种用法：

**扬声器模式**（PC/Mac → 手机）：手机自动变成 PC 的音频输出设备。Windows 右下角音量图标右键 → 声音设置 → 输出设备里选 AudioRelay，之后所有 PC 声音都从手机喇叭出来。适合 PC 没接音箱、或者想躺在沙发上看电影的场景。

**麦克风模式**（手机 → PC/Mac）：手机物理麦克风变成 PC 的输入设备。Windows 声音设置 → 输入设备里选 AudioRelay Microphone，之后 PC 的 Zoom、微信语音、游戏语音都走手机麦克风。

**Apps source**（Android 独有）：Android 端可以把其他 App 正在播放的系统音频转发出去。点主界面的 Apps source 按钮，系统会弹出 MediaProjection 权限请求，允许之后 YouTube、游戏音效、Apple Music 等所有 App 的声音都能被捕获并转发到 PC 或其他设备。iOS 版因为系统架构限制做不到全系统音频捕获，这个功能完全不可用。

### 自动发现失效怎么办

自动发现依赖 mDNS 广播包在局域网内转发。任何一层的隔离都会让广播到不了手机，排查按以下顺序来：

1. 确认两端在**同一个 Wi-Fi SSID**——不同 SSID 之间可能有隔离
2. Windows 网络配置文件必须是**「专用（Private）」**——Public 模式下 Windows 屏蔽 mDNS 广播
3. 确认手机没连**访客网络（Guest Wi-Fi）**——访客网络默认开 AP 隔离，设备间完全不能通信
4. 两端 AudioRelay 都在运行，桌面端 IP 显示正常，手机端前台服务通知存在
5. 防火墙有没有放行 TCP + UDP 59100 端口
6. VPN 有没有开「全隧道（Full Tunnel）」——如果开了，在 VPN 设置里找「Allow LAN traffic」打开

### 手动 IP 连接兜底

如果上面都试过了自动发现还是挂，手动 IP 连接是万能兜底方案。它绕过 mDNS 广播，直接发起 TCP 连接到 IP:59100，只需要两端网络层可达。

操作步骤：PC AudioRelay 主界面顶部记冒号前的 IP 地址（比如 `192.168.1.50`），手机 AudioRelay → 播放器 Tab → 滑到最底部「使用 IP 地址连接」区域，只填 IP 不带端口，点连接。几秒就能连上。

手动 IP 连接的局限是 IP 变了要重新填——路由器 DHCP 租约到期可能换 IP。想固定 IP 有三种办法：路由器里给 PC 的 MAC 地址做 DHCP 保留（推荐）、PC 上手动设静态 IP、或者手动 IP 连接处填主机名（但主机名解析依赖 mDNS，整体挂了的话大概率也不行）。

> [!NOTE]
> **手动 IP 能通但自动发现不能通** → 问题在 mDNS 广播被拦（UDP 5353），不在音频数据通道。这种情况手动 IP 就能日常用了，只是每次开 App 要手动填一下。

### 路由器隔离的隐形坑

路由器里有一些默认开启但没人注意的选项，会让同 Wi-Fi 下的设备互相看不见——AudioRelay 发现和连接全挂。不同品牌叫法不同：

| 路由器品牌 | 隔离选项名 | 说明 |
| --- | --- | --- |
| 通用 | AP Isolation / Client Isolation | 阻止同 AP 下设备互相通信 |
| 通用 | Guest Network Isolation | 访客网络设备被隔离 |
| 华硕 | AI Protection 中的设备隔离 | AI 防护包含隔离功能 |
| TP-Link | AP 隔离 / Guest Network Isolation | 访客网络设置页直接找 |
| 小米 | 防蹭网 / 设备隔离 | 某些型号叫防陌生人设备隔离 |

99% 的路由器访客网络默认开隔离，手动开的「防蹭网」也会把所有人都互相隔离。关掉隔离后设备间可以互相 ping 通，AudioRelay 就能正常工作了。关掉隔离对安全的影响是同 Wi-Fi 设备可以互相访问——家里设备都是自己信任的就没事。公共场所 Wi-Fi 建议用手机热点或 USB Tethering 代替。

快速验证方法：手机装个 ping 工具 App，ping PC 的 IP。ping 通说明网络层 OK，问题在 AudioRelay 防火墙或端口；ping 不通说明网络层就有隔离，回去查 VPN、AP 隔离、访客网络。

### USB Tethering 终极方案

如果家里网络配置实在调不通，USB 共享网络是最干净的绕路方案——彻底跳过 Wi-Fi、路由器、AP 隔离这些变量。PC 连 USB 后会有两个 IP：一个 Wi-Fi IP，一个 USB 网卡 IP（通常是 `192.168.42.x` 段）。手动 IP 连接时用 USB 的那个 IP，更稳、延迟更低。

### iOS 版的固有边界

iOS 版的核心限制是**做不了全系统音频捕获**——不是 AudioRelay 不想做，是 iOS 系统架构不允许。每个 App 的音频流严格隔离，系统不让任何第三方 App 捕获其他 App 正在播放的声音。所以 iOS 上没法把 YouTube、游戏音效等 App 的声音转发出去，只能接收其他设备的声音或者发送自己物理麦克风的声音。所有类似产品（AirPlay、Sonos 等）在 iOS 上也都有同样限制，这是 iOS 生态的固有边界。

---

## 🔗 相关资源

- [audiorelay官网](https://audiorelay.xnyx.quest)
- [audiorelay文档](https://audiorelay.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/audiorelay)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/audiorelay)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/audiorelay)

## 📚 相关文章

- [AudioRelay 这三年：从一根 USB 线到系统音频抓取](https://laoguosofts.github.io/posts/audiorelay.html)
