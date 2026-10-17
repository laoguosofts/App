## ✨ Widgetsmith 是什么

Widgetsmith 是 iOS 平台上一款广受欢迎的小组件自定义工具。它让用户能够从零开始创建属于自己的桌面小组件——从时间、天气、步数、日历这些常用的小组件类型，到完全自由的文本、图片、倒计时等内容，都可以通过 Widgetsmith 实现。

核心亮点在于**完全的视觉控制权**：尺寸、颜色、字体、边框、背景，每一项细节都可以手动调整。普通小组件通常只有几种预设样式可选，而 Widgetsmith 打开的是一个可以随意涂抹的画布。另一个杀手级功能是**定时切换**（Timed Widget）——你可以在不同时段让同一个小组件自动变身为不同内容，比如早上显示天气、下午显示步数、晚上显示日程，让桌面在一天中随场景动态切换。

适合谁用？如果你觉得 iOS 自带的小组件太单调、想让桌面更有个性，或者想把多个信息源整合到同一个位置按时间自动轮转，Widgetsmith 都是一个值得花时间折腾的工具。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://widgetsmith.gerw.hair/download.html) —— 下载页整理了当前可用的版本和安装说明
> - App Store：在 iPhone 上打开 App Store 搜 "Widgetsmith"，下载安装

---

## 🔧 安装步骤与注意事项

Widgetsmith 只能在 iPhone 和 iPad 上使用，需要 **iOS 14.0 或更高版本**。安装过程就是在 App Store 下载的标准流程，没什么特别操作，但有几点值得留意：

1. **确认系统版本**：安装前先去 设置 → 通用 → 关于本机 看一眼系统版本。低于 iOS 14 用不了小组件功能，Widgetsmith 也装不上。

> [!NOTE]
> 如果你是 iPhone 用户，大概率系统版本已经满足；但 iPad 旧机型的用户不妨多确认一下。

2. **下载体积**：Widgetsmith 本体不大，几十 MB 级别，但运行过程中会读取你的日历、步数、照片等数据——这些权限第一次打开 App 时会逐个弹窗询问，按需授权即可。

3. **小组件列表不出现怎么办**：刚装完 Widgetsmith，长按桌面 → 点左上角加号 → 搜索 "Widgetsmith"，如果搜不到，这是 iOS 的已知缓存问题。90% 的情况重启一下 iPhone 就能解决。不行就试一个非常规但有效的 trick：去 设置 → 通用 → 语言与地区 → 日历，临时切到另一种日历格式（比如佛教历），再切回公历——这个操作会强制 iOS 重建整个小组件注册表。

> [!WARNING]
> 少数用户反映重启和日历切换都不管用时，打开 Widgetsmith App 一次再关掉，也能被 iOS 识别到。装完之后建议先打开 App 随便点两下再回桌面。

---

## 📖 使用入门

### 先搞懂 iOS 小组件的刷新规则

Widgetsmith 所有功能都依赖 iOS 本身的小组件调度机制，理解这一点能帮你对一些"为什么没准时刷新"之类的问题建立合理预期。iOS 的规则大致是这样：

- 每个 App 的小组件每 **15~60 分钟**最多刷新一次，由系统智能调度，不是精确准时
- 设备在**充电 + 静止**状态时刷新最频繁
- **低电量**或**后台 App 太多**时刷新速度会明显下降
- **夜间**（通常 23:00 到次日 7:00）刷新极少甚至停止

这意味着：你定了 08:00 切天气、12:00 切步数，实际切换可能在 08:00~09:00 之间某一刻发生。**它不是闹钟**。想让刷新更及时，可以做三件事：保持 Widgetsmith 最近用过（iOS 对近期打开的 App 给更高刷新优先级）、在 设置 → 通用 → 后台 App 刷新 里确保 Widgetsmith 的后台刷新开关是开的、避免长期低电量模式。

### Timed Widget：让同一个小组件按时间自动切换

