## ✨ Stationhead 是什么：一场让粉丝同步听同一首歌的直播实验

Stationhead 是一款主打「同步收听」的音乐直播平台——主播把自己的流媒体账号（Spotify / Apple Music）连进来开一个 Station，听众跟着一起听歌，所有人听到的是**同一时间点的同一首歌**。和传统的音乐 App 里各自戴耳机闷头听不同，Stationhead 把粉丝团的集体行为搬到了实时：聊天框、表情、点歌请求、甚至申请上麦跟主播一起说话。

它的核心玩法围绕 Release Party 展开——新专辑首发夜，粉丝团在 Channel 里集体蹲守，主播按时间表逐首播放，两万人同时听同一首歌、在聊天框里刷同一句感慨，这种「万人同步」的仪式感是 Stationhead 最独特的卖点。后来又拓展了 Shop 周边、Collections 数字藏品、音乐人官方 Station 等 B 端合作形态，逐渐从一个粉丝团工具长成了有商业闭环的平台。

支持的平台包括 iOS App、Web 端，以及 Android App。Android 用户目前只能作为听众参与，不能自己开播，但可以申请上麦当嘉宾跟主播对话。

> [!NOTE]
> Stationhead 的同步体验依赖你自己的 Spotify Premium 或 Apple Music 订阅——没连流媒体服务或 Premium 过期，开播和收听都会卡住。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://stationhead.fewf.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - App Store（iOS）：搜「Stationhead」直接下载
> - Google Play（Android）：搜「Stationhead」，注意 Android 端仅支持收听，不能开播
> - Web 端：直接访问 stationhead.com 注册登录即可使用，无需安装

---

## 🔧 安装与账号连接：三步把「同步收听」跑起来

Stationhead 的安装本身很轻量，但首次配置时的流媒体账号连接是很多新用户卡住的第一步。

### iOS / Android App

1. 从应用商店下载安装，打开后用邮箱或第三方账号（Google / Apple / Twitter）注册登录。
2. 进入设置 → Streaming Service，选择你用的流媒体平台，跳转到授权页面完成 OAuth。Spotify Premium 和 Apple Music 二选一即可，两边都连也可以。
3. 回到 Stationhead 主界面，连接状态显示「已连接」就可以开始收听或开播了。

> [!WARNING]
> 开播前务必断开所有蓝牙设备（AirPods、车载蓝牙等）——蓝牙干扰是 Stationhead 音频异常的头号元凶，很多用户连 AirPods 时音频会被切到蓝牙输出但耳机里也没声音，断开后立刻恢复。

### Web 端

Web 端的优势是**任何设备都能用**——电脑浏览器打开 stationhead.com，注册登录后同样去设置里连流媒体账号。主播如果不想局限在手机屏幕上操作，Web 端的嘉宾管理面板、歌单管理、频道数据看起来更方便。

### 版本核对

安装后进入 About 或版本信息页面，确认你用的是最新版。Stationhead 对同步机制有持续迭代，老版本可能遇到网络延迟敏感、无法匹配最新 Channel 等问题。如果应用商店提示有更新，优先更。

---

## 📖 使用入门：从听众到嘉宾，从单站到粉丝团

Stationhead 的使用场景分两个极端——**普通听众**（大多数人）和**主播**（核心玩法），中间还有「申请上麦当嘉宾」这个 Android 用户也能参与的桥梁角色。

### 听众：跟着主播一起听歌 + 聊天

进入某个正在直播的 Station 或 Channel，点击进入就开始同步播放。聊天框在界面下方，可以发文字、表情，也可以对当前歌曲发「🔥」「😢」等预设反应。如果主播开放了点歌请求，你在播放面板里能看到请求入口，填歌名和艺人名提交，主播看到后决定要不要插到队列里。

听众端的控制项很少——暂停、音量、退出 Station 就这些。真正的交互都在聊天和申请上麦里。

### 申请上麦：从听众变成临时嘉宾

这是 Stationhead 听众参与度最高的玩法——**你不需要自己开 Station，只要在某个主播的 Station 里请求上麦，主播同意后你就能和主播、其他嘉宾一起说话**。

操作步骤：

1. 在收听界面找到「Request to Speak」或麦克风图标按钮（不同版本位置略有差异，通常在聊天框或控制面板区域）。
2. 点击后请求会发送到主播那边，等待主播同意或拒绝。
3. 同意后你会收到通知，自动进入嘉宾状态。界面上会出现静音按钮，点击取消静音就能说话了。

> [!TIP]
> Android 用户虽然不能开播，但**完全可以作为嘉宾上麦**——这是 Stationhead 明确支持的。如果你有朋友在 iOS 或 Web 端开播，或者某个 Channel 正在直播，Android App 里可以正常发开麦请求、正常说话。这是 Android 用户目前能参与 Stationhead 互动的最高权限。

