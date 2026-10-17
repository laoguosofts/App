## ✨ VNC Viewer 是什么

VNC Viewer 是 RealVNC 公司推出的远程桌面客户端，支持 Windows、macOS、Linux、Android、iOS 五大平台，能让你随时随地远程连接并控制另一台装有 RealVNC Server 的设备。它是目前市场上跨平台兼容性最强、企业级功能最完整的远程桌面解决方案之一，同时对个人用户提供免费基础版。

简单理解：被控端装 RealVNC Server，控制端装 VNC Viewer，两边登录同一个 RealVNC 账号，就能通过 Cloud 云连接穿透 NAT 和防火墙实现远程控制。如果两端在同一局域网，也可以用 Direct 直连模式跳过云端，延迟更低。还有一个 Code Connect 模式，被协助方生成一个 9 位码发给你，你输入就能临时接管他的电脑，特别适合远程协助场景。

核心能力包括：全屏远程桌面画面、鼠标键盘完全控制、实时聊天会话、文件传输、远程音频、剪贴板同步、多显示器支持、会话录制等。企业版额外提供精细的权限控制（按用户分配只读/可交互/可配置等权限）、会话审计日志、策略模板批量推送、单点登录等合规级功能。

适用人群非常广：远程办公族用它连接公司电脑、IT 运维用它巡检服务器、技术支持用它帮客户排障、开发者用它远程操作树莓派或云主机。免费版个人日常使用完全够用，订阅 RealVNC Connect 后可以解锁更多高级功能和团队管理能力。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://vnc-viewer.usea.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 在 Google Play 搜 `RealVNC Viewer: Remote Desktop`（约 50MB，支持 Android 7.0+）；iOS 在 App Store 搜同名应用（支持 iOS 12+）；PC 端可在 RealVNC 官网下载对应系统版本安装包

---

## 🔧 安装步骤

### Android 移动端安装

从 Google Play 或 App Store 搜索应用名下载即可，安装包约 50MB~80MB，建议在 WiFi 环境下完成下载。安装过程与普通手机 App 无异，一路确认即可。

首次打开时会提示登录或创建 RealVNC 账号，邮箱验证后就能使用。如果还没有账号，直接在应用内完成注册即可，免费。Android 可能弹"允许定位"请求，Direct 模式下 VNC Viewer 不依赖定位权限，拒绝即可。

> [!TIP] 移动端有个重要限制
> 手机版 VNC Viewer 只能当**客户端**（控别人），不能当被控端。如果想让电脑远程控制手机，需要在手机上另外安装独立的 RealVNC Server 应用，电脑端通过 Code Connect 9 位码连接。

### PC 端安装

Windows、macOS、Linux 版本安装包体积在 50MB~100MB 之间，下载后双击运行安装程序。

Windows 安装向导中注意两点：一是安装路径建议保持默认（`C:\Program Files\RealVNC\VNC Viewer`），避免自定义路径后快捷方式找不到程序；二是如果装了 360 或 Windows Defender 弹出"是否允许此程序修改你的设备"提示，点击允许即可——这是 VNC Viewer 正常申请系统访问权限的表现，不是病毒。

macOS 版本下载的是 `.dmg` 镜像，双击打开后把 VNC Viewer 图标拖进 Applications 文件夹。首次启动时如果 macOS 提示"来自身份不明的开发者"，到系统设置→隐私与安全性→通用，底部点"仍要打开"就行。

Linux 提供 `.deb` 和 `.rpm` 两种包，Ubuntu/Debian 用 `sudo dpkg -i vnc-viewer_*.deb` 安装，Fedora/CentOS 用 `sudo rpm -ivh vnc-viewer-*.rpm`。装完后从应用菜单搜索 "VNC Viewer" 启动。

### 安装后快速验证

打开 VNC Viewer，能正常显示设备列表界面（第一次打开是空列表）就说明安装成功了。点界面上的 **+** 按钮手动添加一台设备，或者输入 Code Connect 9 位码尝试连接，如果能进入认证界面就表示客户端工作正常。

---

## 📖 使用入门

### 三种连接模式怎么选

VNC Viewer 提供三种连接模式，覆盖不同场景：

| 模式 | 连接方式 | 适合场景 | 延迟 |
|---|---|---|---|
| **Cloud 云连接** | 两端登录同一 RealVNC 账号，通过云端中继 | 跨公网远程办公、控制家里/公司电脑 | 中等（100~300ms） |
| **Direct 直连** | Viewer 直接输入 Server 的局域网 IP + 端口 | 同局域网内控制，比如树莓派开发板 | 低（10~30ms） |
| **Code Connect** | Server 生成 9 位码，Viewer 输入后临时连接 | 远程协助陌生人解决电脑问题 | 中等 |

日常使用最频繁的是 Cloud 模式——只要被控端电脑开着且连网，不管它在什么 NAT 后面、有没有固定 IP，你的 VNC Viewer 设备列表里都能看到它，点一下就进入远程桌面。

### 远程控制基础操作

连上远程桌面后，首先根据被控端类型选择合适的交互模式（工具栏里可随时切换）：

