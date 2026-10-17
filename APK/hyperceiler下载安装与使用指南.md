## ✨ HyperCeiler 是什么

HyperCeiler 是专为小米、Redmi、POCO 品牌手机打造的 Xposed/LSPosed 模块。它运行在已 Root 的设备上，通过 Hook 系统进程的方式，为 HyperOS / MIUI 解锁大量系统级自定义能力——从状态栏、锁屏、通知中心的外观调整，到桌面图标布局、相机水印、相册增强，再到应用商店机型伪装、温控禁用、后台保活等框架层修改，覆盖了小米手机用户日常使用中几乎所有想要"再折腾一下"的角落。

和普通 Android App 不同，HyperCeiler 的功能开关只是它的 UI 层——真正的 Hook 逻辑在模块代码里，被 LSPosed 注入到对应的系统应用进程后才会生效。这意味着它的使用门槛比一般软件高：设备必须先 Root、刷入 Magisk、再装 LSPosed 框架，最后在 LSPosed 里手动勾选作用域——跳过任何一步都会遇到"模块装了但不生效"的情况。

适用的设备范围是小米或 Redmi / POCO 品牌的官方原生 HyperOS / MIUI 系统，第三方 ROM、国际版系统、平板设备通常不在适配范围内。当前主流版本推荐 Android 15/16 的 HyperOS 2/3。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://hyperceiler.hfdk.space/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 或小米应用商店搜软件名；部分地区应用商店可能下架，以第三方镜像源为准

---

## 🔧 安装前的准备工作

HyperCeiler 的安装不是简单地装个 APK——它依赖一整套 Root + Xposed 框架链路。按顺序来，缺什么补什么：

**第一步：解锁 Bootloader**。小米手机进入开发者选项 → 设备解锁状态 → 绑定账号并申请解锁。官方解锁工具在 Windows 上操作，解锁后设备数据会清空，提前备份。

**第二步：刷入 Magisk 获取 Root**。解锁后可以通过 Magisk 修补 boot 分区并刷入，拿到 sudaemon，这是所有后续操作的权限基础。没有 Root，HyperCeiler 连读写 `/data` 分区的权限都没有，更别说 Hook 系统进程了。

**第三步：安装 LSPosed 框架**。通过 Magisk 模块的形式安装，LSPosed 会把 Xposed 运行时塞进 zygote 进程——它是 HyperCeiler 真正"注入代码"的载体。

> [!WARNING]
> 以上三步每一步都有各自的风险点：解锁 Bootloader 会失去官方保修、Magisk 刷写失败可能导致无法开机、LSPosed 版本不兼容会导致系统崩溃。请严格按各组件官方文档操作，并确保你刷的是与自己 Android 版本匹配的分支。

**第四步：安装 HyperCeiler 模块 APK**。下载后直接安装到设备即可。打开 App 后先确认两件事：一是设置 → 关于里显示的版本号和 OS2/OS3 分支是否匹配你的 HyperOS 版本（OS2 分支装 HyperOS 2 设备、OS3 分支装 HyperOS 3 设备），二是去 LSPosed → 模块里找到 HyperCeiler，右上角开关是否已打开。

**第五步：配置作用域**。这是最容易被忽略但最关键的一步。HyperCeiler 的功能分布在不同系统进程里，你必须在 LSPosed 的模块详情页手动勾选这些进程作为作用域——想用相机功能就得勾相机包名、想改状态栏就得勾系统界面、想改应用商店就得勾应用商店包名。**系统框架（system）几乎是必勾的**，没有它，绝大多数系统级 Hook 都不会生效。拿不准该勾哪些时，直接用 LSPosed 给 HyperCeiler 推荐的作用域清单，不要自己瞎选。

> [!TIP]
> 作用域的包名必须精确——"系统界面"是 `com.android.systemui`，"系统桌面"是 `com.miui.home`，不要勾成别的名字。如果你不确定某个功能需要哪些作用域，对照 HyperCeiler App 里该功能的说明，或者去它的 GitHub README 查完整的作用域列表。

配置完作用域后，建议**直接重启设备**完成首次加载。后续改单个功能开关时，HyperCeiler App 会提示你重启对应的作用域进程（比如"重启系统界面"），不一定需要整机重启。

---

## 📖 功能概览与使用技巧

HyperCeiler 的功能繁多，按类别梳理一遍能帮你快速找到自己关心的部分。

### 机型伪装：让系统以为你在用旗舰机

HyperOS 根据设备型号分层开放内容——高端机型能看到更多主题、更多应用推荐、更多高级设置选项，而中低端机型被有意隐藏了一部分。HyperCeiler 在应用商店和小米社区两个应用里 Hook 了 `Build.PROP` 读取逻辑：当这些应用查询设备型号时，它返回一个"小米 14 Ultra"或"小米 15 Ultra"这样的高端机型值，让商店和社区误以为你在用旗舰机。

