## ✨ Beyond LIVE 是什么

Beyond LIVE 是由 SM Entertainment 旗下 Beyond Inc. 打造的全球 K-pop 专属线上演唱会平台，自 2021 年底上线以来已服务 130 多个国家的粉丝。与 YouTube Live、抖音直播等通用平台不同，Beyond LIVE 从第一天就是为演唱会场景量身设计的——支持 4~9 路多镜头切换、Unreal Engine 实时 AR 舞台融合、粉丝灯蓝牙同步、Fan Board 双向互动等普通直播根本做不到的功能。

平台底层依赖全球分布式 CDN 边缘节点，把视频推到离观众物理距离最近的节点上，支撑起"60 亿人级并发"的流畅观看体验。每一场演出都是付费内容，票价根据阵容和场次不等，回放 VOD 可在演出结束后通过 Beyond Library 板块单独购买。

Beyond LIVE 提供 PC 网页端和移动端 App 两种观看方式，每账号绑定 2 台设备同步。在隐私方面，App Store 的隐私声明明确写着「不收集任何数据」——不追踪观看行为、不采集设备信息、不做用户画像广告推送，账号登录信息只用于验证身份，不上云。这一点在娱乐产品里比较少见。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://beyond-live.fedq.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：iOS 用户在 App Store 搜 "Beyond LIVE"；Android 用户在 Google Play 搜 "Beyond LIVE"；PC 端推荐直接使用 Edge 或 Safari 浏览器访问 Beyond LIVE 官网观看

---

## 🔧 Beyond LIVE 安装步骤

Beyond LIVE 本质上是一个网页应用，不需要传统意义上的客户端安装——在 PC 上只要浏览器版本够新就能直接看，移动端则需要从应用商店下载 App。

### PC 端：浏览器直接访问

1. **选对浏览器**：Windows 用户首选最新版 Microsoft Edge，macOS 用户首选最新版 Safari。Chrome 也能用但偶尔会有 Web Bluetooth 配对失败，Firefox 完全不支持。Beyond LIVE 会把 Edge 和 Chrome 视为两台独立设备，同时装两个浏览器的同学请谨慎选择。
2. **更新浏览器**：Beyond LIVE 要求使用最新版浏览器。Edge 在"设置 → 关于 Microsoft Edge"里自动更新，Safari 在"系统设置 → 通用 → 软件更新"里更新，Chrome 在"菜单 → 设置 → 关于 Chrome"里更新。
3. **访问官网登录**：打开 Beyond LIVE 官网，用 Weverse 账号或 V LIVE 旧账号登录，绑定兑换码后即可进入观看。

> [!WARNING] 设备额度注意
> 每账号最多绑定 2 台设备同时观看，PC 浏览器和移动端 App 各占一台。如果提示设备数超限，需要去官网设置页解绑旧设备。

### 移动端：App 商店下载

1. **打开应用商店**：iOS 用户打开 App Store，Android 用户打开 Google Play。
2. **搜索并安装**：搜索 "Beyond LIVE"，认准开发者 Beyond Inc. 发布的官方 App，点击安装。
3. **登录激活**：首次打开 App 会引导登录 Weverse 账号，绑定已购买的演唱会门票或兑换码后即可使用。

---

## 📖 Beyond LIVE 使用入门

### 购买演出与入场

Beyond LIVE 的演唱会需要提前购票。通常在演出开始前一周左右开启售票，票价根据阵容和场次在 1~3 万韩元不等。购票后会收到一张兑换码，在 Beyond LIVE 官网或 App 里输入兑换码绑定到自己的账号，演出当天就能直接进场。

演出开场前 10 分钟开启入场通道，进入后会看到虚拟场馆倒计时页面和预热视频。正式开始后倒计时归零自动切换到直播画面。

### Multi-View 多镜头切换

这是 Beyond LIVE 最独特的功能。普通直播只能看导播切好的一路信号，Beyond LIVE 提供主舞台、舞台全景、主唱专属、舞蹈 1 号机位、舞蹈 2 号机位等 4~9 个视角，粉丝可以全程锁定自己最喜欢的成员。

切换方式很简单：画面侧栏有一排缩略图，点击就能跳到对应机位。PC 网页端缩略图在画面右侧，移动端 App 在画面下方。

