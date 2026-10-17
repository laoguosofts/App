## ✨ SuperSU 是什么

SuperSU 是 Android 平台上最老牌、最广泛使用的 Root 权限管理工具之一，由 Chainfire 开发维护。它的核心职责是接管系统的 su（切换用户）请求，在应用需要 Root 权限时弹出授权窗口，让用户决定放行、拒绝或永久允许。

SuperSU 采用的机制与其他 Root 方案（如 Superuser）类似，但实现上更轻量、兼容性更好。它的工作链路可以概括为：**su 二进制**（位于 `/system/bin/su` 或 `/system/xbin/su`）负责在底层拦截每一次 Root 调用，然后通过 socket 通信把请求上报给 SuperSU App；SuperSU App 记录权限规则、显示弹窗、把 Grant 或 Deny 决策回传给 su 二进制执行。App 本身不直接处理 Root 请求，真正干活的是那个几十 KB 大小的 su 可执行文件——App 更像门卫的值班室和记录本。

这款工具适合哪些人用？如果你刷过 TWRP Recovery、装过自定义 ROM、用 Tasker 做自动化、玩 Xposed 框架、或者需要清理系统预装应用——SuperSU 就是你手机里那个不可或缺的权限闸门。Chainfire 在 v2.82 之后宣布停更，但这并不妨碍它在社区的广泛使用：对于不想依赖 Magisk 模块机制、只想要一个干净简单的 Root 管理器的用户来说，SuperSU 依然是首选。

SuperSU 支持几乎所有 Android 版本（从 2.x 到 11+），也兼容绝大多数主流机型和 TWRP Recovery。它提供浅色/深色两套主题、多种图标样式、多语言界面，以及灵活的权限规则配置——包括全局默认策略（Prompt / Grant / Deny）、按应用单独设置、日志记录、通知栏提示等。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://www.fwen.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜索 SuperSU 查看社区参考（注意官方版本已从商店下架）；最新版通常在 XDA Developers 论坛的 SuperSU 原帖中获取 ZIP 刷机包

---

## 🔧 安装 SuperSU 的两种方式

SuperSU 有两种安装路径：**刷 ZIP 包**（推荐，最常用）和 **单独装 APK**（补装场景）。

### 方式一：TWRP Recovery 刷 ZIP（首次安装必选）

1. **确认已经刷入 TWRP Recovery**。如果还没刷，这是前提步骤——没有自定义 Recovery 就无法刷 SuperSU ZIP。
2. **下载 SuperSU ZIP 包**到手机存储根目录或 USB OTG。
3. **重启进 Recovery**：关机状态同时按住电源键 + 音量减（各机型组合不同），在 Bootloader 里选 Recovery。
4. **刷入 ZIP**：TWRP 主界面点 Install → 找到 SuperSU ZIP → 滑动确认 Flash。
5. **重启**：刷完直接 Reboot System。

> [!TIP]
> 刷 ZIP 会同时安装 su 二进制和 SuperSU App 本身，一步到位。刷完首次开机可能比平时慢 30~60 秒，属于正常现象。

### 方式二：单独安装 APK（补装场景）

如果 su 二进制已经存在（比如之前用 Kingroot、360 Root 等一键工具获得了 Root），但缺 SuperSU App：

1. 下载 SuperSU APK 文件。
2. 在手机设置里允许「未知来源应用」安装。
3. 安装 APK 并打开——它会自动检测已有的 su 二进制并接管。

> [!WARNING]
> 用 Kingroot、360 Root 等一键工具 Root 过的手机直接装 SuperSU 可能会出现 su 二进制来源冲突、弹窗不出现等问题。这种情况下建议先进 TWRP 刷 SuperSU ZIP 包覆盖 su 二进制，再装 App。Chainfire 停更后，社区衍生的 SR5-SuperSU-v2.82-SR5-20191101224232.zip 是目前广泛流通的稳定版本。

