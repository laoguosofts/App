## ✨ AdAway 是什么

AdAway 是 Android 平台上一款开源、免费、基于 hosts 文件机制的系统级广告拦截工具。它不是浏览器插件，也不是独立的 APP 内过滤器——它直接在系统的 DNS 解析链最前端介入，把上万个广告和跟踪域名重定向到本地环回地址，让所有 APP 和浏览器在发起网络请求时自动跳过这些广告服务器。

作为 GPLv3 协议的自由软件，AdAway 没有付费版、没有订阅、没有内置广告、没有数据收集。它的核心代码托管在 GitHub，所有版本都可以从源码编译。这一点很重要——在广告拦截这个领域，透明性本身就是安全保障。

适合谁用？首先是已经 root 了 Android 设备的用户——root 模式下 AdAway 的拦截几乎零性能损耗，兼容性最好。其次是不想 root 但也想在 APP 内去广告的用户——v5 之后的版本提供了本地 VPN 模式，非 root 设备也能用，只是会占用一个 VPN 虚拟网卡通道。如果你还在 Android 4.x 的老机器上，AdAway v4 系列是你唯一的系统级广告拦截选择。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://adaway.hfdk.space/download.html) —— 下载页整理了当前可用的版本和安装说明
> - F-Droid 仓库：在 F-Droid 客户端搜索 "AdAway"（包名 `org.adaway`），自动签名可验证
> - GitHub Release：作者直接签名的 APK，适合不想装 F-Droid 的用户
> - Google Play：**没有 AdAway**——Google 的开发者协议禁止干扰其他软件运行的工具，Play 上出现的任何同名 APP 都是假冒

---

## 🔧 安装步骤（Android 平台）

### 先确认你的设备条件

AdAway v6.x 是目前推荐版本，要求 Android 8.0 以上。v5.x 适合 Android 5.x 到 7.x，v4.x 只用于 Android 4.x 老机器。核心差异在于拦截机制：

| 你的设备 | 推荐版本 | 推荐模式 |
| --- | --- | --- |
| Android 8.0+ 已 root（Magisk） | v6.x | root 模式（Systemless Hosts） |
| Android 8.0+ 不想 root | v6.x | VPN 模式 |
| Android 5.x ~ 7.x 已 root | v5.x | root 模式 |
| Android 4.x 老机器 | v4.x | root 模式（唯一可用） |

> [!WARNING] 只认 `org.adaway` 这个包名
> Play Store 上出现过多次第三方假冒的 AdAway——功能缩水、嵌入跟踪、甚至被注入恶意代码。F-Droid 和 GitHub Release 的构建元数据可以通过 PGP 签名验证，第三方下载站（刷机之家、APKPure、Aptoide 等）不可信。

### F-Droid 安装流程（推荐）

1. 手机浏览器打开 F-Droid 官网，下载 F-Droid.apk 并安装。系统提示"允许安装未知来源"时去设置里授权一次即可。
2. 打开 F-Droid，等待它同步完仓库索引（首次启动约 1~2 分钟）。
3. 搜索 "AdAway"，找到 `org.adaway` 包名，点击安装。
4. 更新机制：F-Droid 客户端会定期检查更新，建议保持手动更新——系统级 APP 的变更日志值得看一眼再决定。

### GitHub Release 作为备选

如果你不想装 F-Droid，可以去作者的 GitHub Release 页面下载直接签名的 APK。注意两种渠道的签名不同，不能互相覆盖安装，切换前需要先卸载旧版。功能上完全一致，但失去了自动更新能力。从 GitHub 下载的 APK 可以对比 Release 页面提供的 MD5/SHA256 hash 值来校验完整性。

### 装完怎么核对

打开 AdAway，进入主菜单 → 关于页面。确认三点：版本号符合你的预期、来源标注是 F-Droid 或 GitHub Release、包名确实是 `org.adaway`。这三项都对上了，安装就是干净的。

---

## 📖 深入使用指南

### 工作原理：hosts 文件就是拦截本身

Android 的 hosts 文件位于 `/system/etc/hosts`，格式与 Linux、Windows 完全一致——每行两个字段，左边 IP、右边主机名。系统启动时就会加载它，所有 APP 和浏览器的 DNS 查询都会先查这里。AdAway 做的事情本质上很简单：从公开的 hosts 源拉取上万个广告域名，每个配上一行 `127.0.0.1 域名`，合并成完整表写回系统 hosts 文件。之后任何 APP 请求这些广告域名，系统直接返回本地环回地址——请求打出去，但没人响应，广告就加载不出来。

这个机制的优势是零开销——hosts 拦截发生在 DNS 解析的最前端，不经过任何代理、不创建 VPN 虚拟网卡、不额外耗电。劣势是对 IP 直连请求无能为力，某些游戏硬编码了广告服务器 IP，hosts 文件拦不住；绕过 DNS 的代理请求同样不受影响。

> [!NOTE] 和其他拦截器的对比
> 浏览器插件（AdBlock Plus、uBlock Origin）只能管浏览器进程内的请求。AdGuard、Blokada 等 VPN 模式工具能拦 APP 内广告，但依赖虚拟网卡转发，有一定性能开销。AdAway 的 root 模式是唯一的系统级零开销方案。

### 拦截模式选择：root 还是 VPN