> [!TIP] 丝滑切换的秘密
> 多路镜头流要做到切换时画面和声音完全不脱节，背后需要统一时间戳基准、HEVC 高效编码控制码率总量、客户端本地缓存多路帧做无缝拼接。如果切镜头后音频先出来画面后到，通常是网络波动导致的短暂同步失败，等几秒会自动恢复。

### AR 舞台特效

Beyond LIVE 的 AR 特效不是后期画上去的，而是用 Unreal Engine 实时渲染。艺人在追踪摄像机前表演，运动追踪数据实时传给游戏引擎，引擎把 3D 资产（龙、浮空建筑、抽象图形）叠加到真人画面上——特效会跟着艺人的动作、舞台灯光的节奏实时响应。

这种"演唱会 × 游戏"的混合形态，是 Beyond LIVE 与普通直播拉开差距的关键技术点。

### 画质与投屏

画质按钮在画面右上角（PC）或顶部（移动端），可以切换不同分辨率。Edge 浏览器在 Windows 上支持 HDR 直通，配合 4K HDR 电视观看效果最佳。Chrome 要启用 HDR 需要在 Flag 页面手动开启，稳定性不如 Edge。

投屏方面，PC 端通过浏览器 HDMI 或 Chromecast 输出，移动端通过 AirPlay 或 Chromecast 投屏到电视。

### 粉丝互动：Fan Board 与粉丝灯同步

Beyond LIVE 的 Fan Board 会把全球观众的头像和昵称投到舞台 LED 墙上，艺人在台上能实时看到，甚至可以对特定粉丝打招呼——这是一个"观众也在台上"的双向循环，不是单向直播。

粉丝灯同步跑在浏览器里，用的是 Bluetooth-over-Web 技术（Web Bluetooth API）。把官方应援棒和电脑配对后，舞台灯光系统可以实时下发颜色和节奏指令到每个家庭的应援棒上，让全球粉丝的应援动作和舞台同步。

### Beyond Library 查看历史演出

Beyond LIVE 专门设有 Beyond Library 板块用来存档所有历史演出，在 Beyond LIVE 官网的 contents/library 路径可以进入。

Library 页面以演出卡片网格展示，每张卡片标注艺术家组合名、演出副标题、举办日期和状态：

| 状态标识 | 含义 |
| --- | --- |
| 已结束（Ended） | 没有回放可供购买 |
| 回放可购（VOD Available） | 有 Beyond VOD 上架，可购买观看 |
| 即将开放（Coming Soon） | VOD 还没上架，即将开放 |

支持按艺术家名精确搜索、按年份筛选、按 Beyond LIVE 官方分类（Live Only / Live + VOD / Special）筛选。

> [!NOTE] 什么演出可能找不到
> 非常早期的 Beyond LIVE 演出（平台初期可能没录入）、地区版权限制的日本/韩国限定场次、还没举办但已经开票的演出（这些在 Beyond NEXT 板块，不在 Library）。

### 观看体验优化技巧

- **不要用标签页后台挂着**：Beyond LIVE 对后台标签页有限流，切回前台会自动恢复，但建议单独开浏览器窗口观看。
- **Edge 比 Chrome 更稳**：粉丝灯同步、HDR 画质、设备识别这几个维度 Edge 的表现都优于 Chrome。
- **提前入场**：开场前 10 分钟入场通道开启，趁这段时间调整好浏览器、测试好投屏、检查兑换码绑定状态，避免开场后手忙脚乱。
- **Re-Streaming 别忘了**：部分场次演出后数天会开放重播，通常限时免费，错过直播的同学可以留意 Beyond Library 的状态更新。

---

## 🔗 相关资源

- [beyond live官网](https://beyond-live.fedq.ren)
- [beyond live文档](https://beyond-live.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/beyond%20live)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/beyond%20live)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/beyond%20live)

## 📚 相关文章

- [期末周攒了两周零花钱，我第一次在 Beyond LIVE 里"上了舞台"](https://laoguosofts.github.io/posts/beyond-live.html)
- [本命 solo 被导播切走了？我用 Beyond LIVE 把镜头抢回来的那个夜晚](https://alipymanbu.github.io/posts/beyond-live.html)