开启后你会观察到：应用商店推送了更多精选和独家合作 App，某些只对特定机型开放的应用现在对你可见，主题商店里被隐藏的高端专属主题也能看到了。

### 状态栏、锁屏与通知控制中心

这是 HyperCeiler 用户最常改的部分。你可以在状态栏里加秒数显示、改时间格式、自定义通知图标、隐藏敏感信息；锁屏可以改布局、加快捷入口、调整字体大小；通知控制中心支持自定义开关排序、透明度、深浅色模式等。这些改动大部分作用域是系统界面（`com.android.systemui`），改完重启该进程即可看到效果。

### 桌面与图标

桌面图标布局调整、图标形状自定义、文件夹样式修改、负一屏开关、小部件增强——这些功能的作用域通常是系统桌面（`com.miui.home`）。

### 相机与相册增强

相机解锁限制去除、水印位置和内容自定义、相册里的 PDF 导出、证件照模式、额外的滤镜效果——这些功能分布在相机（`com.android.camera`）和相册（`com.miui.gallery`）、相册编辑（`com.miui.mediaeditor`）几个作用域里。

### 系统框架层的深度开关

这部分功能最"硬核"，作用域需要勾选系统框架（`system`），部分还需要电量与性能（`com.miui.powerkeeper`）配合：

**禁用温控**：让手机在高负载场景下不降频，持续保持高性能输出。配合散热背夹使用效果最佳，但长期 45°C 以上运行会加速电池老化，建议只在短时间高强度任务（跑 benchmark、录 4K 视频、渲染视频）时开启，用完及时关掉。

**允许 Google Play 服务在任何时间联网**：HyperOS 默认在省电模式或后台限制模式下会阻止 Google Play 服务联网，被阻止后会出现应用更新不推送、GMS Push 通知延迟或丢失、依赖 Google 服务的应用报网络错误等问题。开启这个开关后 `com.google.android.gms` 保持联网能力，但其他应用的后台联网限制不受影响。

**禁止自动杀后台 + 让 Millet 更保守**：这两个开关配合使用效果最佳——前者禁止系统自动清理后台应用，后者让 Millet 调度器配合减少误杀。三者叠加（再加上面的 Google Play 服务联网）会让重度使用场景下的后台稳定性显著提升。

| 开关 | 解决的问题 | 作用域 |
| --- | --- | --- |
| 禁止自动杀后台 | 后台应用不被系统清理 | system |
| 让 Millet 更保守 | Millet 调度器减少误杀 | system |
| 允许 Google Play 服务联网 | 后台 Push、更新、定位不断联 | system + com.miui.powerkeeper |

### 模块不生效怎么办

如果你按照前面的步骤都配置好了，但某个功能开了还是没反应，按以下清单逐条核对：

**第一类：LSPosed 和模块基础问题**。打开 LSPosed App 看首页是否显示"已激活"；进入模块页面确认 HyperCeiler 的右上角开关已打开；检查 LSPosed 版本是否兼容你的 Android 版本；确认 HyperCeiler 的 OS2/OS3 分支与你的 HyperOS 版本匹配。

**第二类：作用域问题（最常见的原因）**。对照你想用的功能，检查 LSPosed 里的作用域有没有漏勾——想用相机功能但没勾相机包名肯定不生效；拿不准就用 LSPosed 推荐的默认作用域清单；确保系统框架（system）已勾选，没有它几乎所有系统级 Hook 都不生效。

**第三类：重启问题**。改完功能开关后必须重启对应的作用域进程，光改开关不重启等于进程还在跑旧代码；如果同时改了多个作用域，直接整机重启最稳妥。

**第四类：系统兼容问题**。第三方 ROM（crDroid、LineageOS 等）没有 HyperOS 的组件，Hook 目标找不到；国际版 HyperOS 的系统应用包名与国内版不同，同样 Hook 不到；HyperOS 小版本更新后内部方法签名可能变化，等待 HyperCeiler 出新版本适配即可。

> [!NOTE]
> 检查完以上四类还找不到原因？去 GitHub Issues 提 bug，带上机型、HyperOS 版本、Android 版本、HyperCeiler 版本号、OS2 还是 OS3 分支这些必要信息，并在 HyperCeiler 设置里把日志等级调到 Warn 后附带崩溃日志。不要重复提交已有相同问题。

---

## 🔗 相关资源

- [hyperceiler官网](https://hyperceiler.hfdk.space)
- [hyperceiler文档](https://hyperceiler.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/hyperceiler)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/hyperceiler)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/hyperceiler)

## 📚 相关文章

- [我把 HyperCeiler 嵌进了每周照片整理 + Termux 脚本链，少了至少三次手动介入](https://laoguosofts.github.io/posts/hyperceiler.html)
- [给家里老人配小米手机，我用 HyperCeiler 把半小时活压到五分钟](https://alipymanbu.github.io/posts/hyperceiler.html)
