## ✨ Grayjay 是什么

Grayjay 是一款开源的 Android 端视频与音频聚合播放器。它的核心思路很简单：**用一套界面，统一消费多个平台的内容**——YouTube、Bilibili、Twitch、Odysee、SoundCloud、Apple Podcasts 等等，装几个插件就能在同一套信息流里刷遍不同来源的频道、播放列表和直播。

它走的是「第三方客户端 + 插件化」的路线，不托管任何内容，所有数据来自你自己安装的插件和登录的账户。这种架构的好处是灵活——你需要哪个平台就装哪个插件，不需要的不拖后台；代价是不同插件的成熟度不一样，有的还在 Alpha 阶段，接口偶尔会变。

Grayjay 适合两类人：一类是已经在多个视频平台有关注列表，想把它们收拢到一个 App 里统一管理的；另一类是喜欢折腾、愿意花点时间调插件和设置，换一个更清爽的观看体验的。如果你只刷一个平台、习惯原生 App 的交互，Grayjay 可能显得重了。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://grayjay.gltd.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Grayjay 不在 Google Play 和 App Store 上架，官方只分发 APK，安装后需要在 Android 系统设置里允许「未知来源」应用安装

---

## 🔧 安装步骤

Grayjay 目前只有 Android 版本，iOS 用户暂时没有官方客户端。安装路径和大多数第三方 APK 一样：

1. 从下载页拿到最新 APK 文件，推荐按下载页标注的版本号核对一下，避免拿到旧包
2. 手机打开文件管理器找到 APK，点击安装
3. 如果弹「未知来源安装被阻止」——进入系统设置 → 安全 → 允许 Grayjay 的来源（Chrome / 文件管理器）安装应用，然后回到 APK 重新点安装
4. 装完打开 App，建议先别急着装插件，先跑一遍基础播放确认播放器本身正常

> [!TIP]
> **版本核对小技巧**：Grayjay 迭代不算慢，安装前对比一下下载页标注的版本号和 APK 文件名里的版本号是否一致。如果 APK 文件名比下载页列的旧，说明下载页还没更新，等会儿再下。

> [!WARNING]
> 某些厂商 ROM（尤其是小米、华为）默认会在安装完成后自动弹出「检测到未知来源应用」的提示，这是正常的系统安全警告——只要 APK 来源是下载页，点「仍然安装」即可，不需要卸载重下。

---

## 📖 使用入门

### 插件是一切的起点

Grayjay 本身只是一个播放器壳子，真正的内容能力全靠插件。第一次启动后，先去插件库挑你需要的平台装上。插件分四个成熟度等级：正式版、Beta、Alpha，还有更早的开发版。

| 等级 | 说明 | 典型场景 |
|---|---|---|
| 正式版 | 接口稳定、功能完整 | YouTube、Odysee、Patreon、Bilibili |
| Beta | 核心功能可用，偶有小问题 | Twitch、Kick、Dailymotion |
| Alpha | 功能不完整、接口变化频繁 | NASA+、TED Talks、FOSDEM |

**主流视频平台（正式版）** 包括 YouTube、Odysee、Rumble、Patreon、Nebula、SoundCloud、PeerTube、Bilibili，日常用优先挑这批。**直播平台（Beta）** 有 Twitch、Kick、Dailymotion、BitChute，如果你看直播多可以装上，稳定性比正式版弱一点。**播客与广播** 类插件其实覆盖面更广——Apple Podcasts、Radio Browser（聚合了全球 5 万多个广播台）、Mixcloud（DJ 混音和电台节目）这三个装上，音频消费场景基本就全了。**视频与纪录片频道（Alpha）** 里有 TED Talks、Curiosity Stream、FOSDEM（开源软件会议）、NASA+、Internet Archive（历史视频档案馆）这些偏小众的源，喜欢尝鲜再碰，稳定性要求高的可以等它们升到 Beta 再用。

### 导入订阅：把你的关注列表一键搬过来

插件装好并登录账户后，最实用的功能就是**订阅导入**。以 YouTube 为例：在 Sources 页面点 YouTube 插件卡片进入管理界面，找到「Import Subscriptions」按钮，点一下 Grayjay 会从平台拉取你账户的完整关注列表，逐条加入统一信息流。

