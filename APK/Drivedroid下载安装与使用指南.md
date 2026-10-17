## ✨ Drivedroid 是什么

Drivedroid 是一款能让 Android 手机变身为启动盘的工具——把手机通过 USB 连到电脑上，电脑会把它识别成 U 盘或光驱，直接从手机里存储的 ISO 镜像启动系统。不需要真正的 U 盘、刻光盘，也不用给手机刷任何特殊镜像，装好 App 就能用。

它的核心原理是利用 Android 的 USB Gadget 功能，让手机在连接电脑时模拟成 Mass Storage 设备，然后把手机里某个 ISO 或 img 文件"挂载"成这个虚拟 U 盘的内容。电脑 BIOS 看到的就是一个普通的启动盘，跟你插了个真实 U 盘没区别。

Drivedroid 的典型使用场景：给电脑装 Linux、跑 Kali Live、紧急修复系统、测试新发行版。对于经常折腾系统的人来说，手机 + Drivedroid 比随身带一堆 U 盘灵活得多。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://drivedroid.wxhp.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜 Drivedroid；国内 Android 应用市场可能搜不到，建议直接用上面的下载页

---

## 🔧 安装与首次配置

### 装 App

Drivedroid 主要面向 Android 设备。安装方式跟普通 APK 一样：下载 APK 文件后，在手机设置里开启「允许未知来源应用安装」，然后点开 APK 完成安装。如果是在 Google Play 能搜到的地区，直接在商店安装更方便。

> [!WARNING]
> Drivedroid 需要 root 权限才能正常工作。没有 root 的手机装了也没法挂载镜像——它依赖 root 来操作 USB Gadget 节点。如果你还没给手机 root，先搞定 root 再来装 Drivedroid。

装完打开 App，第一次启动会弹出 root 授权请求，允许即可。

### USB Setup Wizard

Drivedroid 首次启动会自动进入 USB Setup Wizard（USB 设置向导），分几步帮你完成 USB 连接模式的配置：

1. 手机用数据线连电脑，确保电脑能识别到手机
2. 向导会让你选择 USB System 选项——不同手机内核支持的模式不同，通常先试默认的 Mass Storage 模式
3. 按向导提示操作，手机端切换到 Mass Storage 模式
4. 向导最后会让你重启电脑进 BIOS 做一次引导测试

向导完成后，Drivedroid 就配好了。以后每次连电脑，只要在 App 里选个镜像 Host 一下就行，不用再跑向导。

### 空间够不够

Drivedroid 直接占用手机内部存储空间来放 ISO 和 img 文件，不是稀疏文件机制——你创建一个 8GB 的空白 img，手机存储就真的少 8GB。创建镜像前先检查手机剩余空间，避免写到一半空间不够失败。

---

## 📖 使用入门

### 引导测试：怎么验证链路通了

USB Setup Wizard 的最后一步会让你实际重启电脑进 BIOS 测一次。操作流程很简单：

1. **保持手机连接电脑**，Drivedroid 不要关掉——手机必须还在 Mass Storage 模式
2. 重启电脑，进 BIOS/UEFI 设置界面
3. 找到启动项里新出现的那个 USB 设备（名字通常带 Linux Gadget、Android 或手机品牌名）
4. 选择它作为第一启动项，保存退出

电脑重启后如果屏幕显示你挂载的 ISO 的引导界面（比如 Linux 发行版的安装菜单），手机屏幕上 Drivedroid 也提示 Hosting image 成功——那就是成功了。如果直接回到原来的系统、黑屏重启、或者显示 "No bootable device"，那就是引导失败。

> [!TIP]
> 引导失败回到 Windows 后，先关电脑再断开数据线。重新打开 Drivedroid，回到向导换一个 USB System 选项再试。三个选项都不行的话，大概率是手机内核的 Gadget 支持有限，考虑刷自定义 ROM。

### CD-ROM 模式：少部分手机才有的隐藏能力

