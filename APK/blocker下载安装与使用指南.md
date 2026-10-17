## ✨ Blocker 是什么：把 Android 四大组件攥在自己手里

Blocker 是一款面向 Android 的组件管理工具，它让你能够按照应用维度，逐个审视并控制应用的四大组件——activity（界面）、service（后台服务）、receiver（广播接收器）、provider（内容提供者）。通过禁用不需要的组件，你可以直接掐断应用自启动、后台偷跑、关联唤醒、广告拉取、数据上报等行为，而不需要 root（配合 Shizuku 即可使用绝大部分功能）。

它适合谁？如果你对以下场景感到困扰，Blocker 就是为你准备的：

- 手机重启后一堆应用自动启动，清理完又冒出来
- 打开 A 应用时，B、C、D 也跟着被拉起来跑了
- 某个应用发热、耗电、流量偷跑，却找不到原因
- 不想卸载应用，但又想让它安静一点

> [!NOTE]
> Blocker 的核心思路是"禁用组件"而不是"卸载应用"——组件被禁用后，应用在桌面仍然在，但对应的功能入口和后台任务都不再响应系统触发。这种方式比冻结应用更精细，可以做到只禁广告不禁推送，只禁开机自启不禁日常使用。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://blocker.uerg.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - GitHub 仓库：通过仓库 Releases 下载 APK 文件，适合关注最新版本的用户
> - 应用商店：部分第三方应用市场可搜索 Blocker 获取

---

## 🔧 安装步骤：Android 平台常规装法

Blocker 目前只提供 Android 版本，安装过程和普通 APK 没有区别，但需要注意权限和后续授权配置。

### 基本安装流程

1. **允许未知来源安装**：进入系统设置 → 安全 → 允许安装未知来源应用（不同厂商 ROM 路径略有差异，一般在"应用权限管理"里）
2. **下载并安装**：从下载入口拿到 APK，点击安装，等待完成
3. **首次启动授权**：打开 Blocker 后会提示需要 shell 权限，此时需要二选一：
   - **有 root**：直接授予 root 权限，组件控制能力最完整
   - **无 root**：通过 Shizuku 走 shell 权限（需要先配对 Shizuku，用无线调试或 root 授权均可）

### 下载注意点

- 核对包名：`com.merigo.blocker`，避免下载到同名但非官方的变体
- 核对版本号：正式版以数字版本号为主，开发版可能带 build 标识
- 杀毒软件误报：部分国内厂商的安全中心可能把第三方组件控制类工具标记为风险，属于误判，可以选择信任放行

> [!WARNING]
> Blocker 不执行"卸载应用"或"修改系统设置"这类高危操作，仅通过 Android PackageManager 或 Intent Firewall 的公开 API 调整组件启用状态。如果你的设备安全策略对这类操作敏感，请确认来源后再安装。

### Shizuku 配对（无 root 路线必备）

如果你走 Shizuku 授权通道，需要额外完成配对：

1. 开启开发者选项中的"无线调试"
2. 在 Shizuku 应用里选择"通过无线调试配对"
3. 用 adb 或手机端配对工具完成一次 shell 授权
4. 配对成功后，重启手机需要**重新启动 Shizuku 服务**（Shizuku 无法开机自启）

---

## 📖 使用入门：从看懂四大组件开始

Blocker 的学习曲线不陡峭，但要把它用好，需要先搞清楚 Android 四大组件各自扮演什么角色。下面从日常最常遇到的两类组件出发，带你建立起"看组件名就能猜到它在干嘛"的直觉。

### 先把应用筛出来

打开 Blocker 后，你会看到设备上所有已安装应用的列表。建议先做两件事：

- **按使用频率排序**，优先关注你每天都在用但后台行为可疑的应用
- **给应用打标签**，比如"社交""电商""工具"，方便批量处理同类问题

### receiver：应用的"耳朵"，关联唤醒和开机自启的入口

receiver（广播接收器）是应用注册监听系统事件的组件。开机完成、网络变化、应用安装、屏幕解锁、充电状态变化——这些事件发生时系统会广播一个 intent，receiver 收到后就启动后续动作。

**开机自启和关联唤醒链的入口，全在 receiver 里。**

常见关联唤醒场景包括：社交应用互相监听登录状态变化、输入法监听激活广播、文件管理器监听 SD 卡挂载卸载、某电商 App 监听你安装了竞品后自动启动推荐。

在 Blocker 里进应用详情，切到 receiver tab，看名字就能大致判断能不能关：

| receiver 名称特征 | 作用 | 是否建议关 |
| --- | --- | --- |
| 带 `BOOT_COMPLETED` | 开机自启入口 | 想禁开机自启就关 |
| 带 `PACKAGE_ADDED` / `PACKAGE_REMOVED` | 监听应用安装卸载，关联唤醒链一环 | 建议关 |
| 带 `CONNECTIVITY_CHANGE` | 监听网络变化，一开网就跑 | 按需关 |
| 带 `USER_PRESENT` / `SCREEN_ON` | 解锁/亮屏触发 | 按需关 |
| 带其他应用的包名前缀 | 直接监听竞品的广播，关联唤醒实锤 | 优先关 |

