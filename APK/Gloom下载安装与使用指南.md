## ✨ Gloom 是什么 — 碎片时间里的 GitHub 阅读伙伴

Gloom 是 MateriiApps 团队开发的一款 Android 平台 GitHub 只读浏览客户端，核心洞察来自一个简单的场景拆分：GitHub 首先是协作平台——写代码、发 Issue、审 PR、合并分支，每一步都需要键盘和通知；但你不可能一直在工位上。通勤地铁上想翻一眼某个开源项目的 Release Notes，睡前想追几个 Issue 的讨论，排队等餐时想把 Star 列表里的仓库扫一遍看有什么新动静——这些时刻你不想敲代码、不想回评论、不想被通知弹窗打扰，只想安静地读。

GitHub 官方 App 并没有为这个场景专门设计：它带着通知中心、Review 入口、Issue 创建按钮，功能多了也更重，而且默认会推一堆你可能不想在碎片时间看的提醒。Gloom 的出发点就是把 GitHub 拆成两半——协作留给网页端和官方 App，阅读归 Gloom。

它明确不做代码编辑或提交、不做 Issue / PR 创建、不做 Review 发起或合并、不做通知推送、不做 Issue 搜索——把边界摆在明面上的好处是，用的时候不会处处觉得"怎么点不动"，它只负责让你读到，剩下的留给电脑。

在开源安卓生态的第三方 GitHub 客户端中，Material You 完整实现是 Gloom 的独特主张：主色不是固定品牌色，而是从壁纸里动态取；深浅色完全跟随系统；启动图标可切换 Catppuccin 等社区图标包；代码纯 Kotlin 实现，GPL-3.0 开放。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://gloom.boop.uno/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜索「Gloom」；PC 用户建议直接在浏览器访问 GitHub 网页版

---

## 🔧 安装步骤 — Android APK 侧载流程

Gloom 不在部分地区的 Google Play 上架，最通用的安装方式是下载 APK 手动侧载。流程分三步：下载、授权、安装。

**第一步：获取 APK 文件**

从上面的下载入口页面找到对应设备架构的安装包，通常 arm64-v8a 是主流机型的选择。下载后在文件管理器里找到 APK 文件。

> [!WARNING]
> 第三方来源安装前务必核对文件大小和版本号。Gloom 的官方 Release 页面可以看到每个版本的文件哈希值，建议比对 SHA256 确保未被篡改。

**第二步：开启未知来源授权**

Android 系统默认禁止非应用商店来源的安装，需要手动授权：

1. 进入系统 **设置 → 安全 → 应用安装权限**（不同厂商系统路径略有差异，小米在「设置 → 隐私保护 → 特殊权限设置 → 安装未知应用」）
2. 找到你用来打开 APK 的文件管理器或浏览器
3. 打开「允许安装未知应用」开关

**第三步：执行安装**

回到文件管理器，点击 APK 文件，系统弹出安装确认提示。点击安装按钮，等待几秒即可完成。安装完成后首次打开会跳转到 GitHub 登录页面，通过 OAuth 授权后就能访问你的 Star 列表、Activity Feed 等个人数据。

---

## 📖 使用入门 — 从仓库浏览到 Material You 体验

### 打开一个仓库，第一屏能看到什么

在 Gloom 里打开任意一个 GitHub 仓库，首屏从上到下依次呈现：仓库名称（owner/repo 格式）、动态描述、Star / Fork / Watch 三个统计数字、默认分支标签、渲染后的 Markdown README、以及根目录的文件和子目录列表。和 GitHub 网页版相比，Gloom 把不重要的侧边栏（License 详情、Used By、Contributors 网格等）精简掉了，首屏更聚焦在「这个仓库是干什么的 + 核心文件结构」。

首屏有四个可交互区域值得熟悉：

**顶部操作栏**：右上角的星星图标可以一键 Star 或取消 Star，操作后 Gloom 会刷新本地 Star 列表；旁边是分享按钮，调起系统分享面板把仓库链接发给别人。Fork 信息只读展示，Gloom 不支持 Fork 仓库。

**README 区域**：这是 Gloom 读仓库的核心内容，Markdown 渲染器做了移动端适配——标题层级不会过挤，代码块支持横向滚动，图片自动缩放适配屏幕宽度，相对路径链接可以在 Gloom 内部跳转到对应文件。如果 README 很长，Gloom 会自动折叠后半部分，点展开按钮可以完整查看。

**文件列表**：根目录下的文件和子目录按字母排序显示。点目录会进入子目录层级，点文件会按类型渲染——代码文件用语法高亮、图片文件直接展示、二进制文件提示无法预览。在文件树的任意层级，按系统返回键就能回到上一层目录。

