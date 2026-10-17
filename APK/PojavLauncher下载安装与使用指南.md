## ✨ PojavLauncher 是什么——Android 上也能玩 Minecraft Java 版

PojavLauncher 是一个免费开源的 Android 启动器，它让你在手机上直接运行 Minecraft Java Edition。不同于基岩版，PojavLauncher 启动的是完整的 Java 版客户端——意味着你可以登录正版账号、装 Forge/Fabric 模组、加入 Hypixel 等只支持 Java 版的顶级服务器，皮肤和披风也通过 Mojang 官方账号自动同步。

手机上跑 Java 版的门槛比想象中低：一台 RAM 4GB 以上的 Android 设备、一个 Microsoft 账号、以及 PojavLauncher 本身。启动器内置了 Java 运行时，不需要你手动安装 JDK；首次启动时会从 Mojang 官方服务器下载游戏资源，和 PC 上的官方启动器走的是同一条资源通道。

最典型的使用场景：已有 Minecraft Java 账号、想在通勤路上接着玩 PC 上的单人世界；或者听说过模组生态但只有手机，想第一次体验 OptiFine 高清补丁、JEI 物品管理器、Sodium 性能优化这些 Java 版独有的东西；再或者想在手机上连 Java 版 Realms 和朋友一起生存。

> [!NOTE] 不适合的情况
> 如果你只有基岩版账号（没买过 Java 版），或者手机只有 2GB RAM 及以下，PojavLauncher 可能跑不起来。2GB RAM 设备现代版本启动就吃 1GB+，很容易崩溃。另外如果你追求和 PC 一模一样的操作体验——触屏替代不了键鼠，尤其是 PvP 战斗和复杂红石操作。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://pojavlauncher.fewa.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：在 Google Play 或各 Android 应用市场搜索「PojavLauncher」

---

## 🔧 安装步骤

PojavLauncher 以 APK 形式分发，安装流程和大多数第三方 Android 应用一致。

### 第一步：允许安装未知来源应用

Android 默认禁止从应用商店之外安装 APK。安装前需要打开这个权限：

1. 打开手机 **设置 → 安全**（或 **设置 → 隐私保护**）
2. 找到 **未知来源应用安装** 或 **安装未知应用**
3. 选择你用来下载 APK 的浏览器或文件管理器
4. 把 **允许此来源的应用安装** 打开

> [!WARNING] 不要跳过这步
> 如果没开未知来源权限，安装时会弹出「解析包错误」或直接无法点击安装按钮，常见于 MIUI、HarmonyOS、ColorOS 等定制系统，路径可能略有差异但逻辑一样。

### 第二步：安装 PojavLauncher APK

1. 用浏览器下载 APK 文件
2. 下载完成后点击通知栏的文件，或者用文件管理器找到下载目录
3. 点击 APK → 系统弹窗提示「是否安装」→ 确认
4. 等待安装完成，桌面上会出现 PojavLauncher 图标

### 第三步：核对版本号

首次打开 PojavLauncher，进入 **设置 → 关于**，确认版本号和下载页标注的一致。PojavLauncher 迭代快，新版本经常修复内存泄漏和触控适配问题，建议始终用最新稳定版。

---

## 📖 使用入门

### 首次启动与登录

第一次打开 PojavLauncher 会提示你登录 Microsoft 账号——这是必须的，因为 Minecraft Java Edition 已经全部迁移到 Microsoft 账号体系。登录后启动器会自动从 Mojang 服务器拉取你的账号信息，包括皮肤和披风。

版本选择界面会列出所有可下载的 Minecraft Java 版版本，从 1.6.4 到最新快照版都有。如果你只是想玩当前主流版本，选推荐的正式版即可；玩模组则要注意模组要求的版本号——Forge 和 Fabric 各自支持的版本范围不同。

> [!TIP] 新手直接选正式版
> 快照版（Snapshot）是开发测试版，可能有 bug；预发布版（Pre-release/RC）接近正式版但也不稳定。除非你明确知道自己需要测试新特性，否则选带「Release」标记的版本就好。

### 皮肤管理

PojavLauncher 运行的是 Java Edition，皮肤和披风**只存在于 Mojang 的服务器数据库里**，本地没有皮肤文件。每次启动游戏时自动从 Mojang 拉取。好处是不管在哪台设备上、用哪个启动器，皮肤都自动同步。

改皮肤的完整流程：

1. 准备皮肤文件：去 Skindex、MinecraftSkins、NovaSkin 等网站下载或制作皮肤，保存为 PNG 格式。要求 64×64 或 128×128 像素，不能有透明背景（会显示成黑色）。1.8+ 支持「经典」（Steve）和「纤细」（Alex）两种模型。
2. 上传到 Minecraft.net：在浏览器打开 Minecraft.net → 登录 Microsoft 账号 → 进入个人资料 → 找到皮肤设置 → 选择模型 → 上传 PNG → 确认。Mojang 会校验格式，非法文件会拒绝上传。
3. 回到 PojavLauncher：完全退出启动器重开，新皮肤自动生效。

