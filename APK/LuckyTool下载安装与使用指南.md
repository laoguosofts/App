## ✨ LuckyTool 是什么

LuckyTool 是一款面向 ColorOS 系统的 Xposed Hook 模块，基于 YukiHookAPI 框架实现，作用域覆盖 SystemFramework、SystemUI、Launcher、GameAssistant 等 16 类系统进程。它的核心定位是补全 ColorOS 原生设置里做不到或做不好的改造——状态栏时钟显秒、控制中心磁贴长按跳设置、移除安装弹窗广告、游戏助手去掉 VIP 限制……这些零散却高频的体验痛点，在 LuckyTool 里集中成一组组开关，开了就生效。

模块只适配 ColorOS 12 及以上系统，同源的 realme UI 和 OxygenOS 有社区自测可用的部分，但不在官方适配口径内。ColorOS 11 及以下依赖的 OplusFramework API 在 LuckyTool 里不存在，装上也是空壳。

LuckyTool 对 Root 方案没有偏好——Magisk 用户需要确保 Zygisk 已开启，KernelSU 用户需要在超级用户列表里预先勾选模块。没有 Root 授权的话，模块界面能打开但写不了配置等于白装。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://luckytool.fewf.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜 LuckyTool

---

## 🔧 安装步骤：三大硬前提 + 模块挂载

### 前提一：设备已 Root

Xposed 从 Android 5.x 起就必须有 Root 权限才能向系统进程注入 Hook。ColorOS 设备常用两种 Root 方案，LuckyTool 都能适配：

- **Magisk**：最主流的 Root 方案，需在设置页开启 Zygisk（Zygisk 是 LSPosed 在 Android 12+ 上工作的必要条件）
- **KernelSU**：另一个流行的 Root 方案，需要在超级用户列表里预先勾选 LuckyTool

> [!WARNING]
> LuckyTool 首次打开时会弹出 Root 请求，**一定要允许**——没有 Root 授权的话模块界面是个空壳，写不了任何配置。

### 前提二：已安装 LSPosed 框架

LSPosed 是 Xposed 框架在 Android 8+ 上的现代实现。没有它，Xposed 模块根本无处加载。传统 Xposed（rovo89 版）在 Android 8 之后停止更新，LSPosed 是完全重写的版本，基于 Zygisk，启用/禁用模块不需要重启系统（实际生效仍需重启作用域）。

确认 LSPosed 正常工作的方法：打开 LSPosed 管理器，首页能看到已挂载的作用域应用（比如 SystemUI）就说明框架正常；如果显示「框架未挂载」，先检查 Magisk 的 Zygisk 开关和 LSPosed 模块是否在 Magisk 里勾选了。

### 前提三：ColorOS 12 及以上系统

LuckyTool 的 Hook 目标是 ColorOS 特有的系统组件——OplusFramework、SystemUI、Launcher、GameAssistant 等，这些组件在不同版本的 ColorOS 上实现不同。系统版本不够的话，装上模块也找不到要 Hook 的目标，全部功能静默无效。

### 模块安装与作用域勾选

三大前提都就绪后，安装 LuckyTool APK，然后进 LSPosed 管理器给 LuckyTool 勾选作用域。作用域是 LuckyTool 最容易踩坑的环节，三条常见误区：

1. **只勾了 SystemUI 就想开游戏助手开关**——没用，GameAssistant 是独立进程，必须单独勾选
2. **嫌麻烦不勾推荐作用域，只勾了几个**——模块启用了但功能不生效，绝大部分是作用域漏勾导致
3. **勾完作用域就直接开开关**——勾完需要重启作用域才能让 LSPosed 把 Hook 注入进去，否则模块界面能打开但开关不生效

> [!TIP]
> LuckyTool 首次使用时，LSPosed 里建议勾选的作用域包括 SystemFramework、SystemUI、Launcher、GameAssistant，再根据你用到的功能补勾截图、相机、设置、电池、主题商店等。

---

## 📖 使用入门

### Hook 框架与作用域覆盖

LuckyTool 基于 YukiHookAPI 实现，相比传统 Xposed API 对 Android 12+ 兼容性更好，支持 Kotlin 协程和更灵活的作用域配置。16 类作用域里最核心的四个：

