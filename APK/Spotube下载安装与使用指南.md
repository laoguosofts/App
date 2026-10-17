## ✨ Spotube 是什么——插件驱动的跨平台音乐播放器

Spotube 是一款跨平台开源音乐播放器，核心设计理念是把"搜索什么歌"和"用什么音频源"彻底解耦。它自身只提供宿主能力——UI 界面、播放内核、数据存储和插件加载器，所有具体的音乐服务连接都通过插件完成。这意味着你可以用 Spotify 的元数据和歌单，但音频流走 YouTube 或 Piped，也可以搭配 MusicBrainz 的元数据和 Qobuz 的无损音频源。

Spotube v5.0.0 是一次架构级重构，把之前硬编码的 Spotify 元数据和 YouTube 音频流替换成了完整的插件体系。v5.1.0 进一步开放了音频源插件接口，社区可以开发自定义的音频源，比如 Dab Radio、自定义 HTTP 源等。插件不是传统的 Dart 扩展，而是 Kotlin/JS 库，在 Cash App 的 Zipline runtime 中以沙箱方式执行，每个插件有独立的运行环境，无法直接访问系统文件或网络。

支持的平台覆盖全面：Windows、macOS、Linux（含 Flatpak）、Android 和 iOS。Spotube 不做任何遥测上传——没有崩溃报告、没有使用统计、没有分析追踪，本地数据默认全部留在设备上。只有当你主动开启 scrobble（Last.fm 或 ListenBrainz）时，播放记录才会上报到对应服务。

> [!NOTE] 适用人群
> 如果你想摆脱单一音乐平台的绑定、希望自由组合元数据来源和音频源、或者需要跨平台统一的音乐播放体验，Spotube 值得一试。它尤其适合已经在用 Spotify 管理歌单但不想长期订阅 Premium 的用户。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://spotube.hhdc.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 可从 Google Play 搜索 Spotube；iOS 从 App Store 搜索；桌面端从官网分发渠道获取安装包

## 🔧 安装步骤

### Windows 安装

