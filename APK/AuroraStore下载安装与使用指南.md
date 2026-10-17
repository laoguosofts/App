## ✨ Aurora Store 是什么——开源的 Google Play 独立客户端

Aurora Store 是一款开源的 Google Play Store 客户端，它的核心定位可以一句话概括：不创建自己的应用仓库，而是直接对接 Google Play 官方服务器 API，把 Play Store 上能找到的任何应用——搜索、浏览、截图、评论、APK 下载——全部以独立界面呈现给用户。

理解 Aurora Store 的关键是理解它**不做什么**：它不托管应用，所有 APK 文件、应用描述、截图都实时从 Google Play 服务器拉取，Aurora Store 本身不存储、不分发任何应用；它不拥有应用，应用所有权始终归原开发者和 Google Play 所有；它没有 Google 授权，通过反向工程 Play Store 客户端 API 来访问 Google 的服务器。这种架构决定了它的优势和局限——Google Play 有什么它就有什么，但 Play Store API 一旦更新，Aurora Store 可能暂时无法工作，需要开发者跟进适配。

在 Google Play、F-Droid 和 Aurora Store 三者之间，定位差异清晰：Google Play 是官方渠道，必须登录且依赖 GApps；F-Droid 是社区自建的 FOSS 应用仓库，只收录从源码编译的自由软件；Aurora Store 则介于两者之间——它和 Google Play Store 是**同一个后端的两个前端**，就像 Chrome 和 Firefox 都能访问同一个网站，但 Aurora Store 提供了匿名模式、设备伪装、地区伪装等 Play Store 原生没有的能力，同时不受 F-Droid 只收 FOSS 的限制。三者可以共存，很多无 GApps 环境下的用户同时装了 Aurora Store 和 F-Droid。

Aurora Store 的主要特性包括：GPLv3 开源许可、支持匿名登录（使用志愿者提供的伪账号，无需自己的 Google 账号）或个人 Google 账号登录（可访问付费应用/收藏夹/愿望单）、设备伪装和地区伪装（解锁 geo-locked 应用）、Exodus 集成（一键扫描应用内 tracker）、Plexus 集成（检查应用在无 Google Play Services 环境下的兼容性）、更新黑名单、以及独有的手动下载旧版本能力。v4.8 起用 Jetpack Compose 完整重写，采用 Material 3 主题，支持动态取色。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://aurorastore.bmww.store/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜索 Aurora Store；无 GApps 环境可从下方下载页直接获取 APK

---

## 🔧 安装步骤

Aurora Store 是 Android 应用，安装方式与其他 APK 基本一致，但在无 GApps 环境或首次安装时需要注意几个关键点。

**第一步：开启未知来源安装权限。** 进入系统设置 → 安全 → 安装未知应用，找到当前用来打开 APK 的应用（文件管理器或浏览器），允许其安装应用。不同品牌系统路径略有差异，MIUI 在「隐私保护 → 特殊权限 → 安装未知应用」，OneUI 在「生物识别和安全性 → 安装未知应用」。

> [!TIP]
> 如果你用的是 LineageOS、GrapheneOS 等无 GApps 环境，直接安装 Aurora Store 就行，它不依赖 Google Play Services 本体，只需要能访问 Google Play 服务器的网络连接（可能需要代理）。

**第二步：下载并安装。** 从上面的下载入口获取 APK 文件，双击或在文件管理器中打开，系统会弹出安装确认界面，点击安装即可。安装完成后不要急着打开，先确认版本号是否与下载页标注的一致——避免下载过程中拿到损坏或旧版本。

**第三步：首次启动初始化。** 打开应用后，Aurora Store 会让你选择登录方式：匿名模式（使用公共伪账号，推荐大多数用户选这个）或 Google 账号登录（需要自己的账号，可访问付费应用和收藏夹）。选择匿名模式后，应用会自动分配一个伪账号，几秒钟即可进入主界面。

> [!WARNING]
> 不要从第三方镜像站下载 Aurora Store APK——它没有 Play Protect 签名校验，被篡改后你无从识别。只从官方下载渠道获取。

---

## 📖 使用入门

### 界面结构与基础操作

Aurora Store 的主界面分为底部导航的三个 tab：**Apps**、**Games**、**Updates**。Apps tab 是通用应用商店，顶栏横向切换「For you / Top charts / Categories / Editor's choice」；Games tab 专门展示游戏内容，分类更细（Action、Adventure、Arcade、Board、Card、Casual、Puzzle、Racing、Role playing、Simulation、Sports、Strategy 等十几个子类），游戏详情页额外显示 Google Play Games 成就系统、联机/离线标识、控制器支持说明等信息；Updates tab 列出已安装应用的可用更新。

