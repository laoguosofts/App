## ✨ AIDE 是什么

AIDE 是一款专为 Android 设备设计的集成开发环境，让你在手机或平板上就能完成 Android 应用的编写、编译和运行。它内置了代码编辑器、编译器、APK 打包工具和调试器，完整覆盖了从代码到安装包的整条开发链路。

对想在移动端折腾开发的人来说，AIDE 的价值很直接——不必开机、不依赖电脑，掏出手机就能写几行 Java、改个布局、编个 APK 直接装到自己设备上跑。它支持 Java/XML 原生 Android 开发，也能通过 NDK 写 C/C++ 原生代码，还兼容 PhoneGap 做跨平台混合应用。日常的小工具、学习练手项目、甚至一些完整的 Android App，都能在 AIDE 里从零到一完成。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://aide.fewz.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜索 AIDE 即可找到正式版；国内 Android 应用商店可留意各渠道的分发版本

---

## 🔧 安装与环境准备

AIDE 是 Android 应用，安装过程和普通 APK 没有本质区别，但有几个需要注意的细节。

**第一步：允许未知来源**

如果是通过 APK 文件安装而不是走 Google Play，需要先在设备设置里打开「允许安装未知来源应用」。不同品牌的路径略有差异：

1. 进入 **设置 → 安全**（或 **设置 → 应用 → 特殊访问权限**）
2. 找到 **安装未知应用** 选项
3. 给你的文件管理器或浏览器授权允许安装

> [!WARNING]
> 只给你信任的那个渠道授权，装完可以关掉，避免其他来源静默安装。

**第二步：安装 AIDE APK**

下载好 APK 文件后，用文件管理器找到它、点击安装。首次启动时 AIDE 会请求存储权限——这是必须的，因为项目文件、编译产物都要在本地读写，拒绝权限后编辑器和构建功能无法正常工作。

**第三步：（可选）安装移动端 NDK**

AIDE 自身不带 C/C++ 编译器。如果要开发 NDK 项目（游戏、高性能计算、已有 C/C++ 库移植），首次创建 NDK 项目时 AIDE 会自动提示下载移动端版本的 NDK，也可以手动在 **Settings → Install Mobile NDK** 里触发。

> [!NOTE]
> 移动端 NDK 体积不小，建议在 Wi-Fi 环境下载。如果你只写 Java/XML，这一步跳过即可。

---

## 📖 开始你的第一个 AIDE 项目

装好之后打开 AIDE，首页会列出已有项目和新建入口。下面从零开始走一遍典型流程。

### 创建项目

点 **New Project**，AIDE 提供几种模板，最常用的是 **Android App / Java/XML**——标准 Android 原生开发，生成 Activity、布局文件、Manifest 这些标准结构。选好模板、填包名和应用名，AIDE 会自动生成可编译的脚手架代码。

如果你需要用 C/C++，有两条路：

| 模式 | 适用场景 | 模板选择 |
|------|---------|---------|
| Java + C/C++ JNI 混合 | 主体 Java、关键逻辑放原生层 | Android App / C/Java/Xml |
| 纯 C/C++ Native Activity | 整个 Activity 用原生代码实现 | Android App / C/C++ Native Activity |

两种模式构建时 AIDE 都会分别编译 Java 和 C/C++ 部分，最后统一打包 APK。注意 NDK 项目**只能在 ARM 架构设备上运行**，x86 模拟器无法编译 C/C++。

### 代码编辑体验

AIDE 编辑器对 Java、C/C++、XML、HTML、CSS、JavaScript、Gradle 等语言都提供语法高亮，不同元素用不同颜色区分——关键字蓝色、类型名深青、字符串绿色、注释灰色斜体、XML 属性值橙色。代码补全默认开启，输入几个字符后会弹出候选。

Premium 版可以在编辑器主题里切换配色方案或自定义各元素的颜色，免费版用固定的默认配色就够用。

### 调试与断点

AIDE 支持断点调试，操作方式和桌面 IDE 类似：

1. 编辑器左侧行号区域，**点击你想停住的那一行**
2. 出现红色圆点 = 断点已设置，再点一次取消
3. 用 **Debug** 而不是 **Run** 启动应用
4. 程序执行到断点处自动暂停，编辑器高亮当前行，右侧面板显示变量状态

> [!TIP]
> 断点只对 Java 代码生效，XML、PhoneGap HTML、C/C++ NDK 不支持调试。如果设置了断点但程序不停，先确认三个前提：用的是 Debug 启动、执行路径确实经过那行、编译产物和源码对得上（Clean/Rebuild 再试）。

### 大项目性能优化

项目文件多了之后 AIDE 可能变慢、保存卡顿、构建时间拉长。以下几点是最有效的优化手段：

**文件与项目组织**

- Java 源文件控制在 **500 行以内**——超过这个数，代码补全、语法高亮、触屏滚动都会变慢，建议把内部实现提取到 Helper/Util 类、按功能拆小文件
- 资源文件提前压缩，PNG 用 TinyPNG 或转 WebP 格式（Android 原生支持，体积更小）
- 项目放在**设备内部存储**而非 SD 卡或云盘同步目录，IO 速度直接影响构建效率

**设备层面**

- 保持至少 **1GB 可用存储空间**——APK 构建会产生大量中间产物
- 构建前清理后台应用，避免内存竞争导致构建被系统杀掉
- Root 设备可以绕过存储限制，直接访问 `/sdcard/` 和 `/data/`，IO 性能略好

**编辑器设置**

| 设置项 | 建议值 |
|--------|--------|
| Auto Completion Delay | 200~300ms（减少无意义弹窗） |
| Show Whitespace | 关闭（减少渲染开销） |
| Auto Save | 保持开启，可适当调大延迟 |
| Theme | 浅色主题通常比深色渲染快一点 |

> [!NOTE]
> 如果项目已经超过 100 个源文件，务实的做法是主力开发放在电脑上用 Android Studio 或 IntelliJ，AIDE 作为移动端查看和小修改的辅助工具，通过 Git 同步两边代码。

---

## 🔗 相关资源

- [AIDE官网](https://aide.fewz.tech)
- [AIDE文档](https://aide-zh-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/AIDE)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/AIDE)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/AIDE)

---

## 📚 相关文章

- [AIDE 深度盘点：把 Android 开发台搬进手机，它到底能扛多少活？](https://laoguosofts.github.io/posts/aide-zh-docs.html)
- [AIDE 深度盘点：手机上写 Android 应用到底能走多远](https://alipymanbu.github.io/posts/aide-zh-docs.html)
