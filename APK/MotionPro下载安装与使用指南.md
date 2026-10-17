## ✨ MotionPro 是什么

MotionPro 是 Array Networks 推出的企业级 VPN 客户端，广泛用于远程办公场景，让员工从任意设备安全接入公司内网。它的核心定位不是个人翻墙工具，而是为企业 IT 团队提供一套完整的远程办公解决方案——客户端配合 Array AG 网关使用，支持 L3VPN 隧道、企业应用门户发布、远程桌面（DesktopDirect）三大能力。

MotionPro 的跨平台覆盖做得比较完整，Windows、macOS、Linux、iOS、Android 都有官方客户端，同一个账号登录不同平台体验一致。企业 IT 侧可以通过网关集中管控用户权限、发布应用、审计会话，普通员工侧则只要装一个客户端、输账号密码就能用，几乎零配置成本。

典型的目标用户是两类：一类是已经部署了 Array AG 网关的企业员工——IT 部门提供安装包和账号，装好就能连公司内网、访问内部系统、远程接管办公 PC；另一类是 IT 管理员，需要了解客户端部署、排错、与网关功能的配合方式。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://motionpro.fedq.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：iOS / Android 用户在 App Store、Google Play 搜 "MotionPro"，企业分发渠道可能通过 MDM 或组织内部分享

---

## 🔧 Windows 客户端安装全流程

MotionPro Windows 客户端的安装门槛比普通软件稍高，核心原因是它需要安装虚拟网卡驱动（VNIC）来建立 VPN 隧道——这一步必须要管理员权限。

### 系统要求先核对

官方 Release Notes 明确列出的支持范围：

| 系统 | 状态 |
|---|---|
| Windows 11（含 23H2） | ✅ 完全支持 |
| Windows 10 64 位（1809+） | ✅ 完全支持 |
| Windows 7 / 8 / 8.1 | ❌ 不支持 |
| 32 位 Windows 10 | ❌ 不支持 |

装之前先看一眼系统版本，省得走到一半才发现跑不起来。

### 安装的四步走

