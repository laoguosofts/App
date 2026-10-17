## ✨ Guise Reborn 是什么

Guise Reborn 是一款面向 LSPosed / Modern Xposed 框架的运行环境伪装模块。它不修改系统全局设置，而是在每个被选中的目标应用进程中拦截 Android API 的返回值，让不同应用各自看到一台不同的虚拟设备——应用 A 看到小米 11、应用 B 看到 Pixel 7 Pro，系统本身和未勾选的应用则维持真机原貌。

和同类工具的本质区别在于，Guise 要求你选择「完整机器档案」而不是填一堆零散字段。大部分改机工具给你一张 Build.MODEL / Build.BRAND / Build.DEVICE 的表格，各自独立互不关联，很容易造出一台「型号是 Pixel 7、指纹里却写着小米代号、GPU renderer 却是联发科 Mali」的设备。跨通道矛盾恰恰是主流指纹识别 SDK 最先抓的东西，改得越多反而越显眼。

Guise 的核心设计理念就是一台被伪装的设备是一个完整自洽的档案。你选的是「小米 11」或者「Pixel 7」，不是键值对。关键值由模块自动拼接生成，不会和型号打架：Fingerprint 从品牌、产品代号、设备代号、版本号、构建号一路串起来；Android API 级别直接取自真机（API 改了应用会崩）；GPU、平台、ABI、编解码器这些属于芯片的属性按 SoC 复用，几十个芯片就能覆盖上千机型，共享芯片的机型自动保持一致。这种按应用粒度、不碰全局的设计，让每个应用有独立的伪装配置，互不干扰。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://guise.fedq.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - LSPosed 仓库索引：modules.lsposed.org/module/com.houvven.guise
> - 源码发布页：daxiaamu/Guise_Reborn/releases（Stable 标签优先，预发布版带 alpha / beta）

> [!WARNING]
> Guise Reborn 目前只发布 arm64-v8a 架构的 APK，下载前请确认你的设备是 64 位 ARM 架构。如果你的 LSPosed 本身装在 32 位环境里，Guise 也无法正常工作。

---

## 🔧 安装步骤

Guise 的安装分两步走：先把 APK 装到手机上，再去 LSPosed 管理端启用模块并勾选作用域。两步缺一不可。

### 第一步：安装 APK

两种方式任选其一：

1. **直接安装 APK** —— 把 APK 传到手机（文件传输、下载器都行），用系统文件管理器点击安装。如果提示「未知来源」，到设置里允许当前来源的安装即可。装完应用抽屉里会出现 Guise 图标，但此时模块功能还没生效。
2. **ROOT 静默安装** —— Guise Reborn 自带更新系统，从应用内部检查更新、下载后可以直接通过 Root 权限完成安装，不需要手动点击确认。

### 第二步：在 LSPosed 中启用

这是很多新手最容易遗漏的一步。装好 APK 只是把应用放到了手机上，模块本身还没进入 Xposed 框架的 Hook 流程。

1. 打开 LSPosed 管理端，在模块列表里找到 Guise Reborn
2. 勾选右侧开关启用模块
3. 进入作用域设置，勾选你想要伪装的目标应用（微信、抖音、游戏等）
4. 重启设备，或者在 LSPosed 里选择作用域后让框架自行作用于目标应用

> [!TIP]
> Guise 的勾选状态会自动同步为 LSPosed 作用域。你可以在 Guise 应用界面里勾选目标应用，也可以直接到 LSPosed 管理端勾，两边是互通的。

### 安装后的状态检查

装好、启用、勾完作用域之后，建议重启一次设备让 Hook 生效。重启后打开 Guise 应用，进入应用列表确认勾选状态，然后点开某个已勾选的应用看它当前的伪装配置是否已经生效。一个简单的验证方法是找一个能显示设备型号的应用（比如某些跑分 App 或系统信息查看器），确认它看到的型号就是你在 Guise 里配置的那台虚拟设备。

---

## 📖 使用入门

### 两条通道，同时覆盖

Android 应用读取设备信息有两条独立通道——Java Build.* 层和 SystemProperties 层。大部分同类工具只 Hook Java 层，但很多应用和完整性检查 SDK 根本不读 Java Build 类，直接读系统属性。只 Hook Java 层、属性层漏掉，是改机模块「半残」最常见的样子。

