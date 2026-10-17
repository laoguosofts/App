## ✨ Joplin 是什么

Joplin 是一款开源的 Markdown 笔记应用，定位清晰——纯文本、全平台、本地优先。所有笔记以 `.md` 文件形式保存在本地，不锁格式，不依赖任何特定厂商的云端；多端覆盖桌面（Windows / macOS / Linux）、移动（Android / iOS）和终端，底层数据库加密后可选同步到自家服务器或第三方服务。

它适合哪些人？习惯用 Markdown 写作的人、对数据所有权敏感不想把笔记绑在单一平台上的人、需要跨设备同步但又想完全掌控存储路径的人。如果你曾经为某个笔记软件的格式导出头疼过，或者担心服务商哪天关门自己几年的积累打水漂，Joplin 这种"本地 + 可选云"的路线会很对胃口。

核心能力可以归纳成几条：Markdown 原生编辑、端到端加密同步、标签与多级笔记本组织、全文搜索、移动端拍照即存、外部编辑器接入。没有花哨的卡片视图或 AI 功能，主打一个稳和透明。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://joplin.rggd.pw/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 搜 Joplin（F-Droid / Google Play 均有）；iOS 搜 Joplin；Linux 推荐官方一键脚本

---

## 🔧 各平台安装要点

### Windows / macOS

桌面端提供标准安装包（`.exe` / `.dmg`），双击一路下一步即可。安装过程本身没有坑，真正需要注意的是：

> [!NOTE]
> 安装完成后首次启动时，Joplin 会提示选择笔记本保存位置。默认是用户目录下的 `Joplin` 文件夹，但建议改到一个你自己明确知道的路径——比如专门的笔记目录或同步盘符，方便备份和迁移。

macOS 用户如果从非 App Store 渠道安装，首次打开可能被 Gatekeeper 拦截，右键点击应用 → 选择「打开」即可绕过。Windows 上部分杀毒软件会误报 Electron 应用，添加白名单就行。

### Linux

Linux 上官方推荐两种方式：一键安装脚本和直接跑 AppImage。

**官方安装脚本**会自动下载最新版、创建桌面图标、配置更新通道，一条命令搞定：

```bash
wget -O - <官方安装脚本地址> | bash
```

脚本支持传参切换更新通道或指定版本，写法是在 `bash` 后加 `--` 再接参数，具体可用参数见脚本内的源码注释。

**AppImage** 方式更轻量，适合不想装就跑的场景：

```bash
chmod +x Joplin.AppImage
./Joplin.AppImage
```

> [!WARNING]
> AppImage 启动不了时先查两件事：一是手动下载后默认可能没有执行权限，按上面 `chmod +x` 处理；二是 Ubuntu 22.04 等新版发行版默认不带 `libfuse2`，装一下 `sudo apt install libfuse2` 就好——这不是 Joplin 本身的问题，是 AppImage 格式依赖的系统库。

社区也维护了 Snap、Flatpak 等非官方分发包，但官方对这些渠道不直接提供支持，遇到问题需要找对应维护者，介意的话优先用上面两种官方途径。

### 移动端

Android 和 iOS 直接在各自应用商店搜 Joplin 即可。Android 侧 F-Droid 版本完全开源无追踪，Google Play 版本多了一些统计依赖，按偏好选。

---

## 📖 使用入门：从第一则笔记到进阶玩法

### 新建笔记与编辑区操作

整个过程不超过一分钟：左栏选中要放入的笔记本（初次使用选默认笔记本就行），点击左上角「新建笔记」按钮或按快捷键 `Ctrl+N`（macOS 为 `Cmd+N`），光标直接落在正文区开写。Joplin 会把正文第一行自动当作笔记标题，所以常规写法是先敲一个标题再回车。

编辑器上方有三种显示模式：纯 Markdown 源文本、渲染后的富文本、分屏对照——桌面端支持源文本和渲染结果左右并排，移动端则是「编辑」和「查看」两个标签页切换。不想学 Markdown 语法的也可以直接切富文本模式，所见即所得。

几个高频小操作值得记住：

- **插入图片**：直接把图片拖进编辑器，或复制图片后 `Ctrl+V` 粘贴；
- **插入附件**：工具栏的「附加文件」按钮，或者拖拽任意文件进编辑器；
- **插入时间**：`Ctrl+Shift+T` 快速在光标处插入当前日期时间，做工作日志和学习笔记特别方便；
- **自动保存**：Joplin 没有「保存」按钮，所有改动实时落盘，还会记录进笔记历史（默认保留 90 天），误删误改可以恢复。

