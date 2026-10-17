## ✨ APlayer 是什么

APlayer 是一个轻量级、功能完备的网页端音乐播放器组件。它的定位很清晰——给网站站长、博客作者、前端开发者提供一个"拖进页面就能用"的音乐播放器，不需要写复杂的音频处理逻辑，也不用操心浏览器兼容性。

核心能力可以归纳为几个方向：多音频格式支持（原生 HTML5 Audio、HLS 流媒体，还能通过自定义回调接入任何浏览器能解码的格式）、内置歌词显示与自动滚动、灵活的循环模式与播放顺序控制、完整的 JavaScript API 和事件系统。体积上压缩后 JS 只有十几 KB，几乎不拖慢页面加载。

适合的场景相当广泛：个人博客侧边栏挂一个常驻播放器、音乐分享类网站的内嵌播放、直播站点的音频流接入、甚至是做一个小而美的独立音乐站——只要你想在网页里放音乐，APlayer 基本都能胜任。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://aplayer.fewf.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - npm / yarn / pnpm：`npm install aplayer` 一行搞定，适合前端工程化项目
> - CDN：通过 jsDelivr、unpkg 等 CDN 直接引入，适合静态页面快速集成

---

## 🔧 安装方式两选一

APlayer 没有传统意义上的"安装包"——它本身就是一个 JavaScript 库，嵌入网页的方式取决于你的项目类型。总结下来有两条路线：

### 路线一：CDN 引入（适合静态页面 / 快速验证）

