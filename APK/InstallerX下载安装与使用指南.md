## ✨ InstallerX Revived 是什么

InstallerX Revived 是一个面向 Android 设备的现代化 APK 安装器，定位是系统默认包管理器的全面替代品。它解决的核心痛点很明确：原生系统安装器只认标准 APK，遇到 APKS、XAPK、ZIP 里藏 APK 就手足无措；每次安装都要点好几下确认；想批量更新应用更是折磨。

在功能层面，InstallerX Revived 覆盖了格式兼容、安装流程、权限授权、安全控制四大维度的完整能力。格式上支持 APK、APKS、APKM、XAPK 以及 ZIP 压缩包内直接包含 APK 的场景，甚至可以处理 Magisk、KernelSU 等模块 ZIP，省去手动解压的麻烦。安装流程提供了五种模式——对话框、对话框自动、通知栏、通知栏自动、静默，从"我要看清楚再装"到"全自动无人值守"任你选。

授权体系是 InstallerX Revived 的灵魂，支持 Shizuku、Root、Dhizuku、无特权四种授权器，覆盖了从普通玩家到深度折腾党的全部场景。智能授权开启后，主授权器不可用时会自动按顺序切换后备方案，不会因为 Shizuku server 被杀就罢工。

界面设计上采用 Material 3 Expressive，支持动态取色、深色模式、小米超级岛通知，Android 16+ 还能用实时活动在锁屏上展示安装进度。总的来说，如果你经常手动装 APK、刷模块包，或者受够了系统安装器的单一弹窗体验，InstallerX Revived 值得一试。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://installerx.fedq.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play / 各品牌应用商店搜索 InstallerX Revived；部分地区可能仅限官网分发

---

## 🔧 安装与授权配置

### 第一步：安装 APK

InstallerX Revived 的标准 APK 可以直接用系统安装器打开。安装前请确认以下两点：

> [!TIP]
> **核对版本号与来源**：建议从官网或对应文档站下载，第三方镜像可能存在版本滞后或签名差异。

> [!WARNING]
> **未知来源授权**：Android 12+ 在首次安装来自非应用商店的 APK 时，会弹出"此应用可能未知"的风险提示，点击"设置 → 允许此来源"后重新打开安装包即可。

### 第二步：选择授权器

这是 InstallerX Revived 与普通安装器的核心区别。打开应用主页，进入「授权 / 特权状态」，从四种授权器中选一种作为主力：

| 授权器 | 能力 | 适合谁 |
|---|---|---|
| **Shizuku** | 高——shell 级或 root 级（取决于 Shizuku 激活方式） | 大多数普通玩家，不想刷 Root 但想要特权 |
| **Root** | 最高——全部特权操作，包括替换系统包管理器 | 已刷 Magisk/KernelSU/APatch 的深度折腾党 |
| **Dhizuku** | 中等——可锁定安装器、安装应用，但不支持自定义安装者/DexOpt | 无 Root、无 Shizuku 的设备（走 ADB 激活） |
| **无特权** | 最低——完全受系统限制，只能走标准安装会话 | 不折腾，仅用来替代系统安装器的界面 |

> [!NOTE]
> **智能授权建议开启**：启用后主授权器不可用时自动切换后备，例如默认用 Shizuku（更快），被杀时自动降级到 Root（更稳定）。

### Root 授权三步曲（Magisk / KernelSU / APatch）

如果你已经刷入 Root，在 InstallerX 里选「Root」后：

1. 手机会弹出超级用户请求对话框（Magisk / KernelSU / APatch 各自的样式）
2. 点击「允许 / 授予」，建议勾选「始终允许」避免每次弹窗
3. 回到 InstallerX 主页，授权状态卡应显示「Root 已就绪」

KernelSU v3.0+ 用户额外注意：安装系统级模块（包括替换系统包管理器）前，先刷入 **元模块（MetaModule）**，并关闭 KernelSU 的「Umount modules by default」选项——这个开关会在启动时卸载所有模块，与系统包管理器替换逻辑冲突。

### Shizuku 授权（推荐大多数人用）

Shizuku 的激活方式有两种：USB 调试配对（每次重启后失效但不需要电脑常驻）和无线调试配对（Android 11+ 支持，重启后需要重新配对）。InstallerX Revived 主页会引导你完成配对流程，按照提示在手机的「开发者选项 → 无线调试」里获取配对码和 IP 即可。

---

## 📖 核心功能使用入门

### 格式兼容：一次选齐全部 APK

InstallerX Revived 的一大卖点是格式通吃。除了标准 APK，以下几种场景也能直接打开安装：

- **APKS / APKM / XAPK**：拆分 APK 的容器格式，InstallerX 会自动识别并匹配最佳分包组合（不同架构、不同分辨率的 APK 智能配对）
- **ZIP 压缩包内含 APK**：比如 Magisk 模块 ZIP，直接用 InstallerX 打开就能刷写，无需手动解压
- **批量 APK**：一次多选多个 APK，同包名自动去重选最新版本
- **在线直链安装**：网络分享的 APK 直链可以直接丢给 InstallerX，支持完整下载、智能判断、低存储三种策略

