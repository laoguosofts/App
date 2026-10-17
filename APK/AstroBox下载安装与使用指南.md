## ✨ AstroBox 是什么

AstroBox 是一款面向智能穿戴设备的第三方管理工具，它把小米、REDMI、vivo 等品牌手表手环的表盘、快应用、固件刷机等功能集中到一个跨平台应用里。不同于各家厂商自带的运动健康 APP，AstroBox 强调的是社区生态——内置官方社区资源中心，用户可以浏览和安装创作者分享的表盘主题、快应用插件，甚至刷写自定义固件。

AstroBox 目前支持 Windows、macOS、Linux、Android、iOS 等多个平台，其插件系统基于 WebAssembly 技术构建，采用 WASI Preview 3 接口和 wasmtime 运行时。这个技术选型让插件具备跨平台、安全沙箱、多语言开发等优势，一份 .wasm 插件可以在不同操作系统上直接运行，而且默认无法访问文件系统和网络，降低了装恶意插件的风险。

在设备兼容方面，AstroBox 完整支持小米手环 9/9 Pro/10 系列、小米 Watch S3/S4 系列、REDMI Watch 5/6 系列，vivo Watch GT 系列处于实验性支持状态。小米手环 8 及更老系列、REDMI 手环/Active 系列由于蓝牙协议版本较旧，暂不支持。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://astrobox.fedq.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：按软件实际平台写（Google Play / App Store 搜软件名；PC 软件写官网分发渠道）

---

## 🔧 安装步骤

AstroBox 的安装过程因平台而异，但核心流程相似。下面分别说明各平台的安装要点和注意事项。

### Windows 平台

从下载页获取最新版本的 Windows 安装包后，双击运行安装程序。默认安装路径为系统盘的 Program Files 目录，建议保持默认设置以确保插件和数据目录正常工作。如果安装过程中被 Windows Defender 或第三方杀毒软件拦截（这在第三方工具中较为常见），请在杀毒软件中放行 AstroBox 的安装程序，或暂时关闭实时防护后再试。

安装完成后，首次启动时会弹出设备配置引导，按提示完成初始设置即可。

### macOS 平台

macOS 用户从下载页获取 .dmg 镜像文件，双击打开后将 AstroBox 图标拖拽到 Applications 文件夹。由于是第三方应用，首次启动时可能被 Gatekeeper 拦截，此时需要在「系统设置 → 隐私与安全性」中手动点击「仍要打开」。

macOS 同时支持 Intel 和 Apple Silicon (M 系列) 架构，下载时请根据自己的芯片选择对应版本。

### Android 平台

Android 版以 APK 形式分发，安装前需要在系统设置中开启「未知来源」应用的安装权限：

1. 打开手机「设置」应用
2. 进入「安全」或「应用权限」相关菜单
3. 找到「未知来源」或「安装未知应用」选项
4. 授予下载 AstroBox 的应用以安装权限

完成后，直接打开下载的 APK 文件即可安装。

### iOS 平台

iOS 版的安装相对特殊——由于 App Store 的审核限制，AstroBox 不会直接上架 App Store，而是通过侧载（Sideloading）方式分发。常用的侧载工具有 AltStore、Sideloadly 等，需要用电脑配合完成。具体的侧载步骤可参考官方文档中的 iOS 安装指南。

> [!WARNING]
> iOS 上使用 AstroBox 连接 REDMI Watch 5 或小米 Watch S3 时，如果需要安装快应用，必须先进行「安卓伪装」操作。这是因为快应用最初为安卓环境设计，iOS 上的蓝牙连接协议略有差异。跳过这一步直接安装快应用可能导致失败。

### 安装后的首次验证

安装完成后，建议做以下检查确保版本正确：

1. 打开 AstroBox，在「设置 → 关于」中确认版本号与下载页一致
2. 如果是多平台用户，确认桌面端和移动端使用的是同一大版本（主版本号相同即可）
3. 首次连接穿戴设备后，在设备详情页确认同步状态正常

---

## 📖 AstroBox 使用入门

### 设备连接

打开 AstroBox 主界面后，首先需要连接你的穿戴设备。目前支持三种连接方式：

- **小米账号一键同步**（小米/REDMI 设备）：登录你的小米账号，AstroBox 会自动拉取你账号下已绑定的设备列表，选择要管理的设备即可完成连接。这是最便捷的方式，无需额外配置。
- **Authkey 手动连接**（小米/REDMI 设备）：如果不想登录账号，可以从小米运动健康 APP 中提取 Authkey，手动填入 AstroBox 完成连接。这个方法不走小米服务器，适合注重隐私的用户。
- **OpenID 手动连接**（vivo 设备）：vivo 设备需要从 vivo 运动健康 APP 获取 OpenID 进行手动连接。

> [!NOTE]
> iOS 用户切换到 AstroBox 连接之前，如果之前用小米运动健康或 vivo 运动健康 APP 连过同一台设备，很容易触发 TargetRejected 错误——意思是「目标设备拒绝了新的配对请求」。解决方法是：先在官方 APP 里断开设备，然后在 iOS 蓝牙设置里「忽略此设备」，最后在穿戴设备上清除所有已配对记录，再回到 AstroBox 重新连接。

### 从官方社区下载资源

设备连接成功后，就可以从 AstroBox 内置的官方社区源下载表盘、快应用和固件了。这是 AstroBox 的核心功能之一。