搜索功能在顶栏，支持应用名、包名、开发者名称三种搜索方式。每个应用详情页提供完整的截图、描述、评论、下载量、体积、版本号等信息，Exodus 集成会自动扫描应用内包含多少个 tracker，Plexus 集成则提示该应用在无 GApps 环境下能否正常运行。

### 匿名模式与账号选择

Aurora Store 默认的匿名模式使用志愿者提供的伪 Google 账号池，你不需要也不应该在匿名模式下输入自己的 Google 密码。伪账号的权限是受限的——能免费下载、浏览评论和截图，但无法访问付费应用、愿望单、已购记录等需要个人账号的功能。

如果你需要访问付费应用或同步愿望单，可以切换到个人 Google 账号登录模式。登录流程与 Play Store 类似，但同样会绕过 GApps 直接与 Google Play 服务器通信。注意：登录个人账号后，该账号的活动会通过 Aurora Store 而非 Play Store 客户端记录在 Google 服务器上，Google 端可能会检测到异常的客户端签名。

### 手动下载旧版本——独有的降级能力

这是 Aurora Store 最实用的特性之一，也是官方 Play Store 客户端不允许的操作。某些场景下新版本反而不好用：新版本引入了强制登录或广告、对旧设备不兼容（需要更高 Android API）、移除了你常用的功能、或者有严重 bug 还没修——这些情况下，手动下载旧版本就能派上用场。

操作方法有两种。从应用详情页进入：打开应用详情页 → 点右上角的三个点菜单 → 选择 Downloads 或 Versions → Aurora Store 会列出 Google Play 服务器上这个应用的所有可用版本号 → 选一个旧版本 → 下载 → 安装。或者从侧栏的 Downloads 进入，找到已下载过的应用 → 更多选项 → Download older version。

> [!NOTE]
> 旧版本是否可用取决于 Google Play 服务器是否保留了历史版本。大型热门应用（WhatsApp、Spotify）通常保留多个版本，小型冷门应用可能只有最新版，某些开发者主动下架了旧版本的也无法下载。降级安装还需要注意：旧版本依赖的 API 可能在你的 Android 版本上已废弃，装完直接 Force Close；旧版本的数据库 schema 可能和新版本不同，降级后应用提示数据损坏需要清除数据；如果开发者更换了签名密钥，Aurora Store 会拒绝降级安装；银行、DRM 应用可能在启动时检查版本是不是最新版，不是就不让用。

降级后如果发现旧版本也不行，回退方法很简单：卸载当前旧版本 → 重启 Aurora Store → 重新搜索该应用 → 点 INSTALL 装回最新版。

### 设备与地区伪装

Aurora Store 允许你切换设备型号和 API 等级，以及地区和语言。这两项功能的核心价值是解锁 geo-locked 应用——某些应用只在特定地区的 Play Store 上架，用地区伪装就能让 Aurora Store 以目标地区的身份去查询 Google Play 服务器，拿到该地区独有的应用列表。设备伪装则可以模拟不同的 Android 版本和硬件配置，让一些对设备有限制的应用（如只能在特定手机型号上运行的游戏）对你可见。

设备伪装和地区伪装在侧栏的 Settings → Device / Location 里配置。切换后需要重启 Aurora Store 才能生效。注意：伪装的只是查询身份，实际下载下来的 APK 仍然是同一开发者签名的正式版本，Aurora Store 不会也不能修改 APK 内容。

### 更新管理与黑名单

Updates tab 列出所有检测到可用更新的应用。如果你不想让某个应用出现在更新列表里（比如你已经装了一个修改版、或者知道新版本有问题），可以在该应用的详情页或更新列表中找到「忽略此应用的更新」选项，加入黑名单后 Aurora Store 就不会再提示这个应用的更新通知。

游戏下载前需要特别注意体积——很多手机游戏在 500MB~2GB 之间，下载前确认设备存储空间充足、使用稳定的 WiFi 连接（大文件下载中途断了要重来），如果用移动数据注意流量消耗。

---

## 🔗 相关资源

- [AuroraStore官网](https://aurorastore.bmww.store)
- [AuroraStore文档](https://aurorastore-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/APP/AuroraStore)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/APP/AuroraStore)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/APP/AuroraStore)

---

## 📚 相关文章

- [Aurora Store 用了一年，这些坑我替你踩过了](https://laoguosofts.github.io/posts/aurorastore-docs.html)
- [AuroraStore 我踩过的那些坑——从降级崩溃到账号混乱](https://alipymanbu.github.io/posts/aurorastore-docs.html)