| 作用域 | 功能密度 | 典型能力 |
| --- | --- | --- |
| SystemFramework | 底层包管理器 | 允许降级安装、禁用签名验证 |
| SystemUI | 最高密度 | 状态栏时钟、网速、通知、控制中心、锁屏、息屏 |
| Launcher | 桌面启动器 | App 徽章、分页组件、文件夹布局、最近任务 |
| GameAssistant | 游戏助手 | 移除游戏滤镜根限制、启用 GT/X-Mode、移除 VIP 限制 |

其余作用域各对应 1~3 个功能开关，分布在截屏、相机、设置、电池、主题商店、云服务、系统更新、杂项等分区里。

### 状态栏与控制中心改造

LuckyTool 在 SystemUI 里 Hook 了状态栏、锁屏、下拉通知栏和控制中心，功能覆盖密度最高：

**状态栏时钟**：预设格式、自定义格式、农历、双排、字体——想把状态栏时钟玩出花的用户基本都集中在这个分区。

**状态栏网速**：每秒刷新、双排显示、移除单位，适合追求简洁的用户。

**控制中心磁贴长按跳转**是一个典型的小改动大提升功能。ColorOS 原生里长按 WiFi、蓝牙、移动数据等快捷开关也是切换开/关，想进完整设置页只能点旁边的小箭头，非常反直觉。LuckyTool 让每个磁贴的长按都直接跳对应的设置页：WiFi 跳 WiFi 设置、移动数据跳移动数据设置、截屏跳截屏设置、勿扰模式跳勿扰设置，建议全开统一行为形成肌肉记忆。

> [!NOTE]
> 第三方 APP 磁贴和 LuckyTool 自己添加的内置磁贴，长按行为由各自决定，LuckyTool 控制不了。开关开了没反应先确认 LSPosed 里 SystemUI 作用域勾选了，然后重启作用域。

LuckyTool 还能向控制中心添加内置磁贴——显示刷新率、全局 DC、触控采样率、5G 开关等，都是 ColorOS 原生没有但高频需要的快捷入口。

### 应用安装拦截

ColorOS 官方安装器在安装某些来源的 APK 时会弹出全屏或弹窗式的推荐广告，不仅碍事还经常遮挡安装按钮。LuckyTool 在「应用相关 → 应用安装相关」子分区里提供四组开关：

| 开关 | 效果 |
| --- | --- |
| 移除安装广告 | 弹窗式推荐/广告直接消失 |
| 安装时显示包名和版本号 | 安装界面显示 APK 的包名和版本号（调试用） |
| 自动点击安装按钮 | 安装确认界面自动点「安装」 |
| 自动点击卸载按钮 | 卸载确认界面自动点「卸载」 |

移除广告是刚需，装上直接生效。自动点击两个开关则是双刃剑——装好后从打开 APK 到安装完成全程不需要碰屏幕，批量安装 APK 或刷机后快速恢复常用 APP 时特别省心；但也意味着没有「最后确认」环节，误点卸载一个 APP 就直接卸了。

> [!WARNING]
> SystemFramework 相关的功能只 Hook 系统层面的包管理器。如果使用 ColorOS 官方安装器，某些功能需要同时开启「应用相关 → 应用安装相关」里的对应开关——因为官方安装器会做额外的检测和限制。

### 游戏助手解限

OPlus 游戏助手在 ColorOS 里负责游戏空间、游戏模式、性能调度，LuckyTool 能做的改造包括：移除游戏滤镜根限制、移除开机动画和福利页、启用开发者页面、自定义媒体播放支持、启用 X-Mode / GT 模式 / 一加特色、启用 Adreno GPU 控制器、启用赛事模式、移除赛事音效、移除 VIP 限制、移除弹幕通知白名单。这些都是 ColorOS 原生加了锁但硬件本身支持的功能，解限后能释放不少性能潜力。

---

## 🔗 相关资源

- [LuckyTool官网](https://luckytool.fewf.ren)
- [LuckyTool文档](https://luckytool-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/LuckyTool)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/LuckyTool)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/LuckyTool)

## 📚 相关文章

- [LuckyTool 深度盘点：ColorOS 上最能干的 Hook 模块到底能改多少东西](https://laoguosofts.github.io/posts/luckytool-docs.html)
- [LuckyTool 全能力盘点：ColorOS 原生能改的它都能改，原生不能改的它也试着帮你改](https://alipymanbu.github.io/posts/luckytool-docs.html)