1. 下载 `.exe` 安装包后双击运行，如果弹出 Windows Defender SmartScreen 警告，点"更多信息 → 仍要运行"即可（开源项目首次发布的签名可能未被系统完全信任）。
2. 选择安装路径，默认安装到 `C:\Users\<你的用户名>\AppData\Local\Programs\Spotube\`，可以改到非系统盘。
3. 安装完成后首次启动，Spotube 会在 `%APPDATA%\Spotube\` 目录下创建数据文件夹，用于存放数据库、插件和缓存。默认下载目录为 `%USERPROFILE%\Music\Spotube Downloads\`。

> [!WARNING] 杀毒软件误报
> 部分国产杀毒软件可能对未签名的 exe 产生误报。如果遇到拦截，建议将 Spotube 安装目录加入白名单，不要随意关闭杀毒软件。

### macOS 安装

1. 下载 `.dmg` 文件双击打开，把 Spotube 拖入 Applications 文件夹。
2. 首次启动如果被 Gatekeeper 拦截，右键点应用 → 选择"打开"即可绕过。
3. 数据目录在 `~/Library/Application Support/Spotube/`，默认下载目录为 `~/Music/Spotube Downloads/`。

### Linux 安装

Spotube 官方提供 Flatpak 包，推荐通过 Flatpak 安装以获得沙箱隔离体验：

```
flatpak install flathub com.github.KRTirtho.Spotube
flatpak run com.github.KRTirtho.Spotube
```

Flatpak 沙箱默认只能访问 `~/Music`、`~/Downloads`、`~/Documents`。如果想让 Spotube 扫描其他目录，需要用 `flatpak override` 放开权限。数据目录在 `~/.var/app/com.github.KRTirtho.Spotube/data/spotube/`。

### Android 安装

从 Google Play 搜索安装，或下载 `.apk` 手动安装。手动安装需要在系统设置中开启"允许安装未知来源应用"。Android 11+ 的 Scoped Storage 限制下，Spotube 默认下载目录在系统音乐目录 `~/Music/Spotube Downloads/`。数据目录位于 `/data/data/oss.krtirtho.spotube/`。

### iOS 安装

直接从 App Store 搜索下载。iOS 沙箱隔离，Spotube 只能访问自己的容器目录，下载的曲目可以在"文件"App 中找到并导出。

> [!TIP] 版本核对
> 安装完成后建议在设置页检查版本号是否为当前最新版，旧版本可能缺少新的插件接口和音频源支持。v5.0 之前的版本架构完全不同，如果遇到插件无法加载的情况，大概率是版本过低。

---

## 📖 使用入门

### 插件管理——Spotube 的核心

Spotube 的一切能力都来自插件。启动后进入插件页面，你会看到四类插件接口：

| 接口 | 职责 | 典型实现 |
|---|---|---|
| MetadataSearchAPI | 搜索曲目、专辑、歌手、歌单，返回元数据 | Spotify / MusicBrainz / JioSaavn |
| AudioAPI | 根据元数据提供音频流 | YouTube / Piped / Invidious / Qobuz |
| LyricsAPI | 根据曲目信息获取时间同步歌词 | LRCLib |
| ScrobbleAPI | 把播放记录上报到外部服务 | Last.fm / ListenBrainz |

Spotube 内置了从 GitHub 和 Codeberg 拉取的 Available plugins 列表。安装方式有三种：内置搜索框直接下载 `.smplug` 直链、右上角上传按钮从本地安装、或者手动拷贝到插件目录。

> [!WARNING] 插件安全
> 插件拥有你的全部音乐数据访问权限，恶意插件可能窃取登录凭证。优先使用标记 Official 的插件，不要安装来源不明的包。

Spotube 的插件架构之所以重要，是因为它彻底打破了"元数据 = 音频源"的传统绑定。你可以用 Spotify 的歌单和推荐做发现，用 YouTube 或 Piped 做播放；也可以用 Qobuz 的无损音频配上 MusicBrainz 的精确元数据。这正是它能绕过 Spotify Premium 的根本原因——Spotube 根本不播放 Spotify 音频，它只借 Spotify 的元数据。

### 连接 Spotify——导入歌单与收藏

Spotube 不是 Spotify 的播放器，不能播放 Premium 曲目，但它可以用 Spotify 的元数据 API——不登录也能搜索，登录后能导入个人播放列表和收藏。这是 Spotube 最受欢迎的使用方式。

#### OAuth 授权流程

1. 插件页面点 "Connect Spotify →"
2. Spotube 弹出系统浏览器，跳转到 Spotify 登录页
3. 输入 Spotify 账号密码并授权
4. Spotify 回调带临时 auth code 到 Spotube 的 redirect URL
5. 插件拿到 code，调用 Spotify API 换 access token + refresh token
6. Token 用 flutter_secure_storage 加密存储在本地

Token 有效期约 1 小时，Spotube 会用 refresh token 自动续期，用户不需要重新登录。refresh token 本身也会过期（通常半年到一年），过期后 Spotube 会自动弹出重新登录提示。

#### 自动导入的内容

Spotube 登录成功后会自动导入以下内容：

| 导入项目 | 数量上限 |
|---|---|
| 个人播放列表 | 全部（自动翻页处理 50 条/请求的 API 限制） |
| 关注的播放列表 | 全部 |
| 收藏曲目（Liked Songs） | 全部 |
| 收藏专辑 | 全部 |
| 关注的艺人 | 全部 |

所有数据会在本地创建副本，存入 Drift（reactive SQLite）数据库。

#### 同步策略

Spotube 不做实时同步（Spotify 没有 webhook 推送播放列表变化），同步策略是**手动触发**加**定期提醒**：Spotify 连接页有 "Sync now" 按钮，每次全量对比更新；每隔 7 天 Spotube 会提示是否同步一次（可关闭）。

> [!NOTE] 没有双向同步
> 你在 Spotube 里新建或修改播放列表，不会推送到 Spotify。这是 Spotify Web API 的限制，Spotube 无法替你写回 Spotify。

#### 不登录也能用

Spotify Metadata 插件支持匿名使用：不登录时可以搜索曲目、专辑、歌手、分类歌单，也能浏览 Spotify 公开的热门榜和编辑精选，但无法访问个人数据（Liked、个人播放列表、Discover Weekly）。

### 同步歌词——LRCLib 插件

Spotube 的歌词获取完全插件化，官方提供的 LRCLib 插件是默认选择。LRCLib 是一个公开的同步歌词数据库，CC0 协议，覆盖约 200 万首曲目，每条记录包含曲目名、艺术家、专辑、时长和 LRC 格式歌词，由社区贡献维护。

#### 匹配流程

Spotube 的歌词匹配有三级优先级：

1. **musicBrainzId 匹配**（最精确）：如果曲目来自 MusicBrainz 插件，用 MBID 精确匹配
2. **artist + title + duration 完全一致**
3. **artist + title 模糊匹配**（可能选错同名曲目）

拿到 LRC 格式字符串后，Spotube 解析成时间戳列表，UI 层从 libmpv 的当前播放进度拿时间戳进行比对，高亮当前行并滚动到屏幕中央。这个对齐完全在本地完成，不依赖网络——即使离线下载了曲目和歌词，滚动也能正常工作。

#### 跨插件匹配失败

一个常见场景：用 Spotify Metadata 搜到的曲目（带 Spotify track ID）在 LRCLib 里可能没有精确记录，匹配会退到模糊匹配，可能返回同名但不同版本的歌词。解决办法有三种：

1. 换用 MusicBrainz 元数据插件搜到的曲目（MBID 匹配更准）
2. 在设置里开启"手动选择歌词"，自动匹配不唯一时弹出候选列表让你挑
3. 自己放 `.lrc` 文件在音频文件**同名同目录**（比如 `Song.mp3` → `Song.lrc`），Spotube 会自动加载

#### 歌词显示设置

Spotube 设置里可以配置：自动滚动（当前行保持在屏幕中央）、歌词区独立字号、翻译行同时显示、点击歌词跳时间戳。

### 数据存储与备份

Spotube 的数据分两类：**纯本地**和**可能上传到外部服务（主动开启才会）**。

纯本地数据包括：播放列表/收藏（SQLite 数据库）、插件（`.smplug` 文件）、音频缓存（临时缓冲，自动清理）、下载的离线曲目、主题/EQ/偏好设置（shared_preferences）、歌词缓存。

Spotube 自身不做任何遥测上传——没有崩溃报告、没有使用统计。只有当你主动启用 scrobble（ListenBrainz / Last.fm）时，播放记录才会上报；插件正常工作时会连接对应的音乐服务（Spotify / YouTube / MusicBrainz 等）。

#### 各平台数据目录

| 平台 | 数据目录 | 默认下载目录 |
|---|---|---|
| Windows | `%APPDATA%\Spotube\` | `%USERPROFILE%\Music\Spotube Downloads\` |
| macOS | `~/Library/Application Support/Spotube/` | `~/Music/Spotube Downloads/` |
| Linux | `~/.local/share/spotube/` | `~/Music/Spotube Downloads/` |
| Flatpak | `~/.var/app/com.github.KRTirtho.Spotube/data/spotube/` | 同上 |
| Android | `/data/data/oss.krtirtho.spotube/` | `~/Music/Spotube Downloads/` |
| iOS | `/var/mobile/Containers/Data/Application/<UUID>/Documents/` | 容器内 downloads/ |

Spotube 用 flutter_secure_storage 包存储 OAuth token：iOS 用 Keychain（系统加密）、Android 用 KeyStore（硬件安全）、Windows 用 DPAPI、macOS 用 Keychain、Linux 用 libsecret。token 在磁盘上加密存储，明文不会出现在数据库里，进程退出后即从内存清除。

#### 清理 vs 清空

Spotube 设置 → Data Management 有两个按钮，效果完全不同：

| 操作 | 效果 | 能否恢复 |
|---|---|---|
| 清理缓存 | 删除临时文件（音频缓冲、歌词缓存、封面图缓存） | 自动重建，无害 |
| 清空数据 | 删除一切——播放列表、收藏、插件、下载曲目、所有偏好 | 不可恢复（除非有备份） |

建议日常只清缓存，清空数据是出厂重置级别的操作。

#### 备份播放列表

Spotube 没有内置备份/恢复功能，手动备份需要：

1. 关掉 Spotube（防止数据库写入中拷贝）
2. 复制 `spotube.db` 文件到备份位置
3. 可选：同时复制 `preferences` 文件保留主题设置
4. 如果有大量下载曲目，同时备份下载目录

恢复时：新设备装 Spotube → 关掉 → 把备份文件拷回原位置 → 启动。

---

## 🔗 相关资源

- [Spotube官网](https://spotube.hhdc.tech)
- [Spotube文档](https://spotube-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/Spotube)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/Spotube)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/Spotube)

## 📚 相关文章

- [用 Spotube 搭一条「听歌→记谱→出周报」自动化链](https://laoguosofts.github.io/posts/spotube-docs.html)
- [把Spotube嵌进我的独立音乐周报工作流：从搜歌到网盘分享一条链](https://alipymanbu.github.io/posts/spotube-docs.html)