## ✨ Symfonium 是什么

Symfonium 是一款面向 Android 平台的 Hi-Fi 级音乐播放器，核心定位是把本地音乐、Subsonic 家族流媒体服务器（Navidrome、OpenSubsonic 等）以及各种在线音源统一收纳到一个界面里，再通过一套自研的 64 位 DSP 通路把音质拉到发烧级。它不只是一个"能放歌"的播放器——PEQ 参数均衡器最高 64 段、GEQ 图形均衡器不依赖系统库、支持 USB 独占和原生 DSD 输出，这些配置加在一起，基本覆盖了从普通通勤用户到桌面发烧友的全部调音场景。

适合谁用？如果你手上有 Navidrome 自建音乐服务器，或者想在 Android 手机上用 AutoEQ 曲线调耳机，再或者只是嫌系统播放器的 EQ 太弱想找点专业调音工具，Symfonium 都值得一试。2025 年底到 2026 年初发布的 15.0 版本是它的里程碑更新——整个音频引擎被重写，本地播放走自己的 DSP 通路，不再依赖 Android 系统音频框架，这一代用户体验和音质提升非常明显，强烈建议停留在 14.x 或更早版本的用户升级。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://symfonium.laze.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜索 Symfonium；F-Droid 仓库同样有分发（F-Droid 通常比 Google Play 慢 1~2 周推送）

---

## 🔧 安装步骤

Symfonium 目前是 Android 平台独占应用，没有 iOS 或 PC 版本。安装流程分两种情况：从应用商店一键安装，或者手动下载 APK 侧载。

**应用商店安装（推荐）**

1. 打开 Google Play 或 F-Droid App
2. 搜索 "Symfonium"，认准开发者条目后点安装
3. 等待下载完成自动安装，全程无需额外操作

**手动 APK 侧载**

如果需要尝鲜测试版或 Google Play 无法访问，可以从官方渠道下载 APK 安装包侧载：

1. 在手机浏览器打开下载页，下载最新版 APK 文件
2. 打开文件管理器找到下载的 APK，点击运行
3. Android 系统会弹窗提示"未知来源安装被阻止"，点设置 → 允许本次安装
4. 安装完成后打开 Symfonium，首次启动会申请存储权限（用于读取本地音乐文件）

> [!WARNING]
> 侧载时务必核对 APK 文件的版本号和文件大小——第三方下载站可能提供被篡改的安装包。推荐从官方支持平台或 Google Play 下载。

**版本核对**

安装完成后，打开 Symfonium → 设置 → About，确认版本号是否为当前最新版。15.0 以下版本缺少完整的 64 位 DSP 通路，EQ 效果和输出选项都受限。

---

## 📖 使用入门

### 媒体源接入

Symfonium 的核心能力之一是同时管理多种媒体源——本地存储、Subsonic 家族协议服务器、以及一些在线服务。其中 Subsonic 协议家族支持标准 Subsonic、OpenSubsonic 和 Navidrome 三种模式。

连接步骤统一在 **设置 → Manage media providers → Add media provider** 里操作：选择对应的协议类型，输入服务器地址（比如 `https://navidrome.example.com`）、用户名和密码，测试连接通过即可。Symfonium 会先尝试用 OpenSubsonic 扩展握手，如果服务器不支持就自动退回到标准 Subsonic 模式，这个过程完全透明，不用手动切换。

Navidrome 是目前 Subsonic 生态里最活跃的实现，内存占用通常 100MB 以内，支持自动索引增量更新和 Web 界面管理。Symfonium + Navidrome 这组搭配是不少资深用户的常用组合。歌单方面，Navidrome 支持双向写回（在 Symfonium 里改歌单可以同步到服务器），原版 Subsonic 通常只读不写。

> [!TIP]
> 如果老版本 Subsonic 服务器出现歌单写不回、封面图取不上来这类问题，先升级服务器再折腾客户端——绝大多数兼容性问题在升级到最新 Navidrome 或 Subsonic 后自动消失。

### GEQ 图形均衡器

打开 GEQ 的路径有两条：播放界面 → 三点菜单 → Equalizer → 切到 GEQ 标签页；或者设置 → Playback → Output settings → 选一个输出设备 → Equalizer。段数可选 5、10、15、31 四档，新手建议从 10 段开始——覆盖常见调音场景（加强低音、削弱齿音、提亮高频）又不会看花眼，31 段适合对每个八度都想单独调的玩家。

