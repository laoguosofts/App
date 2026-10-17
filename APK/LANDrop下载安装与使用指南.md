## ✨ LANDrop 是什么

LANDrop 是一款**跨平台局域网文件传输工具**，核心功能是在同一 Wi-Fi 或个人热点下，让手机与电脑、手机与手机之间快速互传文件——不管对方是 iOS、Android、Windows、macOS 还是 Linux，只要两台设备能互相组播 mDNS（Bonjour），就能自动发现、一键发送。

它解决的痛点很实际：iPhone 不能直接给 Android 发文件、Windows 和 Mac 之间传文件没有系统级的 AirDrop 替代品、手机用热点连电脑时想传素材要走云盘或数据线。LANDrop 全程走局域网链路，不走蜂窝流量、不上传云端、不需要账号注册。

适合的场景包括：高铁上用 iPhone 开热点传素材给 Mac 剪辑、客户现场用手机开热点当临时局域网发 PPT、两台不同系统的手机互传照片和文档、桌面端把整个照片文件夹原封不动搬给另一台电脑。

LANDrop 的技术基础是 mDNS 组播和本地 TCP 连接，不依赖互联网——同一局域网内的设备组播一轮就能互相发现，传输速度取决于 Wi-Fi 带宽（5GHz 环境下可以到几十 MB/s）。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://landrop.hhdc.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：iOS 端 App Store（ID 1568444438）；Android 端 Google Play；桌面端 Windows / macOS / Linux 从官方下载页获取安装包

---

## 🔧 各平台安装要点

### iOS 端

iOS 版**只能走 App Store**，不支持侧载（sideload），即使通过开发者模式或 AltStore 也不稳定——原因是 LANDrop 内部依赖本地网络 mDNS 组播，iOS 对这个 API 有严格的沙箱限制。App Store 兼容性说明里写明了支持范围：iOS 13.0 / iPadOS 13.0 起的 iPhone 和 iPad，macOS 11.0 / M1+ Mac 也可以通过 Mac Catalyst 运行 iOS 版（visionOS 1.0 起还支持 Apple Vision Pro）。

**关键权限：本地网络访问**。iOS 14+ 强制要求 App 主动请求这个权限，LANDrop 首次启动一定会弹窗请求——必须点"允许"，否则同局域网内的发现服务完全无法工作。这个权限和"位置"权限不是一回事，LANDrop 不需要位置权限；如果启动时不小心点了"不允许"，之后到**设置 → 隐私与安全性 → 本地网络**里手动打开即可重新授予。

iOS 的后台模型是"墓碑式"——App 切到后台几分钟后就会被系统挂起、停止网络活动：传输过程中**不要手动把 LANDrop 切到后台**，它会被挂起导致传输中断；长时间不操作后再回到 LANDrop，它需要重新组播一轮才能恢复设备发现。

### Android 端

Android 端分发渠道较多，可以从 Google Play 或第三方应用市场获取安装包。安装时可能需要授予存储和本地网络权限——Android 13+ 对非媒体类文件的访问路径做了更严格的分区限制，建议用 LANDrop 自己的 Files 标签页查看所有接收的文件，最稳。

### 桌面端（Windows / macOS / Linux）

桌面端从官方下载页获取对应操作系统的安装包，直接运行安装程序即可。Windows 用户可能遇到杀毒软件误报（因为 LANDrop 要监听本地网络端口），添加信任后正常使用；macOS 用户首次打开可能需要在"系统设置 → 隐私与安全性"里手动允许来自开发者的应用。

> [!TIP]
> 安装完成后，建议先在同一 Wi-Fi 下用两台设备做一次小文件互传测试，确认设备能互相发现、传输正常，再用于大文件或文件夹传输。

---

## 📖 使用入门

### 设备发现与传输流程

LANDrop 的基本操作逻辑在所有平台上都类似：启动 App 后，它会在当前局域网内组播 mDNS 包，其他运行着 LANDrop 的设备会自动出现在发现列表里；选中目标设备、选择要发送的文件或文件夹、对方确认接收后传输开始。传输过程不依赖互联网，纯局域网链路，速度取决于 Wi-Fi 带宽。

### 文件夹原结构传输

v2.6.0（2024 年 8 月）引入了文件夹传输特性——在这之前要发一个文件夹得手动打包成 zip 再发，或者逐个文件挑，都很麻烦。v2.6.0 之后，桌面端可以直接把一个文件夹拖进 LANDrop，或者在文件选择器里选中文件夹，对方收到后**原目录结构完整保留**。