移动端有个桌面端没有的实用功能——**直接拍照插入笔记**。课堂板书、合同单据、白板讨论，拍完自动挂进当前笔记，配合桌面端的 OCR 还能搜索图片里的文字。

### 用外部编辑器写笔记

既然笔记是纯文本，就可以交给任何你顺手的编辑器处理。Joplin 原生支持配置「外部编辑器命令」，Vim 党和 VS Code 党都能无缝接入。

打开配置界面（Windows/Linux：`Ctrl+,`；macOS：`Cmd+,`），找到「文本编辑器命令」一项，填入启动编辑器的命令行。几个常用示例：

| 平台 | 编辑器 | 命令 |
| --- | --- | --- |
| Windows | VS Code | `code.exe -n --wait` |
| Windows | Notepad++ | `notepad++.exe` |
| macOS / Linux | VS Code | `code -n --wait` |
| macOS / Linux | Sublime Text | `subl -n -w` |
| Linux | GNOME 文本编辑器 | `gedit --new-window` |

> [!TIP]
> 留意命令里的 `-n` / `-w` 这类参数——它们的作用是新窗口打开并等待文件关闭后返回，不加的话 Joplin 可能感知不到编辑结束，改动也就不会回传。编辑器可执行文件所在目录必须在系统 PATH 里，否则要写完整路径。

官方 FAQ 提到一个 Vim 场景的小坑：如果在 Vim 里打开笔记时光标不可见，通常是 `.vimrc` 里 `set term=ansi` 这行设置导致的，删掉即可恢复。

什么场景值得配外部编辑器？大段重构笔记内容时，外部编辑器的多光标、正则替换比 Joplin 内置编辑器强很多；想批量处理时可以配合脚本对导出的 Markdown 做加工再导回；或者你习惯了自家编辑器的写作环境，不想在两个工具间来回跳。

### 搜索过滤器：从翻找到查询的升级

Joplin 的全文搜索本身够用，但真正能把搜索从「翻找」升级成「查询」的是过滤器机制——在搜索框里加前缀运算符，就能把范围锁到任意维度。

常用的过滤器运算符：

| 运算符 | 示例 | 作用 |
| --- | --- | --- |
| `-`（排除） | `office -trash` | 含 office 且不含 trash |
| `tag:` | `tag:office -tag:spam` | 限标签，可组合可排除 |
| `notebook:` | `notebook:项目A` | 限笔记本（含其子笔记本） |
| `title:` / `body:` | `title:"周报"` | 只搜标题或只搜正文 |
| `created:` / `updated:` | `created:day-2` | 按创建/更新时间 |
| `due:` | `-due:day+7` | 待办到期时间 |
| `type:` | `type:todo` | 只搜笔记或只搜待办 |
| `iscompleted:` | `iscompleted:0` | 未完成的待办 |
| `resource:` | `resource:image/*` | 按附件类型筛 |
| `any:` | `any:1 cat dog` | 条件改 OR（默认全 AND） |

日期语法也挺灵活：绝对日期写 `created:20260101`（当天及以后），区间写 `created:20260101 -created:20260401`（一段时间内），相对时间写 `created:day-2`（近两天）、`updated:year-0`（今年）、`-due:day+7`（一周内到期）。

几个实用的组合示例可以直接照搬：

- 找某笔记本里本周改过的笔记：`notebook:项目A updated:day-7`
- 找没打任何标签的笔记（该补标签了）：`-tag:*`
- 找带 PDF 附件的笔记：`resource:application/pdf`
- 找逾期待办：`type:todo iscompleted:0 -due:day-1`

> [!NOTE]
> 多个过滤器默认 AND 连接；用 `any:1` 切换为 OR；`notebook:` 之间固定是 OR（一条笔记只属于一个笔记本）。写错的过滤器会被当作普通短语搜索，结果不对先检查拼写。

把常用组合记在一条「我的查询」笔记里，随用随复制——比每次现想过滤器语法快得多。

---

## 🔗 相关资源

- [joplin官网](https://joplin.rggd.pw)
- [joplin文档](https://joplin-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/joplin)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/joplin)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/joplin)

## 📚 相关文章

- [用了 Joplin 一周：终于敢把所有笔记托付给一个软件了](https://laoguosofts.github.io/posts/joplin-docs.html)