Guise 两层同步覆盖。Java 层可以替换的字段包括品牌、制造商、型号、设备代号、产品代号、主板代号、硬件代号、构建 ID、构建主机、构建用户、构建时间戳、显示名称等，其中 Fingerprint 不需要手动填，由模块根据品牌、产品代号、设备代号、版本、构建号自动拼接生成。版本号相关字段（RELEASE、INCREMENTAL、SECURITY_PATCH）也一起覆盖，但 SDK_INT 必须保持真机值——应用会根据它做运行时分支，改成不存在的值会直接崩溃。

属性层覆盖了 23 个 ro.* 键，包括 ro.product.brand、ro.product.model、ro.product.device、ro.product.name、ro.product.manufacturer、ro.build.display.id、ro.build.fingerprint、ro.build.id、ro.build.type、ro.build.tags、ro.build.version.release、ro.build.version.incremental、ro.build.version.security_patch 等关键属性。你在 Guise 里改一个字段，两层一起变，不会出现应用 A 用 Build.MODEL 读到 Pixel 7、应用 B 用 SystemProperties.get("ro.product.model") 读到真机 Redmi 的矛盾情况。

Build.getSerial() 也是一个容易踩坑的入口——自 Android 8 起 Build.SERIAL 常量恒为 UNKNOWN，真正被调用的是 Build.getSerial() 方法，Guise 专门 Hook 这个入口返回配置好的序列号。

### 三源交叉哈希诊断

Guise 自带一个三源交叉哈希的诊断工具，用来帮你确认伪装配置是否真的在所有通道都生效了。它把同一批设备事实分别从三个完全独立的通道读取，各自哈希后比对：

| 通道 | 读取方式 | Guise 能 Hook 吗 |
|------|---------|-----------------|
| Java API | Build.MODEL、GLES20.glGetString() 等 | ✅ 大部分 |
| 系统属性 | SystemProperties.get("ro.product.model") | ✅ 23 个 ro.* 键 |
| 内核 | /sys/devices/soc0/machine、/proc/cpuinfo 等 | ❌ 不能 |

理想状态下，Java API 层和系统属性层应该返回同一个伪装值，哈希一致；内核层返回的是真机物理信息，哈希必然不同——这是正常的，内核层就是改不了。如果 Java 和属性层哈希不同，说明你只改了 Build 类忘了 SystemProperties，配置不完整。三层完全一致只有在真机本身就是目标设备时才可能发生。

这个诊断工具还带有基线对照功能：第一次运行时记录当前身份哈希作为基线，之后每次运行对比基线和当前值，显示具体哪些字段变了。用来验证配置修改是否生效、或者排查应用是不是在偷偷采集你的真实设备信息，比凭感觉看配置靠谱得多。

### 常见配置场景

**场景一：给游戏伪装机型**

很多游戏会根据设备型号做画质分级。你可以在 Guise 里选一台旗舰机型（比如 Pixel 7 Pro 或小米 14），然后只把这款游戏勾进作用域。装好配置后用诊断工具跑一遍三源哈希，确认 Java 层和属性层都返回旗舰型号，再进游戏看画质选项是否解锁。

**场景二：给不同应用分配不同机型**

微信里想让对方看到一台小米 11，抖音里想让系统推荐算法以为你是 Pixel 7 用户——Guise 完全支持。每个应用的伪装配置独立保存，互不干扰。在应用列表里点进每个应用单独配置就行。

> [!NOTE]
> GPU 相关的伪装（GL_RENDERER / GL_VENDOR）需要 SoC 匹配。如果你选了一台用骁龙 8 Gen 2 的 Pixel 7，但你的真机是联发科 Helio P95，GPU renderer 信息和内核 SoC 信息的矛盾会被完整性检查 SDK 抓出来。这种情况下要么选一台 SoC 相同的机型，要么接受内核层和 Java 层不一致的事实——大部分应用只看前两层。

**场景三：排查伪装没生效的问题**

如果某个应用显示的还是真机型号，按这个顺序查：

1. LSPosed 里 Guise 模块是否启用
2. 目标应用是否在作用域里
3. 改完配置后是否重启了设备（或者在 LSPosed 里让作用域重新加载）
4. 用三源哈希诊断工具看 Java 层和属性层是否一致
5. 确认版本兼容性——某些 Android 大版本升级后旧版 Guise 可能需要更新

---

## 🔗 相关资源

- [Guise官网](https://guise.fedq.ren)
- [Guise文档](https://guise-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/Guise)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/Guise)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/Guise)

## 📚 相关文章

- [Guise 改机踩过的四个坑——从 Build 层到属性层的血泪教训](https://laoguosofts.github.io/posts/guise-docs.html)
- [Guise 用过之后才懂的几个坑——我重装了两次才踩明白](https://alipymanbu.github.io/posts/guise-docs.html)