定时切换是 Widgetsmith 和 iOS 原生智能叠放最大的区别。原生的智能叠放是根据使用频率随机轮转，Widgetsmith 让你精确规定每个时段显示什么：

- 打开 Widgetsmith App → 选一个小组件尺寸（Small / Medium / Large）
- 创建或编辑一个小组件 → 找到 Timed Widget 选项
- 添加时间段：每个时段指定起止时间和该时段要显示的小组件内容
- 保存后把这个 Timed Widget 拖到桌面

三种桌面切换方案的对比一目了然：

| 方案 | 控制精度 | 系统开销 | 自定义程度 |
|------|----------|----------|------------|
| Widgetsmith Timed Widget | 精确到时段（1~23 小时） | 低 | 完全自定义 |
| iOS 智能叠放 | 按使用频率随机 | 低 | 系统推荐为主 |
| 手动长按切换 | 手动 | 零 | 完全手动 |

### 自定义视觉：从颜色到字体到渐变

不满意内置主题？每一项视觉细节都可以手动调：

**颜色**：任何有 tint color 或 background color 标记的地方都能自定义。点进去可以用色板选颜色，也可以切到 Custom 模式用滑块精确调 RGB 值。Widgetsmith 最多可以帮你保存 **6 个自定义颜色**，之后所有小组件都能从这 6 个里直接选，不用每次重新调色。

**字体**：内置 20+ 种字体，覆盖无衬线（Helvetica、Inter、SF Pro）、衬线（Georgia、Times New Roman）、手写风（适合 Custom Text）、等宽（适合数字和日期显示）几个大类。Premium 用户字体选择更多。

**边框**：Border 栏可以开关边框、调宽度（1px ~ 8px）、选边框颜色，边框颜色还能和 tint 或 background 联动。Large 尺寸 + 细边框 + 纯色背景是一个经典的海报感组合。

**渐变背景**：Background 不止是纯色，很多主题支持双色渐变——选起止颜色和方向（水平、垂直、对角线、辐射），渐变配半透明 tint 能做出非常高级的视觉效果。

> [!TIP]
> Widgetsmith 有个好用的快捷功能：从一张照片自动提取主题配色。编辑任意小组件的 Aesthetic → 点右下角三个点 → Generate Theme from Photo，选一张照片，Widgetsmith 会从照片的主色和辅色里自动提取一套 tint + background + border 的配色方案，预览满意就一键应用。

### 常见问题速查

**小组件卡住不动了**：大概率是 iOS 侧的问题，不是 Widgetsmith 的代码 bug。先打开 Widgetsmith App 一下再关掉——相当于强制告诉 iOS "这个 App 还活着"，系统会重新调度它的刷新。还不行就重启 iPhone。

**升级 iOS 后小组件变黑白**：iOS 17 刚发布时出现过这个系统 bug——升级后第一次启动 iPhone，所有 Widgetsmith 小组件会变黑白灰，通常等 1~5 分钟自动恢复。Apple 已经在后续小版本里修复了。如果你现在还遇到，检查一下 iOS 和 Widgetsmith 是不是都更到最新版。

**出现蓝色方块**：意味着某个子小组件加载的照片或数据超过了 iOS 允许的小组件内存上限。解决办法：重启设备、把 Large 换成 Medium 或 Small、换小一点不那么高清的照片。

---

## 🔗 相关资源

- [Widgetsmith官网](https://widgetsmith.gerw.hair)
- [Widgetsmith文档](https://widgetsmith-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/Widgetsmith)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/Widgetsmith)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/Widgetsmith)

---

## 📚 相关文章

- [把 iPhone 从"解锁—开App"变成"锁屏一眼"：Widgetsmith 嵌进晨间启动流的实操](https://laoguosofts.github.io/posts/widgetsmith-docs.html)
- [我怎么把 Widgetsmith 嵌进每天的晨间工作流：从三步切换到一眼扫完](https://alipymanbu.github.io/posts/widgetsmith-docs.html)