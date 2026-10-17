## ✨ PakePlus 是什么

PakePlus 是一个基于 Rust + Tauri2 的跨平台 GUI 打包工具，核心理念是"把任意网页或前端项目快速变成能双击运行的独立应用"。它接收一个网址、一段 HTML，或者 Vue/React/Cocos/Unity 项目编译后的 dist 目录，输出真正可安装的 Windows/macOS/Linux 桌面应用，以及 Android/iOS 手机应用。

很多人会把 PakePlus 跟 Electron 或 Tauri CLI 混淆，这里先澄清几个关键区别：它不是浏览器插件（产物是独立 exe/dmg/apk），不是 PWA 生成器（产物有独立进程和系统级权限），也不是 Tauri CLI 的简单包装——它额外提供了 GUI 界面、GitHub Actions 云端编译（让你不用本地装 Rust）、项目管理面板、以及 JS 注入等内置功能包。

体积优势是 PakePlus 最直观的卖点。Electron 每个应用内嵌一份完整 Chromium（通常 150-250MB），而 PakePlus 用各系统原生 WebView（Windows 用 WebView2、macOS 用 WKWebView、Linux 用 WebKitGTK），应用本身只是 Rust 编译出来的薄壳加上网页内容，加起来 5MB 上下，冷启动速度也比 Electron 快一个量级。

PakePlus 的使用方式分成两条路：云端打包（需要 GitHub Token，fork 模板仓库后通过 GitHub Actions 自动编译，一次点发布多平台同时出包）和本地打包（无需 Token，直接在本地机器上打包，30 秒左右出一个单平台安装包，不需要本地装任何依赖环境）。两条路都是 PakePlus 客户端驱动的，Token 填了就走云端，没填或填失败就走本地。

适用人群相当广泛：工具爱好者把每天开的网站（小红书、YouTube、ChatGPT 等）变成独立客户端，配合系统托盘常驻和快捷键唤起；前端开发者把 Vue/React 项目编译出的 dist 丢进去几秒出桌面客户端，省去服务器成本；独立开发者为在线 SaaS 快速构建轻量桌面壳；游戏开发者把 Cocos/Unity WebGL 游戏变成多平台独立安装包；内网运维把内部管理后台藏在客户端后面，防止爬虫探测。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://www.usea.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：PakePlus 产物可分发到各系统应用商店；工具本身从官网下载 GUI 客户端（约 10MB）

---

## 🔧 安装步骤

PakePlus 客户端本身是一个轻量的跨平台应用，安装过程在各系统上都比较直接。

### Windows 平台

1. 从官网下载 Windows 版安装包（约 10MB）
2. 双击运行安装程序，选择安装路径（建议默认路径，避免中文目录）
3. 安装完成后桌面会出现 PakePlus 快捷方式

> [!WARNING]
> Windows 杀毒软件可能会对 PakePlus 生成的安装包报误杀——这是因为 PakePlus 使用 Rust + Tauri2 编译，且打包产物体积小、结构轻，部分启发式扫描会误判。遇到这种情况请将文件加入杀毒软件白名单，或从官方渠道重新下载核对哈希值。

### macOS 平台

1. 下载 dmg 文件，双击打开
2. 将 PakePlus 图标拖入 Applications 文件夹
3. 首次运行如提示"无法打开来自未知开发者"，前往「系统设置 → 隐私与安全性」，在底部选择"仍要打开"

### Linux 平台

支持 AppImage、deb、rpm 等多种分发格式，下载对应发行版的包后安装即可。

### 打包产物的安装

PakePlus 生成的打包产物（桌面 exe/dmg/deb 或移动端 apk/ipa）安装方式与普通软件相同。移动端安装包需要注意：

> [!TIP]
> Android 安装 PakePlus 生成的 apk 时，需要在系统设置中允许"未知来源"应用安装（路径通常为「设置 → 安全 → 安装未知应用」）。iOS 的 ipa 安装需要通过 Apple 开发者账号签名或使用 AltStore 等侧载工具。

---

## 📖 功能拆解与实战

### 两条打包路径：云端 vs 本地

这是 PakePlus 最核心的设计选择，直接决定了你的工作流。

| 对比项 | 云端打包（GitHub Actions） | 本地打包 |
| --- | --- | --- |
| 是否需要 GitHub Token | 是 | 否 |
| 是否需要本地 Rust 环境 | 不需要 | 不需要 |
| 一次出几个平台 | Windows + macOS + Linux + Android + iOS | 仅当前平台 |
| 打包速度 | 几分钟（取决于 GitHub Actions 队列） | 30 秒左右 |
| 适合场景 | 正式发版、多平台同时出包 | 快速预览、调试 |

> [!NOTE]
> 云端打包需要先 fork 项目维护者的模板仓库到你自己的 GitHub 账号，然后在 PakePlus GUI 里填入 Token。Token 只需要 `repo` 权限，建议专门为 PakePlus 创建一个最小权限的 Token。

### 项目创建与基本配置

无论走云端还是本地，第一步都是在 PakePlus GUI 里创建一个项目。配置项不多，但每一项都影响最终产物：