LANDrop 传输文件夹时，会把文件夹本身也作为传输的一部分——在接收端的保存目录下先创建同名的目标文件夹，然后把源文件夹里的每个文件按原相对路径写入。举例来说：

- 源路径：`D:/照片/2024/春节/IMG_0001.jpg`
- 接收路径：`~/Downloads/LANDrop/照片/2024/春节/IMG_0001.jpg`

完整保留。Android 和 iOS 端从 v2.x 起也支持**接收**文件夹，收到后整个目录结构原封不动落到目标位置；但**发送文件夹**主要还是桌面端在做——手机端的文件选择器通常以单个文件为中心设计，批量选文件夹不太方便。

文件夹传输极少失败，极个别情况可能是：文件夹里包含系统隐藏文件（Windows 的 `$RECYCLE.BIN`、macOS 的 `.DS_Store` 等，v2.6 开始会自动跳过）、文件夹过大超过设备存储空间、子目录层级过深或文件名含特殊字符。

### 接收文件的保存位置

不同平台的默认保存路径有差异，桌面端可以自定义：

| 平台 | 默认保存路径 | 自定义 |
| --- | --- | --- |
| Windows | `C:\Users\<用户名>\Downloads\LANDrop\` | ✅ 设置 → Receiving → Save to |
| macOS | `~/Downloads/LANDrop/` | ✅ 同 Windows |
| Linux | `~/Downloads/LANDrop/` | ✅ 同 Windows |
| Android | 照片/视频→系统相册；其他文件→LANDrop 内部存储 | ❌ 用 LANDrop Files 标签页查看 |
| iOS | 照片/视频→系统相册；其他文件→Files 应用→我的 iPhone→LANDrop | ❌ 沙箱限制，只能落到容器目录 |

iOS 的沙箱机制决定了它的路径逻辑和 Android 略有不同：LANDrop 不能随便把文件写进系统相册或文件应用——照片/视频自动调用系统相册 API 归档，其他文件落到 LANDrop 自己的容器目录，在 Files 应用里点开就能预览或转发给其他 App。v2.4.0 起还专门支持了 **Live Photo** 的完整保存，之前的版本只能保存静态帧。

> [!NOTE]
> 如果保存目录里已经有同名文件，LANDrop 不会静默覆盖——v2.0 起引入的行为是自动在文件名和扩展名之间加时间戳后缀（`report.pdf` → `report_20240920_153045.pdf`），所有平台一致。

### 个人热点场景：手机开热点也能用

当你打开 iPhone 的"个人热点"或 Android 的"移动热点"时，手机本身就变成了一个**无线接入点（Wi-Fi Access Point）**：手机在 2.4GHz 或 5GHz 上广播一个 SSID，电脑或平板连上去后，手机给它分配一个局域网 IP（通常是 192.168.43.x 或类似的私网段）。对 LANDrop 来说，这种场景和"家里一台路由器 + 几台设备"没有任何区别——局域网就是局域网，不管那个"路由器"是台手机还是一台真正的网络设备。

传输过程中，电脑和手机之间的文件走的是 Wi-Fi 局域网链路，运营商的蜂窝网络完全没看到这些包，所以**不计入手机流量**。但如果电脑同时用热点访问互联网，那部分流量还是会走蜂窝网络——这和 LANDrop 无关。

典型场景包括：高铁上 iPhone 开热点，MacBook 连上，LANDrop 把 iPhone 上拍的素材传给 MacBook 剪辑——全程零蜂窝流量；客户现场 Wi-Fi 连不上或不信任，用手机开热点当临时局域网，LANDrop 把 PPT 从笔记本发给客户电脑；两台手机互传文件，手机 A 开热点、手机 B 连上，同样零流量。

> [!WARNING]
> 老款 iPhone（iPhone 11 及之前）的个人热点默认走 2.4GHz，速度会慢一些；iPhone 12+ 支持 5GHz，更适合大文件传输。如果电脑同时在跑大量外网下载或流媒体，蜂窝网络被占满时可能影响热点 Wi-Fi 稳定性——LANDrop 传输会变慢但不会失败，只是等待时间更长。

---

## 🔗 相关资源

- [LANDrop官网](https://landrop.hhdc.tech)
- [LANDrop文档](https://landrop-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/LANDrop)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/LANDrop)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/LANDrop)

---

## 📚 相关文章

- [LANDrop 快问快答：AirDrop 的跨平替到底行不行](https://laoguosofts.github.io/posts/landrop-docs.html)
- [LANDrop 快问快答：跨平台 AirDrop 到底能不能打](https://alipymanbu.github.io/posts/landrop-docs.html)
