## ✨ Tasker 是什么——Android 自动化的瑞士军刀

Tasker 是 Android 平台上功能最强大的自动化工具之一。它允许用户通过"配置文件"和"任务"的组合，让手机在满足特定条件时自动执行一系列操作——从简单的音量调节到复杂的多步骤工作流，几乎覆盖了 Android 系统能提供的所有接口。

它的核心思路是 **If-This-Then-That**：当某个"上下文"（Context）成立时，自动执行预定义的一组"动作"（Action）。上下文可以是时间、位置、应用状态、电量水平、网络连接状况……动作则涵盖了系统设置、网络开关、应用控制、文件操作、HTTP 请求等数百项能力。

Tasker 没有花哨的界面，也不提供现成的"模板商店"——它给你的是一块空白画布和一套完整的系统 API，你想让手机做什么，就自己把逻辑编排出来。这让它的学习曲线略显陡峭，但换来的是无与伦比的灵活性。对于喜欢折腾、追求效率的 Android 用户来说，Tasker 能把手机从"被操作的对象"变成"主动干活的助手"。

典型的使用场景包括：到家自动开 Wi-Fi、开车时自动静音并回复短信、电量低时自动关闭后台同步、定时切换铃声模式……只要 Android 系统能做的事，Tasker 基本都能编排成自动化流程。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://tasker.fewa.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜 Tasker（注意区分同名 App，认准开发者是 joaomgcd）

---

## 🔧 安装与首次配置

Tasker 的安装本身很简单，但 Android 系统对自动化 App 有诸多权限限制，装完之后需要做一些初始化设置才能让它稳定运行。

### 安装步骤

1. **下载 APK**：从下载页获取最新版本的安装包，注意核对文件大小与版本号
2. **允许未知来源**：如果是首次安装非应用商店来源的 APK，需要在系统设置中允许当前浏览器或文件管理器的"未知来源"安装权限
3. **安装并启动**：装完后打开 Tasker，会看到简洁的主界面——底部四个选项卡分别是 Profiles（配置文件）、Tasks（任务）、Scenes（场景）、Vars（变量）

### 必须授予的权限

首次启动 Tasker 时，它会引导你授予几项关键权限：

| 权限 | 作用 | 不授予的后果 |
| --- | --- | --- |
| **无障碍服务** | 让 Tasker 能够模拟点击、滚动、启动应用 | 无法执行界面自动化类 Action |
| **通知访问** | 监听通知状态、读取通知内容 | 无法触发通知相关的 Event |
| **精确位置** | 支持基于位置的 Profile 触发 | Location Context 精度降低 |
| **后台弹出界面** | 在锁屏状态下也能显示 Tasker 的提示 | 锁屏时 UI 类 Action 被拦截 |
| **忽略电池优化** | 防止系统在后台杀掉 Tasker | Profile 在后台停止触发 |

> [!WARNING]
> **电池优化白名单是重中之重**。Android 6.0+ 的 Doze 机制会把后台 App 进入"打盹模式"，延迟甚至阻止它的闹钟和广播。Tasker 默认会创建一个常驻通知作为前台服务来对抗 Doze，但某些 OEM 系统（小米、OPPO、vivo、华为）的额外限制层可能会把它强制杀掉。务必在系统设置的"电池优化"里把 Tasker 设为"不优化"。

### OEM 系统的额外设置

国内厂商的定制 ROM 对后台管理尤其严格，除了电池优化，还需要检查：

- **自启动权限**：小米在"应用设置 → 应用管理 → Tasker"里开自启动；OPPO 在"自启动管理"里添加；华为在"启动管理"里设为手动管理并勾选自启动
- **后台锁定**：打开最近任务列表，给 Tasker 加锁（小米是下拉卡片，三星是长按选"锁定"），防止一键清理时被回收
- **省电模式全局限制**：如果手机开着省电模式，先关掉——它会无差别限制所有后台 App

Tasker 内部也要确认运行状态：菜单 → 首选项 → 监控，把"屏幕关闭监控"全部开启；菜单 → 首选项 → 其他，确保"开机后自动启动"已打开；最后看主界面右下角的开关是否为绿色（绿色 = Tasker 正在运行）。

---

## 📖 使用入门：从四大构建块到实战自动化

### Tasker 的基本架构

理解 Tasker 需要先掌握它的四个核心概念——Profile、Task、Action、Context。

**Profile（配置文件）** 是触发条件的容器。一个 Profile 由一个或多个 **Context（上下文）** 通过 AND/OR 逻辑组合而成。当 Context 条件满足时，Profile 被"激活"，自动执行绑定的 Enter Task；条件消失时执行 Exit Task。

