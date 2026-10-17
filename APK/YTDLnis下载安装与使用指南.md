## ✨ YTDLnis 是什么

YTDLnis 是一款运行在 Android 上的视频下载工具，底层基于 yt-dlp 引擎，可以从 YouTube、Bilibili 等主流视频平台抓取视频、音频和字幕。和网页端在线下载工具不同，YTDLnis 把下载逻辑完全本地化——不需要浏览器、不依赖第三方服务器转发，意味着下载速度和可用性取决于你的设备和网络，而不是某个随时可能失效的中转站。

核心能力围绕「下载配置自由度」展开：视频画质和格式任选、字幕支持嵌入和烧录硬字幕、SponsorBlock 社区标记自动跳过赞助和片头片尾广告、多链接排队并发下载、后台持续跑不受前台控制影响。对于经常需要离线观看视频的用户来说，YTDLnis 解决了两个最痛的点——平台下载入口要么根本没有、要么会员专属，而第三方在线链接又经常被反爬和限流。

> [!NOTE]
> YTDLnis 本身不托管任何视频资源，它只负责把你在浏览器里能访问到的内容保存到本地。使用前请确认你对目标内容有合法下载权限。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://ytdlnis.wxhp.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：在 Google Play 或 F-Droid 搜索 YTDLnis 查找对应版本；国内安卓生态用户以官方 APK 渠道为主

---

## 🔧 安装步骤

YTDLnis 目前主打 Android 平台，以下以 APK 安装为主线。

### Android APK 安装流程

1. **下载 APK 包**：从下载入口获取最新版本的 APK 文件，文件大小通常在 30MB~50MB 区间。
2. **允许未知来源安装**：首次安装第三方 APK 需要在系统设置中开启「允许安装未知来源应用」权限。不同品牌 ROM 路径略有差异——小米/MIUI 在安全中心 → 应用安全 → 允许安装未知应用；OPPO/ColorOS 在设置 → 安全 → 安装未知应用；原生 Android 在设置 → 应用 → 特殊访问权限 → 安装未知应用。
3. **执行安装**：点击下载好的 APK 文件，系统会弹出安装确认界面，点击「安装」等待完成。如果弹出「签名冲突」提示，说明设备上已有其他来源的 YTDLnis 版本，先卸载旧版再安装。
4. **首次启动授权**：打开应用后会依次请求存储权限（用于保存下载文件）和通知权限（用于下载进度和完成提醒），全部允许即可。

### 安装后核对清单

> [!TIP]
> 安装完成后建议花 30 秒做以下核对，避免后续下载时才发现版本或组件有问题。

- 应用能正常启动、主界面加载无报错
- 进入「更多 → 设置 → Updating → Packages」检查 Python 和 FFmpeg 组件是否已就绪（首次安装通常需要点击 Install 手动补齐）
- 随便贴一个视频链接测试下载能否正常发起

---

## 📖 功能与使用入门

YTDLnis 的操作围绕「链接输入 → 配置参数 → 下载执行」展开。界面分为视频、音频、播放列表几个 Tab，每个 Tab 下面有 Adjust 卡片用来微调本次下载的具体行为。下面重点介绍几个高频使用场景和容易踩坑的细节。

### SponsorBlock 自动去广告

SponsorBlock 是一个社区众包项目——用户在网站上标记 YouTube 视频里哪些时间段是赞助广告、片头片尾、自推广、订阅提醒等非内容片段，其他人下载同一视频时就能自动跳过。YTDLnis 直接接入了这个 API，下载时用社区标记精准裁掉这些片段。

在视频或音频 Tab → Adjust → SponsorBlock 里，每个跳过类型可以单独开关：

| 类型 | 适用场景 |
|---|---|
| Sponsor | 视频里的付费赞助广告段 |
| Non Music Portions | 音乐视频里的口播介绍、片头片尾（下载音乐时建议开） |
| Intro / Outro | 视频固定模板的开头和结尾 |
| Self-Promotion | 创作者推广自己的商品或频道会员 |
| Subscription Reminders | 提醒观众订阅的段落 |

勾选后还有两种处理方式：**Chapters only** 只在下载文件的章节元数据里写入标记（播放器里能看到但不裁剪），**Remove segments** 直接切掉对应的时间段让文件变短。前者适合想保留完整视频但不被广告打断的人，后者追求文件体积最小化。也可以在全局设置里预配置默认值，省掉每次下载都手动调的麻烦。

### 字幕的四种处理方式

视频 Tab → Adjust Video → Subtitles 里有四个可自由组合的选项，对应不同的字幕处理策略：

**Embed Subtitles** 把字幕流作为软字幕轨道嵌入视频文件，播放器里可以开关切换，画面本身不变。但注意它默认**不嵌入自动生成的机器翻译字幕**——那些质量参差不齐，默认被排除在外。如果确实需要，同时开 Save Automatic Subtitles 就能让它们也嵌进去。

