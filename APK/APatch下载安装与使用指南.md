## ✨ APatch 是什么

APatch 是一个 Android 内核级 root 解决方案，和 Magisk、KernelSU 属于同一赛道的工具。它的核心思路是不修改系统分区、只对 boot 分区打补丁，从而在保留 dm-verity 校验完整的前提下获得 root 能力。

和传统 su 守护进程模式不同，APatch 在内核里实现了一套叫 SuperCall 的系统调用。App 请求 root 时，必须在内核态通过 SuperCall 验证正确的 SuperKey 才能拿到权限，整个过程用户空间不可见。这种设计把攻击面从 su 守护进程缩小到内核，安全模型更干净。

除了 root 本身，APatch 还自带一套 APM 模块系统（AndroidPatch Module），实现 systemless 修改——不碰 /system、/vendor 等只读分区，而是通过 overlayfs 在启动时把模块文件"叠加"到原始分区上面。绝大多数 Magisk 模块可以直接在 APatch 上安装使用。

适合谁用？想给 Android 设备 root、又在意系统 OTA 升级兼容性和安全模型的玩家；从 Magisk 迁移过来想试试新方案的用户；需要 systemless 修改能力的折腾党。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://apatch.djnf.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 平台请前往 APatch 管理器对应的 GitHub Releases 获取最新 APK 构建包

---

## 🔧 安装前的准备与注意事项

在动手刷入之前，有几个前置条件和风险点需要明确。

### 解锁 Bootloader

Android root 的前提是 Bootloader 已解锁。各品牌解锁方式不同：小米在开发者选项里申请解锁权限、三星通常需要 Odin、Pixel 系列在 fastboot 模式下直接解锁。解锁会触发工厂数据清空，务必提前备份全部数据。

### 提取原厂 boot.img

这一点**绝对不能跳过**。APatch 的安装本质是用 KernelPatch 工具对你设备当前系统版本的 boot.img 打补丁，然后刷回 boot 分区。你必须有一份和当前系统版本严格匹配的原厂 boot.img——从官方刷机包里解包提取，或从社区的固件分享帖里下载。

> [!WARNING]
> **boot.img 版本必须和当前运行的系统完全一致。** 用了不匹配版本的 boot.img 打补丁，轻则内核 panic 卡第一屏，重则设备无法开机。如果不确定版本号，请在设置里核对系统版本后再去找对应的刷机包。

### 两种安装路径

APatch 有两条主流安装路径，各有取舍：

| 方式 | 适合场景 | 优点 | 缺点 |
| --- | --- | --- | --- |
| 管理器修补 + fastboot 刷入 | 有电脑 + USB 线的标准流程 | 可自定义 SuperKey、流程可控、排障方便 | 需要重启进 bootloader、依赖电脑 |
| REC 直接刷入 | 无电脑 / 不方便插线 | 一步完成、全程在 REC 里 | 不能自定义 SuperKey、依赖第三方 REC |

### 推荐流程（管理器修补 + fastboot）

1. **下载管理器 APK**：去对应渠道拿最新稳定版 APatch 管理器，在手机上安装（允许未知来源）
2. **修补 boot.img**：打开管理器，选择刚才准备好的原厂 boot.img，设置 SuperKey（强密码，8-63 位，数字+字母），等待修补完成，生成的 patched_boot.img 会保存在手机存储
3. **重启到 bootloader**：通过 `adb reboot bootloader` 或按键组合进入 fastboot 界面
4. **刷入修补后的 boot**：`fastboot flash boot patched_boot.img`，等待 OKAY 输出
5. **重启系统**：`fastboot reboot`，进系统后打开管理器验证 root 是否生效

> [!TIP]
> **SuperKey 设成什么？** 它的权限高于 root——拿到密钥的程序都能获取 root。禁止用 `12345678`、`password` 这类弱密码。新版管理器已经强制要求强密码格式，别偷懒。

---

## 📖 使用入门

### SuperKey：root 权限的门禁

SuperKey 是 APatch 区别于其他 root 方案的核心设计。传统 Magisk 用 su 守护进程模式——App 请求 root → su 守护进程检查白名单 → fork 子进程授权——整个流程在用户空间完成，su 守护进程本身就是 root 权限，任何能绕过授权检查的恶意 App 都可能被它放行。

APatch 把这个流程搬进了内核。内核里有个新系统调用叫 SuperCall，App 必须传入正确的 SuperKey 才能让内核在当前线程上直接授予 root。验证失败时内核静默返回，调用方完全感知不到"这里有个 root 权限门"。这意味着：

- 没有 su 守护进程的攻击面
- root 的存在对外是完全隐藏的，检测 App 探测 SuperCall 会被静默忽略
- SuperKey 本身就是钥匙——管理器里维护的 App 白名单，本质上就是"哪些 App 被允许使用这把钥匙"

> [!NOTE]
> **如果忘了 SuperKey 怎么办？** 从原厂 boot.img 重新修补一次就行，修补时会要求你重新设置。

### REC 直接刷入：v10888 引入的一键安装

从版本 `10888` 开始，APatch 支持在 Recovery 里直接刷入——不再需要手机上管理器修补 + bootloader 刷入这两步。整个安装可以在 TWRP 等第三方 REC 里完成，适合没有电脑或不方便插线的场景。

