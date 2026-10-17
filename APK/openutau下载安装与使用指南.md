## ✨ OpenUtau 是什么

OpenUtau 是 UTAU 的开源重制版本，一个把「输入歌词和音符就能合成歌声」这件事做得更现代、更易用的歌声合成编辑器。它完全免费开源，兼容原版 UTAU 的工程和绝大多数声库，同时引入了 DiffSinger、ENUNU、VOICEVOX 等新一代歌声引擎的支持。

和原版 UTAU 相比，OpenUtau 的优势集中在几个地方：跨平台原生支持（Windows / macOS / Linux 都能跑）、内置 Worldline64 重采样器开箱即用、Piano Roll 编辑器更流畅、工程级表达式管理让批量调音不再繁琐。对于刚接触歌声合成的新手来说，OpenUtau 省去了原版 UTAU 那些令人头疼的环境配置和兼容性问题，装上就能开始做。

OpenUtau 支持三大类歌声引擎：UTAU Classic 传统引擎（配合外部重采样器）、DiffSinger 神经歌声引擎，以及 ENUNU / VOICEVOX 等扩展引擎。不同引擎各有偏好——Classic 适合调教老声库、DiffSinger 合成自然度高、ENUNU 在英文歌场景下表现出色。你可以在同一个工程里混合使用不同引擎、按音符切换不同重采样器，灵活度非常高。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://openutau.fedq.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：PC 端请通过上方下载页获取最新版本；移动端暂无正式发行版本

---

## 🔧 Windows 安装 OpenUtau

OpenUtau 在 Windows 上提供两种分发形式：**安装版**（.exe 安装器）和**便携版**（.zip 压缩包）。两种形式功能完全一致，区别在于安装位置和配置文件存放路径。

### 安装版（推荐新手）

1. 从下载页获取最新版本的 .exe 安装器
2. 双击运行安装器，按提示选择安装路径（默认 `C:\Program Files\OpenUtau`）
3. 完成后从开始菜单启动 OpenUtau

安装版的配置文件存放在 `Documents/OpenUtau/` 目录下，包括声库、重采样器、工程文件和偏好配置。

### 便携版（推荐进阶用户）

