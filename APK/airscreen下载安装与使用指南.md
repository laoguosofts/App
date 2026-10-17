## ✨ AirScreen 是什么

AirScreen 是一款专为 Android TV / Google TV 生态设计的多协议投屏接收器，由 Ionitech 开发。它的核心能力是让你的电视盒子同时兼容 AirPlay、Google Cast、DLNA 三种主流投屏协议——iPhone 用户可以直接用控制中心点屏幕镜像，Android 用户可以用系统自带的投屏按钮，电脑也能把浏览器标签页或媒体文件扔到电视上，全平台通吃。

除了基础的投屏接收，AirScreen Pro 版还提供去广告、4K 接收、录屏、自定义设备名等进阶功能。免费版已经能跑通核心接收链路，适合先测后买；Pro 订阅按月、按年或一次性终身购买。

从系统要求看，接收端最低支持 Android 5.0，几乎覆盖所有在售的 Android TV 盒子（小米、当贝、极米、Fire TV 等）。值得注意的是，从 Android 8.0 开始 Google 在系统层面禁用了 Miracast 接收器，所以新版盒子上找不到 Miracast 连接是正常现象，不是 AirScreen 的功能缺失。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://www.bmww.store/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android TV / Google TV 在 Google Play 搜 AirScreen；Amazon Fire TV 在 Amazon Appstore 搜；其他 Android 盒子可通过 APK 侧载安装

---

## 🔧 安装步骤

### 确认你的设备是否兼容

安装前先核对接收端系统版本：

| 平台 | 最低系统版本 | 安装渠道 |
| --- | --- | --- |
| Android TV / Google TV | Android 5.0 | Google Play 直接装 |
| Amazon Fire TV / Firestick | FireOS 7.0（Android 9 等价） | Amazon Appstore 直接装 |
| 其他 Android 盒子（小米、当贝、极米等） | Android 5.0 | 部分机型预装 Google Play，其他需 APK 侧载 |

> [!TIP]
> FireOS 8.0 及以上版本的 Fire TV 安装后需要手动开启 AirScreen 后台服务，否则切出应用后投屏会断开。

### 标准安装流程（Google Play 渠道）

1. 打开电视盒子上的 **Google Play** 应用
2. 搜索 **AirScreen**（注意认准开发者 Ionitech）
3. 点 **安装**，等待下载完成
4. 安装完成后在应用列表里找到 AirScreen 图标打开

### APK 侧载安装流程（无 Google Play 的盒子）

1. 在电脑上下载 AirScreen APK 文件（从可信第三方渠道获取，核对版本号和文件大小）
2. 把 APK 拷到 U 盘或通过局域网传输到盒子
3. 在盒子上打开文件管理器，找到 APK 文件
4. 如果弹出「未知来源安装」提示，去系统设置里给文件管理器授权
5. 点击 APK → 安装 → 完成

### 安装后的首次检查

打开 AirScreen 后，界面应该能正常显示当前设备名称和可用协议标识。建议先用发送端（手机或电脑）测试一次基本投屏，确认 AirPlay、Cast、DLNA 三种协议至少你常用的那些能通。

> [!WARNING]
> 如果打开 AirScreen 后界面都看不到，先别着急重装——按顺序试：强制停止 → 清除缓存 → 清除数据（⚠️ 会丢订阅状态）→ 重启设备 → 更新到最新版 → 最后才是卸载重装。

---

## 📖 入门使用指南

### 三种投屏协议怎么选

AirScreen 支持的三种协议各有适用场景，搞清楚它们的区别能帮你少踩很多坑：

| 协议 | 适合发送端 | 特点 | 限制 |
| --- | --- | --- | --- |
| AirPlay | iPhone / iPad / Mac | 原生系统级投屏，延迟低、画质好 | Android 发送端不支持；DRM 内容受 Apple 策略限制 |
| Google Cast | Android 手机 / Chrome 浏览器 | 支持流媒体推送和标签页 Cast | iOS 发送端需第三方 App；DRM 内容受流媒体服务策略限制 |
| DLNA | Windows 媒体播放器 / Android 相册 | 最基础的媒体文件推送 | 不支持屏幕镜像，只能推视频/图片/音频文件 |

> [!NOTE]
> 不确定当前连接用的是哪种协议？AirScreen 界面上会显示对应的协议标识——AirPlay 连接会出现 Apple 相关标识，Cast 连接会出现 Chromecast 图标，DLNA 连接显示媒体播放提示。

### 快速投屏操作流程

**iPhone / iPad / Mac 用户（AirPlay）：**
1. 确保发送端和电视盒子在 **同一个 Wi-Fi** 下
2. iPhone / iPad：从屏幕右上角下拉控制中心 → 点「屏幕镜像」→ 选 AirScreen 设备名
3. Mac：点菜单栏右上角 AirPlay 图标 → 选 AirScreen 设备名
4. 连接成功后画面自动投到电视上

**Android 用户（Google Cast）：**
1. 确保同一 Wi-Fi
2. 下拉快速设置面板 → 点「投屏」或「Cast」→ 选 AirScreen 设备名
3. 或者在支持 Cast 的 App（如 YouTube、Netflix）里点 App 内的 Cast 按钮

### 常见问题排查

投屏出问题时，按以下顺序排查效率最高：

**黑屏问题：**

