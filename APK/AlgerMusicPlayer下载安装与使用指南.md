## ✨ AlgerMusicPlayer 是什么

AlgerMusicPlayer 是一款基于 Electron 构建的跨平台桌面音乐播放器，核心定位是网易云音乐的第三方客户端。它把网易云的在线曲库、歌单、每日推荐、私人 FM 等能力搬到本地，同时补上了官方客户端没有的本地音乐管理、无损播放、音源解析等实用功能。从最早的网易云 API 壳到现在的多平台桌面客户端，已经迭代了五年，覆盖 Windows、macOS、Linux 三大桌面平台。

如果你有大量本地无损音乐（FLAC / APE / DSD）需要统一管理，或者想在桌面端获得比网易云官方客户端更灵活的音质选择和下载体验，AlgerMusicPlayer 值得一试。它的使用门槛不高——首次安装登录网易云账号就能同步歌单开始听，进阶的格式支持、音源切换、音质参数都藏在设置面板里按需开启。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://algermusicplayer.fedq.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Windows 用户可通过下载页获取 `.exe` 安装包；macOS 用户需根据芯片类型选择 Apple Silicon 或 Intel 版本的 `.dmg`；Linux 用户提供 AppImage / deb / rpm 多种打包格式

---

## 🔧 安装步骤

### macOS 用户：先确认芯片类型

macOS 用户安装前必须先确认自己的 Mac 是 Apple Silicon（M1/M2/M3/M4）还是 Intel 芯片——下载页提供两个独立的 `.dmg` 版本，选错了也能跑（Rosetta 2 自动转译），但性能会打折扣。查看方式：点击左上角苹果图标 → 关于本机 → 看「芯片」或「处理器」一栏，显示 Apple M 开头就是 Silicon 版，显示 Intel 就是 Intel 版。

安装流程很标准：双击 `.dmg` 打开 → 把 AlgerMusicPlayer 图标拖到 Applications 文件夹 → 从 Launchpad 或应用程序文件夹启动。

> [!WARNING] 首次启动的 Gatekeeper 拦截
> macOS 的安全机制 Gatekeeper 可能弹出「应用已损坏」或「无法打开」的提示。解决办法：**右键点击应用图标** → 选择「打开」→ 在确认窗口再点一次「打开」即可。之后就可以正常双击启动了。

### Windows 用户

下载页获取 `.exe` 安装包后双击运行，按向导选择安装路径（默认 `C:\Program Files\AlgerMusicPlayer`），完成后桌面会生成快捷方式。如果 Windows Defender 误报，可以在 Defender 防护历史里将安装目录标记为允许。

### Linux 用户

推荐 AppImage 格式——下载后右键设为可执行权限，双击即可运行，无需安装。deb / rpm 包适合需要集成到系统包管理器的用户。

### 升级

v5.1.0 起内置 electron-updater，应用内会自动检测更新并提示下载覆盖安装，**不会丢失本地数据**（歌单、历史、设置都在用户数据目录里独立存储）。也可以手动去 GitHub Releases 页面下载最新版覆盖。

---

## 📖 使用入门

### 第一次启动：登录网易云账号

打开应用后第一件事是登录网易云账号——支持手机号、邮箱、第三方授权登录。登录成功后 AlgerMusicPlayer 会自动拉取你的歌单、收藏、关注的歌手，同步完成就能直接开始听。

> [!TIP] 没有 VIP 也能听高音质
> AlgerMusicPlayer 内置了 UnblockNeteaseMusic 灰色解析和 GDMusic 音源切换。设置 → 音频 → 音质里选「自动」或固定「极高 / 无损」，非 VIP 用户会自动尝试灰色音源获取高音质，拿不到时降级到网易云官方给的最高可用等级。VIP 用户则直接走官方 API 返回高音质 URL。

### 支持的音频格式：无损与有损全覆盖

AlgerMusicPlayer 基于 Electron 内置的 Chromium 播放引擎，额外集成了 DSD 和 APE 解码器，覆盖了几乎所有主流音频格式：