1. 从下载页获取 .zip 压缩包
2. 解压到任意你喜欢的目录（比如 `D:\OpenUtau\`）
3. 直接双击 `OpenUtau.exe` 启动

便携版的所有文件（包括配置）都在解压目录内，拷走整个文件夹就能移动。如果 OpenUtau 启动就崩溃，**第一件事就是删掉同目录下的 prefs.json**——偏好配置损坏是最常见的启动崩溃原因，删掉后 OpenUtau 会自动重建默认配置，声库、重采样器、工程文件全部不受影响。

> [!TIP] 安装完成后务必核对版本号
>
> OpenUtau 迭代很快，每次更新都会修复 bug 或引入新功能。启动后先看主窗口标题栏或 Help → About 里的版本号，和下载页的最新版本对比，确认装的是你想要的版本。

### 安装外部重采样器

OpenUtau 内置了 Worldline64 重采样器，但很多场景需要外部重采样器——不同重采样器的渲染音质有差异（moresampler 干净、tn_fnds 温暖、strashkat-rs 现代），某些老声库还依赖特定重采样器的专属 flags。

安装方式有两种：

- **拖拽安装（v0.1.119+）**：下载重采样器 zip → 解压出 .exe → 直接拖到 OpenUtau 窗口 → 弹出对话框选 "Install as resampler"，安装完自动出现在齿轮图标下拉里
- **手动放入文件夹**：把 .exe 放进 `OpenUtau/Resamplers/`（便携版）或 `Documents/OpenUtau/Resamplers/`（安装版），支持子文件夹分类管理（比如 `Resamplers/clean/moresampler.exe`）

OpenUtau 官方测试过可用的重采样器包括 moresampler、strashkat-rs、tn_fnds、worldline、doppeltler 等。某些老 .NET 重采样器可能需要额外安装 .NET Framework 运行时，注意重采样器版本和你的系统 32/64 位要匹配。

> [!WARNING] 重采样器放了但下拉里没有？
>
> 先查三件事：文件是不是真的在 Resamplers 文件夹里面？是不是 Windows 格式的 .exe（别下到 Linux/macOS 版本）？某些重采样器除了 .exe 还需要配套的 .dll 或配置文件在同一目录——检查 zip 里所有文件都解压了。

---

## 📖 OpenUtau 使用入门

OpenUtau 的核心工作流可以概括为：**加载声库 → 输入歌词和音符 → 调表达式 → 渲染试听 → 导出音频**。下面按这个流程展开。

### 加载声库

声库是歌声合成的"音源"，每个声库对应一套音素采样。把下载好的声库文件夹放进 `Singers/` 目录，重启 OpenUtau 后就能在轨道头的声库下拉里选到。UTAU Classic 声库、DiffSinger 声库、ENUNU 声库放同一个目录就行，OpenUtau 会自动识别类型。

如果声库放在别的位置（不想复制到 OpenUtau 目录），可以在 Tools → Preferences 里设置 **Additional Singer Path**——添加声库所在的文件夹路径，OpenUtau 启动时会一并扫描。这个偏好设置存在 prefs.json 里，删 prefs.json 后需要重新设置。

### Piano Roll 基本操作

Piano Roll 是 OpenUtau 的核心编辑界面，水平方向是时间、垂直方向是音高：

- **左键点击**空白处创建音符，拖动调整音高位置和时长
- **双击音符**进入歌词编辑状态，输入拼音（中文）或日文假名
- **右键音符**弹出上下文菜单，可以复制、删除、设置属性
- **底部表达式面板**切换不同参数（VEL 速度、VOL 音量、GEN 性别、BRE 气声等）

OpenUtau 内置了丰富的表达式系统，分三层：偏好设置级（所有新建工程的默认值）、工程级（当前工程的默认值）、音符级（单个音符的实际值）。优先级是音符级 > 工程级 > 偏好设置级。

### 工程级表达式管理

这是 OpenUtau 比原版 UTAU 省心的地方。Tools → Expressions 打开表达式管理窗口，可以设置工程级默认值——比如想让全工程的气声（BRE）默认 +10，在这里把 BRE 的 Default 值设为 5，**新建的所有音符自动继承 +5 气声**，已写音符如果之前没手动改过 BRE 也会跟着变。

> [!TIP] 想给某个表达式加自定义 flag？
>
> Tools → Expressions → 左下角 + 号，填 Name（显示名）、Abbr（缩写，Piano Roll 里显示）、Flag（对应重采样器的 flag 名）、Min/Max/Default。比如 moresampler 支持 `Mt`（massage time）flag，OpenUtau 默认没有，你就自己加一个，加完就能在 Piano Roll 里调这个参数了。

ENG（Resampler Engine）表达式特别强大——它让你能**在 Piano Roll 里按音符切换重采样器**。在 ENG 的 Options 里填多个重采样器文件名（逗号分隔），Piano Roll 里改 ENG 值就能切：值 0 回退默认、值 1 用第一个重采样器、值 2 用第二个，以此类推。这样同一个轨道里主旋律用 moresampler、低音部用 worldline64 完全不是问题。

### 界面语言切换

OpenUtau 启动时自动跟随系统语言——Windows 11 简体中文系统 → 界面默认中文，英文系统 → 默认英文。如果想手动切换，Tools → Preferences → Language 下拉选你想要的语言，**重启 OpenUtau 后生效**。

当前支持简体中文、繁體中文、日本語、한국어、Français、Deutsch、Español、Русский 等十几种语言，由社区志愿者翻译。某些小众语言可能翻译不完全（菜单项翻了但错误提示还是英文），这是正常状态。

### 常见问题排查

#### prefs.json 损坏

如果出现以下任意一种情况：启动就崩、打开 Preferences 就崩、切换偏好设置就崩、某些设置丢了——大概率是 prefs.json 损坏了。解决方案非常简单：

1. 完全关掉 OpenUtau（任务管理器确认进程退出）
2. 找到 prefs.json——便携版在 OpenUtau.exe 同目录，安装版在 `Documents/OpenUtau/prefs.json`
3. 删掉它（删前可以复制一份备份）
4. 重新启动 OpenUtau，会自动重建默认配置，然后重新设置 Preferences 即可

> [!NOTE] 删 prefs.json 不会丢什么？
>
> 声库、重采样器、工程文件（.ustx）、DiffSinger vocoder 全部安全不受影响。只有偏好设置会丢：Additional Singer Path、DirectML 设置、界面语言、Custom Expressions 需要重新配置。OpenUtau 崩溃时正好在写 prefs.json、升级版本时旧格式不兼容、杀毒软件拦截都可能导致它损坏——定期备份是个好习惯。

#### 渲染崩溃或静音

渲染崩溃大概率是重采样器版本和系统不兼容（32/64 位），或者重采样器 zip 解压时文件损坏——从官方源重新下载试试。静音问题要查三件事：轨道头引擎是不是 Classic（WORLDLINE-R / DiffSinger / ENUNU 不经过 Classic 重采样器管线）、齿轮图标里选的重采样器是不是正确、ENG 表达式有没有意外覆盖齿轮里的设置。

#### 重采样器不生效

先看轨道头引擎——Classic 引擎才走外部重采样器管线。再查 ENG 表达式：如果你在 ENG 的 Options 里填了多个重采样器，ENG 值 0 才回退齿轮里的默认值，其他值都会覆盖。最后确认重采样器版本和 OpenUtau 版本兼容，特别老的重采样器可能和新版 OpenUtau 有 API 冲突。

---

## 🔗 相关资源

- [openutau官网](https://openutau.fedq.quest)
- [openutau文档](https://openutau.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/openutau)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/openutau)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/openutau)

---

## 📚 相关文章

- [OpenUtau 零基础上手：让虚拟歌手开口唱歌，其实比你想的简单](https://laoguosofts.github.io/posts/openutau.html)
- [零门槛上手 OpenUtau｜第一次做歌声合成就选它](https://alipymanbu.github.io/posts/openutau.html)
