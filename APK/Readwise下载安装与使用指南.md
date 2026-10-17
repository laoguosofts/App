## ✨ Readwise 是什么

Readwise 是一个把你的阅读高亮从各个渠道（Kindle、Apple Books、浏览器、PDF、EPUB）聚合到一起，用间隔重复算法帮你记住、并内置稍后读阅读器的跨平台工具。简单说，它解决三个问题：**高亮收集**、**间隔重复复习**、**网页/文档稍后读**。

核心功能分两块：经典 Readwise 负责把 Kindle、Apple Books 等渠道的高亮同步过来，每天定时推送一轮「今日复习」——基于你的选择（记得/模糊/忘了）动态调整间隔，越难记的出现越频繁；Readwise Reader 是独立的稍后读阅读器，支持保存网页、上传 PDF/EPUB、RSS 订阅、TTS 朗读，内置 Ghostreader AI 可以对整篇文档或选中段落做问答和总结。

Readwise 覆盖 Web、iOS、Android、Mac、Windows 五个平台，以及 Chrome、Firefox、Safari 三系浏览器扩展。新功能上线顺序通常是 Web → 桌面 → iOS → Android，Android 某些功能会晚几个月。适合需要管理大量阅读内容、想把零散高亮变成长期记忆的重度读者和知识工作者。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://readwise.wrwq.fun/download.html) —— 下载页整理了当前可用的版本和安装说明
> - App Store（iOS）/ Google Play（Android）搜索「Readwise」；Mac App Store 搜索「Readwise Reader」；Windows 版从官网下载安装包

---

## 🔧 各平台安装要点

Readwise 跨平台安装，不同平台有各自的前置条件和注意点。

### iOS / iPadOS

- 系统要求 **iOS 15.5 或更高**，iPhone / iPad / iPod touch 均可
- 直接在 App Store 搜「Readwise」下载安装
- iOS 端支持 Apple Books 的快捷分享导入高亮，不需要额外工具

### Android

- 在 Google Play 搜「Readwise」下载
- Android 上部分功能（如 Ghostreader 全局模式、Offline v2）比 iOS 上线晚是正常现象，耐心等待即可
- 安装完成后首次登录需要邮箱或第三方账号（Google / Apple）

### Mac

- 支持 Apple Silicon 和 Intel 芯片，系统要求 **macOS 12 Monterey 或更高**
- 两种安装方式：Mac App Store 搜「Readwise Reader」，或从官网下载 dmg 手动安装
- Mac 独有：Apple Books 专用导入工具，可一键把 Apple Books 的全部高亮同步到 Readwise

### Windows

- 系统要求 **Windows 10 或更高**
- 从官网下载 exe 安装包，双击运行即可
- Windows 版功能与 Mac 基本一致，但 Apple Books 导入工具不可用（这是平台限制）

### 浏览器扩展（Chrome / Firefox / Safari）

- 各浏览器应用商店搜「Readwise」一键安装
- 安装后扩展图标会出现在浏览器工具栏
- 右键扩展图标可配置默认保存行为

### E-Ink 电纸书设备

- BOOX 等 Android 电纸书可以直接装 Google Play 版 Reader App
- Kindle 设备可以用 Send-to-Kindle 把 Reader 里的内容推过去

> [!TIP]
> **Linux 用户**：Readwise 没有原生 Linux 桌面 App，用 Web 端（read.readwise.io）即可，功能基本完整；搭配浏览器扩展体验接近原生。

### 安装后检查清单

1. 登录后进入 Daily Review，能看到示例卡片说明账号状态正常
2. 打开 Sources 页面，确认同步渠道（Kindle、Apple Books 等）连接状态为「已同步」
3. 安装浏览器扩展后，在任意网页选中文本，应该能看到悬浮高亮菜单

---

## 📖 Readwise 使用入门

Readwise 的功能模块可以分为三块：**经典 Readwise（高亮同步 + 间隔重复复习）**、**Reader 阅读器（稍后读 + 保存）**、**频率调优（控制复习节奏）**。掌握这三块基本就能覆盖日常使用场景。

### 界面布局

不管是 Web 还是移动端，Readwise 的界面都围绕「高亮列表 + 复习入口 + 阅读器」组织。Web 端左侧边栏五个常驻区域：

- **Daily Review** —— 今天该复习的高亮和 Mastery 卡片，显示剩余数量和预计耗时
- **Library** —— 所有已导入高亮的完整列表，支持按来源、标签、日期筛选
- **Sources** —— 管理同步渠道，查看上次同步时间，手动触发同步
- **Export** —— 配置 Notion / Obsidian / Roam / Evernote 等导出目标
- **Reader** —— 跳转 Readwise 阅读器

移动端底部三个 Tab：**Home（复习入口 + 今日提醒）**、**Highlights（全部高亮按日期分组）**、**Reader（保存的文章/PDF/EPUB + RSS）**。整体视觉走「极简 + 温暖」路线，主色调是柔和橙红色，衬线字体，卡片式列表。

### 高亮同步

这是 Readwise 的起点——把 Kindle、Apple Books 等设备上的高亮同步过来。

