## ✨ RetroArch 是什么

RetroArch 是 libretro 生态的前端壳——它本身不做模拟，而是把上百个 libretro 兼容的模拟器核心（Core）统一装进一套界面里，一次安装就能跑 FC、超任、PS1、土星、Dreamcast、GBA、世嘉 MD 等几乎所有复古主机。它的设计理念是「核心即插件」，换不同的 Core 就换了一台不同的模拟器，而界面、输入、存档、联机、着色器、成就这些外围能力全部复用。

它跨平台覆盖 Windows、Android、iOS、Linux、macOS，甚至 PS3/PSV/Switch 等主机系统，同一份存档和键位配置跟着 Core 走，跨设备迁移成本极低。核心功能包括：libretro 核心加载器、内置 Online Updater（一键拉核心/数据库/着色器/封面）、RetroPad 统一手柄映射、即时存档（Savestate）与内存档（Savefile）分离、Rewind 实时倒带、GGPO 回滚联机大厅、RetroAchievements 成就同步，以及最出名的 Shader 着色器滤镜系统——CRT 扫描线叠加、游戏机箱外壳（Handheld Border）、运动模糊消除等效果都在这里。

如果你只想玩一款主机、只装一个专用模拟器也行；但当你同时收藏多台主机的 ROM、又不想为每台单独配模拟器+键位+存档+金手指时，RetroArch 的统一壳方案会省掉大量重复劳动。它适合 ROM 收藏党、追求画质的复古玩家、以及愿意花一点时间研究 Shader 和 Core 配置的进阶用户。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://retroarch.djnf.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 用户可在 Google Play 搜索 RetroArch；iOS 用户需通过 AltStore、SideStore 等侧载渠道安装；PC 用户在官网下载页按操作系统选择对应安装包

---

## 🔧 安装步骤

### Android 平台

RetroArch 在 Android 上有两个主要分发渠道：Google Play 版和官网 APK 版。Play 版更新可能滞后 1~2 个版本，官网 APK 版本更新更快，但需要手动授权未知来源安装。

1. 下载 APK 文件到本地（推荐官网渠道）
2. 打开手机设置 → 安全 → 安装未知应用，为浏览器或文件管理器授予安装权限
3. 点击 APK 文件，按提示完成安装
4. 首次启动时建议用 RetroArch 自带的 Online Updater 同步数据库、Core 信息和着色器预设

> [!WARNING]
> Android 11+ 有分区存储限制，RetroArch 的系统目录（BIOS 存放处）默认指向 `Android/data/com.retroarch/files/` 下的子目录。新版 RetroArch 支持 SAF（Storage Access Framework），可以在设置里把系统目录指向任意可见文件夹，比塞进应用沙盒深处更省心，也方便后续用文件管理器复制 BIOS。

### Windows 平台

Windows 上建议用官网的压缩包版本（zip），免安装直接解压即可使用：

