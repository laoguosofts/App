## ✨ MobileGlues 是什么

MobileGlues 是 Android 设备上运行 Minecraft Java 版的原生 OpenGL ES 封装渲染器。它不依赖 Vulkan 中间层，直接走 Android 原生支持的 OpenGL ES 3.x 路径，为 PojavLauncher、Fold Craft Launcher、Zalith Launcher 2 等启动器提供自定义渲染器注入能力，是目前 Android 端玩 Minecraft Java 加光影的主流方案之一。

从 v1.2 引入着色器平台宏（MG_MOBILEGLUES / MG_MOBILEGLUES_VERSION）起，光影开发者就能针对 MobileGlues 写条件编译路径；v1.3.5 修复了 Sodium 渲染错误和纹理子系统问题，同时把权限模型从 SAF（Storage Access Framework）切换为 MANAGE_EXTERNAL_STORAGE；v2.0.0 则是一次全面重构——核心库做了小幅性能和内存优化，专门修复了 JourneyMap 和 Xaero's World Map 的渲染问题，插件端 UI 完全重做为 Home / Settings / Info 三个标签页，新增 Material 和 Miuix 两套主题切换、横竖屏布局适配、从启动器借 ANGLE 等特性，MultiDraw 模拟逻辑也支持了每个函数单独配置 + 内置基准测试一键找最快组合。

适合谁用？主要是手里有骁龙/联发科旗舰机、想在手机上跑 Minecraft Java 加光影加模组的玩家。28 种主流光影包官方验证兼容，Sodium、Iris、OptiFine、JourneyMap、Create 等模组也都有针对性适配，比 Zink 那种多一层 Vulkan 翻译开销的方案更稳。最低 Minecraft 版本要求 1.17，Android 建议 8.0 以上。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://mobileglues.ggcd.uno/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 平台直接从官方渠道获取 APK 文件，安装后在各支持自定义渲染器的启动器内注入使用

---

## 🔧 安装与权限授予

MobileGlues 本身是一个独立的 Android 插件 APK，安装后通过启动器的渲染器设置页注入，不走常规的应用商店更新通道。下面分四步走完整个安装流程。

**第一步：安装 APK**

从下载入口拿到 APK 文件后，在 Android 设备上直接点开安装。如果系统弹出「未知来源应用」的拦截提示，进入设置页开启对应文件管理器或浏览器的安装权限即可。安装完成后，桌面会出现 MobileGlues 的图标（旧版本可能没有图标但插件本身已注册）。

**第二步：授予所有文件访问权限**

打开 MobileGlues 插件应用，首次启动会弹出权限请求。v1.3.5 起权限模型从 SAF 改为 MANAGE_EXTERNAL_STORAGE，需要授予「所有文件访问权限」——这个权限 Android 11（API 30）及以上系统才会看到，之前版本走 SAF 流程。

> [!WARNING]
> v1.3.5 是权限模型切换的分水岭。如果你是从 v1.3.x 升级上来的老用户，升级后第一次打开插件时必须重新授予权限，旧权限不会自动继承。没授权就进游戏大概率报渲染器初始化失败。

**第三步：在启动器里注入渲染器**

打开你正在用的 Minecraft Java 启动器（Pojav Glow Worm、Fold Craft Launcher、Zalith Launcher 2 等），进入设置界面：

1. 找到 **Video and Renderer**（视频与渲染器）区域
2. 点击 **Renderer**（渲染器）选项
3. 从列表里选择 **MobileGlues**

选完后会进入 MobileGlues 的渲染器详细设置页。首次使用建议先用默认配置跑一遍，确认游戏正常启动后再微调。进阶玩家可以在 MultiDraw 模拟方式里选 **Prefer MultiDraw Indirect** 换取最佳性能，或者按需开启 ANGLE 驱动（某些 GPU 原生 ES 驱动有 bug 时用 ANGLE 兜底）。

> [!TIP]
> MultiDraw 模拟逻辑在 v2.0.0 里做了完全重构——每个函数可以单独配置模拟方式，还内置了自动基准测试，一键测出当前设备最快的模拟组合。升级到 v2.0.0 后建议跑一次基准，比手动调参数靠谱得多。

**第四步：核对安装是否成功**

正常启动 Minecraft 后，观察启动器日志。如果配置正确，日志里会出现 `Initializing MobileGlues ...` 字样，并列出当前启用的配置项。进游戏后如果画面正常、无黑块或花屏，说明注入成功。Minecraft 26.3-snapshot-3 及以后版本需要在插件设置里启用「忽略 shader/program 报错」，否则可能卡在着色器编译阶段。

---

## 📖 使用入门：渲染器选型、模组兼容与性能调优

MobileGlues 装完就能用，但想要玩得舒服、不踩坑，下面这些点值得提前搞清楚。

### 为什么选 MobileGlues 而不是 Zink 或 VirGL

Android 上跑 Minecraft Java 目前有三条主要渲染路线，核心差异在于 GPU 路径和模组兼容面：

