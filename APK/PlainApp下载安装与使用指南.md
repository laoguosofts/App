## ✨ PlainApp 是什么

PlainApp 是一款把安卓手机变成本地 Web 服务器的开源工具。手机端 App 负责把联系人、短信、通话记录、文件、笔记、书签、RSS 订阅、已装应用等数据暴露成本地 HTTP 接口，然后你用同局域网的浏览器（Chrome / Brave / Firefox / Safari 都行）就能在电脑上查看和管理手机里的一切。

它的核心理念很鲜明：**反对推荐算法和无限滚动**。官方关于页的原话是——主流互联网产品从"交付价值"转向"占用屏幕时间"，通过 AI 推荐和无限滚动造成全球范围内的数字成瘾。PlainApp 团队故意完全移除推荐算法和社交机制，唯一有意义的指标是响应速度和任务完成率，而不是用户停留时长。

所以你在 PlainApp 里看不到信息流、看不到好友动态、看不到算法给你推的任何东西——**你管什么、什么时候管、管多少，全由你自己决定**。

功能覆盖相当全面：联系人批量导出到 CSV / VCard、浏览器端发短信、通话记录查询、完整的文件管理器（双栏表格、拖拽上传、ZIP 虚拟目录）、RSS 三栏阅读器（订阅 + 条目 + 正文）、笔记（支持 RSS 文章一键存成 Markdown 笔记）、书签、图片视频音乐内置预览、番茄钟、屏幕镜像……浏览器端和手机端看的是同一份数据，改了即同步。

适合谁？想在电脑上高效管理手机数据的人、重视隐私不想让云端碰手机内容的人、喜欢折腾开源工具和自动化脚本的开发者——以及已经被推荐算法搞烦了、想把"选择权"拿回来的普通用户。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://plainapp.fedq.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜 PlainApp（注意：Play 版有阉割，SMS / 通知监听 / 批量导 APK 等功能缺失）
> - F-Droid：搜 `com.ismartcoding.plain`（最严格 FOSS，但更新最慢、缺少 AI 图片搜索）

---

## 🔧 版本选择与安装

PlainApp 的手机端安装渠道有四条，功能完整度、更新速度、隐私表现各不相同，直接上对比表：

| 渠道 | 功能完整度 | 更新速度 | 隐私 / FOSS 兼容 | 适合谁 |
| --- | --- | --- | --- | --- |
| GitHub Release | 完整 ✅ | 最快（开发者直接发） | ✅ 来自源码、无 Play 依赖 | 绝大多数用户 |
| Google Play | **阉割** | 滞后（等 Play 审核） | ❌ 带 Google Play 基础组件 | 不想处理"未知来源安装"的普通用户 |
| F-Droid | 大部分完整 | 最滞后（独立源码编译） | ✅ 最严格 FOSS | 重视纯 FOSS 但能等的用户 |
| 直链 `d.plainapp.app` | 同 GitHub | 同 GitHub | 同 GitHub | GitHub 访问困难时的备用 |

> [!TIP]
> **结论先说**：能访问 GitHub 并允许"未知来源安装"的话，**GitHub Release 永远是首选**——功能完整、更新最快、支持内置自动更新、100% 来自源码编译。

### 安装步骤（以 GitHub Release APK 为例）

1. 在手机浏览器下载最新 Release 的 APK，文件名形如 `PlainApp-<版本>-64bit-Recommended.apk`，体积约 100 MB（同时提供 32bit 旧设备包和 Universal 包）
2. 系统会弹出"是否允许安装未知来源应用"的提示，点击**允许**——这是 Android 的安全机制，只允许当前浏览器安装即可
3. 安装完成后打开 PlainApp，首次启动会依次请求：**所有文件访问权限**、**通知监听权限**、**通话记录权限**、**短信权限**等——按提示逐个授权（**所有文件访问权限（MANAGE_EXTERNAL_STORAGE）是文件管理功能的前提**，没授权的话 Files 页面会显示提示）
4. 授权完，PlainApp 会显示一个本地地址和端口（形如 `https://192.168.x.x:8443`）——这就是你在电脑浏览器里要访问的地址

> [!WARNING]
> **务必核对版本号和文件大小**。安装包体积稳定在 100 MB 左右，远小于这个值可能是不完整的下载；版本号可以和 Release 页面对比确认。

### Google Play 版阉割详情

如果你从 Play Store 下载 PlainApp，**发现 SMS 页面加载不出来是正常的**——这不是 bug，是商店策略的妥协。Play Store 限制了几类敏感权限，导致 Play 版无法包含：

- **SMS 管理** — 无法从浏览器发 SMS / 看通话记录
- **Notification Mirroring** — 通知监听权限被 Play 限制
- **App Manager（批量导 APK / 卸载）** — 需要 `REQUEST_DELETE_PACKAGES`

开发者的说法是："PlainApp 团队通常等一个版本高度稳定了才提交到 Play Store"。

### 桌面端是可选的

注意区分两个仓库：手机端 `plainhub/plain-app`（v3.x 系列，安卓手机核心）和桌面端 `plainhub/plain-desktop`（v0.1.x，Windows / macOS / Linux 的轻量桌面壳）。