### 安装后怎么验证

打开 SuperSU App，看主界面状态提示：

| 提示 | 含义 |
| --- | --- |
| su 二进制已就绪 | 一切正常，可以开始使用 |
| 需要更新 su 二进制 | 点 Continue，选 Normal 模式更新后重启 |
| 无法找到 su 二进制 | 安装不完整，重新刷 ZIP 包 |

也可以装一个需要 Root 的应用试一下——正常情况会在首次调用时弹出 SuperSU 的授权窗口。

---

## 📖 使用入门：权限管理、外观与问题排查

### 权限规则：全局 + 按应用双层控制

SuperSU 的权限管理逻辑分两层：

- **全局默认策略**：Settings → Default access，三个选项——Prompt（每次都弹窗问你，最安全）、Grant（默认全放行）、Deny（默认全拒绝）。新人推荐设 Prompt，跑熟之后可以给常用工具单独设 Grant。
- **按应用单独设置**：Apps 标签页列出了所有请求过 Root 的应用，每个应用可以单独指定 Access 规则（Grant / Deny / Prompt），还能设「仅在屏幕开启时允许」「记录日志」「显示通知」等细项。

> [!NOTE]
> 很多一键 Root 工具（如 Kingroot）自带的 su 二进制会绕过 SuperSU 的拦截，导致 App 不弹窗也不记录日志。这种情况必须先进 TWRP 刷 SuperSU ZIP 覆盖 su 二进制，App 才能正常接管。

### 隐藏图标：低调使用的小技巧

Settings → Launcher icon 里有 5 种图标样式可以选，其中 **Invisible（隐藏）** 比较特别——选了之后 SuperSU 会从桌面消失。如果想低调不想被人一眼看出装了 Root 管理器，这一招很实用。

隐藏之后怎么再打开它？三种方法，从快到慢：

1. **拨号启动码**：在电话拨号界面输入 `*#*#1234#*#*` 或 `*#*#7873778#*#*`（后者对应 S-U-P-E-R-S-U 的字符数），输入最后一个 `*` 后自动执行，不用点拨出。
2. **ADB 命令**：电脑连手机开 USB 调试，执行 `adb shell am start -a android.intent.action.MAIN -n eu.chainfire.supersu/.MainActivity`。
3. **Terminal Emulator**：手机上装一个终端模拟器，本地执行同样的 `am start` 命令——不需要先 su，因为启动 Intent 本身不需要 Root。

成功唤起后把 Launcher icon 改回 Material 或其他样式，图标就回来了。

### su 二进制：理解它才能高效排障

su 二进制是整个 Root 链条的核心。SuperSU App 只是图形化界面，真正拦截 Root 请求、判断 Grant/Deny 的是那个位于 `/system/bin/su` 的可执行文件。

| 路径 | 说明 |
| --- | --- |
| `/system/bin/su` | 最常见的位置 |
| `/system/xbin/su` | 备选位置（部分 ROM 用 xbin 分区） |
| `/su/bin/su` | SuperSU v2.11+ 引入的特殊挂载点 |
| `/data/adb/su/` | 某些自定义 ROM 的自定义位置 |

SuperSU v2.11 之后还附带了 `supolicy` 工具，用来在启动时修改 SELinux 策略并运行 `/su.d/` 目录下的自定义脚本——这就是为什么有些人会在 `/su.d/` 里放开机自启脚本。

**su 二进制的更新有两种方式**：一种是 SuperSU App 自动引导，打开后点 Continue → 选 Normal 模式 → 重启；另一种是直接刷 SuperSU ZIP 覆盖——遇到更新反复失败、或者怀疑二进制来源混乱时，刷 ZIP 是最彻底的解法。

