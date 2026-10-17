## ✨ EtchDroid 是什么——用手机做启动盘的开源利器

EtchDroid 是一款跑在 Android 手机上的磁盘镜像写入工具，核心用途只有一个：把 `.img` 或 `.iso` 镜像文件直接写到 U 盘或 SD 卡里，做成可引导启动盘。它不做 ISO 转制、不做压缩包管理、不做分区编辑器——职责边界非常清晰，就是"手机版的 Rufus / balenaEtcher"。

这个定位让它在一类特定场景下不可替代：**当你的电脑系统挂了、却没有第二台电脑可以用来做启动盘的时候**。你只需要一部装了 EtchDroid 的 Android 手机、一根 OTG 转接线、一个 U 盘，就能把 Ubuntu Live、System Rescue、Raspberry Pi OS 等镜像直接烧进去，省掉了"借电脑 → 装刻录工具 → 插 U 盘 → 写镜像"那一套在应急时刻根本凑不齐的流程。

EtchDroid 整个项目托管在 GitHub，采用 GNU General Public License v3.0 开源，源代码包括应用主模块（Kotlin + Jetpack Compose + Material You）、USB MSC 协议栈封装、镜像解析和写入引擎等全部组件。超过 200 名贡献者参与过代码审查，F-Droid 构建流水线保证了发布版本与源码的对应关系。

> [!NOTE]
> 关于隐私：EtchDroid 的 F-Droid 版本完全不收集任何数据、不包含遥测代码；Google Play / GitHub Releases 版本会收集匿名遥测（可在设置里关闭），但无论哪个版本都不会读取或上传你的镜像文件内容、U 盘数据或手机上的其他文件，它只需要知道镜像文件的路径和大小来完成写入。

EtchDroid 适合谁用？Linux 双系统用户、树莓派/单板机折腾党、系统重装频率高的运维人员、以及任何"不想专门为做启动盘开一台 PC"的人。如果你平时只在 Windows 上用 Rufus 做 U 盘系统盘，EtchDroid 可能用不上；但只要你有过"系统挂了找不到备用机"的经历，就会知道手机上常备一个 EtchDroid + 几 GB 空间的 ISO 有多么救命。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://etchdroid.wrwq.fun/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜 EtchDroid（有 Google Play / GitHub Releases / F-Droid 三个渠道版本，F-Droid 版隐私最严格、完全无遥测）

---

## 🔧 安装步骤——Android 应用的常规流程

EtchDroid 是 Android 应用，安装方式和其他 APK 一样简单：

1. 从下载页获取 APK 文件，保存到手机存储
2. 打开文件管理器找到 APK，点击安装
3. 如果提示"未知来源"未授权，进入手机设置 → 安全 → 允许安装未知来源应用（MIUI / ColorOS 等定制系统路径可能略有差异）
4. 安装完成后桌面出现 EtchDroid 图标，点开即用

> [!TIP]
> 版本核对：EtchDroid 在 v1.9 做了完全重写，引入了 Material You UI 和强制写入后校验机制。如果你下载的版本号低于 1.9，建议直接下最新版——旧版本既没有现代 UI，也没有写入校验，应急场景下容易写出"看起来成功但数据损坏"的假启动盘。

安装完成后第一次打开，EtchDroid 会请求**存储权限**和**USB 设备访问权限**，这是 Android 系统对敏感操作的常规授权流程，全部点允许即可。没有 root 也不需要 root——EtchDroid 通过 Android 标准的 USB Host API 工作，普通 Android 设备（OTG 功能正常）就能用。

---

## 📖 使用入门——从镜像到启动盘的完整链路

EtchDroid 的核心流程就三步：**选镜像 → 选设备 → 写入**。但要做到"写出来的启动盘真的能用"，还有一些细节需要注意。

### 准备阶段：镜像文件和存储

EtchDroid 支持直接写入 `.img` 和 `.iso` 格式，**不支持 `.xz`、`.zip`、`.7z` 等压缩格式**。这是刻意的产品决策——镜像解压是文件管理层面的事，EtchDroid 把自己的职责边界划在"只处理未压缩的磁盘镜像"。

最常见的解压场景是树莓派镜像：官方分发的 Raspberry Pi OS 通常以 `.img.xz` 格式打包，体积小但需要先解压才能写入。手机端解压推荐用 MT 文件管理器或 Solid Explorer，解压后得到 `.img` 文件——注意解压后的文件会比压缩包大很多（`.xz` 对空分区压缩率极高，1.2GB 压缩包解压后可能是 8GB），你的 SD 卡容量必须 ≥ 解压后的镜像大小。

### 写入阶段：设备选择与进度监控

把 U 盘或 SD 卡通过 USB 读卡器接到手机 OTG 口，EtchDroid 会自动检测到新的 USB 设备。界面上会列出所有可写入的存储设备——**务必认准是你要烧录的那个 U 盘，别选错了手机自带存储**。