打开 AstroBox 主界面，默认进入**探索页**。这里展示的是官方精选的热门作品，有可滑动的预览图卡片。点击顶部或底部的「全部资源」可以进入完整列表，支持按类型筛选和关键词搜索。2.1 版本新增了悬浮搜索框和创作者自定义栏目，搜索时会展示对应插件图标，定位更快。

在资源详情页可以看到：资源截图和简介、适配设备型号、下载量和用户评分、关联的多个资源、创作者信息。确认适配你的设备后，点击下载按钮，AstroBox 会自动检查兼容性、下载资源文件、推送到穿戴设备并触发安装。

> [!TIP]
> 如果点击下载后提示「资源未适配你的设备」，不要直接放弃。先在资源详情页的版本选择器里切换试试——创作者有时会为不同型号做适配版本，默认显示的可能不是你的设备那一版。如果所有版本都不适配，可以在评论区询问创作者有没有适配计划，或者暂时等待更新。

付费资源需要先在 AstroBox 账号中绑定爱发电账号，完成付费购买后重新登录刷新权益，即可解锁下载权限。

### 插件系统与 WASI 运行时

AstroBox 的插件系统是其技术亮点之一。它不像传统软件那样使用原生动态库（DLL/.so）或嵌入脚本语言，而是选择了 WebAssembly 作为插件载体。

为什么选 WebAssembly？主要有四个原因：

1. **跨平台**：一份 .wasm 插件可以同时在 Windows、macOS（Intel 和 Apple Silicon）、Linux、Android、iOS 上运行，不需要为每个平台分别编译。这对 AstroBox 的多平台定位至关重要。
2. **安全沙箱**：WebAssembly 默认无法访问文件系统、网络和系统 API，必须由 AstroBox 通过 WASI 接口显式授权。恶意插件在沙箱里最多搞坏自己，无法逃逸到操作系统层面。
3. **多语言支持**：任何能编译成 WebAssembly 的语言都能写 AstroBox 插件。Rust 是官方推荐的首选，性能最好；Level 4 版本新增了对 Python 和 JavaScript 的官方支持；C/C++/Go/Swift/Kotlin 等也可以间接支持。
4. **性能**：WebAssembly 性能接近原生，远好于嵌入式脚本语言的解释执行。wasmtime 作为 AOT 编译运行时，启动速度快、执行效率高。

AstroBox 的插件系统目前有两层运行时共存：旧版 Level 1/2/3 继续使用旧版 wasmtime 和旧版 WASI 接口，为老插件提供向后兼容；新版 Level 4 则使用 WASI Preview 3，新增了 HTTP 服务器和应用内浏览器能力，宿主接口全部改成了 async func。两套运行时独立运行，互不干扰。

对于普通用户来说，虽然不会直接和 WASI 或 wasmtime 打交道，但这个技术选型直接影响到你能用到的插件数量（多语言意味着更多开发者能参与）、插件安全性（沙箱隔离降低风险）和性能表现（插件运行流畅不卡顿）。

### iOS TargetRejected 错误排查

这是 iOS 用户最常遇到的问题之一，集中记录一下完整的解决路径：

**根本原因**：iOS 蓝牙不会让一个设备同时和多个 iOS 设备配对。如果穿戴设备之前被小米运动健康或 vivo 运动健康 APP 连过，iOS 里已有配对记录，此时 AstroBox 发起的新配对请求就会被设备拒绝。

**标准解决流程**：

1. 在 iOS 系统蓝牙设置里「忽略此设备」——这是最有效的一步，打开「设置 → 蓝牙 → 找到穿戴设备 → 点击右边 ℹ️ 图标 → 忽略此设备」
2. 在穿戴设备上清除所有已配对记录——打开设备设置，找到蓝牙相关设置或「我的设备」，清除后让设备重新进入「连接新手机」模式
3. 回到 AstroBox，按正常流程重新连接

如果以上步骤还不行，可以尝试：

- **重启一切**：关闭 AstroBox、关闭 iOS 蓝牙几秒再打开、重启穿戴设备，全部重新来过
- **换手动连接方式**：跳过小米账号同步，试试 Authkey 手动连接或 OpenID 手动连接，手动连接不走厂商账号登录流程，有时能绕过 iOS 的配对限制
- **先在官方 APP 里断开**：打开小米运动健康或 vivo 运动健康，在设备详情页找到「断开连接」或「清除设备」，确认完全断开后再回到 AstroBox
- **换一台 iOS 设备测试**：如果别的 iOS 设备能连上，说明你的 iOS 有特殊的蓝牙配置问题

**防止下次再出现**：以后从官方 APP 切换到 AstroBox，按「官方 APP 断开 → iOS 蓝牙忽略 → 穿戴设备清除配对 → AstroBox 重新连接」这个顺序操作，就不会再撞 TargetRejected 了。

---

## 🔗 相关资源

- [AstroBox官网](https://astrobox.fedq.quest)
- [AstroBox文档](https://astrobox-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/AstroBox)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/AstroBox)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/AstroBox)

---

## 📚 相关文章

- [深夜折腾 vivo 手表表盘到三点——我是怎么被 AstroBox 救了的](https://laoguosofts.github.io/posts/astrobox-docs.html)
- [用 iPhone 连 REDMI Watch 装快应用？我差点把手表退了](https://alipymanbu.github.io/posts/astrobox-docs.html)