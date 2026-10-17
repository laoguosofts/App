## ✨ NetShare 是什么：无需 Root 的 Android 热点方案

NetShare 是一款专为 Android 设备设计的共享网络应用，最大亮点在于**不需要 Root 权限**就能实现热点功能。它绕过了 Android 系统对原生热点的运营商限制，通过 WiFi Direct P2P 网络 + HTTP 代理的组合方案，把手机的移动数据或 WiFi 连接共享给身边的其他设备。

与常见的热点共享工具相比，NetShare 的定位很清晰：它不追求花哨的界面，而是专注于把"无 Root 开热点"这件事做稳。对于那些运营商套餐不包含热点、ROM 厂商阉割了热点功能、或者暂时不想折腾 Root 的用户来说，这几乎是最实用的替代方案。

NetShare 目前仅支持 Android 平台，要求系统版本 7.0 以上。接收端则不挑设备——笔记本、平板、甚至另一台手机都能连上，只需在接收端配置好代理即可。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://netshare.boop.uno/download.html) —— 下载页整理了当前可用的版本和安装说明
> - Google Play：搜索 "NetShare - no-root-tethering"，包名 `kha.prog.mikrotik`
> - 侧加载渠道：Aptoide、APKPure 等镜像站可获取 APK 文件（适合没有 Google Play 服务的设备）

---

## 🔧 安装步骤与注意事项

### Google Play 安装（推荐）

在应用商店搜索关键词 "NetShare - no-root-tethering" 找到应用，点击安装即可。通过 Google Play 安装有几个好处：

- 应用签名由开发者私钥管理，完整性有保障
- 有自动更新推送，新版本上线后会收到提醒
- 应用已声明不收集任何数据、不与第三方共享

### APK 侧加载

如果你的手机没有 Google Play 服务（国行、刷机、海外受限区域），可以从可信镜像站获取 APK 文件。首次侧加载的步骤：

1. 下载最新版 APK 到手机
2. 用文件管理器找到 APK，点击安装
3. 系统弹出"来源不明"提示时，进入设置页允许该文件管理器的"安装未知应用"权限
4. 返回安装界面继续完成

> [!WARNING]
> 开启"安装未知应用"权限后，请及时关闭，避免后续误装其他来源不明的应用。

### 版本更新

侧加载 APK 的用户，更新时**直接覆盖安装**即可，不需要先卸载旧版本。NetShare 的自定义设置（SSID、密码等）会被保留，不会丢失。

### 安装包信息速查

| 项目 | 值 |
| --- | --- |
| 最新版本号 | 2.46 |
| 更新时间 | 2025 年 5 月 |
| 文件大小 | 约 950 KB |
| 目标 Android | 7.0+ |
| 包名 | kha.prog.mikrotik |

### 卸载清理

标准 Android 卸载流程：设置 → 应用 → NetShare → 卸载。如果之前手动配置过自启动权限等额外设置，卸载前可以先在应用详情页手动清除数据。

---

## 📖 核心功能与使用方法

### 首次启用：开启热点

打开 NetShare 应用后，主界面结构很简单，核心操作就是勾选 **Start Hotspot**。勾选后应用会在后台创建 WiFi Direct P2P Group，同时启动 HTTP 代理服务。

界面底部会显示实时状态信息，包括 Connected Clients 计数（当前连接的设备数量）以及 HTTP 代理地址和端口。代理的默认端口是 **8282**，P2P 网络的网关地址固定为 **192.168.49.1**。

> [!NOTE]
> Connected Clients 计数统计的是 P2P 层面加入的设备，和这些设备能不能正常上网是两件事。一台设备可能连上了 P2P 但没配代理，此时会被计数但无法访问外网。

### 接收端配置代理

NetShare 的工作原理是 WiFi Direct 组网 + HTTP 代理转发。接收端设备连上 P2P 网络后，还需要配置代理才能真正上网。不同平台的配置方式略有差异：

**macOS 接收端**：
- 系统设置（推荐）：网络 → 选中当前 WiFi → 详细信息 → 代理 → 勾选"自动代理配置"，URL 填 `http://192.168.49.1:8282`
- 终端一键脚本：`bash <(curl -s http://192.168.49.1:8181/msh)`（需要 sudo 权限）
- PAC 文件方案实际上是 NetShare 提供的 Proxy Auto-Configuration 文件，浏览器和大多数应用能自动解析