操作流程很简单：

1. 下载 APatch 管理器 APK
2. 把 `.apk` 后缀直接改成 `.zip`（**不要解压、不要改内容**，单纯重命名）
3. 确保原厂 boot.img 已经在手机存储里（通常放在 `/sdcard/boot.img`）
4. 重启进第三方 Recovery（关机后长按音量上 + 电源键）
5. 选择"安装"，找到改了后缀的 zip，滑动确认刷入
6. 刷完重启系统

刷入脚本会自动在 REC 环境里解包 APK、读取 boot.img、用 KernelPatch 修补、然后刷入修补后的 boot 分区。

> [!WARNING]
> **REC 直接刷入的超级密钥是随机生成的**——你没法在刷入前指定自己的密钥。刷完开机后，打开管理器会看到它生成的随机密钥，此时需要点击它重新修补一次 boot.img，这次才能设置你自己的 SuperKey，然后再 fastboot 刷一遍。这一步不能省，否则你拿到的是一把随机钥匙。

支持这一功能的 Recovery 包括 TWRP（最主流）、LineageOS recovery、Pixel Experience 自带的 recovery。小米 MIUI/HyperOS 官方 Recovery 不支持刷入第三方 zip，需要先刷 TWRP。

### APM 模块：不碰系统分区的 Systemless 修改

APM 全称 AndroidPatch Module，是 APatch 的模块系统——工作机制和文件格式与 Magisk Module 几乎一致。模块系统的核心价值是 systemless 修改：在不触碰 /system、/vendor、/product 等 Android 只读分区的前提下，实现安装字体、更换主题、添加系统属性、注入开机脚本等各种定制。

为什么叫 Systemless？Android 的系统分区在正常状态下以只读方式挂载，直接写入会触发 dm-verity 校验失败。Magisk 和 APatch 的做法是：**不修改原始分区**，在启动时通过 overlayfs 机制把模块里的文件"叠加"到原始分区上面。App 访问 `/system/libxxx.so` 时，实际上读到的是模块提供的修改版本，但原始分区里的文件丝毫未变。这样做的好处是 OTA 升级时系统分区没被改动，OTA 包可以正常应用，不需要先卸载 root。

一个典型的 APM 模块本质上是个 zip 文件，解压后包含：

- `module.prop`：模块元信息（名称、版本号、作者）
- `system/` 目录：要叠加到系统分区的文件
- `service.d/` 或 `post-fs-data.sh`：开机时自动执行的脚本
- `sepolicy.rule`：SELinux 规则
- `uninstall.sh`：卸载脚本

由于 APatch 的模块实现源自对 KernelSU 模块系统的复制，而 KernelSU 的模块格式本身就是 Magisk 的格式——**绝大多数 Magisk 模块可以直接在 APatch 上安装**，模块作者不需要针对 APatch 单独适配。安装后的模块存放在 `/data/adb/ap/modules/` 目录下。

### 救砖：刷回原厂 boot 的完整流程

当刷入修补 boot 后出现 bootloop（循环开机）、内核 panic、卡第一屏、或只是想彻底卸载 APatch 回归无 root 状态，最稳妥的恢复方式是刷回原厂 boot.img。

前提是你手里有**原厂 boot.img 的备份**——就是最开始从官方刷机包里提取的那一份。如果没有备份，去设备厂商的固件下载页或社区找当前系统版本的刷机包重新提取，确保版本号完全匹配。

操作步骤：

1. **进入 bootloader**：如果系统还能起来，用 `adb reboot bootloader` 最方便；系统完全起不来就关机后长按音量下 + 电源键 10 秒以上
2. **确认设备连接**：`fastboot devices`，输出应该包含设备 ID，没有就检查 USB 线和驱动
3. **刷入原厂 boot**：A/B 分区设备建议先查活动槽位（`fastboot getvar current-slot`），但最稳的做法是直接刷 boot 分区——fastboot 会自动定位到活动槽位：
   ```bash
   fastboot flash boot stock-boot.img
   ```
   等待两次 OKAY 输出就说明刷成功了
4. **重启设备**：`fastboot reboot`，手机会正常启动，root 消失

刷回原厂后，系统可能弹出"SELinux 已修改"或类似警告——正常，重启一次就好。某些开机自启模块的脚本可能残留，但因为内核补丁已经没了也不会有影响。想重新 root 的话，从原厂 boot 重新开始修补流程即可。

> [!TIP]
> **原厂 boot 备份请妥善保管。** 不管是换 root 方案（比如从 Magisk 换 APatch）还是救砖恢复，这份备份都是你最大的安全垫。丢了就只能去网上重新找固件包解包，麻烦不少。

---

## 🔗 相关资源

- [APatch官网](https://apatch.djnf.quest)
- [APatch文档](https://apatch-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/APatch)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/APatch)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/APatch)

## 📚 相关文章

- [APatch 值不值得我花这三小时折腾 Root？一笔时间账](https://laoguosofts.github.io/posts/apatch-docs.html)

- [APatch 值不值得折腾？一份刷机老手的迁移账本](https://alipymanbu.github.io/posts/apatch-docs.html)