| 渲染器 | 核心思路 | GPU 路径 | 模组/光影支持 |
| --- | --- | --- | --- |
| **MobileGlues** | 原生 OpenGL ES 封装 | OpenGL ES 3.x | Sodium、Iris、OptiFine、JourneyMap、Create 等 |
| **Zink** | Mesa 项目的 OpenGL over Vulkan | Vulkan | 依赖驱动和 Zink 实现版本 |
| **VirGL** | Mesa 的虚拟 GL 实现 | 依赖宿主 GPU | 作为后备方案，兼容性有限 |

MobileGlues 的优势很明确：原生 ES 路径理论上开销最低，专门针对 Sodium、Iris 等模组做了兼容处理，28 种主流光影包官方验证兼容，内置着色器缓存加快重载速度，而且 v1.3.5 和 v2.0.0 连续快速迭代，修掉了大量实际使用中遇到的问题。

Zink 是 Mesa 项目把 OpenGL 翻译成 Vulkan 的方案，优点是 Mesa 生态完整、兼容面广，但多了一层 Vulkan 翻译开销，不同设备的 Vulkan 驱动差异大，性能表现不稳定。某些 Mali GPU 上 Zink 可能比 MobileGlues 快，但在很多 Adreno GPU 上不如原生 ES 路径。VirGL 主要作为后备或虚拟机场景使用，Android 上性能通常不如 MobileGlues。

**选哪个的建议**：优先 MobileGlues。除非遇到三种情况才考虑换路线——你的设备 OpenGL ES 驱动有无法绕过的 bug（试了 ANGLE 还是不行）、你需要某个特定模组或光影只在 Zink 路径下能工作、或者你的 GPU 的 Vulkan 驱动特别优秀而原生 ES 驱动拉胯。实测建议也很朴素：在同一设备上用同一组模组和光影，分别跑两条路线对比 FPS 和渲染正确性，不要只听别人说。

### 启动器里的关键设置项

Pojav Glow Worm 是 Android 上最早支持自定义渲染器注入的启动器，配置流程最典型。Fold Craft Launcher 和 Zalith Launcher 2 的界面略有差异，但核心步骤是一样的：选渲染器 → 进详细设置 → 微调参数 → 启动游戏。

进入 MobileGlues 渲染器设置页后，几个值得留意的参数：

- **MultiDraw 模拟方式**：v2.0.0 起每个函数可以单独配置，首次进游戏跑一次内置基准测试，它会自动测出当前设备最快的模拟组合，比手动逐个试高效得多
- **ANGLE 驱动开关**：当某些 GPU 原生 ES 驱动有兼容性问题（比如 Xaero's World Map 在 0.5x 缩放下颜色异常）时，ANGLE 可以作为兜底方案绕过驱动 bug
- **主题切换**：插件 UI 支持 Material 和 Miuix 两套主题，横竖屏布局自动适配，长时间用下来体验比 v1.3.x 好不少

### JourneyMap 等地图模组的兼容情况

JourneyMap 是 Minecraft 社区最流行的实时地图模组，在游戏内提供小地图、世界地图、传送点标记等功能。它有自己的自定义渲染管线，会深度介入 Minecraft 的渲染系统，因此对渲染器的兼容性要求比较高。

MobileGlues 从 v2.0.0 开始专门修复了 JourneyMap 在上面的渲染问题，之前版本可能存在地图不更新或渲染不正确的情况。使用建议很简单：直接用 v2.0.0 或更高版本，JourneyMap 的任何更新版本都能正常工作，不需要特殊配置。

Xaero's Minimap 和 Xaero's World Map 也被列为兼容模组，但 Xaero's World Map 在 0.5x 缩放下存在颜色显示问题，这是已知的 GPU 驱动兼容性问题，通过 ANGLE 开关可能绕过。JourneyMap 和 Xaero 系列功能有重复，建议只装一个。

### 升级策略

从 v1.3.x 升级到 v2.0.0 的收益远大于成本。虽然 UI 变化很大（旧版是单页设置，新版改成三个标签页），但核心库向下兼容，升级后记得重新授予权限（v1.3.5 改了权限模型，旧权限不继承），然后跑一次 MultiDraw 基准测试找最佳配置。首次使用直接上 v2.0.0 就行，没必要从旧版本过渡。

---

## 🔗 相关资源

- [MobileGlues官网](https://mobileglues.ggcd.uno)
- [MobileGlues文档](https://mobileglues-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/MobileGlues)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/MobileGlues)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/MobileGlues)

## 📚 相关文章

- [MobileGlues 值不值得花时间折腾？一笔 Android 玩 Minecraft 光影的决策账](https://laoguosofts.github.io/posts/mobileglues-docs.html)
- [MobileGlues 值不值得掏钱？——Android 玩 Minecraft Java 的真实成本账](https://alipymanbu.github.io/posts/mobileglues-docs.html)