**Windows 接收端**：
- 设置 → 网络和 Internet → 代理 → 手动设置代理，服务器 `192.168.49.1`，端口 `8282`
- 用完记得关闭代理，否则会影响后续正常上网

**浏览器特殊处理**：
- Safari 自动跟随系统代理，无需额外设置
- Chrome 默认也跟随系统，但如果装了 SwitchyOmega 等独立代理插件，要检查插件有没有覆盖系统设置
- Chrome 还支持命令行指定代理启动：`Chrome --proxy-server="http://192.168.49.1:8282"`

**不遵循系统代理的应用**：
Steam、部分游戏、Docker Desktop 等应用有自己的代理设置入口，需要在各自应用的设置面板里单独填写 `192.168.49.1:8282`。

### 热点自动关闭的排查

这是用户反馈最多的问题，具体表现分三类，解法各不同：

**情况一：勾选后立即取消或点不动（灰色）**

常见原因包括：

| 可能原因 | 排查与解决 |
| --- | --- |
| WiFi Direct 未启用 | 设置 → WiFi → 高级 → WiFi Direct → 手动开启 |
| WiFi 芯片被占用 | 关掉原生热点、停掉 WiFi Direct 传输、强制停止占用 WiFi 的应用（抓包工具、VPN 等） |
| 端口冲突 | HTTP 代理端口 8282 被占用，强制停止使用 HTTP 代理的应用后重启 NetShare |
| 系统版本过旧 | 确保 Android 7.0 以上，6 及以下的 WiFi Direct 实现有缺陷 |

**情况二：运行几秒后自动关闭**

这是最常见的情况，多半和电池优化有关：

1. **电池优化杀后台**：把 NetShare 加入后台白名单、在最近任务界面锁定应用、电池优化设为"无限制"
2. **手机过热**：CPU/WiFi 芯片温度过高时系统会强制关闭高功耗进程，把手机放通风处、拆壳散热、接充电器、调低亮度
3. **VPN 冲突**：同时开 VPN 可能把 P2P 流量加密转发走，在 VPN 应用里开启"允许本地网络"或临时关掉 VPN 测试

**终极手段**（重启也没用时）：重启手机 → 清除 NetShare 数据（会丢失自定义 SSID 和密码）→ 卸载重装最新版。

### 连接状态与代理验证

NetShare 目前不内置流量统计功能，但可以通过以下方式监控链路健康：

**接收端快速验证**：打开命令行执行 `curl -v https://www.google.com`，如果 HTTP 代理正常，输出里会显示 "Using proxy 192.168.49.1:8282"。如果显示 "Connection refused"，说明代理没启动或端口不对。

**HTTP 代理健康检查**：`curl -x http://192.168.49.1:8282 https://httpbin.org/ip`，返回 NetShare 手机的公网 IP 就说明整条链路通了。

**流量间接监控**：接收端访问测速网站评估带宽，或在 Android 系统的"设置 → 网络与互联网 → 流量使用情况"里查看总流量。需要精细到每台设备的话，可以在接收端安装 NetGuard、GlassWire 等网络监控应用。

> [!TIP]
> 如果发现界面底部的 HTTP 代理地址和端口消失了，说明代理进程可能被系统杀掉了。此时取消再重新勾选 Start Hotspot 就能恢复，不需要重启应用。

### 使用收尾

用完 NetShare 后记得做两件事：

1. **关闭热点**：在 NetShare 主界面取消 Start Hotspot 勾选
2. **接收端恢复代理设置**：macOS 在系统设置里取消代理勾选，或终端执行 `networksetup -setwebproxystate Wi-Fi off`

---

## 🔗 相关资源

- [NetShare官网](https://netshare.boop.uno)
- [NetShare文档](https://netshare-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD/NetShare)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD/NetShare)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD/NetShare)

---

## 📚 相关文章

- [NetShare 为什么不需要 Root 就能开热点？WiFi Direct 背后的设计巧思](https://laoguosofts.github.io/posts/netshare-docs.html)
- [NetShare 为什么不走原生热点？一套 WiFi Direct + HTTP 代理的技术取舍](https://alipymanbu.github.io/posts/netshare-docs.html)