## ✨ Ruffle 是什么：让消失的 Flash 内容重新活起来

2020 年底 Adobe Flash Player 彻底停止维护，浏览器厂商陆续移除了原生 Flash 支持——那些年攒下来的 Flash 小游戏、动画、教育课件、甚至一些老网站的交互组件，一夜之间全部变成了打不开的"古董"。

Ruffle 就是为这个场景而生的。它是一个用 Rust 编写的 Flash Player 模拟器，通过 WebAssembly 运行在现代浏览器里，**不需要任何浏览器插件或扩展**——只要嵌入一行脚本，就能自动接管页面上所有旧 Flash 标签，把 `.swf` 文件在当前页面里重新播放出来。除了 Web 版，Ruffle 还有独立的桌面播放器，可以直接双击打开本地 SWF 文件，像用 VLC 打开视频一样方便。

它的兼容性覆盖了 Flash Player 9 到 AS3 的大部分特性，常见的 Flash 小游戏（特别是塔防、射击、益智类）和动画基本都能正常跑。开发团队活跃更新中，对一些边缘特性的支持还在持续补齐。如果你手头还攒着当年的 Flash 作品、或者偶然翻到一个老 Flash 站想看看，Ruffle 是目前最靠谱的方案。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://ruffle.fewa.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Ruffle 无移动端 App，PC 端直接下载桌面版安装包；Web 版通过 CDN 一行脚本接入网站

---

## 🔧 安装步骤

### 桌面版（Windows）

1. 从下载页找到最新版 Windows 安装包，下载到本地
2. 双击安装包，选择安装路径（默认 `C:\Program Files\Ruffle`），一路下一步即可
3. 安装完成后，开始菜单里会出现 Ruffle 快捷方式；同时 `.swf` 文件默认关联到 Ruffle，双击就能打开

> [!TIP]
> 如果安装包被杀毒软件误报，直接在杀毒软件里加白名单即可——Ruffle 是开源项目，代码托管在 GitHub，无恶意行为。

### 桌面版（macOS）

下载 `.dmg` 文件，双击打开后把 Ruffle 图标拖进「应用程序」文件夹。首次启动如果遇到 Gatekeeper 拦截，在「系统设置 → 隐私与安全性」里点「仍要打开」就行。

### Web 版（网站嵌入）

Web 版不需要安装，只需要在你的网页里加一行 script 标签：

```
<script src="https://unpkg.com/@ruffle-rs/ruffle"></script>
```

放在 `<head>` 或 `</body>` 前都行。浏览器加载完这行脚本后，Ruffle 会自动扫描页面上的 Flash 元素并接管播放——你的访客不需要做任何事，打开网站就能看到旧 Flash 内容正常运行。

> [!NOTE]
> 中国大陆访客访问 unpkg 可能较慢，建议换成 jsDelivr 镜像：`<script src="https://cdn.jsdelivr.net/npm/@ruffle-rs/ruffle"></script>`

---

## 📖 使用入门

### 桌面播放器：像 VLC 一样打开 SWF

桌面版的界面很干净，上方是菜单栏，中间是播放区域。最常用的操作：

- **打开本地文件**：Ctrl+O 选择 SWF 文件，或者直接把 SWF 拖进窗口
- **打开远程 SWF**：文件菜单选「打开」，输入 SWF 的 URL 即可加载
- **全屏播放**：F11 进入全屏，Esc 退出
- **缩放**：视图菜单里有从 25% 到 200% 六档预设，还有「适合窗口」「保持比例」「拉伸」三种自适应模式

桌面版的菜单结构非常完整，六大菜单覆盖了日常使用和调试所需的全部功能：

| 菜单 | 核心功能 | 什么时候用 |
| --- | --- | --- |
| **文件** | 打开 SWF、保存播放状态、退出 | 日常打开文件 |
| **视图** | 全屏、缩放、显示 FPS、打开调试器 | 调性能、看帧率 |
| **控制** | 播放/暂停、单帧步进、循环播放 | 逐帧调试动画 |
| **书签** | 存时间点和变量值、快速回档 | 有存档系统的游戏 |
| **调试** | 场景信息、MovieClip 列表、显示对象 | 开发者排查问题 |
| **帮助** | 文档链接、检查更新、版本信息 | 查版本、看文档 |

书签菜单是个被低估的实用功能——它允许你把「某个 SWF + 某个时间点 + 某些变量值」存成一个书签。下次打开书签时，Ruffle 会加载对应 SWF、跳到对应时间点、恢复之前设置的参数。对于有存档系统的 Flash 游戏来说，这相当于自带了一个快速回档工具。

参数设置面板（调试菜单里进入）里可以配置一些跨文件的全局偏好，比如图形处理后端（默认自动选择 wgpu，也可以手动切 CPU 纯软件渲染）、首选负载档位（高性能/平衡/低功耗，笔记本省电时有用）、音频设备选择（蓝牙耳机切换）、语言（支持包括中文在内的多语言）、主题（深色/浅色/跟随系统）等。如果某个 SWF 渲染有问题，可以试试切换图形后端到 CPU 模式。

