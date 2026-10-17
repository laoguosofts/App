## ✨ OwnDroid 是什么

OwnDroid 是一款面向 Android 设备的 Device Owner 管理工具。它的核心价值在于——把 Android Enterprise 框架里原本只有企业 IT 管理员才能动用的设备管控能力，开放给普通用户。

Device Owner 是 Android 安全模型中的最高管控角色。一旦 OwnDroid 成功激活为 Device Owner，它就能解锁一整套此前被系统隐藏的开关：锁定任务模式（Kiosk 单应用终端）、强制 Always-on VPN、批量应用用户限制、禁用物理存储挂载、禁止恢复出厂设置等。这些功能单拎出来看，每一个都是"企业设备才有的待遇"；OwnDroid 把它们整合进一个界面，让个人用户也能拿来做家长控制、自助终端搭建、个人设备加固。

典型使用场景包括：把旧 Android 平板锁成只能跑单一学习 App 的儿童学习机；把一台 Android 设备改造成商场导购机或餐厅点餐机；为自己的手机启用强制 DNS 过滤 + VPN 锁定，杜绝恶意网站绕过；在 root 设备上批量开启安全类限制防止误操作。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://owndroid.fewa.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：F-Droid 的 IzzyOnDroid 仓库，或 GitHub Releases 页面搜索 OwnDroid

---

## 🔧 激活步骤：四种路径怎么选

OwnDroid 的安装不等于激活——安装完 apk 只是起点，**激活 Device Owner 才是真正解锁全部功能的关口**。这道关口有一个 Android 安全模型的硬约束：设备必须在"还没有任何账号、还没有任何多用户"的状态下才能设置 Device Owner。也就是说，**大概率需要先恢复出厂设置**，否则系统层面就不允许激活。

确认设备满足前置条件后，有四种激活路径可选：

### Shizuku（手机端无线激活，最推荐）

不需要电脑、不需要 USB 线，全程在手机上完成。前提是设备的开发者选项里能开启无线调试（Wireless Debugging）并配对成功。激活流程：

1. 恢复出厂设置后，**不要登录任何账号、不要创建任何用户**
2. 安装 OwnDroid 和 Shizuku 两个应用
3. 打开开发者选项 → 启用无线调试 → 配对设备
4. 在 Shizuku 里点 Start 让服务运行起来
5. 打开 OwnDroid，选择 Device Owner → Shizuku 路径，等待 Success 提示

![OwnDroid 激活成功](/images/owndroid-shizuku.webp)

GrapheneOS、Pixel 等原生 Android 设备上这条路非常顺畅。MIUI / HyperOS 用户需要额外在开发者选项里打开"USB 调试（安全设置）"。

> [!NOTE]
> ColorOS 用户如果遇到 Shizuku 激活失败，可以尝试下载 OwnDroid 的 testkey 版本——签名不同，某些 OEM ROM 的 Device Owner 校验逻辑对 testkey 签名更友好。

### Dhizuku（Shizuku 的备选路径）

Dhizuku 和 Shizuku 属于同一类工具，都提供 adb shell 权限给应用调用。它的特点是：Dhizuku 自身也可以先被设为 Device Owner，然后再把所有权转移给 OwnDroid。某些 Shizuku 激活失败的国产 ROM 上，这条路能走通。流程与 Shizuku 类似——装 Dhizuku → 启动服务 → OwnDroid 选 Dhizuku 路径 → 激活。

### Root 权限（最暴力最稳）

设备已经 Magisk/Root 过的话，打开 OwnDroid 选 Root 路径，授予 su 权限即可。但注意：**即便有 Root，恢复出厂设置这个前置条件仍然需要**。Root 只是让激活命令的执行变得无阻碍，并不能绕过 Android Device Owner 安全模型本身的限制。

### ADB Shell 命令（电脑端最可控）

用电脑 USB 连接设备，运行一条 adb 命令直接完成激活：

```bash
adb shell dpm set-device-owner --user 0 com.bintianqi.owndroid/.Receiver
```

前提是开发者选项已开、USB 调试已启用、设备已授权电脑。这条命令执行成功后 OwnDroid 就已经是 Device Owner 了——打开 OwnDroid 应该能直接看到盾牌图标激活的状态。如果界面没及时反应，可以强制停止 OwnDroid 再重开，让应用重新检测权限。

### 路径选择建议

| 你的情况 | 推荐路径 |
|---|---|
| 有电脑、熟悉 adb | ADB Shell |
| 手机能开无线调试、不想用电脑 | Shizuku |
| Shizuku 激活失败（某些国产 ROM） | Dhizuku |
| 设备已 Root | Root |
| MIUI / HyperOS 用户 | Shizuku + 开发者选项开「USB调试（安全设置）」 |
| ColorOS 用户 | OwnDroid testkey 版本 + Shizuku |
| Samsung 用户 | 有 Root 才可能绕过多用户限制 |