1. 下载对应架构的压缩包（64 位系统选 x86_64 版本）
2. 解压到一个非系统盘、路径无空格的目录，例如 `D:\Emulators\RetroArch\`
3. 双击 `retroarch.exe` 启动。首次运行会自动创建配置文件和默认目录结构
4. 杀毒软件如果误报（部分核心代码特征可能触发启发式扫描），把解压目录加进白名单即可

### 通用安装后自检

- 启动后进入 **Settings → Directory**，确认系统目录、游戏目录、存档目录指向合理路径
- 进入 **Main Menu → Online Updater → Update Installed Cores** 同步核心列表
- 第一次加载某个 Core 时，RetroArch 会提示是否下载——选是即可从官方源获取

---

## 📖 使用入门

### 界面导航：Ozone 总览

RetroArch 自 1.8.5 起默认使用 Ozone 菜单驱动，左侧标签栏 + 右侧内容区的 settop box 风格布局，针对手柄导航优化。默认八个标签：

| 标签 | 功能 |
| --- | --- |
| **Main Menu** | 加载核心、加载内容、在线更新、退出 |
| **Settings** | 视频、音频、输入、核心、存档等全部设置 |
| **History** | 最近玩过的游戏 |
| **Netplay Rooms** | 联机大厅 |
| **Add** | 扫描目录导入 ROM |

扫描完游戏后，按主机分组的播放列表会自动追加到标签栏最右侧。手柄默认映射下，D-Pad 切标签、A 确认、B 返回，游戏中按 F1 调出 Quick Menu——这里是调试的主战场，视频调参、换键位、存进度、开 Shader 都在这里。

### Shader 着色器：复古画质的灵魂

Shader 是 RetroArch 的杀手级功能，游戏画面渲染前会经过 Shader 二次处理。最多可叠加 16 层（Shader Pass），常见玩法：

- **CRT 模拟**：在现代 LCD 上复刻老式显像管电视的扫描线、桶形畸变、色彩偏移。三个常用预设：
  - `crt-lottes`：业界标杆，色彩和扫描线精度最高，适合追求极致的玩家
  - `crt-geom`：最流行的通用预设，轻微弧度 + 可调扫描线密度
  - `crt-pi`：树莓派级轻量版，ARM 设备首选

> [!TIP]
> 加载 CRT 预设后进 Quick Menu → Shaders → Preview Shader Parameters，可以实时调参。推荐起始值：Scanline Density 90、Brightness 稍亮 +10% 抵消扫描线间隙暗化、Curvature 0.02~0.05（弧度不要太大，太夸张反而假）。

- **Handheld Border**：给 GBA/Game Boy/DS 模拟画面加上游戏机外壳，拍照发圈很有代入感
- **Beam Racing**：BlurBusters 出品，大幅减少 LCD 上复古游戏的运动模糊

加载流程：进游戏 → Quick Menu → Shaders → Load Shader Preset → 选对应目录下的 `.slangp` 预设。调好后选 Save Core Override，下次该 Core 下的游戏自动应用。

### BIOS 文件：光驱类主机的刚需

BIOS 是主机的开机固件，带光驱、有开机画面的主机（PS1、Dreamcast、土星、3DO 等）模拟时必须读取。RetroArch 和所有 Core 都**不附带 BIOS**——版权原因，Libretro 团队不能分发，需要自行准备。

| 主机 | 是否需要 BIOS |
| --- | --- |
| FC / 超任 / Game Boy / GBA / MD | 不需要 |
| PS1 | 部分需要（PCSX-ReARMed 多数游戏免 BIOS） |
| 土星 / Dreamcast / 3DO | **必须** |
| PS2 | **必须** |

> [!NOTE]
> BIOS 损坏或版本不对的症状很模糊：黑屏、闪退、卡在开机画面都有可能。务必去 Libretro 官方文档的 BIOS 库页面核对文件名和 MD5——对不上改名没用，换个来源重新找才是正解。

BIOS 统一放在系统目录下（Settings → Directory → System/BIOS 查看路径），多数核心直接平铺即可，个别核心要求放进子文件夹，以核心文档为准。放好后重新加载对应 Core 的 ROM，能正常出现开机画面就算成功。

### 常用快捷键与设置覆盖机制

RetroArch 的快捷键系统基于 Hotkey Enable 修饰键——其他快捷键都要先按它再按功能键。默认 Hotkey Enable 是手柄 Select 或键盘 Shift+F1：

| 快捷键（先按修饰键） | 功能 |
| --- | --- |
| **F1** | 游戏中调出 Quick Menu |
| **F2** | 即时存档 |
| **F4** | 即时读档 |
| **R**（按住） | Rewind 实时倒带 |
| **+ / -** | 快进 / 减速 |
| **Next / Previous Shader** | 快速切换着色器预设 |

Override（设置覆盖）是 RetroArch 最灵活的设置机制：

| 覆盖类型 | 作用范围 | 优先级 |
| --- | --- | --- |
| **Global** | 全局，所有 Core | 最低 |
| **Core Override** | 单个 Core 下的所有游戏 | 中 |
| **Game Override** | 某一款游戏 | 最高 |

进 Quick Menu 改完设置 → Overrides → Save Core Override 或 Save Game Override，下次启动自动应用——不同游戏/不同 Core 可以完全独立的视频、输入、Shader 配置，互不干扰。

### 联机大厅与成就

Netplay 基于 GGPO 回滚重放技术，支持跨地域联机。Main Menu → Netplay Rooms 可以浏览公开房间、也可以自己开房间等朋友进来。RetroAchievements 则是第三方复古游戏成就平台——注册账号后在 Settings → Achievements 开启，游戏运行时自动检测解锁条件，和玩家社区同步进度。

---

## 🔗 相关资源

- [retroarch官网](https://retroarch.djnf.quest)
- [retroarch文档](https://retroarch-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/retroarch)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/retroarch)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/retroarch)

## 📚 相关文章

- [把RetroArch翻个底朝天后，我挖到了5个官方没说的隐藏玩法](https://laoguosofts.github.io/posts/retroarch-docs.html)
- [RetroArch 用了三年才发现的四个狠活——着色器叠 16 层、观战大厅、按游戏存配置……](https://alipymanbu.github.io/posts/retroarch-docs.html)
