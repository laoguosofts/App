## ✨ PCAPdroid 是什么

PCAPdroid 是一款运行在 Android 设备上的**网络流量抓包与分析工具**。它的核心定位是让普通用户也能看清自己手机上的 App 到底在跟谁联网、发了什么数据，同时内置了 TLS 解密、防火墙拦截等实用能力。

和传统 PC 端的 Wireshark 不同，PCAPdroid 专门针对移动端场景做了大量适配——不需要 root 就能通过 VpnService 抓到所有 App 的出站流量，配合 mitmproxy 的 TLS 解密能力，可以直接看到 HTTPS 加密连接里的明文请求和响应。这使得它成为排查"哪个 App 偷偷上传隐私数据""为什么某个服务连不上"这类问题的首选工具。

PCAPdroid 的主要功能包括：实时连接监控（Connections 视图）、按应用分组查看流量（Apps 视图）、TLS 解密（需要安装自签证书）、防火墙拦截规则（按应用/域名/IP 三种粒度）、PCAP 文件导出、SOCKS5 代理模式等。对于安全研究人员、App 开发者、隐私敏感用户来说，它都是一款值得常备的工具。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://pcapdroid.fewz.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜索 PCAPdroid；F-Droid 也有开源版本分发

---

## 🔧 安装步骤

PCAPdroid 运行在 Android 设备上，安装过程与普通 App 类似，但因为涉及 VpnService 和 TLS 证书，有几个关键点需要注意。

### 下载与安装

1. 打开下载页，找到最新版本的 APK 文件
2. 将 APK 传到手机上，点击安装
3. 如果系统提示"未知来源"，需要在设置中允许该来源安装应用

> [!NOTE]
> 建议优先从官方渠道获取安装包，第三方镜像站点可能存在版本滞后或二次打包的风险。安装完成后可以在应用内查看版本号，与发布说明进行核对。

### 首次启动配置

第一次打开 PCAPdroid 时，需要完成以下初始化：

1. **授予 VpnService 权限**：系统会弹出 VPN 授权对话框，点击"确定"——PCAPdroid 通过 VpnService 机制接管所有网络流量，这是它能抓到 App 联网数据的基础
2. **安装 CA 证书（可选但推荐）**：如果需要解密 HTTPS 流量，需要将 PCAPdroid 的自签证书安装到系统证书存储中。证书安装路径在应用设置里有引导，Android 7.0+ 对用户证书有限制，部分 App 可能仍无法解密

### Root 模式说明

如果设备已 root，PCAPdroid 还提供 **Root Capture 模式**——直接从网卡层抓包，不受 VpnService 限制，可以抓到入站连接和系统进程流量。但 Root 模式下**防火墙功能不可用**，两者互斥，需要根据实际需求选择。

---

## 📖 使用入门

PCAPdroid 的界面设计以实用为导向，主要分为 Connections（连接列表）、Apps（应用分组）、Firewall（防火墙）三大视图。下面结合常见使用场景介绍核心操作。

### 实时监控 App 联网行为

打开 PCAPdroid 启动抓包后，进入 **Connections** 标签页就能看到所有 App 当前活跃的网络连接。每条连接显示目标域名/IP、端口、协议类型、流量大小和状态。

锁图标是判断连接加密状态的关键指示器：
- **绿色锁**：TLS 加密且已成功解密，可以查看明文请求内容
- **灰色锁**：TLS 加密但未解密（通常是没装证书或证书不被 App 信任）
- **橙色锁**：协议暂时不支持解密（QUIC、STARTTLS 等）
- **无锁图标**：明文连接（HTTP、DNS 等），直接可读

在 Connections 列表里可以使用顶栏的**漏斗图标（Edit Filter）**进行筛选——按应用、按协议、按是否被防火墙拦截，快速定位你关心的连接。长按某条连接可以弹出操作菜单，查看详细信息或直接添加拦截规则。

### 防火墙拦截规则

PCAPdroid 的防火墙把**流量可见性**和**流量阻断**结合在一起——你先看到一条连接，当场就能把它封掉，不像系统防火墙那样盲封端口。拦截规则支持三种粒度：