> [!TIP]
> 关掉 receiver 不是"卸载功能"，而是让应用不再响应那个系统事件。开机不会自启了，亮屏不会被唤醒了，装个别的应用也不会被它关联启动了。不确定的先关一个试试，别一次性全关。

### service：后台偷偷干活的那个，耗电和流量偷跑的元凶

service 是 Android 的后台服务——不显示任何界面，一直在后台运行，做下载、播放、保活、数据上报、心跳、广告拉取这些事。**发热、耗电、流量偷跑，十有八九是后台 service 在值班。**

应用开发者喜欢把各种东西塞进 service：广告 SDK 的 service 定时拉取物料、推送通道的 service 常驻后台维持长连接、数据上报 service 定时打包上传、保活 service 被设计成前台服务（带通知栏常驻图标）。这些 service 加在一起，就是"应用看起来没在跑，但后台一直在忙"的原因。

在 Blocker 里进应用详情，切到 service tab，按以下优先级辨认：

- **优先关**：带 `Ad`、`TTAd`、`Pangle`、`OpenAd`、`GDT` 的广告 SDK service
- **建议关**：带 `Track`、`Report`、`Analytics`、`Umeng`、`Bugly` 的数据上报 service
- **按需关**：带 `Push`、`JPush`、`IGet`、厂商推送关键词的推送 service（权衡一下你是想收推送还是想省流量）
- **谨慎关**：带 `Core`、`Main`、`Engine`、`Download` 的核心功能 service

> [!NOTE]
> service 还分前台 service（带通知栏图标，优先级高，系统不容易杀）和普通 service（内存紧张时可能被回收）。Blocker 对两种类型一视同仁——只要你把开关关了，不管优先级多高都不会被启动。

### 控制器选择：PM、IFW、Shizuku 三种路线怎么选

Blocker 提供四种控制器模式，在设置页顶部就能切换。切换时不会丢失你已经配置好的规则，但不同控制器的原理和行为不同：

| 你的情况 | 推荐控制器 | 原因 |
| --- | --- | --- |
| 应用少，简单场景 | PackageManager（PM） | 效果直观，设置简单 |
| 大厂应用，组件被 PM 复活 | Intent Firewall（IFW） | 应用察觉不到，不会复活 |
| 不 root 但有 Shizuku 配对 | Shizuku 或 IFW | 看 Shizuku 权限够不够 |
| 想最彻底 | IFW + PM | 双层保障 |

简单理解：PM 方式是直接把组件状态改成 disabled，应用一查自己的状态就知道"我被禁了"，有些流氓应用会反复把它改回来（俗称复活）；IFW 方式是在启动意图层面拦截，组件状态保持 enabled，但谁来启动它都会被挡在门外——应用察觉不到，自然也不会复活。

遇到某个组件的 IFW 规则不起作用？可以临时切到 PM 看看效果，确认组件本身可以被控制后再切回来。Blocker 的切换是无缝的，规则会自动翻译到新控制器上。

### 重启手机后 Bloker 开关没反应？检查 Shizuku

如果你走 Shizuku 授权通道，可能遇到过这样的场景：重启手机后打开 Blocker，应用列表加载很慢或加载不出来，所有组件开关点了没反应，设置页里 Shizuku 状态显示断开。

**原因很明确**：Shizuku 服务无法开机自启——这是 Android 系统的限制，第三方应用的后台服务不能开机自启（除非是系统应用）。

解决办法按彻底程度排序：

1. **最快：手动重启 Shizuku**。打开 Shizuku 应用，点"启动"按钮，等状态变成 Running 后回 Blocker 下拉刷新即可
2. **有 root：给 Shizuku 和 Blocker 加开机自启权限**。root 进程不受 Android 普通应用开机自启限制
3. **最彻底：换 root 路线**。root 后 Blocker 直接用 PM/IFW 控制器，完全不依赖 Shizuku，组件状态写在系统 XML 文件里，开机自动生效

> [!TIP]
> 如果你还没 root 但经常碰到 Shizuku 重启问题，考虑给设备 root——一劳永逸，而且 root 后 Blocker 的控制能力不受 Shizuku 权限范围限制。

### 实战组合拳：让一个应用安静下来

把上面的知识串起来，对一个让你头疼的应用，可以按这个流程操作：

1. 进应用详情，先看 receiver tab：关 `BOOT_COMPLETED` → 禁开机自启；关竞品关联广播 → 禁关联唤醒
2. 切 service tab：关广告类 service → 禁弹窗；关上报类 service → 禁数据外传；推送 service 权衡后决定留不留
3. 控制器选 IFW（或 IFW + PM），防应用复活
4. 重启手机验证：应用不会自启、没有广告弹窗、后台不会偷跑
5. 用几天，有功能缺失再回来开对应的开关

---

## 🔗 相关资源

- [blocker官网](https://blocker.uerg.quest)
- [blocker文档](https://blocker-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/APP/blocker)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/APP/blocker)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/APP/blocker)

---

## 📚 相关文章

- [Blocker vs 同类组件管理：为什么它是现在还在维护里的独一份](https://laoguosofts.github.io/posts/blocker-docs.html)

- [安卓组件拦截工具横评，Bloker 为什么值得单独拿出来说](https://alipymanbu.github.io/posts/blocker-docs.html)