plain-desktop 本质上是**把浏览器面板"包"成一个独立窗口**——不是"不用手机也能跑"的桌面版，底层还是和手机端通信，手机必须在同一 Wi-Fi 上开着。它的优势是独立窗口不会被误关、原生系统通知比 Web Notification 更稳定、可以常驻托盘。

就算完全不装桌面端，只用浏览器访问，PlainApp 也能正常工作。

---

## 📖 使用入门

PlainApp 的功能点很多，这里挑几个最有代表性的深入讲。

### 浏览器端文件管理：比手机屏幕好用

左侧侧栏点 **Files** 就是完整的浏览器端文件管理器。界面是**双栏布局**——顶栏面包屑导航 + 搜索框，中间是文件表格（名称 / 大小 / 修改时间三列可排序，也可切图标视图），选中文件后顶部出现 Download / Copy / Cut / Delete 等批量按钮。从电脑拖文件进窗口任意位置，表格区自动变成高亮 Drop Zone，松手即上传，中途断了再拖同一个文件会自动续传。

和手机端文件管理器看的是**同一份文件**（手机存储上的同一个目录），浏览器做的任何修改（重命名、删除、移动）在手机端同步可见。浏览器端独有的优势：批量拖拽上传、多文件即时打包下载 ZIP、排序 + 搜索 + 分页——手机屏幕小，表格类操作还是浏览器高效得多。

还有一个巧妙的设计：**ZIP 虚拟目录**。手机端点 `.zip` 文件，PlainApp 会把它当成可浏览的虚拟文件夹打开，不解压到存储里，图片视频直接给缩略图。

### RSS 阅读器：三栏面板 + 同步全文 + 隐私优先

PlainApp 自带的 RSS 阅读器完全贴合它的哲学——**你决定订什么、什么时候看、看多少**，没有算法替你选、没有广告插在信息流里。

侧栏点 **RSS / Feeds** 进入三栏面板：左栏订阅源列表（All / Today 过滤 + 已添加订阅 + 标签 Chip），中栏条目列表（当前选中订阅源的所有文章，按时间倒序），右栏阅读器正文。默认选 **All** 汇总全部订阅的最新条目，点 **Today** 只显示过去 24 小时内发布的文章。

有几个值得单独说的特性：

- **同步全文（Fetch Content）**：很多 RSS 订阅源只给摘要（"本文共 2000 字，这是前 300 字"），阅读器工具栏有个同步全文按钮——点一下，手机会去订阅源网站抓完整文章正文存进本地数据库，之后阅读器就显示完整内容了
- **存到笔记**：点一下自动把当前文章创建成 Markdown 笔记，正文就是文章内容，自动加 `<blockquote>` 块引用标明来源，文章已有的标签会自动复制给新笔记
- **OPML 导入 / 导出**：通用格式——从 Feedly、Reeder、NetNewsWire 等任何 RSS 阅读器导出的 OPML 都能直接导进来，嵌套分类也会保留
- **网络请求全部走手机**：这是 PlainApp RSS 最关键的隐私特性——订阅源的 Feed 拉取在**手机端**完成，浏览器端只是读手机上已经缓存好的内容，出版商看到的请求全部来自手机 IP，你的浏览器 IP 从未出现

> [!NOTE]
> 这点和 Feedly 这类云端 RSS 阅读器完全不同（所有流量过 Feedly 服务器）。如果你在意浏览痕迹，PlainApp 的 RSS 是更好的选择。

### 桌面端 CLI：给开发者和 AI Agent 用

`plainapp-cli` 不是给普通用户用的——它让开发者、自动化脚本、**AI Agent** 通过命令行直接查询和控制 PlainApp 管理的手机数据：

```
# 查看联系人列表
plainapp-cli contacts list

# 发一条 SMS
plainapp-cli sms send --to +86138xxxxxxx --text "你好"

# 列出已装 App
plainapp-cli apps list

# 读取一个笔记内容
plainapp-cli notes read <note-id>
```

CLI 通过 HTTP 直接调用 PlainApp 本地服务器的 API（和浏览器面板走的是同一个 HTTP Routes，浏览器渲染成 UI、CLI 直接返回 JSON）。所有操作都走 PlainApp 的本地 HTTPS 端点，需要在手机端设置里生成 API Token 配到 CLI，且 CLI 和手机必须在同一网络（或 Tailscale / WireGuard 虚拟局域网）。每一个写操作（发 SMS、删文件、卸 App）都经过手机端权限检查。

PlainApp 自带完整 API 文档，浏览器面板访问 `https://<手机IP>:8443/docs` 就能看到所有 HTTP Routes——包括 Auth / App / Chat / Contacts / SMS / Calls / Files / Notes / Bookmarks / Feeds / Images / Videos / Audio / Packages / Notifications / Tags / Pomodoro / Screen Mirror 等完整端点清单，任何人都可以基于它写自己的工具。

---

## 🔗 相关资源

- [PlainApp官网](https://plainapp.fedq.quest)
- [PlainApp文档](https://plainapp.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/PlainApp)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/PlainApp)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/PlainApp)

## 📚 相关文章

- [PlainApp 到底值不值得掏钱？——一篇"付费决策账本"](https://laoguosofts.github.io/posts/plainapp.html)

- [PlainApp付费账本：免费已够用，剩下那点值不值掏钱？](https://alipymanbu.github.io/posts/plainapp.html)