**Save Subtitles** 把字幕下载成独立的 VTT 或 SRT 文件，和视频放同一个目录。适合后期剪辑或需要字幕文件单独使用的场景。

**Save Automatic Subtitles** 下载平台提供的机器翻译字幕，质量取决于平台翻译能力和视频语音清晰度，作为多语言参考够用但别指望和人工字幕一样准。

**Burn Subtitles** 把字幕画面直接画进视频画面里变成硬字幕。优点是任何播放器都能显示、不需要字幕支持；缺点是**必须重新编码整个视频**——耗时长、字幕永久烙印在画面里无法去掉。适合发给没有字幕开关的设备或分享给其他人。

| 观看场景 | 推荐配置 |
|---|---|
| 手机本地看，随时关字幕 | Embed Subtitles（只用人工字幕） |
| 收集字幕做翻译学习 | Save Subtitles + Save Automatic Subtitles |
| 发给智能电视没字幕开关 | Burn Subtitles |
| 想嵌入也想留文件 | Embed + Save 同时开 |

字幕语言也可以在全局设置里精准控制——Preferred languages 设成简体中文、英文这样的优先级，Excluded languages 排除不想要的，Subtitle format 选 VTT 还是 SRT，改一次对后续所有下载生效。

### 后台下载与通知稳定化

Android 厂商定制 ROM 经常激进杀后台，YTDLnis 的下载队列和定时观察会被中途砍掉。如果你遇到下载锁屏回来就停、完成通知收不到这些问题，按下面三步走。

**第一步（必做）：关掉电池优化**。在 YTDLnis 里进入「更多 → 设置 → 常规 → Ignore battery optimization」打开开关，系统会跳转设置页面让你把应用加入电池优化白名单。没做这一步，Android 在空闲时大概率会把它杀掉。

**第二步（强烈建议）：开启精确闹钟**。进入「更多 → 设置 → Downloads → Use AlarmManager instead of WorkManager for scheduling」打开，这会申请 Android 12+ 的 SCHEDULE_EXACT_ALARM 权限。AlarmManager 是系统级闹钟，比 WorkManager 更可靠——定时观察源检查和计划下载到点时能准时拉起应用。

**第三步：厂商 ROM 专属设置**。不同品牌的 Android 有额外的后台限制开关需要手动放开：

| ROM | 需要额外做的事 |
|---|---|
| MIUI / HyperOS | 安全中心 → 电池和性能 → 应用智能省电 → YTDLnis → 无限制 |
| OneUI | 设置 → 应用 → 电池优化 → YTDLnis → 不允许 |
| ColorOS | 设置 → 电池 → 应用耗电管理 → YTDLnis → 允许完全后台行为 |
| EMUI / HarmonyOS | 设置 → 电池 → 启动管理 → YTDLnis → 手动管理 → 三个开关全开 |
| Pixel / 原生 | 一般前两步就够了 |

> [!WARNING]
> 电池优化白名单只是让 YTDLnis 后台时能继续跑，但**不会明显增加功耗**——下载完自然退出、定时检查间隔默认几小时一次几乎不耗电。耗电集中在并发下载期间，那时候你也需要它跑完。

### Packages 扩展组件管理

YTDLnis 的下载引擎依赖一些外围组件——Python 解释器（运行 yt-dlp 脚本）、FFmpeg（视频重编码、字幕烧录）、aria2c（多线程下载）等。这些组件统一从官方 Packages 仓库发布，在「更多 → 设置 → Updating → Packages」里可以查看、安装和更新。

首次安装 YTDLnis 后，Packages 页面大概率会显示 Python 和 FFmpeg 未就绪——手动点 Install 让 YTDLnis 根据你的设备架构自动拉取。ARMv8（arm64-v8a）是目前绝大多数手机用的架构，YTDLnis 会自动过滤只显示你设备能用的版本。如果某个包显示「不支持的架构」，等下一个版本就好，不是设备问题。

升级 Packages 时注意：如果有正在跑的处理任务（比如视频重编码、字幕烧录），升级可能会导致任务中途失败。建议升级前先把 Running 区域的条目等完成或暂停。也可以在「更多 → 设置 → Backup」里把全部设置（包括 Packages 清单）导出成 JSON 备份文件，换设备恢复时 YTDLnis 会自动帮你把组件也装上。

---

## 🔗 相关资源

- [YTDLnis官网](https://ytdlnis.wxhp.skin)
- [YTDLnis文档](https://ytdlnis.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/YTDLnis)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/YTDLnis)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/YTDLnis)

---

## 📚 相关文章

- [YTDLnis 新手入门：手机上就能搞定的视频下载全攻略](https://laoguosofts.github.io/posts/ytdlnis.html)
- [从零开始用 YTDLnis：五分钟装好，立刻能下](https://alipymanbu.github.io/posts/ytdlnis.html)