1. **下载安装包**：从组织支持门户或网关获取 `MotionPro_Windows_x.x.x.exe`
2. **以管理员身份运行**：右键 exe → 以管理员身份运行；或者双击后在 UAC 弹窗里点"是"——直接双击如果不允许提权，驱动就装不上
3. **按向导走**：欢迎页 → 接受许可协议 → 安装路径（默认 `C:\Program Files\Array\MotionPro\`，一般不用改）→ 开始安装（1~2 分钟）→ 完成后勾选"立即启动"
4. **验证安装**：桌面出现 MotionPro 图标，系统托盘右下角出现红/黄/绿的 Array "A" 标志；打开客户端在"帮助 → 关于"里确认版本号

### 安装时常见的安全拦截

MotionPro 虽然是正规厂商的产品，但因为面向企业客户而非海量个人用户，签名证书在 Windows SmartScreen 的信誉体系中积累不够，新下载的 exe 可能触发 SmartScreen 警告：

> [!WARNING]
> 遇到 SmartScreen 弹窗（"Windows 已保护你的电脑"）别慌——点"更多信息"，检查应用名称和发布者（Array Networks）是否正确，确认无误后点"仍要运行"。这是 SmartScreen 对下载量不够大的应用的保守策略，不是安全问题。

部分杀毒软件（360、火绒、卡巴斯基等）可能在安装或首次启动时拦截驱动安装或网络活动。处理方式：把 MotionPro 安装目录加入杀毒白名单，或者暂时退出杀毒再装，装完重新开启。Windows Defender 一般不会误报——如果有警告，大概率是你下的安装包来源不对。

### 安装后检查虚拟网卡是否就位

装完后打开 **设备管理器 → 网络适配器**，应该能看到 Array 开头的虚拟网卡：`Array MotionPro Virtual Adapter`（VNIC 模式）或者 Windows 自带的 SSTP VPN Adapter。如果找不到，说明驱动安装失败——可以重新跑一遍安装包，或者用 MotionPro 内置的修复功能（开始 → 选项 → 修复）。

---

## 📖 使用入门：从连接到进阶场景

装好客户端、拿到组织账号之后，日常使用基本围绕三件事：连 VPN 隧道、访问发布的企业应用、以及（如果网关开通了的话）远程桌面 DesktopDirect。

### 第一步：建立 VPN 隧道

打开 MotionPro，在登录界面输入组织网关地址、账号、密码，点连接。成功后系统托盘的 Array 图标变绿，客户端主界面显示"安全隧道已建立"。此时打开内网域名应该就能访问了。

> [!TIP]
> 如果 MotionPro 显示"已连接"但内网页面打不开，90% 的情况是 DNS 缓存问题——Windows 上跑 `ipconfig /flushdns`（管理员权限的命令提示符），macOS 上是 `sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder`，清空后断开 VPN 重连通常就好。

### VPN 已连但内网不通的排查思路

这是 MotionPro 排错里最高频的场景，典型表现是托盘绿着、VPN 图标显示活动，但浏览器打开内网域名超时。排查顺序建议按以下流程走：

**① 确认 VPN IP 真的分配了**

Windows 跑 `ipconfig`，找到 MotionPro 虚拟网卡那一行，应该有一个内网 IP（通常是 10.x.x.x / 172.16.x.x / 192.168.x.x）。如果没有，说明隧道没真正建立好，断了重连试试。

**② 绕开 DNS 试直接访问 IP**

如果内网资源有已知的 IP 地址，直接在浏览器里输 IP 访问——能打开就是 DNS 问题，按上面说的清缓存就行；IP 也不通，继续往下查路由和代理。

**③ 检查残留代理设置和多 VPN 冲突**

之前手动在系统里设过 HTTP 代理、或者装了其他 VPN 客户端（包括浏览器扩展），都可能拦截隧道流量。Windows 去"控制面板 → Internet 选项 → 连接 → 局域网设置"，确认"为 LAN 使用代理服务器"没勾；macOS 在"系统设置 → 网络 → 详细信息 → 代理"里全部设为关闭。多 VPN 同时跑会严重冲突，退出其他 VPN 客户端再重连 MotionPro。

**④ 查路由表**

Windows 跑 `route print`，看 Active Routes 里有没有内网网段（比如 10.0.0.0/8）走 MotionPro 虚拟网卡的条目；macOS/Linux 跑 `netstat -rn` 看内网网段是不是走 VPN 接口（utun0）。如果路由表里缺了内网网段，可能是网关下发路由时出了问题，断了重连或者用 MotionPro 的修复功能重置驱动和路由表。

### DesktopDirect 远程桌面：MotionPro 最有价值的进阶功能

如果你的组织的 AG 网关开通了 DesktopDirect，那 MotionPro 的价值就不止是访问内网了——你可以把自己的办公 PC 自助注册到网关，然后从家里的任何设备远程接管它。

**它和传统 VPN + RDP 的核心区别**：传统方式需要记住办公 PC 的内网 IP、配置路由器端口转发、处理动态 IP 变化的问题，用户体验是割裂的（先连 VPN → 开 RDP 客户端 → 手动填 IP 和账号）。DesktopDirect 让员工在门户里点一下就能把自己的 PC 注册到网关，之后任何设备上的 MotionPro 客户端登录后都能在门户里看到自己的 PC，一键远程。更重要的是，数据始终留在办公室的 PC 上，家里设备只是"遥控器"——不会把企业数据下载到个人设备。

DesktopDirect 的反向上报机制是关键设计：PC 主动连网关建立反向上报通道，而不是网关或外部设备去连 PC。这意味着在任何网络环境下（企业 NAT 后面、运营商 CGNAT、无法获取公网 IP 的场景）都能工作，用户不需要配置任何路由器或 DDNS。

| 对比项 | 传统 RDP + 端口转发 | DesktopDirect |
|---|---|---|
| 连接方向 | 外部设备 → 路由器 → 内网 PC | 内网 PC → AG 网关（反向） |
| 需要配置路由器 | ✅ 端口转发 + 动态域名 | ❌ 不需要 |
| 需要知道 PC IP | ✅ | ❌ |
| 动态 IP 问题 | 需要 DDNS 方案 | ✅ 自动处理 |
| 暴露端口风险 | 有，被扫描风险 | 只有网关能找到 PC |

### 自助注册办公 PC

注册本身不在 MotionPro 客户端里完成——必须在公司 PC 上通过浏览器登录 AG 网关门户，找到"远程桌面"或"DesktopDirect"入口，点"注册我的 PC"。系统会自动检测主机名和 Windows 用户，给 PC 起一个便于识别的名字（推荐用真实姓名 + 设备名，比如"张三-办公室工作站"），确认后几秒钟注册完成。注册过程中 AG 网关会在 PC 上安装一个轻量级的 Array Registration Agent，让 PC 主动连接网关建立持久的反向上报通道。

注册完成后，在任何设备上打开 MotionPro Plus 客户端 → 连接公司 AG 网关 → 进入企业应用门户 → 远程桌面分类，就能看到刚才注册的 PC。点图标 → 输入 PC 的 Windows 用户密码（不是 MotionPro 登录密码）→ 开始远程。

> [!NOTE]
> 如果门户里"注册我的 PC"按钮灰着点不了，基本原因只有两个：组织的 AG 网关没启用 DesktopDirect，或者你的账号没有远程桌面权限——找 IT 部门确认一下。另一个高频问题是注册后家里设备看不到 PC，回到公司 PC 检查 Array Registration Agent 服务是否在运行、AG 网关地址（端口 443）有没有被公司防火墙拦截，手动重新注册一次试试。

### DesktopDirect 的其他实用能力

网关端可以配置远程桌面的会话时长、带宽限制、剪贴板策略（IT 可以控制是否允许远程桌面和本地设备之间的剪贴板同步）、会话审计（网关记录所有远程桌面会话的发起者、时长、目标 PC）。还支持 Wake-on-LAN 远程唤醒关机的办公 PC（需要 PC 硬件和 BIOS 支持），以及远程 PC 的多显示器在本地逐屏显示。

---

## 🔗 相关资源

- [MotionPro官网](https://motionpro.fedq.quest)
- [MotionPro文档](https://motionpro.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/MotionPro)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/MotionPro)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/MotionPro)

---

## 📚 相关文章

- [装了快一年MotionPro，聊聊这款企业VPN客户端的真实体验](https://laoguosofts.github.io/posts/motionpro.html)

- [MotionPro 用了一年半：从"凑合用"到"离不开"的 VPN 客户端体验](https://alipymanbu.github.io/posts/motionpro.html)