> [!TIP]
> **流式安装的验签限制**：在线直链安装默认走"流式"模式不下载完整文件，因此无法完整验签。对安全性敏感的应用建议选「完整下载」策略再安装。

### 安装模式：从"看清楚再装"到"全自动"

五种安装模式覆盖了不同场景：

| 模式 | 交互方式 | 典型场景 |
|---|---|---|
| 对话框安装 | 弹应用详情，手动点"安装" | 首次装某应用，要看权限列表 |
| 对话框自动 | 弹对话框后自动开始装 | 想看详情但不想多点一下 |
| 通知栏安装 | 后台装，通知进度条，需手动确认 | 装大型应用，想继续刷微信 |
| 通知栏自动 | 后台装，通知显示，尽量少手动 | 批量更新 |
| 静默安装 | 后台装，无通知无确认（需特权） | 自动更新任务、批量合集 |

安装后想自动删除 APK 文件？在配置文件里勾上「安装后自动删除 APK + ZIP」即可，省得每次手动清理下载目录。

### 配置文件（Profiles）：让不同来源的安装走不同路径

配置文件是 InstallerX Revived 的精髓所在。它解决的问题是：酷安下载的 APK 想静默装，文件管理器里选的想弹对话框，浏览器下的想自动删除——这些差异化需求全部通过配置文件实现。

每个配置文件可以绑定一个**作用域**（比如包名匹配酷安），然后指定：使用哪个授权器、安装模式、安装者声明、是否启用 Dex2oat 优化、签名拦截策略等。全局默认配置覆盖所有未被特殊指定的场景。

### 卸载器：比系统卸载多做两件事

InstallerX Revived 的卸载功能走的是默认配置的授权器（和安装器可以不同），在「设置 → 卸载器设置」里可以开启几个实用选项：

- **保留数据卸载**（默认关闭）：卸载后保留应用内部数据目录，下次重装同一应用可以恢复。注意部分 ROM 会在重装时触发数据清理，这不是 InstallerX 的问题
- **为所有用户卸载**：多用户设备（工作资料 + 私人资料）上同时卸载所有用户里的该应用
- **手动调用卸载器**：直接输入包名就打开卸载流程，不用先翻应用列表

卸载常见失败场景和解决方法：

| 错误提示 | 原因 | 解决 |
|---|---|---|
| 设备策略 / 所有者禁用 | 工作资料、策略管理器限制卸载 | 移除工作资料或联系设备所有者 |
| HyperOS 不允许 | HyperOS 阻止了某些系统应用的卸载路径 | 尝试换 Root 授权器，或恢复出厂设置 |
| Dhizuku 权限不够 | Dhizuku 无法执行某些卸载 | 换 Root 或 Shizuku |

### HyperOS 特别注意

如果你用的是小米 HyperOS，有几个坑值得提前知道：

**安装者声明自动配置**：HyperOS 上安装或更新系统应用时，系统会检查安装者包名是否为有效系统应用。InstallerX Revived 启动时会自动把安装者声明设为 `com.android.shell`（Shell 进程），普通用户不用手动改。如果这个包名在你的 HyperOS 版本上不生效，可以在配置文件里试其他系统级包名如 `com.android.fileexplorer`（小米文件管理器）或 `com.android.vending`（Google Play）。

**USB 安装拦截会导致锁定失效**：HyperOS 会拦截 ADB/Shizuku 发起的 USB 安装请求。如果你点了"拒绝"，系统会撤销 InstallerX 的默认安装器身份，强制切回系统默认。恢复方法：主页 → 默认安装器状态卡 → 重新手动锁定。想根治就装 InxLocker Xposed 模块，它直接 Hook 层拦截安装 intent 转发给 InstallerX，绕过 HyperOS 的拦截对话框。

**OTA 更新后检查安装器状态**：如果你把 InstallerX 刷成了系统包管理器，OTA 更新可能会被系统恢复原包管理器——OTA 包里写死了原包管理器的包名和路径。更新后检查一下，被恢复了就重新刷一次模块。

### 安全控制总览

InstallerX Revived 在安全层面做了全方位覆盖：

- **签名校验**：安装前验证 APK 与已安装版本的签名是否匹配，防止"同名不同包"的替换攻击
- **应用黑名单**：按包名或 SharedUID 禁止安装，可设置排除包名临时放行
- **权限预览**：安装对话框完整展示应用申请的所有权限
- **HTTP 安全策略**：三档可选——仅 HTTPS / 允许本地明文 / 允许所有
- **生物识别认证**：安装前、卸载前可分别强制启用，防止误操作或设备被盗时被恶意安装

---

## 🔗 相关资源

- [InstallerX官网](https://installerx.fedq.ren)
- [InstallerX文档](https://installerx.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/InstallerX)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/InstallerX)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/InstallerX)

## 📚 相关文章

- [InstallerX 对比评测：为什么我换掉了用了三年的 SAI](https://laoguosofts.github.io/posts/installerx.html)
- [InstallerX Revived：当一个 APK 安装器开始接管系统级权限](https://alipymanbu.github.io/posts/installerx.html)
