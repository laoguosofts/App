## ✨ Flipboard 是什么

Flipboard 是一款主打「个性化杂志流」的新闻聚合与内容策展应用。它把你关注的媒体、话题、社交账号和联邦宇宙（fediverse）内容编排成一份份可翻阅的数字杂志——每一页都是算法 + 编辑双引擎挑选的精选文章，而不是冷冰冰的时间线。

从 2010 年诞生至今，Flipboard 已经从单纯的 RSS 阅读器演变成一个完整的信息策展平台：你可以用它刷每日新闻、按兴趣订阅 Smart Magazine、把喜欢的文章 Flip 进自己的私人杂志，甚至直接在 App 里浏览和发帖到 Mastodon 等联邦宇宙社区。

适合谁用？如果你每天需要扫一遍行业动态、又不想被微博式的短信息流牵着走，Flipboard 的杂志翻页体验和精细调校能力会很对胃口；如果你是内容创作者或策展爱好者，Flipboard 的 Group Magazine 协作和 flipboard.social 社区也值得一试。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://flipboard.hhdc.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play / App Store 搜「Flipboard」直接安装

---

## 🔧 安装步骤

Flipboard 目前的主力形态是移动 App，同时提供 Web 端（flipboard.com）。下面分平台说安装注意点。

### Android

1. 打开 Google Play，搜索 **Flipboard**，认准官方开发者名安装
2. 首次启动时按提示授予必要权限（网络访问、通知、位置可选）
3. 完成后会进入 Passion Picker 兴趣选择界面——这是 For You feed 的基础画像，尽量多选几个你真正在意的领域，后面可以随时改

> [!TIP]
> 如果你所在地区无法访问 Google Play，可以从下载页获取 APK 安装包，记得在系统设置里开启「允许安装未知来源应用」，并核对 APK 文件大小与官方说明是否一致。

### iOS

1. App Store 搜 **Flipboard** 直接安装
2. iOS 的通知权限会在首次启动时弹窗询问——建议先开「通知」权限、位置权限选「使用 App 期间」，后面在 App 内部再细调

### Web 端

1. 浏览器访问 flipboard.com，用邮箱或社交账号登录
2. Web 端功能比移动端轻量一些，但基本的阅读、Flip 进杂志、关注媒体都能用；fediverse 和 Smart Magazine 协作等高级功能主要在移动端

> [!WARNING]
> 安装完成后第一次打开，Flipboard 可能需要花 30 秒到 1 分钟同步你的初始兴趣源，期间 For You feed 可能显示空白或加载动画——这是正常现象，耐心等一下就好。

---

## 📖 个性化与深度玩法

安装只是开始，Flipboard 的价值在于持续调校出真正懂你的信息流。下面四个方向覆盖了从地区设置到 fediverse 社交的全部深度玩法。

### 切换地区版，让本地内容自动进来

Flipboard 提供 Global、UK、France、Japan、Germany、Brazil、Mexico、Canada 等多个地区版，不同地区版的 Explore 推荐、本地媒体源、Neighborhood feeds 都会跟着变。

**怎么切换**：移动端在 Profile → Settings → Region 入口，Web 端在头像 → Settings → Region，选完保存或刷新即可。切换不会清空你的关注列表和已 Flip 的杂志，只是在算法推荐里补充目标地区的本地内容。

**不要混淆地区版和 fediverse 可用性**——fediverse 功能目前仅美国和德国可用，这是 Flipboard 服务端按 IP 判断的，不是 App 里地区设置能改的。即使你把地区设成 Germany，如果网络 IP 不在美/德，fediverse 入口也不会出现。

**本地 Neighborhood feeds** 是地区版最实用的功能：开启位置服务或手动指定城市后，Explore Tab 会出现 Local / Neighborhood feeds，内容来自你所在城镇的本地媒体，覆盖新闻、天气、通勤、体育、餐饮推荐等日常信息。搬家到新城市或短期旅行时，这个功能能帮你快速了解当地动态。中国用户如果 GPS 不准，建议手动设香港或其他熟悉的城市。

### 在 flipboard.social 上参与联邦宇宙

flipboard.social 是 Flipboard 官方运营的 Mastodon 实例，任何人都可以在上面注册 Mastodon 账号，然后在整个 fediverse 里社交——关注 Mastodon、Bluesky、Threads 等兼容 ActivityPub 的平台用户，在 Flipboard App 里直接刷到他们的帖子，甚至自己发帖。