**Task（任务）** 是 Action 的有序集合。一个 Task 可以包含 1 到上百个 Action，从上到下依次执行。Task 分两种：**具名任务**有名字，可以被多个 Profile 引用、可以手动触发、可以导出分享；**匿名任务**内嵌在 Profile 内部，适合一次性的简单逻辑。大多数场景下推荐用具名任务——复用方便、便于管理。

**Action（动作）** 是 Tasker 能执行的最小操作单元。点击 Task 编辑界面底部的 + 号可以按分类浏览：Net 类管理 Wi-Fi、蓝牙、移动网络、VPN；Phone 类处理通话、短信；System 类控制音量、屏幕、飞行模式；Alert 类负责弹窗、通知、震动；还有 File、Media、Plugin、Script 等十几个分类，每个分类下又有数量不等的具体动作。

### 条件控制与流程跳转

Task 内的 Action 默认从上到下顺序执行，但通过流程控制指令可以实现条件分支和循环：

- **If/Else/Endif**：条件分支，检查变量值或系统状态来决定走哪条路
- **Goto** + Label：跳转到 Task 里标记的位置，配合 Label 可以实现循环或清理逻辑
- **Perform Task**：暂停当前 Task，完整执行另一个 Task 后返回——这是实现代码复用的关键机制
- **Stop**：立即终止当前 Task，不执行剩余任何 Action
- **Return**：从被调用的子任务返回到调用方（只在 Perform Task 调用的 Task 里有意义）

> [!TIP]
> 子任务里用 **Return** 而不是 **Stop**。Stop 不区分位置——If/Else 块内、For 循环内、嵌套 Perform Task 内，Stop 会终止**整个当前 Task**，包括调用方。如果你只想退出子任务、让调用方继续跑下去，用 Return。

### 用 Perform Task 构建函数库

没有 Perform Task 时，如果两个 Profile 需要相同的动作序列，只能在每个 Profile 的 Task 里各写一遍——复制粘贴、改一处漏一处。Perform Task 让 Task 调用另一个 Task 作为"子任务"，支持传参和返回值，把 Tasker 的 Task 从"串行命令队列"升级成了可互相调用的函数体系。

**传参方式**：Perform Task 调用时可以给目标 Task 传递命名参数，目标 Task 里以局部变量形式自动可用。比如调用方传 `Action = "On"`，被调用方直接用 `%Action` 就能拿到值。

**返回值**：被调用 Task 通过 **Return** 动作返回一个值，调用方在 Perform Task 的设置里指定把返回值存入哪个变量。

一个实用的例子——构建统一的"省电模式"子任务：

```
# 定义通用子任务：Toggle Power Save
Task：Toggle Power Save
  参数：%Action = "On" 或 "Off"

  If → %Action = On
    Wifi → Off
    Bluetooth → Off
    Display Brightness → 最低
    Screen Timeout → 15秒
    Do Not Disturb → 完全静默
  Else
    Wifi → On
    Bluetooth → On
    Display Brightness → Auto
    Screen Timeout → 2分钟
    Do Not Disturb → Off
  Endif
```

然后多个 Profile 各自调用同一个子任务：晚上自动省电的 Profile 传 `Action = On`，电量低于 20% 的 Profile 也传 `Action = On`，充电时恢复的 Profile 传 `Action = Off`。逻辑只写一遍，多处复用，改的时候也只改一处。

> [!NOTE]
> **匿名 Task 无法被 Perform Task 引用**。想做函数库的 Task 一定要在任务选项卡里创建为具名任务。匿名 Task 没有名字，Perform Task 找不到它。

### Net 动作：网络与连接管理

Net 类包含 Android 所有网络相关的开关和配置，在"到家自动开 Wi-Fi、出门自动关"这类经典场景中是主力。

核心动作包括 Wi-Fi 开关、Wi-Fi Connect（连接指定 SSID）、Bluetooth 开关、Mobile Data（蜂窝数据）、Airplane Mode、VPN 连接、Hotspot（热点）、NFC 等。Android 版本不同，部分开关的可用能力也不同——比如 Android 10+ 限制了 Mobile Data 的直接开关，需要 Shell Command 或 Device Admin 权限辅助；但 Airplane Mode 在 Tasker 6.0+ 里已经原生支持无 Root 开关。

**Bluetooth Auto On/Off** 是一个容易被忽视但很实用的功能。它可以指定"进入家或公司时自动打开蓝牙"以及"蓝牙空闲 N 分钟后自动关闭"，配合 Location Context 实现车载蓝牙或智能手表的自动连接与断开，省电又省心。