| 规则类型 | 效果 | 适用场景 |
|---|---|---|
| 按应用 | 整个应用的所有出站流量被阻断 | 离线 App 根本不需要联网 |
| 按域名 | 匹配域名和所有子域（如 example.org 会阻断 api.example.org） | 阻止特定广告 SDK、统计 SDK |
| 按 IP | 特定 IP 地址被阻断 | 精准拦截已知恶意 IP |

最常用的添加方式是在 Connections 列表里**长按一条连接**，从弹出菜单选择 Block app / Block host / Block IP。也可以在 Apps 视图长按整个应用快速拦截。

汉堡菜单进入 **Firewall** 面板可以管理所有规则：Status 页显示当前拦截统计和总开关，Blocklist 页列出所有已添加规则，支持批量删除和 JSON 导出。开启"Block newly installed apps"开关后，新安装的应用默认被拦截，需要手动放行——这对隐私敏感用户来说是一道很好的防线。

> [!NOTE]
> 防火墙功能依赖 VpnService 代理层，在 Root Capture 模式下不可用。两者互斥，需要根据当前任务选择合适的抓包模式。

### TLS 解密与已知限制

HTTPS 流量解密是 PCAPdroid 的核心能力之一，但受限于底层 mitmproxy 库的支持范围，有几种协议暂时无法解密：

**QUIC（HTTP/3）**：基于 UDP 的新传输协议，Google Chrome、YouTube、Gmail、Facebook 等已全面迁移。mitmproxy 的 QUIC 支持仍在开发中，遇到 QUIC 连接会显示橙色锁。目前唯一可行的变通方案是在 PCAPdroid 设置里开启**主动阻断 QUIC**开关——应用发现 QUIC 走不通会自动回退到传统 TLS 1.3，回退后解密就能正常生效。但某些应用强制 QUIC，被阻断后直接拒绝联网，这种情况暂时没有好办法。

**STARTTLS**：在明文协议连接中动态升级到 TLS 的模式，常见于 SMTP（邮件发送端口 587）、IMAP（邮件接收端口 143）、XMPP（即时通讯端口 5222）等传统互联网协议。mitmproxy 的 TLS 处理逻辑假设握手从连接一开始就要发生，还没适配这种"先明文、后升级"的时序差异。变通方案是让邮件客户端改用 SMTPS（465）/ IMAPS（993）等直接 TLS 端口，PCAPdroid 就能正常解密。

### 非 Root 模式抓不到入站连接

在 VpnService 模式下，PCAPdroid 只能抓到**手机自己发出去的出站连接**。其他设备（比如电脑）想主动连进手机的连接，在 Connections 列表里是看不到的。

原因很简单：Android 设备几乎总是在 NAT 后面——路由器给手机分配私有 IP（192.168.x.x 之类），外部请求想主动连进来会被路由器的 NAT/防火墙挡住，根本到不了手机。少数例外场景包括同一局域网内路由器做了端口映射、移动数据网络分配了 IPv6 地址（IPv6 无 NAT）、或者 PCAPdroid 自身 HTTP Server 模式下电脑主动访问 PCAPdroid 显示的 URL。

如果确实需要抓入站流量，要么切换到 Root Capture 模式（绕过 VpnService 直接从网卡抓），要么在 PCAPdroid 的 SOCKS5 代理或端口映射规则里显式配置，把入站代理流量也纳入 VpnService 路径。

### PCAP 文件导出与 HTTP Server

PCAPdroid 支持将抓到的流量导出为标准 PCAP/PCAPNG 格式文件，可以用 Wireshark 打开做更深入的分析。导出路径在应用设置中可配置。

另一个实用功能是 **HTTP Server dump mode**——启动后 PCAPdroid 在手机上开一个本地 HTTP 服务器，显示一个 URL。在同一网络的电脑上访问这个 URL，就能实时查看和下载抓到的 PCAP 文件，不需要 USB 数据线传文件。

---

## 🔗 相关资源

- [PCAPdroid官网](https://pcapdroid.fewz.tech)
- [PCAPdroid文档](https://pcapdroid.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/PCAPdroid)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/PCAPdroid)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/PCAPdroid)

---

## 📚 相关文章

- [PCAPdroid 藏得最深的五个冷功能——用了半年才发现它能这么灵活](https://laoguosofts.github.io/posts/pcapdroid.html)
- [PCAPdroid 挖到底：五个藏得深到官方文档都没说的冷门玩法](https://alipymanbu.github.io/posts/pcapdroid.html)
