## ✨ KingRoot 是什么

KingRoot 是一款面向 Android 设备的一键 root 工具，最早在 2014 年左右由国内开发团队推出。它的核心思路很简单——通过云端匹配 exploit 方案，在手机上点一个按钮就能自动完成从漏洞利用到 su 二进制注入的全过程，不需要手动刷 Recovery、不需要解锁 bootloader，对新手非常友好。

在 KingRoot 出现之前，Android root 基本上是极客圈子的事：解锁 bootloader 要等官网审核、刷 Recovery 要找对应机型的镜像、刷 su 包还要选对版本号，任何一步走错都可能变砖。KingRoot 把这些都藏在了一个 APK 里，把整个流程压缩成"打开 App → 点一下 → 等两三分钟"。

当然，简单是有代价的。KingRoot 的 exploit 方案依赖 Android 早期版本的若干内核漏洞（比如 CVE-2014-5333、CVE-2015-1532 等），这些漏洞在 Android 6.0 之后被各家厂商陆续封堵，到 Android 8.0+ 上成功率已经很低。另外，KingRoot 采用的是把 su 二进制直接写入 /system 分区的方案，这种方式对现代设备来说会破坏分区完整性，也给 SafetyNet / Google Play Integrity 等 DRM 验证留下痕迹。

即便如此，KingRoot 仍然有它的适用场景：手里有一台老安卓设备（Android 5.x/6.x）、想快速拿 root 权限用 Greenify 省电、删预装软件，或者给父母的老手机精简一下——KingRoot 依然是成本最低的选择。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://kingroot.fewz.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 设备请从可信渠道下载 APK；Google Play 与 App Store 均不提供此类工具分发

---

## 🔧 安装步骤

KingRoot 分两个版本——手机端 APK 和 PC 版。绝大多数场景用手机端就够了，只有当手机端卡死或不支持时才需要 PC 版兜底。

### 手机端 APK 安装

1. **开启未知来源安装**：设置 → 安全 → 未知来源 / 安装未知应用 → 允许文件管理器或浏览器安装 APK
2. **下载 APK**：从上面的下载入口获取最新版本（当前主流版本为 v5.4.0）
3. **安装**：点击 APK 包，确认权限请求，等待安装完成
4. **验证版本**：打开 KingRoot App → 菜单 → 关于，确认版本号与下载页一致

> [!TIP] 务必核对版本号
>
> KingRoot 老版本（v5.0 以下）的云端 API 可能已经失效，表现为打开 App 后进度条一直停在 0% 转圈。如果遇到这种情况，先检查是不是版本太老，优先升级到 v5.4.0。

### PC 版安装

PC 版是一个 Windows 下的绿色工具，不需要安装，解压后直接运行 KingRoot.exe 即可。使用前需要：

- 手机开启 **USB 调试**（设置 → 开发者选项 → USB 调试打勾）
- 用数据线连接手机，Windows 自动安装 ADB 驱动
- 如果 Windows 没自动识别，去下载 ADB 驱动包手动安装

> [!WARNING] 杀毒软件误报
>
> KingRoot 的 exploit payload 会被部分杀毒软件标记为可疑行为。这是因为 exploit 本身就涉及内核提权，行为模式和恶意软件类似。建议在可信下载渠道获取 APK / EXE 后，暂时关闭杀毒软件再操作。

### 装完先做三项检查

1. **电量 50% 以上**——root 过程中手机可能重启一次，电量太低中途关机有风险
2. **Wi-Fi 已连**——exploit 方案从云端拉取，没网 KingRoot 只会转圈
3. **备份重要数据**——虽然 KingRoot 不改分区表，但万事留一手

---

## 📖 使用入门

### 一键 root 的完整流程

打开 KingRoot 后主界面只有一个按钮——"尝试 root"。点一下之后，整个过程大约 2-5 分钟，按阶段拆开：

| 进度区间 | 对应阶段 | 耗时 |
|---------|---------|------|
| 0-15% | 机型探测 + 拉取云端 exploit 方案 | 10-30 秒 |
| 15-60% | 执行 exploit payload（拿临时 root shell） | 30 秒 - 2 分钟 |
| 60-90% | 注入 su 二进制 + KingUser 管理 App | 约 30 秒 |
| 90-100% | 重启验证 | - |

阶段 1 如果"暂不支持此设备"或"无法获取云端方案"，先检查网络和 DNS。阶段 2 是最容易出问题的环节——如果卡在 40% 不动，大概率是 exploit 被厂商安全加固封堵了。阶段 3 完成后会提示重启，重启后桌面上应该能看到 KingUser 图标。

拿到永久 root 后，装一个需要 root 的 App（比如 RE 文件管理器或 Greenify），能弹出 KingUser 权限请求并成功授权，说明 root 正常工作了。

### 卡在进度条 40% 不动

这是 KingRoot 最常见的失败场景。卡在 40% 正好是 exploit payload 执行阶段——漏洞触发了、payload 跑起来了，但厂商在这个环节拦截了提权操作。主要原因：