**VPN 自动连接** 的用法类似：先在系统设置里配好 VPN，Tasker 通过 VPN 动作触发连接或断开，不管理 VPN 本身的配置。配合位置上下文——进公司 Wi-Fi 时自动断开 VPN，离开时自动连接。

夜间飞行模式是一个进阶玩法：

```
Profile：夜间飞行 + 每 15 分钟检查短信
  Time：23:00 - 07:00
  AND
  State：Variable Value %LastCheckTime > 15 分钟前
Enter Task：
  1. Airplane Mode → Off
  2. Wait → 30 秒（接收短信）
  3. Airplane Mode → On
  4. Variable Set %LastCheckTime, %TIME
```

这样既享受了飞行模式的深度省电，又不会漏掉紧急短信——每 15 分钟醒 30 秒检查一次。

### Phone 动作：通话与短信自动化

Phone 类负责与通话、来电、短信交互的所有操作，包括 Call（拨打电话）、Auto Answer（自动接听）、Auto Silence（来电静音不挂断）、Send SMS（发送短信）、Call End（结束通话）、Record Call（通话录音）等。

开车或开会时自动静音来电是高频需求：

```
Profile：会议中自动静音来电
  State：Bluetooth Connected → 车载蓝牙设备
  OR
  Calendar → 有进行中的会议事件
Event：Phone → Incoming Call
Enter Task：
  1. Auto Silence（来电静音，对方还在响铃但本机不响）
  2. Send SMS（自动回复"我正在开车/开会"）
  3. Notify（常驻）→ "已自动回复 %CNUM"
```

这里用到了 `%CNUM` 这个内置变量——它保存来电号码。Tasker 提供了大量系统变量（以 `%` 开头），在 Action 里可以直接引用，比如 `%LOC`（当前位置）、`%BAT`（电量百分比）、`%SMSRF`（短信发送方号码）、`%TIME`（当前时间）等。

短信自动回复还可以做进阶：用 Variable Split 解析短信内容，如果对方发来"在哪里"，自动回复 GPS 位置。不过要注意自动发送短信可能被运营商判定为垃圾短信，建议只在特定条件下开启（比如车载蓝牙连接时），且不要高频发送。

### Doze 期间哪些 Context 能正常工作

Android 的 Doze 模式（打盹模式）在设备静止一段时间后会大幅限制后台 App 的活跃度。不同类型的 Context 受影响程度不同：

| Context 类型 | Doze 期间正常？ | 原因 |
| --- | --- | --- |
| **时间 / 日期** | ✅ | AlarmManager 高优先级 |
| **状态（State）** | ✅ | 系统广播 |
| **事件（Event）** | ✅ | 系统广播 |
| **应用（App）** | ⚠️ | 部分被限制 |
| **位置（Location）** | ❌ | GPS 在 Doze 期间被大幅限制 |

如果发现 Location Context 在屏幕关闭后不触发，这不是 Tasker 的 bug——是 Android 的设计。解决办法是把 GPS 换成 Wi-Fi 或基站粗略检测，或者用 AutoLocation 插件的被动监听模式。

### Tasker 的诊断工具

Tasker 自带几个实用的诊断功能，出问题时先查它们：

**运行日志**（菜单 → 数据 → 运行日志）会记录每个 Profile 的激活状态、每个 Action 的执行结果、变量值的变化。如果看到 Profile 被激活但没执行完整 Action，或者执行间隔异常（应该 1 分钟检查一次结果几小时才检查一次），那就是被系统限制了。

**状态检查**（菜单 → 数据 → 状态）会显示 Tasker 是否在运行、各监控器是否活跃、电池优化状态、最后一次执行时间。正常应该全部显示绿色。

**电池消耗分析**在系统设置 → 电池里看 Tasker 的耗电占比。正常情况后台耗电应该很低（< 10%）——它只是被动监听系统广播。如果耗电超过 20%，可能是 Location Context 在持续 GPS，或者某个死循环 Task 在跑。

---

## 🔗 相关资源

- [tasker官网](https://tasker.fewa.skin)
- [tasker文档](https://tasker-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Tasker)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Tasker)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Tasker)

## 📚 相关文章

- [Tasker 上手记：用了三天，我把手机里一半的重复操作都删了](https://laoguosofts.github.io/posts/tasker-docs.html)

- [Tasker 上手一周，我把手机交给了一个会自己思考的助手](https://alipymanbu.github.io/posts/tasker-docs.html)