Stationhead 对每个 Station 限制最多 4 人同时在麦（主播 1 + 嘉宾 3）。满员时新的请求会被主播跳过，也可能主播踢一个人下去再拉新的。

### 嘉宾能做什么、不能做什么

嘉宾上麦后可以：

- 开麦说话（和主播、其他嘉宾的声音一起广播给所有听众）
- 听到主播和其他嘉宾的声音
- 随时自己下麦（点自己头像旁边的离开按钮）
- 继续参与实时聊天、发表情、点歌请求

嘉宾**不能**做的事：

- 切换 Talk + Music / Music only / Talk only 模式（只有主播能切）
- 结束直播（只有主播能结束）
- 踢掉其他嘉宾（只有主播能）
- 管理聊天（主播能关掉聊天，嘉宾不能）
- 添加或删除歌单里的歌曲（只有主播能管理）

嘉宾的声音是「广播式」的——一旦上麦就会和主播、其他嘉宾的声音一起通过 Stationhead 服务器广播给所有正在收听的人。建议上麦前想清楚要说什么，用有线耳机的麦或 AirPods 消除回声和背景噪音。

### 主播：开一个 Station，当个粉丝团的临时 DJ

主播需要 iOS App 或 Web 端才能开播（Android 端目前不支持开播）。开播前确认流媒体账号已连接、蓝牙已断开，然后点「Start a Station」——选择 Talk + Music / Music only / Talk only 模式，设置 Station 标题和封面，就开始了。

开播时的核心面板：

- **歌单管理**：从你的流媒体库选歌、拖拽排序、随时调整播放顺序。
- **嘉宾管理**：看到所有发过开麦请求的听众列表，决定接受或拒绝。
- **聊天面板**：实时监控聊天内容，必要时可以关掉聊天。
- **模式切换**：随时在 Talk + Music、Music only、Talk only 之间切换——嘉宾看不到这个开关。

### 直播自动录音

Stationhead 在你开播时会**自动录制整场直播**，结束后保存在你的 Station 里随时可以回放。录音内容包括播放过的所有音乐片段、你的人声、嘉宾的人声，但**实时聊天的内容不会被录音**。

回放路径：

- iOS App：进入 Station 主页 → 已结束的直播录音按时间顺序排列 → 点击某条回放，可以随时跳转到任意位置。
- Web 端：登录 stationhead.com → Profile → Past Shows → 点击某条回放。
- Android App：因为不能开播，所以没有自己的录音；但同一账号下在 iOS / Web 端开过播的录音，Android App 里可以回放。

删除录音的操作是 iOS App 里点录音 → 右上角垃圾桶图标 → 确认删除，删除后不可恢复。

> [!NOTE]
> Stationhead 的录音目前**不支持导出到本地**，只能在 Stationhead App 里回放。如果你想保存自己的直播内容，只能用系统级录屏（iOS 控制中心屏幕录制、Android 下拉通知栏录屏按钮）。听众在你下播后也无法再进入你的 Station——录音是主播的私人档案，不对外公开。

### 听不到音乐怎么办

Stationhead 的同步机制对网络和音频配置比较敏感，常见问题可以按下面的路径排查：

| 场景 | 排查方向 |
| --- | --- |
| 主播自己听不到音乐 | 确认流媒体连接、断开所有蓝牙设备、检查播放模式不是 Talk only、音乐音量没被拉到最小 |
| 听众听不到音乐 | 听众自己的流媒体连接状态、断开蓝牙、切换 WiFi / 蜂窝数据 |
| 音乐断断续续 / 不同步 | 网络延迟——测网速，主播关其他吃带宽的 App，听众切换网络试试 |
| 只有主播声音没有音乐 | 检查是不是 Talk only 模式，音乐音量有没有被拉到最小 |

极端情况全部排障都不行时，Stationhead 有个兜底功能——**轻晃手机**触发内置的 Bug 上报，描述清楚问题场景（什么时候开始、用的什么网络、什么设备、连的哪个流媒体）提交即可。也可以发邮件 support@stationhead.com，说明你的 Stationhead 用户名和问题。

---

## 🔗 相关资源

- [stationhead官网](https://stationhead.fewf.ren)
- [stationhead文档](https://stationhead.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/stationhead)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/stationhead)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/stationhead)

---

## 📚 相关文章

- [BTS 新专辑首发夜，我在 Stationhead 跟两万 ARMY 同步听了一整夜](https://laoguosofts.github.io/posts/stationhead.html)
- [凌晨两点，我和三百陌生人在同一个频道里听完了Taylor的新专辑](https://alipymanbu.github.io/posts/stationhead.html)