最简单，在 HTML 里加两行标签：

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.js"></script>
```

> [!TIP]
> CDN 方式没有离线可用性，也不支持 Tree-shaking，但上手速度最快，三五秒就能跑起来。

### 路线二：npm 安装（适合 Vue / React / Vite 等工程化项目）

```bash
npm install aplayer --save
# 或 yarn add aplayer / pnpm add aplayer
```

然后在代码里 ES Module 方式引入：

```javascript
import 'APlayer/dist/APlayer.min.css';
import APlayer from 'APlayer';
```

npm 安装后 Webpack / Vite / Rollup 会自动把 APlayer 打包进项目的 vendor chunk，压缩后体积大约 **JS ~15KB gzip / CSS ~4KB gzip**。版本由 package.json 管理，升级时改版本号重新 install 就行。

| 对比项 | CDN 引入 | npm 安装 |
|---|---|---|
| 上手速度 | 30 秒 | 几分钟 |
| 需要构建工具 | 不需要 | 需要 |
| 与 Vue/React 配合 | 比较麻烦 | 自然 |
| 版本管理 | URL 里写版本号 | package.json 管理 |
| Tree-shaking | 不支持 | 支持 |
| 离线可用 | 不行 | 可以 |

---

## 📖 使用入门

### 最小可用示例

无论哪种安装方式，初始化都一样——new 一个 APlayer 实例，传一个配置对象：

```javascript
const ap = new APlayer({
    container: document.getElementById('aplayer'),
    audio: [{
        name: '歌曲名称',
        artist: '艺术家',
        url: 'audio.mp3',
        cover: 'cover.jpg'
    }]
});
```

`container` 指向页面上一个空的 div，播放器会渲染到那里。`audio` 是歌曲列表，每首歌至少要有 name 和 url，artist 和 cover 可选。

### 自定义音频类型扩展

这是 APlayer 一个比较强大但容易被忽略的特性——`customAudioType` 参数让你可以接入任何浏览器能解码的音频格式或流媒体协议。思路很简单：告诉 APlayer "遇到 type: 'xxx' 的音频时，调用这个回调函数来处理加载和播放"。

```javascript
const ap = new APlayer({
    container: document.getElementById('aplayer'),
    audio: [{
        name: '歌曲名称',
        url: 'audio-url',
        type: 'myCustomType'
    }],
    customAudioType: {
        'myCustomType': function(audioElement, audio, player) {
            // audioElement 是原生 HTMLAudioElement
            // audio 是当前音频的配置对象（name/artist/url/cover）
            // player 是 APlayer 实例
            audioElement.src = audio.url;
        }
    }
});
```

回调函数的三个参数分工明确：`audioElement` 是你最终要操作的原生 audio 元素，设置它的 src 就能播放；`audio` 是当前歌曲的完整配置，方便你读 name/artist 等字段；`player` 是 APlayer 实例本身，可以调用 notice 弹提示、theme 改主题色等。

> [!WARNING]
> 自定义类型名不能是 `'auto'`、`'normal'`、`'hls'`——这些被 APlayer 内部占用了。

这个机制的实际用途很多。比如做一个带重试的网络电台：在回调里监听 audioElement 的 error 事件，失败时计数重试；或者做先下载再播放的场景——用 fetch 把音频拉成 Blob，再通过 URL.createObjectURL 赋值给 audioElement，播放结束后记得 revoke 释放资源。如果你的自定义类型创建了第三方播放器实例（比如自行接入 HLS），记得在 APlayer 的 destroy 事件里把那些实例也销毁掉，避免内存泄漏。

### 歌词显示控制 API

APlayer 把歌词相关的操作统一放在 `ap.lrc` 命名空间下，只有三个方法，全部用来控制歌词区域的显隐：

| 方法 | 作用 |
|---|---|
| `ap.lrc.show()` | 强制显示歌词区域 |
| `ap.lrc.hide()` | 强制隐藏歌词区域 |
| `ap.lrc.toggle()` | 切换显隐状态 |

这三个方法跟歌词的传递方式无关——无论你用 LRC 文件、JS 字符串还是 HTML 内嵌，只要配置了歌词，它们就能正常工作。唯一的前提是**必须先配置了歌词**，如果所有歌曲都没有 LRC，`lrc.show()` 什么都不会显示。

歌词显隐切换时会触发 `lrcshow` 和 `lrchide` 事件，你可以监听它们来做一些联动——比如在页面上放一个自定义按钮，点击调用 `ap.lrc.toggle()`，同时根据事件同步按钮的文字从"显示歌词"切成"隐藏歌词"。另一个实用场景是响应式布局：窗口宽度小于某个阈值时自动隐藏歌词区域，给播放列表腾出空间。

> [!NOTE]
> 歌词的实时滚动（高亮当前行、自动滚动容器）是 APlayer 内部自动处理的，每播放一帧它都会找到最接近当前时间戳的那一行歌词并滚动到可见位置——你不需要写任何代码。

如果调用了 `ap.lrc.show()` 但歌词仍然空白，优先检查这几个点：`lrcType` 是否正确设置（1=JS 字符串、2=HTML 内嵌、3=文件方式）、每首歌曲对应的 `audio.lrc` 字段是否有值、LRC 格式是否正确、控制台有没有网络错误（文件方式常见 404 或 CORS 问题）。

### 循环模式与播放顺序

这两个参数是两个独立维度，可以自由组合。循环模式 `loop` 控制列表播完后怎么办：

| 取值 | 含义 |
|---|---|
| `'all'` | 全部循环，播完最后一首回到第一首 |
| `'one'` | 单曲循环，当前歌曲播完自动重新播放 |
| `'none'` | 不循环，播完最后一首停止 |

播放顺序 `order` 控制切歌时选列表里的哪一首：

| 取值 | 含义 |
|---|---|
| `'list'` | 顺序播放，严格按 audio 数组的先后顺序切换 |
| `'random'` | 随机播放，每次切换随机选一首 |

组合起来效果一目了然：

| 组合 | 效果 |
|---|---|
| loop:'all' + order:'list' | 按顺序播完一整轮后重头再来 |
| loop:'all' + order:'random' | 随机播，永不停止 |
| loop:'one' + order:'list' | 单曲重复（order 被忽略） |
| loop:'none' + order:'list' | 按顺序播一遍，最后一首停 |
| loop:'none' + order:'random' | 随机播，每首播一次，全部播完停 |

> [!TIP]
> APlayer 会通过 localStorage 记住用户在界面上手动切换的循环和顺序选择，下次加载页面时会覆盖你初始化时传的默认值。如果你希望强制使用代码里设定的值，可以在初始化前 `localStorage.removeItem('aplayer-setting')` 清除用户上次的设置。

默认值是 `loop: 'all'` + `order: 'list'`——也就是"按数组顺序无限循环播放全部歌曲"。初始化之后也可以通过修改 `ap.options.loop` 和 `ap.options.order` 来切换，但需要重新播放或触发一次渲染才会生效。

### 在 Vue / React 中使用

在组件化项目里，通常会把 APlayer 封装成一个组合式函数或 Hook：

```javascript
// Vue 3 示例
import { onMounted, onBeforeUnmounted, ref } from 'vue';
import 'APlayer/dist/APlayer.min.css';
import APlayer from 'APlayer';

export function useAPlayer(containerRef, options) {
    let ap = null;

    onMounted(() => {
        ap = new APlayer({
            container: containerRef.value,
            ...options
        });
    });

    onBeforeUnmounted(() => {
        if (ap) {
            ap.destroy();
            ap = null;
        }
    });

    return { ap };
}
```

关键是**一定要在组件销毁时调用 `ap.destroy()`**，否则会造成内存泄漏。同时如果你的项目需要接入 HLS 流媒体，安装 `hls.js` 后记得 `window.Hls = Hls` 全局挂载一下，让 APlayer 能检测到它。

---

## 🔗 相关资源

- [APlayer官网](https://aplayer.fewf.ren)
- [APlayer文档](https://aplayer-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/APlayer)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/APlayer)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/APlayer)

## 📚 相关文章

- [APlayer 新手入门：给你的网页装个音乐播放器，五分钟搞定](https://laoguosofts.github.io/posts/aplayer-docs.html)
- [APlayer 从入门到上手：5 分钟把音乐播放器嵌进你的网页](https://alipymanbu.github.io/posts/aplayer-docs.html)