> [!WARNING]
> 激活前务必确认设备已恢复出厂、无账号无多用户。如果 OwnDroid 报错"设备已有账号"或"设备已有用户"，不用折腾激活路径——直接恢复出厂再来。

---

## 📖 核心功能使用入门

激活完成后 OwnDroid 会展示完整的功能列表。四个方向的能力最值得先上手：锁定任务模式（Kiosk）、用户限制、Always-on VPN 强制、应用与安全限制。

### Kiosk 单应用模式：把设备变成自助终端

锁定任务模式（Lock Task Mode）是 OwnDroid 最直观的功能——把 Android 设备锁成只能跑一个 App 的 kiosk 自助终端。用户无法切换应用、无法按 Home 键、无法查看通知。典型场景包括商场导购机（只能跑 WebView Kiosk 浏览商品）、餐厅点餐机、公共信息查询屏、考试答题设备、酒店房间控制屏、数字标牌。

#### 配置 Kiosk 的五个步骤

**第一步，安装 Kiosk App。** 选一个支持 Device Owner Lock Task Mode 的应用——开源的 WebView Kiosk（把任意 URL 作为全屏 WebView 运行）是最灵活的选择；Chrome / Edge 企业版支持单标签锁定；也可以用 Android Enterprise 框架自定义开发。

**第二步，配置锁定任务模式白名单。** OwnDroid → 系统 → 锁定任务模式 → 添加应用。把 Kiosk App 的包名、**系统 Home 应用的包名**、以及 Kiosk 模式下可能调用的辅助应用（如打印机、扫码器）全部加入白名单。

> [!WARNING]
> 白名单里**必须包含系统 Home 应用**。否则触发锁定后设备会卡在无响应状态——系统无法在锁定模式下启动任何非白名单应用，包括 Home。

**第三步，配置辅助限制。** Kiosk 设备建议同时开启以下限制：

| 限制项 | 理由 |
|---|---|
| 禁止截屏 | 保护 Kiosk 内容版权或安全信息 |
| 禁用摄像头 | 防止用户用摄像头拍摄屏幕 |
| 禁用 USB 信号 | 防止外部 USB 设备窃取数据 |
| 禁止恢复出厂设置 | 防止用户重置设备绕过 kiosk |
| 禁止安装/卸载应用 | 锁定的 kiosk 不需要其他 App |
| 全局静音 | 商场/餐厅环境需要安静 |
| 禁用蓝牙/NFC | 防止蓝牙劫持或 NFC 支付干扰 |

**第四步，触发锁定。** 在 OwnDroid 的锁定任务模式区域点击启动按钮即可手动触发。也可以通过 API 远程触发——先确保 Kiosk App 在前台启动，然后发送广播：

```bash
am broadcast -a com.bintianqi.owndroid.action.LOCK_TASK_START ^
  -n com.bintianqi.owndroid/.ApiReceiver ^
  -e key YOUR_API_KEY
```

**第五步，处理重启后的持久化问题。** Android 的 Lock Task Mode 在重启设备后**会自动解除**。有两种方案：一是用 OwnDroid API 配合 Automate 等自动化工具，监听 BOOT_COMPLETED 广播后自动再次触发锁定；二是用支持自恢复的 Kiosk App——它本身可能在启动时调用 DevicePolicyManager.startLockTask()。

#### 退出 Kiosk 模式

只有 Device Owner 能退出：OwnDroid 界面点击"停止锁定任务模式"，或通过 API 调用 dpm.stopLockTask()。**没有退出密码或用户手势**——普通用户不可能自行退出。

### 用户限制：一键批量开关

用户限制是 OwnDroid 最常用的一组功能——同一层级的布尔开关，开启或关闭。涵盖应用、隐私、安全三大类共数十个限制项。

**应用类限制**适合 kiosk 和企业管控场景：

| 限制键 | 作用 |
|---|---|
| no_install_apps | 禁止安装任何应用 |
| no_uninstall_apps | 禁止卸载任何应用 |
| no_install_unknown_sources | 禁止来源不明的 APK 安装 |
| no_cursor_usage | 禁止使用光标（触摸板/鼠标） |
| no_clipboard | 禁止访问剪贴板 |
| no_clipboard_sharing | 禁止跨设备剪贴板共享 |

**隐私类限制**主要做隐私保护和 kiosk 防改动：