如果你已经通过 Magisk 获得了 root 权限，优先使用 root 模式。v6.x 与 Magisk 的 Systemless Hosts 集成，可以在不修改系统分区的情况下管理 hosts 文件，OTA 升级时也不会丢失。root 模式下 AdAway 还提供了 DNS 查询日志、通配符 Allowlist、批量导入导出等完整功能。

如果你不想 root，v6.x 的 VPN 模式是目前开源方案里最成熟的选择。它在本机创建一个虚拟网卡，拦截所有 DNS 请求，匹配 hosts 规则的域名返回 127.0.0.1，其他请求放行。相比 v5.x 被诟病的网络波动和连接监控不完善，v6 重写了 VPN 拦截实现，补上了连接监控、心跳和节流器——网络切换时能自动恢复。代价是设备上只能同时开一个 VPN（Android 系统限制），并且会持续占用一个 VPN 通道。

### hosts 源管理：这才是拦截效果的核心

AdAway 本身不维护广告域名列表，它从外部的 hosts 源拉取。不同的 hosts 源覆盖的广告类型不同：有的专门拦追踪 SDK，有的专注移动 APP 广告，有的覆盖范围极广但可能误杀正常域名。你可以同时启用多个 hosts 源，AdAway 会自动去重合并。

> [!TIP] 首次使用建议先用默认 hosts 源跑一周
> 观察有没有某些 APP 打不开或闪退——如果没有，说明默认源对你的 APP 兼容性不错。如果有，再按故障排查流程加 Allowlist，不要一开始就追求"装最多的源拦最狠的广告"。

### 故障排查：APP 打不开怎么办

最常见的症状是开了 AdAway 后某个 APP 打不开、闪退、加载不出数据，关掉 AdAway 就正常。核心原因几乎一定是 hosts 源误拦了该 APP 必需的某个域名——AdAway 本身不会让 APP 崩溃，是 hosts 文件里的重定向条目把必需域名指向了 127.0.0.1。

排查分三步走：

**第一步：确认是 AdAway 的问题**。关掉 AdAway → 打开目标 APP → 能正常运行。再开 AdAway → 又崩了。确认是 hosts 拦截导致的，别去重装 APP 或重启手机。

**第二步：查已知问题列表**。AdAway 的问题追踪页面维护了大量已知误杀域名——Facebook 必需 `graph.facebook.com` 和 `edge.mqtt.facebook.com`，Amazon Shopping 必需 `aan.amazon.com`，CamScanner 必需 `d117.intsig.net`，Fitbit 必需 `www.google-analytics.com`。对照列表快速加 Allowlist，能解决很多常见 APP 的兼容性问题。

**第三步：用 DNS 日志精准定位**。如果已知列表里没有，跑 DNS 日志：临时关掉 AdAway 让 APP 能正常运行 → 打开 AdAway → 主菜单 → Log DNS Requests → 启动 → 操作目标 APP 1~3 分钟触发各种网络请求 → 回到 AdAway 看日志。找出 APP 正常运行时访问的那些域名——这些就是它的必需依赖。然后重新开 AdAway、重跑 APP，看日志里哪些域名被 Block 了。被 Block 且 APP 必需的，长按加进 Allowlist。

> [!WARNING] 通配符放行要精确
> `*.facebook.com` 会把 Facebook 的广告域名也一起放了。应该精确放行 `graph.facebook.com` 和 `edge.mqtt.facebook.com` 这两个必需域名，广告域名（如 `ads.facebook.com` 和 `doubleclick.net`）仍然被拦。

加完 Allowlist 后**必须重新应用 hosts**——主界面点一下应用按钮，否则新规则不会生效。如果 DNS 日志里被 Block 的域名都加了 Allowlist 但 APP 还是崩，可能是 APP 用了 IP 直连（hosts 无能为力）、VPN 模式下的虚拟网卡冲突、或者 APP 自带了自建 DNS 解析器。

### DNS over HTTPS（DOH）的协调

从 Android 9 开始，Google 默认在"私人 DNS"里启用 DOH。DOH 绕过了传统 DNS 查询路径——理论上会让 hosts 拦截失效。v6.x 专门为此补上了 DOH 协调机制：如果你开启了系统的私人 DNS（DOH），AdAway 会在 hosts 文件里追加 DOH 服务器域名的重定向，迫使其回退到传统 DNS 通道，让 hosts 拦截重新生效。

### Allowlist 的长期维护

建议给每个 Allowlist 条目写注释——AdAway 支持在主机名后加 `#` 注释。格式类似 `graph.facebook.com # Facebook 必需，hosts 源误拦`。半年后你回头看 Allowlist 还能知道每条规则为什么加的。

换设备或刷 ROM 之前，把 Allowlist 导出（主菜单 → Allowlist → 导出）存到云盘。新装 AdAway 后直接导入，省得重新排查。

---

## 🔗 相关资源

- [AdAway官网](https://adaway.hfdk.space)
- [AdAway文档](https://adaway.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/AdAway)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/AdAway)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/AdAway)

---

## 📚 相关文章

- [AdAway 上手测评：我用了一周的安卓 root 广告拦截神器](https://laoguosofts.github.io/posts/adaway.html)
- [AdAway 上手测评：给 Android 装个"去广告的脑子"](https://alipymanbu.github.io/posts/adaway.html)