注册后你会得到一个 `@用户名@flipboard.social` 的 Mastodon 账号，它和 Flipboard 自己的账号是两个独立系统：

| 账号体系 | 用途 |
| --- | --- |
| Flipboard 账号 | 策展、关注媒体、管理私人杂志 |
| flipboard.social Mastodon 账号 | fediverse 社交、发帖、关注联邦宇宙用户 |

两者可以在 Flipboard App 设置里关联，关联后你的 flipboard.social 动态会在 App 内展示，其他 Flipboard 用户也能找到你的 fediverse 账号。

> [!NOTE]
> 如果你已经在 Mastodon 或 Bluesky 上活跃，没必要再注册一个 flipboard.social——直接在 Flipboard App 里用现有账号的用户名搜索关注就行。flipboard.social 更适合想在 fediverse 上拥有「专业策展人」身份或第一次接触联邦宇宙的新手。

### 通知调校：只接你真的想知道的

Flipboard 默认开了不少推送，但大部分可以关掉。通知分五大类：

| 类型 | 建议 | 理由 |
| --- | --- | --- |
| Daily Edition | **必开** | 美东早 7 点（北京时间晚 7-8 点）送达的每日精选，一条轻量推送，点进去直接看 |
| 协作 Magazine 更新 | 按需 | 如果参与了 Group Magazine 协作就开，否则关 |
| 社交互动 | **全关** | Flipboard 社区规模不大，一天 0-2 条，偶尔看 Notification Tab 就够 |
| Fediverse 动态 | 按需 | 关注了 fediverse 账号就开，否则关 |
| 系统更新 / 营销 | **全关** | 有新版本系统会自动更新，不需要推送 |

iOS 和 Android 的通知设置都有两套入口——App 内和系统级——两者是叠加生效的，任何一层关掉都不会推。Android 11+ 还支持给不同通知类型设不同优先级，建议只给 Daily Edition 设高优先级，其余放中低或完全关闭。

通知之外，Flipboard 还有桌面 Widget 和 Newsletter 邮件两种提醒渠道。三者组合的推荐模式是：**Widget 扫一眼今日、推送只接 Daily Edition、邮件放专题深度内容**，这样 Flipboard 就变成一个轻量的信息提醒系统，不会淹没在通知噪音里。

### For You feed 持续调校：三种手段由温和到强力

第一次 Passion Picker 选完标签后，For You feed 往往还会混入一些不对味的内容，这是正常的。Flipboard 给了三种持续调校手段：

1. **Tune 滑块**：For You feed 右上角的调音台图标，打开后能看到你当前关注的所有标签和 Smart Magazine，左右滑动临时调整权重。不会删除标签，适合某个 Smart Magazine 最近内容变得太激进或太水时临时降频。

2. **「Less like this」**：给算法最精确的负面信号。在任意文章上点右上角三个点，选「Less like this」然后指定原因——可以是对话题没兴趣、减少带这类标签的文章、减少这个媒体的文章。连续点 3-5 次针对同一个话题，算法基本就不会再推了。

3. **静音来源（Mute Source）**：彻底拉黑。被静音的媒体或账号不会以任何形式出现在 For You 里，即使你在 Passion Picker 里关注了相关标签。这是强操作，慎用。

对应的正面信号是 **点赞** 和 **Flip**——Flip 的信号更强，相当于告诉算法「这类内容请放在更靠前的位置」。遇到真喜欢的文章不妨 Flip 进自己的杂志，既做了策展又帮算法优化了推荐。

> [!TIP]
> 新用户前两周每天花 2-3 分钟点几个「Less like this」，For You 的精度会提升非常快。老用户每周扫一眼、遇到跑偏的随手调校就行。

---

## 🔗 相关资源

- [flipboard官网](https://flipboard.hhdc.tech)
- [flipboard文档](https://flipboard-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/Flipboard)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/Flipboard)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/Flipboard)

---

## 📚 相关文章

- [Flipboard 到底值不值得花时间？——一个信息瘾君子的付费账本](https://laoguosofts.github.io/posts/flipboard-docs.html)
- [用了三年Flipboard，这笔账我得好好算算](https://alipymanbu.github.io/posts/flipboard-docs.html)