> [!WARNING]
> EtchDroid 写入是**全扇区覆盖**，被写入设备上的所有数据都会被抹掉。U 盘里有重要文件的话，先备份再写。

写入过程是纯进度条显示，没有额外操作。一个 1.5GB 镜像的典型写入时间约 2 分钟，具体取决于手机 USB 口的传输速度和 U 盘本身的读写性能。

### 校验阶段——v1.9 之后的强制流程

写入完成后 EtchDroid 会**自动进入校验阶段**，这是 v1.9 引入的重大改进，也是它和 Rufus / balenaEtcher 的一个关键差异点——EtchDroid **不允许跳过校验**。

校验做了什么？从 U 盘扇区 0 开始逐块（通常 4MB 一块）读回刚写入的数据，与手机存储里的源镜像文件逐字节比对。所有块完全一致才提示"写入成功"，任何一块不匹配都会提示校验失败并建议重新写入。

校验时间 ≈ 写入时间。v1.9 rc3 之后通过调整缓冲区和队列策略实现了校验与写入的流水线化，总耗时有所缩短，但等待仍然不可避免。

校验失败通常意味着以下几种情况：

| 原因 | 解决方式 |
| --- | --- |
| 写入中途 USB 连接有瞬时抖动 | 重新完整写入一次 |
| U 盘有坏块 | 换一个 U 盘 |
| 手机 USB 口供电不足 | 换一根更粗的 OTG 线或换 U 盘 |
| 源镜像文件本身有损坏 | 重新下载镜像，校验 MD5/SHA256 |

### 典型场景一：双系统引导全坏，用手机救 Grub

多系统用户最害怕的时刻：Grub 坏了、Windows Boot Manager 也失效，电脑完全无法引导任何系统。传统做法需要一台备用 PC 来做启动盘，EtchDroid 就是在这种"没有备用机"的情况下补位：

1. EtchDroid 做一个 System Rescue 或 Ubuntu Live 启动盘
2. 插入电脑从 U 盘启动，进入 Live 环境后可以：
   - 挂载 Linux 分区 → chroot → `grub-install /dev/sda` → 更新 Grub 配置
   - 挂载 Windows 分区 → 命令行执行 `bootrec /fixmbr` / `bootrec /rebuildbcd`
   - 备份所有分区上的重要数据到外接盘
3. 修复完成后重启从硬盘引导

社区报告最多的 EtchDroid 使用场景就是这种——**在 Windows 还能临时进但不想折腾刻录工具时，用手机做启动盘是最快的"两系统一起救"方案**。

多系统应急救场时，建议在手机里常备这些 ISO，按优先级排列：

| 优先级 | ISO | 用途 |
| --- | --- | --- |
| 1 | System Rescue | 修复引导、chroot、分区修复、文件系统检查 |
| 2 | Ubuntu Live | 通用重装、图形化工具更全 |
| 3 | Alpine Live | 极轻量，网络修复场景 |
| 4 | MemTest86+ | 内存检测 |
| 5 | Clonezilla Live | 全盘备份/恢复 |

### 典型场景二：树莓派镜像烧录

树莓派玩家对 EtchDroid 也青睐有加，因为不必为了烧一个 SD 卡专门开 PC。完整链路：

```
下载官方镜像 (.img.xz) → 手机端文件管理器解压 → 得到 .img
        → SD 卡通过 USB 读卡器接手机
        → EtchDroid 选择 .img + 选择读卡器设备 → 写入 → 校验
        → SD 卡插入树莓派 → 开机启动
```

> [!NOTE]
> EtchDroid 写入树莓派镜像后，镜像本身的 resize 文件系统机制不会自动执行——这不是问题，树莓派的 resize 脚本会在首次启动时由 Raspberry Pi OS 内部运行，插入树莓派开机就好。

### 一个硬限制：不支持 Windows 官方 ISO

EtchDroid 不支持 Windows 官方 ISO 的直接写入。如果你需要做 Windows 安装盘，还是得用 Rufus 或 ventoy 这类 PC 端工具。EtchDroid 的主战场是 Linux Live、救援系统、单板机镜像这三类场景，Windows 安装盘制作不在它的设计范围内。

---

## 🔗 相关资源

- [EtchDroid官网](https://etchdroid.wrwq.fun)
- [EtchDroid文档](https://etchdroid.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/EtchDroid)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/EtchDroid)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/EtchDroid)

---

## 📚 相关文章

- [EtchDroid 到底值不值得掏钱？一张手机解决启动盘的时间账](https://laoguosofts.github.io/posts/etchdroid.html)
- [EtchDroid 到底值不值得掏钱——一个免费工具的决策账本](https://alipymanbu.github.io/posts/etchdroid.html)