黑屏有三种典型表现，对号入座找原因：

- **完全黑屏，连 AirScreen 界面都看不到** → AirScreen 应用本身问题，按强制停止 → 清缓存 → 清数据 → 重装的顺序试
- **AirScreen 界面能看到，投屏后画面消失只剩黑底** → 连接成功但解码失败。最常见的原因是解码器兼容性问题，去 AirScreen 设置里切换硬件解码器和软件解码器试试——硬件解码器兼容性因盒子型号差异很大，软件解码器虽然慢但兼容性好。另外也可以试试降低 Android TV 的 HDMI 分辨率（从 4K 降到 1080p）或切换 HDMI 色彩空间
- **投屏短暂显示几秒后变黑** → 通常是网络不稳定（检查 Wi-Fi 信号，切 5GHz 频段，重启路由器）、发送端主动断开（iPhone 锁屏或切 App 会断屏幕镜像）、或 AirScreen 后台被杀（确认系统电池优化没限制 AirScreen，设为「无限制」）

> [!WARNING]
> 如果黑屏发生在 Netflix、Disney+、Prime Video 等受 DRM 保护的流媒体上，大概率是正常限制——AirScreen 的 Widevine 支持取决于接收端设备的安全等级和流媒体服务的推送策略，不是 AirScreen 的 bug。先用免费版测试 DRM 兼容性再决定是否升级 Pro。

**网络问题：**

- 投屏断断续续 → 检查 Wi-Fi 信号强度，发送端和接收端尽量靠近路由器
- 延迟明显 → 确认双方在同一频段（都用 5GHz 或都用 2.4GHz），避免一个连 5GHz 一个连 2.4GHz
- 找不到设备 → 确认发送端和接收端在 **同一个 Wi-Fi** 下（有些路由器会把 2.4GHz 和 5GHz 分成两个网络）

### 免费版 vs Pro 版怎么选

免费版已经能跑通三种协议的基本接收功能，**不是所有人都需要 Pro**。先看肯定不值得升级的情况：

| 情况 | 为什么不值得 |
| --- | --- |
| 只是偶尔投个屏，一个月用一两次 | 免费版够用 |
| 电视是 1080p 的，不需要 4K | 4K 功能对你没用 |
| 只用到 AirPlay 接收，其他协议不用 | 免费版的 AirPlay 接收和 Pro 版一样好 |
| 免费版广告不影响使用 | 广告是免费版唯一的"成本" |
| 不确定 AirScreen 在设备上能不能正常工作 | 先测通链路再考虑付费 |

值得升级的典型场景：

**场景一：广告真的很烦。** 免费版投屏前可能要等几秒广告，投屏过程中也可能突然弹出打断体验。家里老人小孩用电视时广告显得不专业。这种情况下 Pro 的去广告是最直接的价值回报。

**场景二：录屏是刚需。** 经常做会议演示需要同步录制、投屏播放电影时想截取精彩片段、给家人做投屏操作教程、教学场景下录制远程授课画面——这些都是 Pro 录屏功能的典型使用场景。

**场景三：多设备场景需要自定义名字。** 如果有两台及以上的 AirScreen 接收端（客厅电视 + 主卧电视 + 投影仪），自定义设备名能让投屏列表从一堆 "MIBOX 4A" "Android TV" 变成清晰的 "客厅电视" "主卧投影仪"，朋友来家投屏时不会选错设备。

**场景四：4K 电视 + Mac。** AirScreen Pro 是少数能在 Android TV 上接收到 4K AirPlay 的方案（前提是 Mac 支持 AirPlay 4K 编码、Wi-Fi 带宽足够、AirScreen 硬件加速开启）。

**场景五：替代 Apple TV。** 一台 400 元级的 Chromecast with Google TV + AirScreen Pro 订阅的总成本远低于 Apple TV 4K，虽然体验有差距但功能覆盖足够。

### 付费模式对比

AirScreen Pro 有三种付费模式：

| 模式 | 适合谁 | 国内参考价格（Google Play） | 性价比 |
| --- | --- | --- | --- |
| 月度订阅 | 第一次用，不确定要不要长期用 | ¥15-22 / 月 | 最低（短期灵活） |
| 年度订阅 | 确定长期用，不想被自动续费太频繁提醒 | ¥120-180 / 年 | 中等 |
| 一次性终身购买 | 讨厌订阅模式，或者已经续费了两年以上 | ¥200-300 | 最高（一次买断） |

简单公式：**如果年付价格 × 2 > 终身购价格 → 买终身；如果用了两年以上 → 该转终身了**。

订阅是和 Google Play / App Store 账号绑定的，用同一个商店账号在多台设备上都能激活 Pro。取消订阅在 Google Play → 支付和订阅 → 订阅里操作，注意取消后当前计费周期内 Pro 功能仍然可用。

---

## 🔗 相关资源

- [airscreen官网](https://www.bmww.store)
- [airscreen文档](https://airscreen.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/APP/airscreen)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/APP/airscreen)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/APP/airscreen)

---

## 📚 相关文章

- [AirScreen 避坑指南：投屏神器也有这几处容易栽跟头的地方](https://laoguosofts.github.io/posts/airscreen.html)
- [AirScreen 投屏踩坑实录：那些让你以为电视坏了的小问题](https://alipymanbu.github.io/posts/airscreen.html)
