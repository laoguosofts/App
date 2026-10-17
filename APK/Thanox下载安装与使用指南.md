## ✨ Thanox 是什么

Thanox 是一款 Android 平台上的系统级自动化框架应用。它在 Framework 层对系统 API 进行 Hook，让用户能够以规则的形式定义"在什么条件下做什么动作"，从而实现大量原本需要手动操作的自动化流程。

Thanox 的核心能力可以概括为三大方向：一是**情景模式规则引擎**，通过监听系统事件（应用切换、开关屏幕、蓝牙连接等），结合内置的动作库（杀进程、点击界面、发送通知等），构建出高度可定制的自动化规则；二是**隐私身份隐匿**，针对指定应用返回虚拟的设备标识数据（IMEI、Android ID、SIM 卡信息等），防止真实身份被采集；三是**移花接木**，通过替换应用内部的 Activity 跳转，跳过开屏广告或重定向页面路径。

这类系统级工具通常需要 root 权限或 Shizuku 辅助服务来激活 Framework 层的 Hook 能力。对于喜欢折腾 Android、追求自动化效率和隐私保护的玩家来说，Thanox 提供了一个低代码甚至零代码的规则编写体验——用 JSON 就能编排复杂的自动化逻辑，不需要写一行 Java 或 Kotlin。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://thanox.wrwq.fun/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜索 Thanox；部分地区可能需要从官方渠道获取

---

## 🔧 安装步骤

Thanox 的安装过程分为两个阶段：安装 APK 本体、激活 Framework 层服务。由于它需要在系统层面 Hook API，激活环节是必不可少的。

### 第一阶段：安装 APK

1. 从下载入口获取最新版本的 APK 文件，注意核对文件名中的版本号和发布日期。
2. 将 APK 传输到手机内部存储，通过文件管理器点击安装。
3. 如果系统弹出"未知来源安装"的提示，前往系统设置中为当前文件管理器授予安装未知应用的权限。

> [!WARNING]
> 安装前请确认设备架构（arm64-v8a 为主流机型）和 Android 版本兼容性，Thanox 对 Android 11 及以上版本的支持较为完善，旧版本系统可能存在功能缺失。

### 第二阶段：激活服务

Thanox 提供两种激活方式，根据你的设备情况选择：

| 激活方式 | 适用场景 | 说明 |
|---|---|---|
| Root 权限 | 已 Magisk / KernelSU root | 最稳定，所有功能完整可用 |
| Shizuku | 无 root 但有 adb 调试 | 需先安装 Shizuku 应用并通过 adb 授权 |

激活完成后，Thanox 主界面会显示已激活状态，悬浮窗权限和通知权限也建议一并授予，否则部分动作（如 Toast 提示、杀进程通知）无法正常工作。

---

## 📖 使用入门

Thanox 的功能体系可以按三大模块来理解：情景模式规则引擎是核心，隐匿和移花接木是两个独立功能。下面从最常用的情景模式开始，逐步展开。

### 情景模式：用 JSON 编写自动化规则

情景模式的基本单位是**规则（Profile）**，每条规则由三个要素构成：`condition`（触发条件）、`actions`（执行动作）和可选的 `delay`（延迟毫秒数）。当系统中某个事件发生时，Thanox Framework 层会把事件的参数注入到规则引擎中，引擎判断条件是否成立，成立则依次执行动作。

规则的条件判断基于一组叫 **Facts** 的事实参数。这些参数涵盖了系统运行中几乎所有你能感知到的事件类别：

**应用生命周期类 Facts** 是最常用的一组。当应用安装、卸载、更新时，`pkgAdded`、`pkgRemoved`、`pkgUpdated` 会变为 true，同时 `pkgName` 携带被操作应用的包名。前台应用切换时，`frontPkgChanged` 为 true，`from` 和 `to` 分别记录切换前后的包名。Activity 级别还有更精细的事件：`activityResumed` 表示某个 Activity 已经显示并可交互，`componentNameAsShortString` 给出了 Activity 的组件短名——这两个参数配合使用，就是跳过开屏广告的关键。

```
# 微信退到后台时触发
frontPkgChanged == true && from == "com.tencent.mm"

# 某个广告 Activity 显示时触发
activityResumed == true && componentNameAsShortString == "com.nowcasting.activity/.SplashActivity"
```

Facts 参数有一个很强的特性：同一个参数可以在 condition 里用来判断，然后在 action 里直接引用值，不需要再硬编码一遍。比如条件里写了 `pkgName == "com.tencent.mm"`，动作里就可以直接用 `killer.killPackage(pkgName)`，此时 `pkgName` 已经是条件匹配到的那个值了。

### 实用案例：从跳过广告到一键锁屏

理解了 Facts 和动作的配合方式，就可以动手写规则了。以下几个是社区中流传最广的实战用法：