| 配置项 | 说明 | 注意点 |
| --- | --- | --- |
| 软件名称 | 显示在应用窗口标题和安装包里的名称 | 中英文均可，避免特殊字符 |
| 网站地址 | 要打包的网页 URL，或者 dist 目录路径 | URL 必须能正常访问；dist 目录必须包含 index.html |
| 软件标识 | 类似 `com.example.app` 的反向域名格式 | 移动端 APP 的包名由此决定，必须全局唯一 |
| 软件版本 | 语义化版本号，如 `1.0.0` | 发版时递增，应用商店审核要求 |

### 移动端窗口尺寸预设

打包到移动端时，PakePlus 提供 iPhone、Android、iPad 三种窗口尺寸预设，对应不同设备的常见屏幕比例：

| 预设 | 默认宽度 | 默认高度 | 覆盖的典型设备 |
| --- | --- | --- | --- |
| **iPhone** | 390 pt | 844 pt | iPhone 12 / 13 / 14 标准系列 |
| **Android** | 412 dp | 915 dp | Android 中高端主流机型 |
| **iPad** | 820 pt | 1180 pt | iPad Air 4/5、iPad 10 代 |

这些数字是 PakePlus 设定的逻辑尺寸（pt/dp），不是物理像素——实际渲染时系统会根据设备的像素密度自动缩放。PakePlus 移动端 APP 运行时，壳先启动，WebView 按预设尺寸渲染内容，但最终显示区域会填充设备的整个屏幕，WebView 会自动缩放以适应屏幕物理尺寸。

> [!TIP]
> 预设尺寸更多是告诉 WebView 初始渲染用什么逻辑分辨率——最终显示效果由设备屏幕加上 CSS 响应式布局共同决定。如果要同时出桌面端和移动端 APP，最好让 Web 内容本身就是响应式的（`body { max-width: 100%; overflow-x: hidden; }`、`img { max-width: 100%; height: auto; }`、配合 `@media` 查询）。

移动端还支持横竖屏切换（由设备重力感应决定），如果想锁定方向需要在"更多配置"里手动写 Info.plist 和 AndroidManifest.xml，没有 GUI 入口。移动端独有的 WebView 特性包括自动适配刘海屏/灵动岛、CSS 可用 `env(safe-area-inset-bottom)` 适配底部安全区、iOS WKWebView 默认支持长按弹出系统菜单等。

### 内网壳应用实战

一个很实用的场景是把公司内部管理后台打包成 PakePlus 客户端，隐藏内网地址、只允许客户端访问。痛点很明确：内网 URL 明文可能被截图外泄、员工浏览器书签暴露内网结构、多个内网系统缺少统一入口。

最简单的做法：PakePlus 创建项目，软件地址直接填内网 URL，打包后生成的 APP 里内网地址被嵌入二进制文件——用户不会看到 URL，双击 APP 直接进入系统。如果想更灵活，比如加一个"连接中…"过渡页面或者做 JS 注入，可以先写一个本地代理 HTML，里面用 JavaScript 的 `window.location.replace()` 跳转到内网地址，再把这个 HTML 丢进 PakePlus 打包。

附加安全措施可以通过 JS 注入实现：隐藏页面底部的内网 IP 提示元素、禁用右键防止查看源码、禁用 F12 和 Ctrl+Shift+I 开发者工具快捷键。不过要注意，这些措施只能"防君子不防小人"——有经验的逆向工程师还是能从二进制里提取出 URL。真正的内网隔离还是要靠 VPN + 防火墙 + IP 白名单，PakePlus 壳的核心作用是防爬虫、防意外泄露。

内网多系统整合时，可以为每个系统分别打包一个 PakePlus APP，员工桌面放一排快捷方式，双击就进对应系统，不用记 URL、不用存书签。内网分发最方便的方式是 PakePlus 云端打包生成绿色版 zip，放到内网文件服务器，员工下载解压双击 exe 即可，不需要管理员权限、不需要安装器、不写注册表。

### JS 注入与进阶配置

PakePlus 内置了 JS 注入功能，适合在不修改原始网页的情况下做一些定制化处理。常见用法包括：注入去广告脚本、隐藏页面敏感元素、禁用特定交互（右键、F12）、或者注入自定义样式调整外观。

> [!WARNING]
> PakePlus 明确声明打包产物仅限个人使用，不得商用、不得传播。这是项目维护者设定的法律边界，使用者需要自行遵守。如果你需要签名分发商用软件，应该考虑其他方案。

---

## 🔗 相关资源

- [PakePlus官网](https://www.usea.quest)
- [PakePlus文档](https://pakeplus-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/PakePlus)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/PakePlus)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/PakePlus)

---

## 📚 相关文章

- [把 PakePlus 翻了个底朝天——六个你可能没注意到的冷门玩法](https://laoguosofts.github.io/posts/pakeplus-docs.html)

- [PakePlus 藏着的五个冷门开关——从"网页壳"到真正桌面应用的进阶玩法](https://alipymanbu.github.io/posts/pakeplus-docs.html)