有些手机的 Drivedroid 会多出一个 CD-ROM 挂载选项，模拟成真实光驱。这个功能很有用——某些老版本 Windows 7/8 的 ISO 只能当光驱用，U 盘模式启不动，CD-ROM 模式刚好能绕过去。但大部分手机出厂内核默认只编译了 Mass Storage Gadget（U 盘模式），CD-ROM Gadget 因为用的人少，厂商通常不编入内核。

怎么判断自己手机有没有 CD-ROM 支持？看向导 Summary 页面——如果提示 "The device cannot act as CD-ROM device"，说明没有。挂载 ISO 时看不到 CD-ROM 选项也一样。想开启的话，刷自定义 ROM（LineageOS、Pixel Experience 大多同时编入两种 Gadget）是最靠谱的办法；也可以给现有内核打补丁，但需要懂编译内核；Magisk 模块有些社区版本能动态加载缺少的 Gadget，不过效果不稳定。

没有 CD-ROM 模式也能工作——Windows ISO 通常不是 Hybrid 的，直接挂启不动，但可以用创建空白 img + Rufus 写盘的方式绕过去，跟手机有没有 CD-ROM 能力无关。

### 创建空白 img 镜像：Windows ISO 绕路的关键

什么时候需要空白 img？典型用途有三个：

| 用途 | 说明 |
| --- | --- |
| Windows ISO 挂载 | 大部分 Android 只有 U 盘模式，没有 CD-ROM 模式。Windows ISO 通常不是 Hybrid 的，直接挂启不动。创建空白 img + Rufus 写盘就能绕过去 |
| Kali 持久化 | 创建一个小分区作为 Kali Live 的数据持久化区，重启后数据不丢 |
| 普通 U 盘 | 把空白 img 模拟成普通 U 盘，可以在电脑上格式化、存文件 |

创建步骤：主界面右下角的加号按钮 → **Create blank image**，弹出对话框填三项：

- **名称**：建议用英文，比如 win10、kali-persistence
- **大小**：**一步到位填准**——免费版创建后不能改，付费版才能动态调整。Windows 10 建议 8GB 起，Windows 11 建议 16GB 起，Kali 持久化 2GB 足够
- **文件系统**：选 **None** 就行，后面 Rufus 会格式化

完成后 Drivedroid 在镜像目录里生成一个 `<名称>.img` 文件。空白 img 创建完，在 Drivedroid 里点击它 → 选 **Host image as writable USB device**，连电脑后用 Rufus 往里面写盘，写完卸载，之后就可以以只读 USB 模式挂着启动了。

### 开源状态与社区 fork

Drivedroid 是开源的，源码托管在 github.com/Softwarebakery 组织下。对普通用户来说，知道它开源最大的意义是安全性有保障——第三方开发者可以检查有没有恶意代码、有没有偷偷上传数据。

开源不代表有人维护。Drivedroid 的作者可能因为个人原因放弃了维护，或者 Google Play 的生态变化（Android 权限收紧、USB 模式变化）让持续维护变得困难，开源项目没人维护就成了孤儿项目。

> [!NOTE]
> 部分社区 fork（比如 XDA 上的）可能包含 Android 11+ 兼容性修复、去掉广告的内部版本、或者支持更多 USB System 选项。如果你在新版 Android 上原版跑不通，可以去 XDA 搜 Drivedroid 找 fork 版本试试。

---

## 🔗 相关资源

- [Drivedroid官网](https://drivedroid.wxhp.skin)
- [Drivedroid文档](https://drivedroid.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/Drivedroid)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/Drivedroid)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/Drivedroid)

---

## 📚 相关文章

- [用了 Drivedroid 半年才发现的三个隐藏玩法，难怪停更七年还死不了](https://laoguosofts.github.io/posts/drivedroid.html)
- [DriveDroid 藏得最深的 5 个冷门玩法，把手机当启动盘用透](https://alipymanbu.github.io/posts/drivedroid.html)