1. **厂商安全加固封堵**（概率最高）：Android 6.0+ 开始，越来越多厂商在系统层面拦截 KingRoot 所用的老漏洞。典型受影响机型包括小米 5 系列（MIUI 8+）、华为 Mate 8（EMUI 4.1+）、三星 S6（Android 7.0+）、一加 3（OxygenOS 4+）
2. **Android 版本太新**：Android 7.0 及以上 KingRoot 成功率骤降，新机型基本没戏
3. **临时 root 路径被封**：exploit 能执行、也能拿到临时 root shell，但 su 写入 /system 的路径被厂商限制

排查步骤：

1. **先等够时间**——KingRoot exploit 执行有时确实需要几分钟，等满 5 分钟再判断真的卡死
2. **强制重启后换 PC 版试试**——PC 版的 ADB 通道和手机端 APK 的 exploit 注入路径不同，部分厂商只拦了其中一条
3. **刷入第三方 Recovery 换路线**——临时 root 状态下用 fastboot 刷 TWRP，再刷入 SuperSU 或 Magisk zip，绕过 KingRoot
4. **接受 KingRoot 路线走不通**——新机型直接走 Magisk 教程（解锁 bootloader + 刷 patched boot.img），老机型去酷安或 XDA 找专属方案

> [!NOTE] 40% 卡死不会变砖
>
> KingRoot exploit 在内存里运行，不改磁盘分区。强制重启（长按电源键 10 秒以上）通常能正常进系统。只有系统反复重启时才需要进 Recovery 双清或刷官方固件。

### 无法获取云端 root 方案

表现为进度条一直停在 0-15%，提示"无法获取云端 root 方案"或一直转圈。手机浏览器能正常上网但 KingRoot 拉不到方案，问题出在 KingRoot 和它的云端服务器之间：

- **VPN 干扰**：KingRoot 服务器部分在阿里云，境外 VPN 可能把请求路由到境外节点导致超时。关掉 VPN 用纯净 Wi-Fi 重试
- **DNS 劫持**：运营商 DNS 解析异常。手动设置 DNS 为 `114.114.114.114` + `8.8.8.8` 后重连 Wi-Fi
- **系统时间不准**：HTTPS 请求依赖时间校验证书。去设置里打开"自动确定日期和时间"，等同步准确后再试
- **版本太老**：2017 年之前的老版本云端 API 可能已失效，升级到 v5.4.0

终极测试方法：找一张没装任何 APK、没开任何 VPN 的纯净 SIM 卡 + 运营商 Wi-Fi，只装 KingRoot 一个 App。如果这次能拿到方案，说明之前是环境干扰；还是不行就大概率是机型/系统版本本身 KingRoot 不支持。

### 一键 unroot：移除 root 权限恢复未 root 状态

拿到 root 后又想恢复干净状态的场景很常见——手机要送修、要出二手、root 后性能异常、或者想换 Magisk。KingRoot 自带的 Remove Root 功能比手动删文件靠谱得多。

手机端操作：打开 KingRoot App → 点右上角菜单 → 找到"移除 Root" → 弹窗确认 → KingRoot 自动清理 su 二进制、KingUser、init.d 脚本和修改过的 build.prop → 清理完重启。PC 版操作类似，连接手机后找"一键 Unroot"按钮。

如果 KingRoot App 已经被删了但 su 权限还在，用 RE 文件管理器手动清理：删除 `/system/xbin/su`、`/system/bin/.su`、`/system/app/KingUser/`、`/data/app/KingUser*/`、`/data/local.prop` 以及 `/system/etc/init.d/` 下所有 kingroot 相关脚本。如果之前修改过 `/system/build.prop`，恢复备份或手动删掉自己加的那些行。最后清一下 dalvik 缓存：终端执行 `rm -rf /data/dalvik-cache/*`，重启即可。

验证 unroot 成功的标志：KingUser 图标从桌面消失、再开 KingRoot App 显示"您的设备尚未获取 root 权限"、RE 管理器打开根目录提示没有 root 权限。

> [!WARNING] 有些厂商还是能检测到 root 痕迹
>
> 即使 KingRoot 清理干净了，三星 Knox 芯片在第一次刷入非官方 boot 时就会把熔丝位烧断（硬件级的，刷固件也恢复不了）；小米/华为等厂商在云服务器上记录过 bootloader 解锁事件，即使重新上锁也可能查到。送修或出机的话，刷回官方固件是最彻底的方式——小米用 MiFlash、华为用 HiSuite 系统修复、三星用 Odin 工具五件套刷入。刷之前务必备份所有数据。

---

## 🔗 相关资源

- [kingroot官网](https://kingroot.fewz.tech)
- [kingroot文档](https://kingroot-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/kingroot)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/kingroot)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/kingroot)

---

## 📚 相关文章

- [KingRoot 上手体验：一台老安卓的 root 自救日记](https://laoguosofts.github.io/posts/kingroot-docs.html)
- [我用 KingRoot 把三年没动过的老手机救活了](https://alipymanbu.github.io/posts/kingroot-docs.html)