**底部 Tab 栏**：仓库页底部有几个 Tab 可切换——Code（文件）、Issues、Pull Requests、Release、Commits，进入对应浏览模块。长按仓库名可以弹出菜单，可能包含跳转到 owner 用户主页的快捷入口。

### 深浅色主题，完全跟随系统

Gloom 的设计哲学是不做独立主题管理——它相信系统的决定。深浅色模式直接读取 Android 系统的「深色/浅色」显示设置：系统设为深色，Gloom 全程深色背景；系统设为浅色，全程浅色背景；自动切换模式下跟着系统在早/晚自动切换。

在 Android 13+ 上，Material You 动态取色会在深浅色切换时自动计算对应的深色/浅色调色板——不是简单反色，而是 Material Design 3 规范里的配色映射，颜色对比度符合无障碍标准。主题切换时有大约 200-300ms 的颜色渐变动画，整个界面从一套配色平滑过渡到另一套，不会出现瞬间跳色。代码高亮也会跟随深浅色切换：深色模式下类似 GitHub 深色主题（深色背景 + 亮色文本），浅色模式下类似 GitHub 浅色主题。

Gloom 没有内置「强制深色/强制浅色」的开关。如果你在系统是浅色模式时想让 Gloom 单独用深色，Android 14+ 可以在系统设置里给单个应用强制深色（设置 → 显示 → 深色模式）；Android 11-13 部分厂商系统提供了应用级深色覆盖功能，搜「深色模式」或「夜间模式」能找到。强退重开 Gloom 通常就能拿到最新的系统主题设置。

### 壁纸驱动的动态取色 — 换壁纸即换装

Material You（原名 Material Design 3）是 Google 在 Android 12 上引入的设计语言核心特性——用户的手机壁纸是应用配色的种子，系统从壁纸里提取若干种颜色生成调色板，所有实现了 Material You API 的应用都从这个调色板里取色。换壁纸，所有应用的 UI 颜色自动同步更新。Gloom 是 Material You 的早期积极实现者，整个 MateriiApps 系列（Gloom、Upvote、Unbound 等）都是完整的 Material You 应用。

具体的取色效果因壁纸主色而异：

| 壁纸主色调 | Gloom 界面效果 |
|---|---|
| 暖紫、薰衣草色 | 按钮、卡片底色变成紫色系深浅色 |
| 绿色、薄荷色 | 整屏冷青色基调 |
| 蓝色、靛蓝色 | 海蓝系配色 |
| 红色、橙色调 | 暖红系配色 |
| 深色壁纸 | 动态取色提取高亮色作为 accent |

Gloom 的 Material You 取色实现覆盖了几乎所有可视觉感知的界面元素：顶部导航栏 Tab 选中色、底部操作栏图标高亮色、各种对话框的确认/取消按钮、首页 Activity Feed 的卡片边框和阴影、仓库详情页的统计数字卡片背景、Issue / PR 评论卡片底色、应用内各种功能图标（Material Symbols 库着色）、以及代码高亮配色——代码高亮会参考 Material You 的 color scheme，让代码颜色和界面主色协调。

Android 不同版本的 Material You 取色能力有细微差异：Android 12 提供基础动态取色（从壁纸提取 4 种颜色生成调色板）；Android 13 增加了基于用户选择的「基础色 + 动态色」切换；Android 14 引入更精细的色板生成算法；Android 15+ 进一步优化深色模式下的对比度。Android 11 及以下不提供 Material You API，Gloom 会回退到一套精心设计的内置中性配色方案，功能完整，只是少了壁纸联动这层视觉特色。

Gloom 完全用 Kotlin 编写，Material You API 封装在 Jetpack Compose 的 Material 3 组件里——整个 UI 层用 Compose 实现，动态取色是零成本接入，不需要额外代码，这也是为什么 Gloom 的取色体验比很多老 View 系统写的应用更流畅。

> [!TIP]
> Gloom 支持切换启动图标（Catppuccin 等社区图标包），但图标包只换桌面图标外观，不影响应用内部的深浅色主题。两者独立运作，互不干扰。

---

## 🔗 相关资源

- [Gloom官网](https://gloom.boop.uno)
- [Gloom文档](https://gloom-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%AD%A3%E7%89%88/Gloom)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%AD%A3%E7%89%88/Gloom)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%AD%A3%E7%89%88/Gloom)

---

## 📚 相关文章

- [我装了 Gloom，终于在通勤路上能好好刷 GitHub 了](https://laoguosofts.github.io/posts/gloom-docs.html)