导入是**增量合并**的——不会删掉你在 Grayjay 里手动添加的订阅，导入完成后你可以在频道管理界面逐个检查，把不想在 Grayjay 继续刷的取消关注就行。这个功能不是所有平台都支持，接口开放度决定了插件能做多少。目前确认支持导入的有 YouTube 和 Twitch，其他平台点进去没有「Import Subscriptions」按钮就说明不支持，别硬找了。

### 播放列表导入：同步你在平台上整理好的合集

除了频道订阅，Grayjay 也能导入你在原平台的播放列表。操作流程和订阅导入几乎一样：Sources 页面点插件卡片 → 找到「Import Playlists」按钮 → 选择要导入的列表。导入后的播放列表出现在 Grayjay 自带的「Playlists」标签页，你可以浏览、搜索，对里面的视频执行下载或加入自建列表等操作。

Grayjay 里有两种播放列表，别混了：

- **平台导入的**：同步自原平台，原平台上列表内容变动时，Grayjay 可以拉取最新状态（前提是插件实现了这个功能）
- **Grayjay 自建的**：完全在本地创建管理，用来整理你想批量下载或串看的内容，不会同步到任何外部服务

播放列表导入的整体支持度比订阅导入低。YouTube 插件通常做得最好，其他平台可能只有部分列表能导入，或者导入后不会自动更新。导入前看看插件的文档说明。

### 最容易踩的坑：锁屏后播放被掐断

这是 Android 用户报告最多的问题：切后台或锁屏后 10~30 秒内播放自动停了，重新打开 App 进度还在但视频已经不响了。**不是 Grayjay 的 bug，是厂商系统在杀后台进程**。现代 Android 厂商系统默认都会限制长时间后台运行的应用，Grayjay 这种后台持续播放音频的正好撞枪口上。

不管什么厂商系统，按这个顺序排查基本都能解决：

1. **系统设置 → 电池 → 应用权限**：把 Grayjay 设为「无限制」，允许后台活动
2. **系统设置 → 应用 → Grayjay → 权限**：确保所有权限授予，特别是「后台运行」「不受限制」这类
3. **最近任务界面 → 把 Grayjay 下拉锁定**（不同厂商叫「加锁」「固定」「不清理」）

各厂商有一些特殊路径：小米 MIUI/HyperOS 还要去「应用设置 → 应用管理 → Grayjay → 省电策略」改成无限制，同时允许「后台弹出界面」；华为 EMUI/鸿蒙要去「应用和服务 → Grayjay → 电池 → 允许后台活动」再加上「启动管理 → 手动管理」；OPPO/OnePlus 在「电池 → 耗电保护」里设无限制，还要在应用管理里允许自启动和后台弹出。原生 Android 最简单：设置 → 应用 → 所有应用 → Grayjay → 电池 → 无限制。

> [!NOTE]
> Grayjay 应用内也有后台播放相关的设置项，打开设置搜「后台播放」「锁屏播放」「后台音频」，确保对应开关已经打开。系统层面放行 + App 内开关打开，双重保障。

### 一个容易忽略的定位

Grayjay 虽然主打「视频播放器」，但装上播客和广播插件之后，它完全可以当一个统一的音频内容客户端用。SoundCloud + Apple Podcasts + Radio Browser 三件套组合起来，播客、DJ 混音、全球广播台基本全覆盖，不需要再单独装一个播客 App。这一点很多用户上手之后才发现，算个隐藏的加分项。

---

## 🔗 相关资源

- [Grayjay官网](https://grayjay.gltd.quest)
- [Grayjay文档](https://grayjay.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%AD%A3%E7%89%88/Grayjay)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%AD%A3%E7%89%88/Grayjay)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%AD%A3%E7%89%88/Grayjay)

---

## 📚 相关文章

- [用 Grayjay 看视频的四个暗坑，踩过才知道](https://laoguosofts.github.io/posts/grayjay.html)
- [Grayjay 入坑两月踩过的那些坑——从黑屏播放到数据全丢](https://alipymanbu.github.io/posts/grayjay.html)