| 类型 | 格式 | 扩展名 | 说明 |
| --- | --- | --- | --- |
| 有损 | MP3 | .mp3 | 最常见，128-320kbps |
| 有损 | AAC | .aac / .m4a | 苹果生态常用，同码率优于 MP3 |
| 有损 | OGG | .ogg | 开源格式，流媒体常用 |
| 有损 | OPUS | .opus | 新一代高压缩比格式 |
| 无损 | FLAC | .flac | 开源无损，发烧友主流选择 |
| 无损 | APE | .ape | Monkey's Audio，压缩率高于 FLAC |
| 无损 | WAV | .wav | PCM 原始波形，体积最大 |
| 高清 | DSD | .dff / .dsf | Direct Stream Digital，发烧友顶级格式 |

下载时 AlgerMusicPlayer **不做格式转换**——音源返回什么格式就保存什么，保持原始质量不损失。无损格式体积约有损格式的 5-10 倍，下载前注意磁盘空间。

### 音质选择与带宽权衡

设置 → 音频 → 音质面板可以选「自动」或固定等级。网易云的音质分级从低到高是：标准（128kbps）→ 较高（192kbps）→ 极高（320kbps）→ 无损（FLAC ~1000kbps）→ 母带（~2000kbps，需要黑胶 VIP）。灰色音源不会精确标注等级，AlgerMusicPlayer 会根据比特率和采样率粗略判断显示一个大致档位。

| 音质 | 带宽占用 | 存储空间 | 听力收益 |
| --- | --- | --- | --- |
| 128kbps mp3 | ~1MB/min | 小 | 一般 |
| 320kbps mp3 | ~2.5MB/min | 中 | 好 |
| FLAC 无损 | ~10MB/min | 大 | 发烧友可闻 |

家庭 100M+ 宽带建议直接无损，手机流量环境固定 192 或 320 更划算。

### 播放历史：自动追踪与跨端同步

每播放一首歌，AlgerMusicPlayer 都会自动记录歌曲 ID、标题、歌手、专辑、播放开始时间、实际播放时长、是否完整听完、来源（歌单 / 搜索 / 每日推荐 / 私人 FM / 本地）、平台标记等字段。暂停、恢复、跳下一首会实时更新时长和完整标记。

v5.1.0 重构后历史记录做了三项优化：精简序列化字段减少存储体积、添加防抖写入避免播放时频繁写磁盘、自动清理过期旧记录（保留最近约 1000 条）。数据存储在 Electron 用户数据目录的 SQLite 数据库里，也会同步到网易云账号——但网易云官方 App 上的历史不会全部拉过来，同步数量上限约 1000 条。

播放历史和推荐算法形成正反馈循环：真实播放记录 → 同步到网易云 → 推荐算法学到喜好 → 每日推荐 / 私人 FM 更精准 → 推荐到更对胃口的歌 → 历史更丰富。如果在意隐私，可以定期在设置 → 历史 → 清空里清理，或关闭网易云账号同步。

### 本地音乐库管理

扫描本地音乐文件夹时，AlgerMusicPlayer 根据扩展名自动分类，无损格式会在详情页标注「无损」标识方便识别。在线播放时音源返回什么格式就播放什么，本地文件则上述全部格式都能正常播放。

### 音源解析机制

没有 VIP 时网易云官方 API 最多给 128/192kbps。AlgerMusicPlayer 的降级策略是：先尝试 UnblockNeteaseMusic 灰色解析获取高音质 → 如果灰色音源也没高音质，降级到网易云官方给的最高可用等级 → 界面显示当前实际使用的音质等级。GDMusic 是另一个可选音源项目，在手动音源选择界面可以切换，它接入的音源有时比 UnblockNeteaseMusic 覆盖面更广，特别是高音质版本。

---

## 🔗 相关资源

- [AlgerMusicPlayer官网](https://algermusicplayer.fedq.quest)
- [AlgerMusicPlayer文档](https://algermusicplayer-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/AlgerMusicPlayer)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/AlgerMusicPlayer)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/AlgerMusicPlayer)

---

## 📚 相关文章

- [AlgerMusicPlayer 五年迭代：从网易云 API 壳到桌面歌词标杆](https://laoguosofts.github.io/posts/algermusicplayer-docs.html)
- [AlgerMusicPlayer 五年迭代：从一个播放器到会自己管安全的桌面客户端](https://alipymanbu.github.io/posts/algermusicplayer-docs.html)
