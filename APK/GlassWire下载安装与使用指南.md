## ✨ GlassWire 是什么——把你的网络画成实时动画图

GlassWire 是一款面向个人用户的网络流量可视化监控工具，同时内置了基于 Windows 防火墙 API 的出站防火墙能力。跟那些弹出冰冷数字的传统流量监控器不同，GlassWire 把所有联网活动画成一张实时动画图——哪个应用在跟哪个 IP 说话、流量是大是小、协议是 HTTP 还是 UDP，一眼就能看明白。

它的定位很清晰：**不装内核驱动、不抓原始包、不碰协议细节**，只做应用级的流量统计 + 可视化展示 + 一键防火墙拦截。这套轻量架构带来的好处是几乎零性能开销，同时也意味着它不适合网络工程师做协议分析——那是 Wireshark 的主场。

核心功能大致分三块：

- **实时流量图 + 时间机器**：Graph 视图用动画曲线展示流量起伏，鼠标悬停能看到具体是哪个进程贡献的；时间机器可以拖动时间轴回看任意历史时刻的网络画面，排查"昨天下午电脑偷偷干了什么"特别有用。
- **多维度 Usage 分组**：按应用、按主机 IP、按协议类型三个维度拆解流量，快速定位"是谁在占带宽""连到了哪个国家""用的什么协议"。
- **一键 Block All + 出站规则**：离开电脑时点一下 Block All 就能瞬断全部出站连接，防止木马上传数据；也能按应用逐个设置允许/拒绝出站规则，本质上是 Windows 防火墙的可视化皮肤。

GlassWire 支持 Windows 桌面版（主力）和 Android 版。Windows 版功能完整，Android 版偏流量管理 + 简单防火墙（按应用列流量、设月度配额、开关联网权限），不支持按协议或主机分组。macOS 和 Linux 用户没有官方版本，同类替代品可以考虑 Little Snitch（macOS）或 UFW + 可视化前端。

适合谁？普通用户想搞明白自己电脑在联网干什么、隐私敏感的人想揪出偷偷联网的进程、离开电脑时想一键封网防泄漏——这三类场景 GlassWire 比任何同类工具都顺手。但如果你是网络工程师要抓包分析协议细节，或者需要在 Android 上按网络类型精细控制 App 联网，那得另找工具。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://glasswire.ggcd.uno/download.html) —— 下载页整理了当前可用的版本和安装说明
> - Android 版：Google Play 搜索 GlassWire；Windows 版从上方下载页获取安装包

---

## 🔧 安装步骤——Windows 和 Android 两条线

### Windows 桌面版

1. 从下载页拿到安装包后双击运行，安装向导很简单：一路下一步即可。
2. 安装过程中 GlassWire 会自动调用 Windows 防火墙 API 写入出站规则——**不需要手动开启 Windows 防火墙**，也不需要装任何内核驱动。
3. 首次启动会弹出一个引导窗口，询问是否启用实时防火墙监控，建议保持默认（启用）。
4. 装完后在托盘区会出现 GlassWire 的图标，双击或右键打开主界面。

> [!TIP]
> 安装前建议暂时关闭其他第三方防火墙（Comodo、ZoneAlarm、ESET 自带防火墙等）。GlassWire 因为不装自己的驱动，理论上可以和它们共存，但同一时刻启用多套出站防火墙规则集可能导致拦截逻辑冲突，表现为某些应用突然连不上网。

### Android 版

1. Google Play 搜索 GlassWire 下载安装。
2. 首次启动需要授予 VPN 服务权限——Android 版 GlassWire 使用 VPN 隧道劫持法拦截应用流量，这是它实现应用级防火墙的底层原理，**必须允许**。
3. 设置里可以开启月度流量配额告警，到了阈值会提醒你注意。
4. Android 版不支持 root 模式下的防火墙拦截，所有流量都通过 VPN 隧道处理，对不懂技术的用户来说配置门槛比 NetGuard 低很多。

> [!NOTE]
> Android 同时装多个防火墙类工具（NetGuard、GlassWire、NoRoot Firewall）会冲突，因为它们都抢 VPN 隧道权限，系统只允许一个 VPN 活跃。

---

## 📖 使用入门——从看图到排障

### 看懂实时流量图

打开 GlassWire 第一眼看到的就是 Graph 视图——一张实时动画曲线。横轴是时间窗口（默认 5 分钟，可以拖），纵轴是流量大小。鼠标悬停在曲线最高点上会弹出 tooltip，告诉你是哪个应用、在什么时刻、跟哪个 IP 交换了多少字节。

如果某个陌生进程突然在跟国外 IP 高速连接，而且你当前没开任何相关软件——问题几乎就锁定了。GlassWire 不会替你判断"这是木马"，但它会把异常暴露出来，剩下的交给你自己查。

### 三个维度拆解 Usage

Usage 视图是日常排查用得最多的部分，支持三种分组方式切换：

**按应用（Apps）**：列表里每一行对应一个进程，显示累计流量和实时速率。Steam 更新、Windows Update、浏览器偷偷开的视频——谁在占带宽一目了然。

