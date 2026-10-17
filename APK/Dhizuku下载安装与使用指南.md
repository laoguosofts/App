## ✨ Dhizuku 是什么：拿设备所有者身份的权限中介

Dhizuku 是一个面向 Android 的权限分发工具，核心思路很直接：通过 ADB 或 Shizuku 执行一条短命令，把自己注册为设备所有者（Device Owner），然后把这个系统级身份按需分配给其他应用。拿到设备所有者身份的应用可以冻结/隐藏应用、读写所有账户、控制前台服务——这些操作在非 root 设备上本来是系统专属的。

它和 Shizuku 最大的区别在于持久性：Shizuku 给的是临时 ADB 会话，重启就失效；Dhizuku 走设备所有者路线，身份由 Android 系统本身持有，重启不丢。对不想 root、又想让日常自动化工具（比如 Hail 冻结应用、Tasker 做系统级动作）长期可用的人来说，这条路绕开了解锁 bootloader 和修改分区的风险。

项目基于 **GPL-3.0** 协议全量开源，源码托管在 GitHub，激活命令到底做了什么、权限申请逐条对应什么功能——都可以在源码里审计。从 2.10 版本起内置了使用条款与隐私政策，应用本身不联网上报使用数据，权限清单里没有网络通信类条目。对拿系统最高身份的第三方应用来说，代码透明就是最实在的信任背书。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://dhizuku.fewa.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：F-Droid 可搜到（F-Droid 附恶意软件检测结果和权限披露页，可逐条核对）；Google Play 上架情况以商店实际为准

---

## 🔧 安装与激活：三条路径挑一条走通

安装本身就是装 APK，但激活是绕不开的坎——设备所有者身份只能通过 ADB 命令或已有 Shizuku 权限间接注册，系统不让用户手动勾选。三条路径按便捷度排序：

### 路径一：Shizuku 激活（最省心）

如果你已经在用 Shizuku，这条路最快：

1. 确认 Shizuku 正在运行（主界面显示运行中）；
2. 打开 Dhizuku，点「激活（通过 Shizuku）」卡片；
3. 应用通过 Shizuku 代执行设备所有者设置命令，成功后状态卡切换为已激活。

> [!TIP]
> Shizuku 本身需要先启动：Android 11+ 走开发者选项「无线调试」配对后免电脑，Android 10 及以下需要 USB 连电脑跑启动脚本。激活成功后 Shizuku 就可以卸载——设备所有者身份由系统持有，与 Shizuku 无关。

### 路径二：ADB 激活（最通用）

电脑用 USB 连设备，打开开发者选项的 USB 调试，执行一条命令：

```
adb shell dpm set-device-owner com.rosan.dhizuku/.server.DhizukuDAReceiver
```

命令返回成功后回应用内刷新状态。这条命令只注册设备管理员身份，不动系统分区、不解锁 bootloader。

### 路径三：直接激活（部分厂商）

少数 ROM 对设备所有者路径做了便利封装，应用内有直接激活按钮。如果按钮能点就先试试，点不动再退回前两条。

### 激活命令报错的常见情况

**`device owner is already set`**：设备上已有别的应用占着设备所有者身份。先查是谁占的：

```
adb shell dumpsys device_policy
```

输出里 `Device Owner` 字段后面的包名就是当前持有者。处理顺序：强停重进（部分场景身份已设成功但界面没刷）→ 从原持有者转移（OwnDroid 等支持转移的工具在自身设置里把身份转给 Dhizuku）→ 停用原持有者（解除它的设备管理员身份或直接卸载）。恢复出厂能解决一切但代价最大，留到最后。

> [!WARNING]
> OPPO 系 ROM 可能报错误码 99，这是厂商签名白名单限制，不是操作问题——需要用正确签名重签安装包才能绕过。

---

## 📖 使用入门：拿到身份之后怎么用

激活成功只是第一步，真正的价值在于把设备所有者权限分发给其他应用。以下按高频场景梳理。

### 给应用授权

打开 Dhizuku 主界面，会列出所有可申请设备所有者权限的应用。每个应用点进去勾选需要的权限项（查询所有应用、账户读写、前台服务控制等），确认后应用就能在 Dhizuku 的身份下执行对应操作。

> [!NOTE]
> 权限是按需分配的——不勾选的功能永远不会被调用。Dhizuku 本身只做权限中介，主动权在每个授权弹窗。

### 配合 Hail 冻结/隐藏应用

这是 Dhizuku 最常见的搭子。Hail 用设备所有者身份把不常用的应用冻结或隐藏，释放后台占用。注意冻结后的应用在桌面和应用列表里都看不到，解冻要进 Hail 操作——激活后如果做过冻结，**重启前务必解冻**，否则进系统可能找不到应用而无法恢复。

### 中介服务被系统杀掉怎么办

Dhizuku 有一个常驻的前台服务作为权限中介。某些厂商的电池优化会把这个服务干掉，其他依赖 Dhizuku 的工具就会报「服务不可用」。根治办法：在系统设置里把 Dhizuku 加入电池优化白名单，允许自启动。

### 重启、安全、银行类应用

重启完全不需要重新激活——设备所有者身份是系统级的，持久生效，这正是 Dhizuku 比 Shizuku 省心的地方。安全方面，GPL-3.0 全量开源意味着行为可审计，代码里没有网络通信权限，不会偷偷上报数据。银行类应用一般不受影响——没解锁 bootloader、没 root，Android 完整性校验照常通过。

### 维护与退出

想停用设备所有者身份：在 Dhizuku 的设置里点「停用设备管理员」，身份解除后再卸载应用。如果卸载前忘了停用，或者卸载时应用已损坏打不开，可以用 ADB 命令强制释放：

```
adb shell dpm remove-active-admin com.rosan.dhizuku/.server.DhizukuDAReceiver
```

卸载后想重装再用，流程跟第一次完全一样——重新激活、重新授权，没有遗留状态需要清理。

---

## 🔗 相关资源

- [Dhizuku官网](https://dhizuku.fewa.skin)
- [Dhizuku文档](https://dhizuku.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/Dhizuku)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/Dhizuku)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/Dhizuku)

---

## 📚 相关文章

- [把 Dhizuku 嵌进 Android 新机初始化链：一次激活，全链通](https://laoguosofts.github.io/posts/dhizuku.html)
- [我给五台 Android 测试机搭了一条自动管控流水线](https://alipymanbu.github.io/posts/dhizuku.html)