- **Kindle**：依赖浏览器扩展，在 Sources 页面连接 Kindle 账号后，扩展会周期性拉取你的 Kindle 标注
- **Apple Books（Mac 独有）**：安装 Mac 版后有专用工具一键导入
- **浏览器扩展**：安装后在任意网页选中文本，悬浮菜单点 Readwise 高亮按钮，不保存整篇也能单独高亮
- **同步状态**：Sources 页面能看到每个来源的上次同步时间，点「Sync Now」可手动触发

### Daily Review 复习流程

每天打开 Readwise 的第一件事就是 Daily Review：

1. 点击进入复习界面，逐条过卡片
2. 每张卡片从「记得」「模糊」「忘了」三个选项中选一个
3. Readwise 用间隔重复算法根据你的选择动态调整下次出现的间隔
4. Mastery 卡片会让你再回忆一遍该条高亮的上下文，巩固记忆

> [!NOTE]
> 间隔重复算法的核心逻辑是：选「忘了」→ 间隔大幅缩短，选「记得」→ 间隔逐渐拉长。所以诚实选择很重要，不要一律点「记得」骗自己。

### Reader 阅读器：三种保存方式

Readwise Reader 是独立的稍后读产品，三种保存方式各有适用场景：

**方式一：浏览器扩展（最推荐）**

- 点击扩展图标 → 当前页面保存到 Reader
- 右键菜单 → Save to Readwise（支持先选中文本再只保存选中部分）
- 扩展图标右下角有红色徽章，数字表示 Reader 里未读文章数

**方式二：Reader 内输入 URL**

1. 打开 read.readwise.io 或 Reader App
2. 点左侧边栏 `+` 号 → Add URL
3. 粘贴链接回车即可

**方式三：通过 Pocket / Instapaper 间接导入**

如果已经在用这两个稍后读工具，Reader 可以直接拉取你里面保存的文章。

Reader 保存网页后会自动做：正文提取（去广告、导航栏、侧边栏）、格式统一（清理花哨样式）、图片保留并优化加载、阅读时间估算、网页快照存档（原网页挂了也能读）。

阅读时可以调整字体大小、行距、主题色（日间/夜间/护眼），桌面端支持完整键盘导航。

### 频率调优：控制复习节奏

Readwise 默认算法对所有高亮一视同仁，但不同内容价值密度不一样。频率调优让你手动干预某本书/文章的复习频率。

**方式一：从 Daily Review 中直接调**

1. 某个文档出现在复习中时，点右上角下拉箭头
2. 选 Adjust frequency
3. 五个选项：

| 选项 | 效果 |
|---|---|
| Increase frequency | 更频繁出现在复习中 |
| Decrease frequency | 更少出现 |
| Once a month | 约每月一次 |
| Once every 3 months | 约每季度一次 |
| Archive | 归档，不再参与自动复习 |

**方式二：账号设置里批量调**

Account Preferences → Review → Frequency Tuning，可以按来源类型（比如所有 Podcast 类文档只每月复习一次）或来源平台批量调整。

**实战建议**：新导入的书籍先让默认算法跑一段时间再调，不要一上来全部手动干预；正在做项目相关的内容提高频率；已经复习过几轮的经典可以降频或归档；**用 Archive 代替删除**——今天觉得没用的高亮半年后做新项目时可能突然想起，归档更安全。

### Ghostreader AI

Readwise 内置 AI 助手，有两个层级：

- **文档级**：在 Reader 里打开任何保存的网页、PDF、EPUB，可以让 Ghostreader 做总结、问答、提取要点
- **全局模式**（Web / 桌面端已上线，移动端即将推出）：不打开具体文档也能全局搜索和问答你的高亮库

### 平台功能差异总览

不同平台的功能支持有细微差别：

| 功能 | Web | Mac | Windows | iOS | Android |
|---|---|---|---|---|---|
| 经典 Readwise | ✅ | ✅ | ✅ | ✅ | ✅ |
| Reader 阅读器 | ✅ | ✅ | ✅ | ✅ | ✅ |
| Ghostreader 文档级 | ✅ | ✅ | ✅ | ✅ | ✅ |
| Ghostreader 全局 | ✅ | ✅ | ✅ | 即将 | 即将 |
| Apple Books 导入 | ❌ | ✅（专用工具） | ❌ | ✅（分享） | ❌ |
| Kindle 自动同步 | ✅（依赖扩展） | ✅（依赖扩展） | ✅（依赖扩展） | 手动 | 手动 |
| TTS 语音朗读 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 离线阅读 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 键盘快捷键 | ⭐ 最全 | ✅ | ✅ | 有限 | 有限 |

---

## 🔗 相关资源

- [Readwise官网](https://readwise.wrwq.fun)
- [Readwise文档](https://readwise-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/Readwise)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/Readwise)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/Readwise)

## 📚 相关文章

- [Readwise 深度用了一年：它凭什么在高亮管理这个冷门赛道里扎稳了根](https://laoguosofts.github.io/posts/readwise-docs.html)

- [试过十几款阅读复习工具后，我为什么把 Readwise 留在了主力位](https://alipymanbu.github.io/posts/readwise-docs.html)