**跳过开屏广告**是 Thanox 情景模式最经典的场景。核心思路是：监听目标广告页 Activity 的 resumed 事件，延迟几百毫秒等页面渲染完成，然后调用 `ui.findAndClickViewByText` 自动点击"跳过"按钮。delay 参数很重要——它是触发后的执行延迟，给广告页留出渲染时间。

**微信退后台自动杀进程**则利用了 `frontPkgChanged` 事件，条件判断 `from` 是否为微信包名，动作调用 `killer.killPackage` 彻底清理。需要注意的是，杀掉微信意味着收不到消息推送，重度用户可能更适合加上息屏条件：`screenOff == true && from == "com.tencent.mm"`，只在息屏后才触发。

**一键锁屏**通过 Thanox 的快捷方式引擎实现。先在引擎中创建一个桌面快捷方式并设值为 `turn screen off`，然后写一条规则监听 `shortcutLaunched == true && shortcutValue == "turn screen off"`，触发后执行 `power.sleep(0)` 即可。类似的玩法还有蓝牙连接提示、GPS 自动开关、时间段内 5G/4G 切换等。

> [!TIP]
> 调试规则时，多放 `ui.showShortToast` 来确认规则真的被触发了——这是最直观的验证方式。规则 name 必须唯一，同名规则会互相覆盖。字符串中的双引号需要用 `\"` 转义。

### 隐匿：为应用打造虚拟身份

隐私身份隐匿是 Thanox 的另一个独立模块，它的工作原理和情景模式不同——情景模式是事件驱动的规则执行，隐匿则是被动的 API Hook 拦截。

开启隐匿后，被选中的应用调用系统 API 读取设备信息时，Thanox Framework 层会拦截这些调用并返回你在模板里配置的虚拟数据。比如某 app 调用 `TelephonyManager.getDeviceId()` 想读取你的 IMEI，Thanox 会返回模板里填的虚拟 IMEI，应用看到的是假数据，真实身份不会泄露。

使用隐匿需要两步：先创建**身份模板**，再把模板分配给需要隐匿的应用。模板里可配置的字段覆盖了常见的敏感信息：IMEI/MEID（手机设备标识）、SIM 卡序列号 ICCID、Android ID（重置即变的设备标识）、Build Serial（硬件序列号）、WiFi MAC 地址等。Thanox 不会帮你生成虚拟值，需要自己填——IMEI 可以找合法号段编，Android ID 随便写 16 位十六进制串，ICCID 写 20 位数字即可。

多个模板的场景包括：工作 app 用一套模板、个人 app 用另一套模板实现账号隔离；某些 app 会根据设备标识做 AB 测试，你可以换不同模板触发不同版本；不同国家的 ICCID 可以模拟地区伪装。

### 移花接木：Activity 替换与查找

移花接木功能可以把应用内部的某个 Activity 替换成另一个——最常见的用途就是跳过开屏广告。你需要知道两个信息：被替换的广告页 Activity 名称，以及替换后的主界面 Activity 名称。

Thanox 内置了一个找 Activity 的工具。打开设置 → 开发者设置 → 开启"显示当前 Activity"，屏幕上会出现一个悬浮窗，无论你在哪个 app 界面，浮窗都会实时显示当前 Activity 的组件全名（格式为 `包名/.Activity短名`）。切换 Activity 时浮窗内容自动更新，点击浮窗就能把组件名复制到剪贴板。

实战流程很简单：打开有开屏广告的 app，广告页出现时看一眼浮窗记下广告 Activity 名称，等主界面出来再记下主界面 Activity 名称，然后去移花接木模块添加替换规则即可。这个浮窗功能比 adb dumpsys 或第三方探测器方便得多——实时、一键复制、不需要电脑。

### 规则编写的注意事项

Thanox 的规则引擎使用 MVEL 表达式语法，条件判断和动作都是用它来解析的。几个常见的坑：

- `delay` 是触发后的执行延迟，不是定期触发。Thanox 没有内置的"每 X 分钟"定时器，周期性任务需要用其他机制绕过。
- 规则是 JSON 格式，多个规则放在同一个方括号数组里，用逗号分隔。
- 同一个 Facts 参数可以同时出现在条件判断和动作执行中，减少重复硬编码。
- Activity 生命周期正常顺序是 Created → Resumed → Paused → Destroyed。如果一个 Resumed 的 Activity 被其他 Activity 临时覆盖（比如支付弹窗盖在微信上），它会走 Resumed → Paused → Resumed，不会重新 Created。

---

## 🔗 相关资源

- [Thanox官网](https://thanox.wrwq.fun)
- [Thanox文档](https://thanox.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Thanox)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Thanox)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Thanox)

---

## 📚 相关文章

- [Thanox 上手三天：终于有人把 Android 的自启动乱象治住了](https://laoguosofts.github.io/posts/thanox.html)
- [Thanox 上手体验：给你的 Android 装个"全自动管家"](https://alipymanbu.github.io/posts/thanox.html)
