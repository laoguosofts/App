## ✨ Markor 是什么

Markor 是一款专门为 Android 打造的纯文本编辑器，主打轻量、快速和开箱即用。它不是那种花里胡哨的写作工具，定位更接近「手机上的 vim」——打开就能写、写完一按眼睛图标就能看渲染效果，没有多余的功能堆砌。

支持的文件格式相当丰富：Markdown、todo.txt、Zim、AsciiDoc、Org-Mode、Vimwiki 都在原生支持列表里，代码文件（Python、Java、JavaScript 等）也能打开，配合行号显示和语法高亮，在手机上读代码也不费劲。编辑器基于 Android EditText 组件构建，性能开销很低，老设备跑起来也流畅。

功能侧是一个持续迭代的状态：v2.7 新增了完整的设置项备份与恢复，换手机或重置系统后能秒级还原所有配置；v2.9 引入了 Admonition 彩色提示框、YAML front-matter 支持和 Mermaid 图表渲染；v2.11 把行号显示做到了编辑器和预览模式的代码块里，这在同类 Markdown 编辑器中很少见；v2.12 又补上了自带的 Markdown 格式参考模板。

适合的使用场景很广：写 Markdown 笔记和技术文档、维护 todo.txt 待办清单、快速记下灵感、在通勤时刷代码片段，甚至写带 KaTeX 数学公式的学术笔记都能胜任。如果你想要一款在 Android 上干净利落、不搞订阅不弹广告的文本编辑器，Markor 是值得考虑的选项。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://markor.qjxz.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：在 Google Play 或各大安卓应用市场搜索「Markor」即可找到对应分发渠道

---

## 🔧 安装步骤

Markor 是 Android 平台的应用，安装方式和普通 APK 类似，但有几个细节值得注意。

1. **核对文件大小和版本号**。APK 文件下载完成后，先和下载页标注的大小、版本号对一下。Markor 的安装包通常在几 MB 量级，如果下载下来明显偏大或偏小，建议重新下载，避免拿到被二次打包的版本。

2. **允许未知来源安装**。如果通过 Google Play 安装可以跳过这步；直接装 APK 的话，系统会弹出「阻止安装未知来源应用」的提示，需要先在系统设置里给文件管理器或浏览器开启「允许安装未知应用」权限，然后再点安装包。

> [!TIP]
> 安装完成后，第一次打开 Markor 会申请存储权限。这是正常的——Markor 需要读写内部存储里的 Markdown 文件。记得点允许，否则会出现「无法保存文件」的报错。

3. **验证是否安装正确**。打开应用后，首页会显示 Notebook 根目录和 QuickNote、ToDo、LinkBox 三个默认文件。如果这些都正常出现，说明安装没问题。

> [!WARNING]
> 不要同时装多个版本的 Markor（比如一个从 Google Play 装、一个从第三方 APK 装）。Android 不允许同包名应用并存，重复安装会直接覆盖，数据文件虽然不会丢，但容易造成版本错乱。

---

## 📖 使用入门

### 设置备份与恢复——v2.7 新增的实用功能

Markor 的设置项超过 50 种，包括 Notebook 根目录路径、QuickNote/ToDo/LinkBox 的文件位置、主题选择（浅色/深色/跟随系统）、语言、字体、Markdown/数学/行号等格式开关、底部操作栏的按钮顺序、Snippets 目录内容、收藏文件夹列表，以及加密主密码的哈希值。换手机、系统重置或者 Markor 更新后设置项丢失，手动一项项重设会非常痛苦。

v2.7 起新增了设置备份功能：进入 Settings 滚动到底部，找到「Backup / Restore settings」入口，点 Backup 后选择一个方便找到的位置（比如 Notebook 根目录或 Downloads 文件夹），Markor 会生成一个 `.markor-settings.json` 备份文件。这个文件非常小——通常不到 100 KB，Snippets 内容是内联在 JSON 里的，不需要单独复制。

恢复时，在新设备或重置后的 Markor 里同样进入 Settings → Restore，选中之前的备份文件，确认后重启 Markor 就能还原到原来的状态。恢复后有几个项目需要手动检查：

| 项目 | 原因 |
| --- | --- |
| Notebook 路径 | 不同设备或 Android 版本路径可能变化 |
| QuickNote/ToDo/LinkBox 路径 | 同上 |
| 加密主密码 | 备份里只有哈希值，恢复后需要重新输入 |
| 收藏文件夹 | 旧路径在新设备上可能不存在 |

> [!IMPORTANT]
> 备份只存设置项，不存你的 Markdown 或 todo.txt 文件内容。内容迁移需要通过 Syncthing、Dropbox 等同步客户端，或者手动复制文件完成。

### 行号显示——编辑器和预览模式都能开

v2.11 起 Markor 可以在编辑器模式显示行号，打开任意文件后，点顶部菜单找到「Line numbers」勾选即可。开启后，编辑器里每行左边会出现从 1 开始的数字。

