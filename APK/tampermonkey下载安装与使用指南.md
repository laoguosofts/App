## ✨ Tampermonkey 浏览器脚本管理器

Tampermonkey，中文圈俗称「油猴」，是目前最主流的浏览器用户脚本管理器。它本身不是一个独立的网页应用，而是以浏览器扩展的形态存在——装完之后，你的浏览器就获得了一个全新能力：**让用户自己写或从社区安装的脚本，在打开任意网页时自动运行**，改写页面的 DOM、样式、请求，甚至注入新功能。

它的核心价值很朴素：浏览器给你的是一个通用的网页视图，但每个人的使用习惯都不一样。油猴让你不再被动接受网页的样子——想改哪里改哪里，想加什么加什么。你可以给视频网站加倍速按钮、给论坛去广告、给电商比价、给后台管理系统加快捷键……社区里沉淀了成千上万现成的脚本，复制安装就能用；开发者也可以基于它写出完整的自动化工作流。

支持的浏览器覆盖了目前主流的全部阵营：Chrome、Edge、Firefox、Safari、Opera、Brave 等都能装，跨平台（Windows、macOS、Linux、移动端浏览器）。如果你的日常有一半以上时间耗在网页上，油猴大概率能帮你省掉一些重复点击和视觉干扰。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://tampermonkey.qhah.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Chrome Web Store、Microsoft Edge Add-ons、Firefox Browser Add-ons、Safari Extensions 搜「Tampermonkey」即可

---

## 🔧 安装步骤

油猴是浏览器扩展，不是常规的软件安装包——你不需要下载 exe 或 dmg，全程在浏览器扩展商店里点几下就好。不同浏览器的入口叫法略有差异，但流程一致。

**Chrome / Edge 用户：**

1. 打开浏览器的扩展管理页面（地址栏输入 `chrome://extensions` 或 `edge://extensions`）；
2. 打开右上角「开发者模式」开关（某些商店安装不需要这步，但了解一下没坏处）；
3. 访问 Chrome Web Store 或 Edge Add-ons，搜索「Tampermonkey」；
4. 点击「添加到浏览器」/「获取」，确认弹窗里的权限请求；
5. 安装完成后，浏览器工具栏右上角会出现一个油猴的黑色圆形图标。

**Firefox / Safari 用户：**

Firefox 到 Add-ons 页面、Safari 到 Safari Extensions 里搜索安装，流程类似。Safari 用户需要注意，较新版本的 macOS 在「Safari 设置 → 扩展」里管理开关权限，首次安装后可能需要手动勾选启用。

> [!WARNING] 别在非正规渠道下载
>
> 油猴本身是开源项目，但网上流传的所谓「破解版」「增强版」大部分捆绑了广告追踪器或恶意代码。始终从官方扩展商店安装——商店有签名校验，被篡改的版本上不了架。

装完之后，随便打开一个网页，点工具栏的油猴图标，弹出菜单里能看到「管理面板」「启用/禁用」「获取新脚本」这些选项，说明安装成功。如果菜单里全是灰的或点不开，先检查扩展是否被浏览器禁用了。

---

## 📖 使用入门

油猴的使用分两种场景：**作为普通用户安装现成脚本**，和**作为开发者自己写脚本**。两种场景共享同一套运行机制，但日常体验差异很大。

### 脚本是什么

一个油猴脚本本质上就是一段 JavaScript 代码，包在一个固定格式的元数据头里。当你打开某个网页，油猴会遍历自己管理的所有脚本，看哪个脚本声明了「我要在这个域名上运行」，匹配上了就把代码注入页面执行。

脚本头部（也就是所谓的 UserScript 头）里的声明字段，决定了脚本的行为边界。几个关键的：

- `@name` 脚本名称、`@description` 脚本描述、`@version` 版本号——这些是给人看的；
- `@match` / `@include` 声明脚本在哪些 URL 上生效——可以写精确域名，也可以写 `*://*.example.com/*` 这种通配；
- `@grant` 是权限白名单——脚本要调用哪些特殊能力，必须列在这里才能用；
- `@connect` 允许脚本向哪些外部域名发请求。

### 跨域请求：GM_xmlhttpRequest

这是油猴提供的 GM 函数里最重要也最需要谨慎的一个。普通网页里的 JavaScript 受浏览器同源策略限制，只能向当前域名发请求——这是浏览器的安全基础。但 `GM_xmlhttpRequest` 让脚本绕过同源策略，向任意域名发 HTTP 请求。

```javascript
GM_xmlhttpRequest({
  method: "GET",
  url: "https://api.example.com/data",
  headers: { "User-Agent": "my-script" },
  onload: (response) => console.log(response.responseText),
  onerror: (error) => console.log(error)
});
```

使用前必须做两件事：在 `@grant` 里声明 `// @grant GM_xmlhttpRequest`，以及在 `@connect` 里声明脚本允许请求的域名（含子域）。声明不全时，未覆盖的域名会触发用户确认弹窗。官方建议脚本作者把已知域名全声明出来减少弹窗，再补一条 `@connect *` 让用户可以一键全放行。

> [!NOTE] 安全视角
>
> 这个函数是脚本「往外送数据」的主要通道。一个恶意脚本可以把你正在浏览的页面内容、表单数据打包发往第三方服务器。安装脚本前，看到声明了 `GM_xmlhttpRequest` 但功能描述里完全没提到需要联网的脚本，多留个心眼。

函数还支持 `redirect` 控制重定向处理、`anonymous` 不带 Cookie 发请求（走 fetch 模式）、`timeout` 设置超时毫秒数、`responseType` 选择返回格式（arraybuffer / blob / json / stream）。较新版本支持 `fetch: true` 指定用 fetch 而非 XMLHttpRequest 实现。