> [!WARNING]
> su 二进制版本与 SuperSU App 版本如果差距太大或来源不一致，会出现授权弹窗能显示但点击 Allow/Deny 没反应、应用请求 Root 直接崩溃、日志不记录任何条目、Enable SuperSU 开关无效等症状。这种情况不要手动去调 su 位置或 SELinux 策略，直接刷 SuperSU ZIP 覆盖最快。

### 弹窗不出现的六步排查法

这是 SuperSU 最常见的故障：应用明确需要 Root（打开提示需要 Root 然后退出），su 二进制也在工作（Terminal Emulator 里 `su --version` 有输出），但 SuperSU 什么反应都没有。按以下顺序排查：

**第一步：检查 Enable SuperSU 开关**。打开 SuperSU → Settings → 确认最顶部的 Enable SuperSU 是开启状态。如果被关掉了，SuperSU 会静默拒绝所有 Root 请求——不弹窗、不 Grant、不记录日志。很多人调试或临时隐藏 Root 后忘了重新打开。

**第二步：检查 Default access**。Settings → Default access，确认不是设为 Deny。推荐设 Prompt。

**第三步：检查应用是否被设为 Deny**。Apps 标签页找到那个应用，看 Access 设置——如果之前不小心设成了 Deny，改成 Prompt 或 Grant。

**第四步：检查 su 二进制是否真的在工作**。Terminal Emulator 里执行 `su -c "echo hello"`：

- 输出 hello → su 在工作，但 SuperSU 弹窗拦截没生效
- 报错或卡住 → su 本身有问题
- 弹窗出现了 → su 正常，问题在 App 层对 Root 请求的处理方式

**第五步：重启整个权限链**。如果以上都没问题但还是不弹窗，重启手机，打开 SuperSU App 确保它处于活跃状态（别被后台清理掉），再试一次需要 Root 的应用。SuperSU 的 su 守护进程在开机后启动，守护进程没起来就会导致 su 直通不经过拦截。

**第六步：刷 ZIP 重装**。重启后依然不弹窗 → 进 TWRP 刷 SuperSU ZIP，彻底覆盖 su 二进制和 App 配置。

还有一种特殊情况需要单独说：如果你发现 `su -c "echo hello"` 能直接输出 hello、弹窗完全不出现，这意味着 su 二进制在工作但它**没有连接到 SuperSU 的守护进程**。su 二进制与 App 之间通过 socket 通信，socket 不存在时 su 就不会经过 SuperSU 的拦截逻辑。修复很简单——重新安装 SuperSU APK，守护进程会重新建立连接。

### 外观与个性化

Settings 里还有几个值得折腾的选项：

- **Theme（主题）**：浅色和深色两套，跟随系统夜间模式切换就行。
- **Language（语言）**：Chainfire 停更后部分新 Android 版本的中文翻译有小瑕疵（个别选项英文、个别中文混合），不影响功能。选 Default language 跟随系统语言即可。

SuperSU 整体是 Material Design 风格，界面干净利落，设置项按功能分组排列，上手没什么门槛。

### 终极救命招：刷 SuperSU ZIP

不管遇到什么问题——su 二进制丢了、弹窗不出来、App 起不来、图标设隐藏找不到了——**刷 SuperSU ZIP 是终极解法**。进 TWRP → Install → 选 ZIP → 滑动确认 → Reboot System。刷完 su 二进制和 SuperSU App 会被彻底覆盖重写，图标默认恢复 Material 风格，App 自动出现在桌面。这一招几乎能解决所有疑难杂症。

---

## 🔗 相关资源

- [SuperSU官网](https://www.fwen.ren)
- [SuperSU文档](https://supersu-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/SuperSU)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/SuperSU)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/SuperSU)

---

## 📚 相关文章

- [我的 SuperSU 突然不弹窗了，折腾半小时才找到那个开关](https://laoguosofts.github.io/posts/supersu-docs.html)
- [朋友那台被 Kingroot 搞残的旧手机，我用 SuperSU 救回来了](https://alipymanbu.github.io/posts/supersu-docs.html)