| 限制键 | 作用 |
|---|---|
| no_location | 禁用位置服务 |
| no_location_sharing | 禁止应用间共享位置数据 |
| no_add_wallpaper / no_set_wallpaper_component | 禁止修改壁纸及壁纸组件 |

**安全类限制**是企业/kiosk 的最后一道防线：

| 限制键 | 作用 |
|---|---|
| no_factory_reset | 禁止恢复出厂设置 |
| no_safe_boot | 禁止安全模式启动（防止绕过管控） |
| no_mount_physical_media | 禁止挂载物理存储介质（防 OTG USB 读取） |
| no_remove_user | 禁止删除用户 |
| no_debugging_features | 禁止开发者调试功能 |

批量开启的操作方式：打开 OwnDroid → 用户限制 → 按类别批量勾选（比如先把系统类全部勾上）→ 确认保存 → 后续单独取消那些不需要的。

> [!NOTE]
> Device Owner 设置的限制优先级最高——用户在系统设置里的任何相关尝试都会被拦截。所有限制持久化到系统，重启后保持。Profile Owner 和 Dhizuku 模式下能设置的限制项比 Device Owner 少得多，这也是为什么激活 Device Owner 仍然是 OwnDroid 的核心前提。

### Always-on VPN 强制锁定：防止流量绕过

Android 的 Always-on VPN 普通用户可以随时关闭、切换 VPN 或手动关掉系统开关。OwnDroid 作为 Device Owner 可以**强制 VPN 模式**——调用 DevicePolicyManager 的 setAlwaysOnVpnPackage() 后：

- VPN 断开自动重连
- 用户**无法在系统设置里关闭** Always-on VPN 选项
- 如果 VPN App 被禁用或卸载，设备进入 **Blocked 状态**（断网，直到 VPN 恢复）
- 还能开启「锁 VPN」模式——split tunnel 被禁用，所有流量必须走 VPN，杜绝绕过

配置步骤很直接：先装好 VPN App 并正常连接一次（确保 App 支持 Always-on VPN）→ OwnDroid → 网络 → Always-on VPN → 选择 VPN App 包名 → 打开"强制 VPN 模式"（VPN Lockdown）→ 可选地配置"允许绕过 VPN"的应用白名单。

#### 强制 VPN + DNS 过滤 = 家长控制 / 自我管控

这是 OwnDroid 最强大的组合玩法：

1. 安装支持 DNS 过滤的 VPN App（ReThinkDNS、Blokada、AdGuard Home 的 VPN 客户端）
2. 在 VPN App 里配好要拦截的域名列表（色情、赌博、恶意网站）
3. OwnDroid 强制 Always-on VPN + VPN Lockdown
4. 用户无法绕过 VPN → 所有 DNS 查询都经过过滤
5. 配合"阻止卸载 + 挂起 VPN App"，用户连关 VPN App 都做不到

> [!WARNING]
> Blocked 状态是最强制的 VPN 策略，但也是最危险的——VPN App 意外退出（比如被 MIUI 后台杀掉）就会导致设备完全断网。MIUI/HyperOS 用户务必把 VPN App 加入电池白名单（无限制）。VPN Lockdown 模式下某些系统功能也可能异常（如 Android Pay/NFC 支付可能需要加入白名单）。

### 安装与使用的常见误区

OwnDroid 的几个高频"踩坑"点值得提前了解：

**先有账号再激活等于白跑。** Android 的 Device Owner 不是"给已使用的设备加分"，而是"在设备空白状态时写入一个身份"。恢复出厂设置不是 OwnDroid 找事——它就是 Android 安全模型的硬约束，OwnDroid 破解不了。

**某些 OEM ROM 有厂商级绕过后门。** Samsung Secure Folder 里的应用可能不受主用户限制影响；部分国产 ROM 提供安全模式或厂商级恢复出厂设置绕过 OwnDroid 的禁止开关。实测之前需要确认目标设备的 ROM 行为。

**重启后自动解除的功能。** Lock Task Mode 重启后自动解除，这是 Android 设计如此，不是 OwnDroid 的 bug。用开机自触发自动化脚本解决。

---

## 🔗 相关资源

- [OwnDroid官网](https://owndroid.fewa.skin)
- [OwnDroid文档](https://owndroid-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/OwnDroid)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/OwnDroid)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/OwnDroid)

## 📚 相关文章

- [暑假把旧手机给娃当学习机，我是怎么用 OwnDroid 把它锁成一台纯学习设备的](https://laoguosofts.github.io/posts/owndroid-docs.html)
- [那次出差差点被酒店 USB 坑了之后，我给手机装了 OwnDroid](https://alipymanbu.github.io/posts/owndroid-docs.html)