> [!NOTE] 离线模式皮肤
> 如果你不登录 Microsoft 账号玩离线模式，部分版本的 PojavLauncher 允许在设置里手动指定本地皮肤文件。但这种方式只有你本机能看到，联机时别人看不到——因为你没登录 Mojang 服务器。

皮肤不更新时排查：服务器有缓存就退出服务器等几分钟再进；换个服务器确认是缓存问题还是本地问题；完全退出 PojavLauncher 重开；最后确认登录的就是上传新皮肤的那个账号。披风同理，Migrator Cape（2021~2023 间迁移过账号的玩家获得）和其他活动披风都会自动同步，不需要手动操作。

### 控制与输入

PojavLauncher 提供了三种输入方式：触屏虚拟按键、外接键鼠、蓝牙手柄。

**触屏按键**是最基础的方式。如果按键完全没反应，最常见的原因是 Android 系统抢走了触屏焦点——强制停止 PojavLauncher 后，在 **设置 → 显示 → 沉浸式模式** 里把 PojavLauncher 加入白名单，重开就正常了。某些定制系统的游戏助手（MIUI 游戏空间、华为游戏中心）也可能拦截触控，需要在游戏助手里把启动器加入白名单。

**外接键鼠**需要 USB OTG 转接头。插上后先在 Android 设置里确认系统能识别到键鼠，再进 PojavLauncher 设置 → 输入，看有没有显示「物理键鼠已连接」。廉价转接头经常识别失败，换一个靠谱的通常能解决。蓝牙键鼠要先在 Android 设置里配对成功，完全退出启动器重开——启动时键鼠没插好、之后插上不生效是常见的小 bug。

键鼠按键不对位（按 E 不是背包、按 Shift 不是潜行）时，进入游戏内 **ESC → 选项 → 控制**，找到对应功能重新绑定物理按键即可。或者在启动器设置里的自定义键位映射里修正。

**手柄映射错乱**（左摇杆变转视角、右摇杆变移动）时，去启动器设置 → 输入 → 手柄映射编辑器，把左摇杆绑到 WASD、右摇杆绑到鼠标移动，保存重进游戏。如果用了第三方手柄映射 App，考虑关掉它——两套映射同时作用一定乱。

> [!WARNING] 屏幕漂移
> 手指按的位置和游戏响应位置有偏移时，去启动器设置里找触屏校准重新校准。如果校准没用，检查手机的「全局缩放」设置——某些 ROM 的显示缩放会导致坐标偏移。

### 联机玩法：Minecraft Realms

Realms 是 Mojang 官方的小型多人主机服务，每月约 7 美元订阅费，房主出钱，最多邀请 10 个朋友同时在线。服务器由 Mojang 托管，不需要自己开服、不需要配置端口和白名单，适合 5~10 个朋友经常一起玩生存的小圈子。

| 维度 | Realms | 独立服务器 |
|---|---|---|
| 费用 | 每月 7 美元（房主付） | 免费自开或租服约 20~50 元/月 |
| 配置难度 | 零，官方托管 | 需配置端口、插件、白名单 |
| 在线人数 | 最多 10 人同时 | 取决于服务器配置，10~100+ |
| 自定义 | 极低，只有有限世界类型 | 极高，插件模组自定义地图全支持 |
| 模组支持 | 不支持 | 完全支持 |

加入 Realms 很简单：房主在 Minecraft.net 购买订阅后，在 PojavLauncher 里启动游戏，主菜单的 Minecraft Realms 面板会自动显示。如果你是被朋友邀请的，朋友需要在他的 Realms 里打开玩家管理、输入你的 Microsoft 账号邮箱发送邀请，你收到邮件后在启动器里进入 Realms 面板接受邀请就行。

手机上 Realms 的已知小问题：首次加载可能要 10~30 秒，卡很久就完全关闭启动器重开；房主到期没续费 Realms 会离线，续费后世界保留；被邀请者没法手动下载 Realms 世界保存到本地——只有房主能导出。

### 模组生态

PojavLauncher 的核心优势之一是完整支持 Forge 和 Fabric 两套模组加载器。启动器内置了 Forge/Fabric 安装器，在版本选择界面点对应版本旁边的「安装模组加载器」就能一键装好。

新手推荐从轻量模组包开始——比如 All the Mods Lite 或类似的合集，几十种模组联动，不用开电脑就能体验模组生态的深度。单独安装模组时要注意版本匹配：模组文件要放在启动器创建的 `.minecraft/mods` 目录下，版本号必须和你选的游戏版本一致。

---

## 🔗 相关资源

- [PojavLauncher官网](https://pojavlauncher.fewa.skin)
- [PojavLauncher文档](https://pojavlauncher-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/PojavLauncher)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/PojavLauncher)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/PojavLauncher)