**按主机（Hosts）**：显示你当前正在连接的所有 IP/域名。点开某一行能看到是**哪个应用**在连这个 IP、已经传了多少数据。怀疑数据被上传到某个未知服务器时用这个视图。

**按协议类型（Traffic Types）**：把所有流量按协议归类——HTTPS、HTTP、DNS、TCP、UDP 各占多少。这个分组在两个场景下特别有价值：

| 协议 | 正常用途 | 异常信号 |
| --- | --- | --- |
| HTTPS | 浏览器、微信、视频网站、云盘 | 长期低流量不算异常（多数 App 都走 HTTPS） |
| HTTP | 极少数老网站、内部系统 | 现代网站已全面 HTTPS，大量 HTTP 可能是明文传输 |
| DNS | 浏览器、系统、App 查域名 | DNS 流量持续远超正常水平需排查 |
| UDP | 游戏、视频会议、P2P | UDP 本身不算异常，但不知名进程大量 UDP 要关注 |
| FTP/SMTP/POP3 | 文件上传下载、邮件客户端 | 普通人几乎不会用到，看到了基本是后台服务 |

如果你开了 Steam 玩游戏，UDP 占比高是正常的。但如果你没开任何游戏和视频会议，UDP 流量却很高——切到 Hosts 视图看看这些 UDP 连接到了谁。

### 时间机器回看历史

Graph 视图的时间轴可以往回拖——拖到上周三下午 3 点，就能看到那个时刻你的电脑在干什么。这在排查"数据什么时候被偷走的""那天网速为什么突然变慢"这类问题时极其好用。

历史数据默认存在 `%LOCALAPPDATA%\GlassWire\` 目录下（即 `C:\Users\<你的用户名>\AppData\Local\GlassWire\`）。如果某天打开 GlassWire 发现历史数据全没了，常见原因有五个：手动清除过历史、升级/重装时数据库损坏、系统盘空间不足、Incognito 模式开着忘了关（该模式下不写历史）、或者历史保留时长上限触发被自动滚动删除。磁盘空间不足是最容易忽略的——GlassWire 的数据目录跟系统盘同盘，系统盘满了它既写不进新数据，旧数据还可能被 Windows 磁盘清理工具删掉，建议至少保留 10% 剩余空间。

> [!WARNING]
> Incognito 无痕模式开启时，GlassWire 完全不写入历史数据。这个模式默认在会话结束后自动关闭，但如果你在 Incognito 期间丢了数据，**找不回来**——这就是它的设计目标。

### 远程监控多台机器

GlassWire Windows 版支持远程监控——你可以在控制端电脑上看到被控端的实时流量画面，适合盯家里另一台电脑或远程服务器。配置步骤：

1. 被控端 GlassWire → Settings → Remote Access → 勾选开启 Remote Access Server，记下显示的 IP 和端口。
2. 被控端 Windows 防火墙需要放行这个端口（TCP）。云服务器还要去安全组里放行，来源 IP 只填你自己的公网 IP，**不要全网开放**。
3. 控制端添加远程服务器，填入被控端的 IP 和端口。

如果远程连接失败，从网络层逐层排查：先用 PowerShell 测端口可达性（`Test-NetConnection <被控端IP> -Port <端口>`），再确认被控端 Remote Access Server 确实开着、Windows 防火墙和云安全组都放行、VPN/SSH 隧道没断。家里设备在 NAT 后面没有公网 IP 的话，推荐用 Tailscale 或 ZeroTier 做 Mesh VPN，自动组网、自动加密、不需要路由器端口映射。

### 一键 Block All

想临时让电脑断网？离开工位时防止木马上传数据？GlassWire 右下角托盘图标右键 → Block All，瞬间切断全部出站连接。这是它比 Wireshark 和 NetGuard 都便捷的功能——另外两个工具做不到"一键全断"。

### 跟 Wireshark、NetGuard 怎么选

| 维度 | GlassWire | Wireshark | NetGuard |
| --- | --- | --- | --- |
| 定位 | 个人用户网络可视化 + 防火墙 | 专业级网络包分析 | Android 专属应用级防火墙 |
| 底层能力 | Windows 防火墙 API + 系统级流量采集 | 内核驱动抓原始包 | VPN 隧道劫持拦截 |
| 学习曲线 | 非常低（看图说话） | 极高（协议栈 + 过滤表达式） | 中等 |
| 平台 | Windows（主力）、Android（简化版） | Windows / macOS / Linux | 仅 Android |
| 防火墙 | Windows 防火墙皮肤 | 无 | 独立防火墙 |

选型很简单：Windows 用户想搞懂自己电脑在联网干什么 → GlassWire；网络工程师要抓包排障 → Wireshark；Android 高级用户要精细控制每个 App 的联网 → NetGuard。

---

## 🔗 相关资源

- [GlassWire官网](https://glasswire.ggcd.uno)
- [GlassWire文档](https://glasswire-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/GlassWire)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/GlassWire)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/GlassWire)

---

## 📚 相关文章

- [GlassWire 为什么能把你的网络画成心电图？](https://laoguosofts.github.io/posts/glasswire-docs.html)
- [GlassWire 的零影响架构：不装内核驱动的防火墙到底靠什么干活](https://alipymanbu.github.io/posts/glasswire-docs.html)