Symfonium 的 GEQ 有一个关键优势：**不走 Android 系统 EQ 库**，是自己在 64 位 DSP 通路里实现的。这意味着段数、频带位置、滤波形状全部不被系统锁死，不同 ROM 之间表现一致，不会出现某些厂商定制 ROM 把 EQ 参数吃掉的情况。

GEQ 和 PEQ 不是互斥的——可以在 GEQ 里保存一套日常调音预设，同时在 PEQ 里导入一套 AutoEQ 耳机曲线，随时切换。两者在 DSP 通路里的叠加顺序是：虚拟器 → GEQ/PEQ（二选一）→ 压缩器/限制器 → 响度提升/低频提升 → 交叉馈送 → ReplayGain，每个效果独立开关。

### PEQ 参数均衡器

PEQ 默认 10 段，打开 **专家模式** 后可以加到 64 段，频点最低 5Hz，每段可以指定中心频率、Q 值（带宽）和增益，还支持左右声道独立控制。专家模式在设置 → Playback → Advanced settings 里开启，打开后重复添加的段会自动去重，压缩器和限制器也解锁更多参数。

GEQ 的滑块式调音量直观但精度有限——你想只降 3kHz 附近那一个齿音尖峰，可能一不小心把 2kHz 和 4kHz 也一起拉下去了。PEQ 的中心频率 + Q 值让你可以**只瞄准那个尖峰**，其他频率不受影响。这也是为什么 AutoEQ 生成的耳机曲线都是 PEQ 参数。

声道独立控制适合单声道不平衡校正——比如某副耳机左单元输出偏强，给左声道加一个负增益段就能补偿。精准输入的方式是直接点滑块左边的数值，弹出输入框后频率、增益、Q 值都可以精确到小数。

### 什么时候用 GEQ 什么时候用 PEQ

| 场景 | 推荐工具 |
|---|---|
| 快速加强低音、削弱齿音、提亮高频 | GEQ（滑块直动，立即可听） |
| 精准瞄准某一个频率尖峰（比如 3.2kHz 那个齿音） | PEQ |
| 导入社区 AutoEQ / APO 耳机曲线 | PEQ |
| 多设备多耳机各自一套快速预设 | GEQ（保存多个命名配置） |
| 左右声道不平衡校正 | PEQ（声道独立） |

### 输出配置按设备保存

Symfonium 会为每个输出设备独立保存一套 DSP 配置——手机扬声器、有线耳机、每副蓝牙耳机、USB DAC 各一套，换设备不用每次重调 EQ。15.0 版开始新增 Android DAP（HiBy / FiiO / iBasso / Shanling）原生适配，对桌面 DAC 用户也支持 USB 独占和比特完美输出（Android 14+）。

### 版本更新与社区

Symfonium 的更新节奏大致是：大版本（14.x → 15.0）半年到一年一次，引入重大改动；小版本每月 1~2 次，修 Bug 和加小功能；补丁版紧急修复。Google Play 和 F-Droid 分发节奏略有差异，F-Droid 通常比 Google Play 慢 1~2 周。

关注更新的方式：Google Play 里打开 My apps & games 手动检查，或 F-Droid 自动提示；官方支持平台有每次更新的详细 changelog，比应用商店的简短描述详细得多。官方 Wiki 按 Settings、Providers、Playback & offline、Interface & library、Playlists & smart features、Support & reference 六大板块组织，覆盖了几乎所有功能的详细说明。

反馈 Bug 时建议附上清晰的复现步骤、Symfonium 版本号、Android 版本和设备型号、Bug 出现频率，如果能重现就用 **设置 → Support & reference → Secure file sharing with support** 生成诊断日志——这个日志只包含版本号、媒体源类型列表、最近错误栈，**不包含密码、文件内容、播放历史**等敏感信息，可以放心提交。

---

## 🔗 相关资源

- [Symfonium官网](https://symfonium.laze.skin)
- [Symfonium文档](https://symfonium.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Symfonium)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Symfonium)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Symfonium)

## 📚 相关文章

- [Symfonium 15.0 为什么是一次"推倒重来"](https://laoguosofts.github.io/posts/symfonium.html)