| 远程设备类型 | 推荐模式 | 原因 |
|---|---|---|
| Windows 桌面 | 鼠标模式 | 需要精确点击按钮、拖拽文件、右键菜单 |
| macOS 桌面 | 触控面板模式 | 与 Trackpad 手势一致，操作更自然 |
| 树莓派（触摸屏） | 直接触控面板 | 远程本身就是触屏 |

PC 端常用快捷键：

- **Ctrl+Alt+C**（Windows/Linux）呼出会话内聊天面板
- **Ctrl+Alt+Del**（工具栏里找，Windows 远程桌面专用）
- **Ctrl+Shift+Esc** 远程打开任务管理器
- **双指捏合缩放**（移动端）放大屏幕局部细节

移动端有几个特别实用的手势和功能：

1. **长按屏幕 1 秒弹出工具栏**——远程电脑卡死时点工具栏里的 Ctrl+Alt+Del 救急，比远程重启温和得多
2. **剪贴板同步**——远程桌面里复制的内容，手机任何 App 里都能粘贴；反过来也行
3. **虚拟键盘**——远程设备没接物理键盘时（比如 headless 树莓派），工具栏调出虚拟全键盘，支持 F1~F12 功能键和修饰键组合
4. **分辨率缩放**——双指捏合放大到局部细节区域，单指拖动平移，松开回到全局

### 会话内聊天与录制

远程会话里如果两边都在屏幕前，不用另外开微信或 Teams，直接在 VNC Viewer 会话窗口里聊天：

- **Windows/macOS/Linux**：按 **Ctrl+Alt+C** 呼出聊天面板，或者从顶部菜单找 Chat
- 聊天面板支持文字消息、系统通知（对方断开连接、改分辨率等），会话期间的消息历史结束后自动清除

实际场景中聊天功能特别好用：远程协助时语音通话叠加文字，可以把命令、IP 地址、参数直接发到聊天框让对方复制粘贴，不会因为口头传达出错误。

> [!NOTE] 会话录制功能需要付费订阅
> 录制整个远程会话是 Plus 和 Enterprise 档才有：Server 端开启自动录制（推荐更安全），或 Viewer 端顶部菜单手动录制。录制文件包含完整桌面画面，Enterprise 档还额外生成操作日志，记录谁什么时候连的、点了什么、打了哪些键。

录制的典型使用场景：企业 IT 运维审计（每次远程操作都录像，事后可追溯）、远程培训（操作一遍录下来发给学员回放）、客服支撑（帮助中心把解决用户问题的全过程录成知识库视频）。

### 权限控制

如果用的是 RealVNC Connect Plus 或 Enterprise 订阅，权限控制分两层：

**团队级**——决定谁能看到哪些设备。Owner 角色可以看全部设备、邀请成员、管理订阅；Member 默认也能看到全部设备，但不能管理订阅。团队里可以创建子设备组进一步精细化，比如 IT 团队只能看到服务器组，看不到研发组的开发机。

**设备级**——这是最精细的权限控制，针对每一台远程设备可以给不同用户分配不同权限：

| 权限 | 说明 | 风险等级 |
|---|---|---|
| Full Control | 完全控制远程桌面 | 最高 |
| View Only | 只能看屏幕不能操作 | 最低 |
| Chat Only | 只能聊天，不能看也不能操作 | 极低 |
| File Transfer | 允许文件传输 | 中 |
| Recording | 允许录制会话 | 中 |

配置方式：RealVNC Server → Options → Users and Permissions，添加用户后勾选或取消权限项，保存即时生效。Enterprise 档还支持 **Policy Templates**（策略模板），可以把权限配置打包成一个模板批量推送给几十上百台设备，类似 Windows 组策略。

只读（View Only）权限特别适合远程培训——讲师操作，学员只能看+提问，不会误触破坏会话。会议室投屏场景也常用，一台电脑设 Server，所有人用 View Only 看演示。

### 权限配置：RealVNC Server 端

RealVNC Server 端是权限管控的核心。打开 RealVNC Server → Options → Users and Permissions，可以针对特定用户配置访问级别。Plus 和 Enterprise 订阅还能用 VNC Permissions Creator 这个独立工具做更精细的批量权限设置。

> [!WARNING] 权限与安全提示
> 服务器端的 VNC Server 默认密码不要用简单的数字或出生日期，至少 8 位包含大小写字母和数字。团队共享订阅时，务必给每个成员分配独立账号和最小必要权限，不要大家共用同一个 Full Control 账号。

---

## 🔗 相关资源

- [vnc viewer官网](https://vnc-viewer.usea.quest)
- [vnc viewer文档](https://vnc-viewer.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/APP/vnc%20viewer)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/APP/vnc%20viewer)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/APP/vnc%20viewer)

---

## 📚 相关文章

- [VNC Viewer 快问快答——远程控制你想知道的都在这](https://laoguosofts.github.io/posts/vnc-viewer.html)
- [VNC Viewer 快问快答——从临时协助到跨平台控机一份讲清](https://alipymanbu.github.io/posts/vnc-viewer.html)