特别值得一提的是，Markor 是少数在**预览模式的代码块里也显示行号**的 Markdown 编辑器。普通 Markdown 文本部分不会出现行号，只有围栏代码块（```python / ```java 等）里才会渲染行号，这样既方便代码定位，又不让正文被行号干扰。

适合开行号的场景：

| 场景 | 行号价值 |
| --- | --- |
| 500 行以上的长 Markdown | 讨论时说「改第 287 行」比「第三个代码块」精确 |
| 手机上读 .py / .java / .js 文件 | 帮助定位函数定义和调用 |
| 调试 Markdown 预览 | 「第 156 行之后的表格渲染错了」直接定位 |
| 和桌面端 git diff 对照 | 桌面端 diff 有行号，手机端能对应上 |

性能方面，编辑器模式的行号影响几乎为零，预览模式代码块的行号通过 WebView CSS 实现，大文档（>1000 行代码）可能有轻微延迟。设备性能较弱时可以关掉，这是一个可选项，默认不开启。

### Markdown 预览模式——WebView 实时渲染

Markor 有两种模式：Edit Mode 是语法高亮的源码编辑，View Mode 是 WebView 实时渲染的 HTML 预览。切换方式是打开 Markdown 文档后，点顶部菜单里的「眼睛」图标，或按返回键时选择切换。v2.11 之后，预览模式里可以直接打开图片、音频、视频文件，不需要跳到外部应用。

渲染器基于 flexmark-java + Android WebView，支持的 Markdown 特性相当完整：

| 类型 | 支持情况 |
| --- | --- |
| CommonMark 标准 | ✅ 完整 |
| KaTeX 数学公式 | ✅ 行内 `$` 和块级 `$$` |
| Admonition 提示框 | ✅ 彩色块样式（v2.9） |
| YAML front-matter | ✅ 可配置显示字段（v2.9） |
| Mermaid 图表 | ✅ 流程图、时序图等（v2.9） |
| 表格 | ✅ 标准 Markdown + Markor 表格编辑器格式 |
| 代码高亮 | ✅ 按语言着色 |
| 化学式 | ✅ 支持渲染（v2.9） |

预览模式还能做两件实用的事：一是保存为 HTML 文件——Markor 会把渲染好的 HTML 和引用的图片一起打包；二是导出为 PDF——通过 Android 系统打印服务实现，需要 Android 8.0 以上。

> [!NOTE]
> Markor 没有做桌面端常见的「左边编辑右边预览」双栏布局，原因很直接：Android 手机屏幕窄，分屏后编辑区太挤。Markor 的设计节奏是「写完一段 → 切预览看效果 → 切回编辑继续写」，在手机上这个模式反而更顺手。

### KaTeX 数学公式——从 $E=mc^2$ 到完整 LaTeX 块

数学公式功能默认关闭，需要手动开启：进入 Settings → Format → Markdown，勾选「Enable Math」。开启后预览模式就能识别 KaTeX 公式了。

行内公式用单 `$` 包裹，比如 `$E = mc^2$` 渲染后变成 E = mc²；块级公式用双 `$$` 独占一行，适合积分、求和、矩阵等：

$$
\int_{0}^{\infty} e^{-x^2} dx = \frac{\sqrt{\pi}}{2}
$$

KaTeX 相比 MathJax 在手机 WebView 上渲染性能更快，支持绝大多数 LaTeX 数学语法——希腊字母、上下标、分数、求和积分、根号、矩阵、运算符（∀ ∃ ∈ ⊂ ¬ ∧ ∨）、三角函数对数函数等都没问题。完整参考可以看 KaTeX 官方文档。

> [!TIP]
> v2.12 之后，Markor 在 Snippets 里自带了一个「Markor-markdown-reference.md」模板，里面包含了所有支持的 Markdown 格式示例，包括一整段 KaTeX 公式。新建 Markdown 文件后可以在 Snippets 菜单里调出来当参考手册用。

| 公式类型 | 写法示例 |
| --- | --- |
| 行内 | `$x^2 + y^2 = r^2$` |
| 分数 | `$\frac{a}{b}$` |
| 希腊字母 | `$\alpha + \beta = \gamma$` |
| 求和 | `$\sum_{i=1}^{n} i = \frac{n(n+1)}{2}$` |
| 积分 | `$\int_0^1 x^2 dx$` |
| 矩阵 | `$\begin{pmatrix} a & b \\ c & d \end{pmatrix}$` |
| 极限 | `$\lim_{x \to \infty} f(x)$` |

关闭 Math 功能后，预览模式里的 `$...$` 会被当成普通文本原样显示，渲染速度会略微提升。如果完全不用数学公式，保持关闭即可。

---

## 🔗 相关资源

- [Markor官网](https://markor.qjxz.skin)
- [Markor文档](https://markor-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Markor)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Markor)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Markor)

---

## 📚 相关文章

- [Markor 问答速查——手机上写 Markdown，这一份够用吗？](https://laoguosofts.github.io/posts/markor-docs.html)
- [Markor 快问快答——Android 上最干净的纯文本编辑器](https://alipymanbu.github.io/posts/markor-docs.html)