### 轻量 GM 函数：通知、菜单、剪贴板

除了跨域请求和数据存储这两件重头戏，日常脚本最常用的是四组轻量函数：

| 函数 | 用途 | 常见场景 |
| --- | --- | --- |
| `GM_notification` | 弹出桌面通知 | 脚本后台任务完成时提醒用户 |
| `GM_registerMenuCommand` | 在油猴弹出菜单里注册自定义命令 | 提供「设置」「开关功能」的交互入口 |
| `GM_setClipboard` | 写入剪贴板 | 一键复制页面上的文本或链接 |
| `GM_openInTab` | 以可控方式打开新标签页 | 后台打开关联页面、无痕模式打开 |

`GM_notification` 支持 `silent: true` 不响铃、`timeout` 自动关闭毫秒数、`tag` 复用更新同一条通知、`url` 指定点击跳转地址。既没 `url` 也没 `tag` 时，通知会随页面卸载自动消失。

`GM_registerMenuCommand` 注册的命令会出现在油猴弹出菜单的脚本菜单区，也可以出现在页面右键菜单。支持 `accessKey` 提供键盘快捷字母（弹出菜单打开时按该字母直接触发）、`autoClose: true` 点完自动收菜单。这是脚本给用户提供设置面板、功能开关的主流做法。

> [!TIP] 剪贴板是单向的
>
> 出于安全，脚本**只能写不能读**剪贴板。如果你需要读取剪贴板内容，得让用户手动粘贴或改用页面自有的方案。

这四个函数都需要在头部 `@grant` 里列出对应函数名才能使用。

### @grant：权限白名单机制

`@grant` 是脚本头部里「要什么权限」的声明。官方定义很直白：**用于把 GM_* 函数、unsafeWindow 对象以及部分强大的 window 函数加入白名单**。关闭标签页、聚焦窗口这类「强力」的 window 能力，也要走 `@grant` 声明。

不写任何 `@grant` 时，脚本仍运行在沙箱里——只是没申请任何 GM 函数，直接调用会报错。这和 `// @grant none` 完全不是一回事：`none` 意味着**沙箱被关闭**，脚本直接运行在页面环境里，与页面自身的脚本同处一个世界。代价是所有 GM 函数不可用（唯一例外是只读的 `GM_info`），但好处是代码可以直接访问页面的全局变量与函数。

| 脚本要做什么 | grant 选择 |
| --- | --- |
| 只改 DOM、改样式 | `none` 或空列表 |
| 存数据、跨域请求、剪贴板、通知 | 列出对应 GM 函数 |
| 要读页面自己的 JS 变量 | `@grant unsafeWindow` |

安装脚本时确认页展示的「权限」就是 `@grant` 列表。列表越长，脚本对浏览器能力的索取越多。一个声明了跨域请求的脚本，安装时你在确认页就能看到它要什么能力——这也是油猴作为扩展比浏览器控制台「直接跑代码」更透明的地方。

### 外部编辑器工作流：@require 本地文件

油猴内置的编辑器够用，但脚本写大了（几百行以上），大家还是想回自己顺手的编辑器——VS Code、WebStorm、Vim，或者任何支持补全和 git 管理的工具。官方给了两条路。

**路径一：@require 本地文件（实时生效）**

1. 把脚本内容复制出来，存成本地文件；
2. Tampermonkey 里打开该脚本，删掉代码区全部内容，只保留 `// ==UserScript==` 元数据头；
3. 在头部加一行 `@require`，指向本地文件：

```javascript
// @require file:///C:/Users/用户名/Documents/myscript.js
```

4. 开启浏览器的本地文件访问权限——没有这步，扩展读不到磁盘文件。

之后的工作流就很舒服了：本地文件用任何编辑器改 → Ctrl+S 保存 → 刷新目标页面，脚本用的就是最新代码。官方较新版本还支持打开本地脚本文件并跟踪磁盘变更来加速开发。

**路径二：Tampermonkey Editors 扩展 + vscode.dev**

官方另发了一个独立扩展 **Tampermonkey Editors**，用途是把编辑工作接到 **vscode.dev**——在浏览器里的 VS Code 环境编辑脚本，享受 IDE 补全和调试体验，又不用折腾本地文件权限。

| 场景 | 建议 |
| --- | --- |
| 偶尔改改、脚本很小 | 内置编辑器 |
| 脚本有几百行、要 git 管理 | 路径一（本地文件） |
| 想要 IDE 补全、不想动本地权限 | 路径二（Editors 扩展） |

> [!WARNING] @require 的路径格式
>
> `file://` 路径里的斜杠方向与盘符格式要按 `file:///C:/...` 的**三斜杠写法**（`file:` 后面两个斜杠，再加盘符的一个斜杠）。很多人写成 `file://C:/...` 或 `file:///C:\...` 导致扩展读不到文件。权限开了但脚本没更新时，先检查保存的文件路径与 `@require` 声明的是否为同一个文件。

两条路径有个共同的边界：`@require file://` 只对**代码区**生效，元数据头（`@name`、`@match`、`@grant` 这些）仍在 Tampermonkey 里维护，没法迁到外部文件。

---

## 🔗 相关资源

- [tampermonkey官网](https://tampermonkey.qhah.skin)
- [tampermonkey文档](https://tampermonkey-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/tampermonkey)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/tampermonkey)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/tampermonkey)

---

## 📚 相关文章

- [油猴用了两年，我想说说那些当初没注意到的事](https://laoguosofts.github.io/posts/tampermonkey-docs.html)

- [用了三年油猴，我对 Tampermonkey 的真实感受](https://alipymanbu.github.io/posts/tampermonkey-docs.html)