### Web 版：一行脚本的魔力

Web 版最省心的地方就是 polyfill 模式——放一行 `<script src="ruffle.js">`，Ruffle 自己扫描 DOM、自己替换、自己开始播放，你不需要写任何 JS 代码。

它会扫描三类 Flash 相关元素：原生的 `<object>`（带 Flash Player 的 classid）、`<embed>`（带 `application/x-shockwave-flash` type）、以及流行的 SWF 嵌入库生成的标签（SWFObject、SWFAction、Adobe 官方的 AC_RunActiveContent.js）。这些库在当年几乎统治了所有 Flash 网站，所以 Ruffle 对它们有专门的识别逻辑。甚至 iframe 里直接指向 `.swf` 的情况也能识别——但如果 iframe 指的是另一个 HTML 页面、那个页面里才嵌了 Flash，Ruffle 默认扫不到 iframe 内部，需要在 iframe 页面里也引一份脚本。

替换过程是这样的：Ruffle 找到一个 Flash 元素 → 读它的宽高、SWF URL、wmode 参数（透明/窗口/不透明）→ 从 DOM 里**移除**原始元素 → 插入自己的 `<div id="ruffle-player-xxx">` 容器 → 异步加载 WASM 运行时 → 把 SWF 喂进去开始播放。原始元素不会被恢复，所以如果你的页面有依赖原始 Flash Player 的 JavaScript 代码（比如 `document.getElementById('flash-id').GetVariable()`），那段代码会失效，需要用 Ruffle 的 JS API 来访问播放器实例。

> [!WARNING]
> Polyfill 不是万能的。如果你的 Flash 标签是 JavaScript 动态插入的（比如页面加载后 3 秒用户点按钮才生成），Ruffle 可能在动态插入之前就扫完 DOM 了，这个新元素不会被接管。解决办法是动态插入之后手动触发一次扫描：`window.RufflePlayer.polyfill();`

### 网站嵌入常见坑

如果你是站长，把 Ruffle 嵌进自己的网站时，最容易遇到三类问题：

**CORS 跨域**：Console 里报 "Access to fetch has been blocked by CORS policy"。原因是 Ruffle Web 版内部用 `fetch()` 加载 SWF，浏览器对 fetch 有跨域限制。最简单的解法——把 SWF 和页面放在同一个域名下，完全不涉及跨域。如果必须从别的域名加载，在那个域名的服务器上给 `.swf` 响应加 `Access-Control-Allow-Origin: *` 头即可。Nginx 一行配置就能搞定，Cloudflare 用户用免费的响应头 Transform Rule 5 分钟也能搞定。

**混合内容**：Console 里报 "Mixed Content: The page was loaded over HTTPS, but requested an insecure resource"。现代浏览器禁止 HTTPS 页面里加载 HTTP 资源。Ruffle 默认会尝试把 HTTP URL 自动升级成 HTTPS（`upgradeToHttps: true`），但如果目标服务器没配 HTTPS，升级还是会失败。三条路：开目标站点的 HTTPS、用 JPEXS 反编译 SWF 把硬编码的 HTTP URL 改成 HTTPS、或者临时用代理中转调试。

**CSP 严格策略**：如果你的网站启用了严格的 Content-Security-Policy，需要确保 CSP 头里包含 `worker-src blob:` 或 `'wasm-unsafe-eval'`——WebAssembly 编译需要这些权限。少了这个，Ruffle 脚本本身能加载，但 WASM 运行时起不来，SWF 播放不出来。

### 自定义配置

默认 Ruffle 用最小配置启动。如果你想自定义缩放模式、自动播放策略等，可以在 `<script src>` 之前写一个配置对象：

```
<script>
window.RufflePlayer = window.RufflePlayer || {};
window.RufflePlayer.config = {
  scale: "showAll",
  forceScale: true,
  autoStart: "auto"
};
</script>
<script src="https://unpkg.com/@ruffle-rs/ruffle"></script>
```

配置对象里能控制的项很多，常用的包括缩放模式（`showAll` 保持比例适配容器、`exactFit` 拉伸填满、`noScale` 按原始尺寸）、是否强制缩放、自动播放策略、以及 polyfill 的启用开关。如果你只想完全手动控制、不让 Ruffle 自动扫描页面上的 Flash 标签，可以在配置里关掉 `enableFlashPlayerFix` 和 `enableScriptFix`，之后用 JS API 的 `createPlayer()` 手动创建播放器实例。

---

## 🔗 相关资源

- [ruffle官网](https://ruffle.fewa.skin)
- [ruffle文档](https://ruffle-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/ruffle)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/ruffle)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/ruffle)

---

## 📚 相关文章

- [Ruffle 到底值不值得掏钱](https://laoguosofts.github.io/posts/ruffle-docs.html)
- [Ruffle 到底值不值得掏钱？一个老 Flash 玩家的付费账本](https://alipymanbu.github.io/posts/ruffle-docs.html)
