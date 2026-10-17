## ✨ Azahar 是什么

Azahar 是一款开源的任天堂 3DS 模拟器，脱胎于 Lime3DS 项目，目前已经发展成社区活跃度很高的独立分支。它跨平台支持 Windows、macOS、Linux 和 Android，对原版 Citra 的大量功能做了维护和扩展，同时引入了独家的 Z3DS 压缩格式，解决了 3DS 游戏存档长期以来的空间痛点。

相比更早的 Citra 和中途分叉的 Lime3DS，Azahar 目前的更新频率更高，对新 Android 设备的适配也更积极。Android 11 分区存储、Apple Silicon 原生支持、Vulkan 渲染后端这些早期模拟器没完全搞定的场景，Azahar 都有比较成熟的方案。对于想在手机或电脑上重温 3DS 经典游戏的玩家来说，它是目前综合体验最好的选择之一。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://azahar.gltd.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - Android：Google Play 搜索 Azahar Emulator；进阶用户可以从 GitHub Releases 下载 Vanilla APK（I/O 性能更好）
> - Windows / macOS / Linux：从官方渠道或 GitHub Releases 下载对应平台的安装包

---

## 🔧 各平台安装步骤

Azahar 的安装在不同平台上各有细节，但核心流程是一致的：下载本体 → 授予权限 → 导入密钥文件 → 放入游戏。下面按平台逐一说明。

### Windows

从官网或 GitHub Releases 下载最新的安装包（通常是 `.zip` 免解压版或安装向导）。解压到任意目录即可，不需要安装到系统路径。首次启动后，模拟器会在 `%APPDATA%\Azahar\` 下创建配置目录。

> [!WARNING]
> Windows Defender 或其他杀毒软件可能会误报开源模拟器。这是常见的误报，把 Azahar 目录加入白名单即可，不要随意从第三方渠道下载所谓"安全版"。

### Android

Android 端有两条安装路径可以选：

| 方式 | 适合谁 | 特点 |
|---|---|---|
| Google Play | 普通用户 | 自动更新，包名 `io.github.lime3ds.android` |
| Vanilla APK | 进阶用户 | I/O 速度更快，Play 商店不分发 |

基本步骤：打开 Google Play 搜索安装，或下载 APK 在设置里允许未知来源后安装。首次启动时**必须授予存储权限**——模拟器需要读写游戏文件和系统密钥。关闭应用一次再重新打开，它会自动创建 `3DS/roms/`、`3DS/cia/`、`3DS/sysdata/` 等目录结构。

### macOS

Azahar 提供 macOS Universal 构建，原生支持 Intel 和 Apple Silicon（M 系列）。下载 `.dmg` 打开，把应用拖进 `/Applications/` 即可。首次启动如果被 Gatekeeper 拦截，到"系统设置 → 隐私与安全性"里允许一下就行，这是未签名开源应用的常规流程。

### Linux

Linux 用户有三种选择，推荐 Flatpak：

| 方式 | 命令 | 特点 |
|---|---|---|
| Flatpak（推荐） | `flatpak install flathub org.azahar_emu.Azahar` | 跨发行版通用、沙盒隔离、自动更新 |
| AppImage | 下载 `.AppImage` 后 `chmod +x` 双击运行 | 单文件便携，不需要 root |
| 源码编译 | 从 GitHub clone 后 CMake 构建 | 适合开发者 |

> [!NOTE]
> Flatpak 和 AppImage 都自带了运行时依赖（Vulkan、Qt6、SDL2 等），不需要额外装系统库。只有源码编译才需要手动安装这些依赖。

### 安装后的关键一步：导入密钥

3DS 从出厂开始就对游戏卡带和数字下载包做了加密。模拟器要把游戏跑起来，必须用你自己的 3DS 硬件里烧录的密钥把游戏解密。**没有密钥文件，模拟器打开游戏只会黑屏或直接报错**。

你必须有一台属于自己的 3DS 主机，通过 homebrew 工具（通常是 GodMode9）从里面导出 `aes_keys.txt`。不同地区、不同系统版本的主机密钥不通用，网上流传的别人的密钥也不要用——有版权风险。

各平台密钥文件的放置路径：

| 平台 | 路径 |
|---|---|
| Windows | `%APPDATA%\Azahar\sysdata\aes_keys.txt` |
| Linux (Flatpak) | `~/.var/app/org.azahar_emu.Azahar/config/Azahar/sysdata/aes_keys.txt` |
| macOS | `~/Library/Application Support/Azahar/sysdata/aes_keys.txt` |
| Android | `3DS/sysdata/aes_keys.txt` |

嫌手动找路径麻烦？直接在模拟器菜单里选"文件 → 打开 sysdata 目录"，把 `aes_keys.txt` 拖进去就行。

---

## 📖 使用入门

安装和密钥都搞定之后，把游戏文件放进模拟器就能开玩了。下面是一些从基础到进阶的实用技巧。

### 游戏文件放哪

Android 端的 `3DS/` 目录下有几个子目录，各有用途：

- `3DS/roms/` —— 放 `.3ds` / `.cci` / `.cxi` / `.3dsx` 文件，直接在模拟器主界面扫描加载
- `3DS/cia/` —— 放 `.cia` 文件，需要通过模拟器菜单里的"安装 CIA"功能导入
- `3DS/textures/` —— HD 纹理包，开启后能显著提升画面分辨率
- `3DS/sysdata/` —— 系统文件（密钥、固件）

> [!TIP]
> 3DS 游戏通常体积较大，单作 2-4 GB 很常见，建议把游戏文件放在 SD 卡上。Android 11+ 的分区存储对模拟器比较友好，只要授予了"所有文件访问权限"，Azahar 就能正常读写。

### 存档迁移

如果你之前用过 Citra 或 Lime3DS，Azahar 完全兼容它们的存档格式。直接把旧模拟器的存档目录拷贝到 Azahar 对应的存档位置就行，进度不会丢。

### Z3DS 压缩格式：省空间神器

3DS 游戏单个 2GB 左右，收集多了硬盘很快就满。市面上常见的两种省空间办法各有短板：修剪（trimming）去掉卡带 dump 里的空白区域，效果显著但不可逆；压缩用算法减小体积，但大多数压缩格式在模拟器里不支持随机访问——要先解压再加载。

Azahar 引入的 Z3DS 格式解决了这个矛盾：它用 Facebook 开源的 Seekable ZStandard 算法，压缩后不需要解压就能直接加载游戏，是目前 3DS 模拟器里唯一支持随机访问的压缩方案。

| 特性 | 说明 |
|---|---|
| 算法 | Seekable ZStandard（Facebook 开源） |
| 随机访问 | ✓ 不需要解压就能直接加载游戏 |
| 支持格式 | `.zcci` `.zcxi` `.z3dsx` `.zcia` |
| 节省率 | 5-45%，依游戏素材内部压缩程度 |
| 兼容性 | **Azahar 专属**，其他模拟器目前不支持 |

官方博客给出了几个典型游戏的实际数据：

| 游戏 | 原大小 | 压缩后 | 节省 |
|---|---|---|---|
| 新超级马里奥兄弟 2 | 350 MB | 242 MB | 30.8% |
| 宝可梦 X | 1764 MB | 1117 MB | 36.7% |
| 马里奥赛车 7 | 649 MB | 612 MB | 5.7% |
| 路易吉洋馆 2 | 864 MB | 649 MB | 24.9% |
| 动物之森新叶 | 800 MB | 459 MB | **42.6%** |

可以看到，像马里奥赛车 7 这类纹理和音频本身就是高度压缩的游戏，节省率很低；但宝可梦、动物之森这类内部素材压缩率低的游戏，节省非常可观——动物之森新叶居然能省掉 42.6% 的体积。

#### 怎么压缩

**桌面版**：菜单"工具 → 转换备份"选择压缩，选一个或多个文件。转换是可选的——你想压缩哪些就压缩哪些，不想动的保持原样。

**Android 版**：从 2123 版本起支持安装 CIA 时自动压缩内部 NCCH（在模拟存储设置里开启）。安装后的游戏直接就是 `.zcia`，比手动转换更省心。

万一以后需要用其他模拟器打开这些文件，也可以在"工具 → 转换备份"里把 `.zcci` 解压回 `.cci` 格式，兼容性就回来了。

### 其他进阶玩法

- **HD 纹理包**：把社区制作的高清纹理放进 `3DS/textures/` 目录，在模拟器设置里开启即可，画质提升明显。
- **自定义屏幕布局**：Android 端支持自由调整上下屏位置和大小，横屏玩动作游戏时特别有用。
- **金手指（Cheats）**：在模拟器设置里开启后，可以加载网上社区分享的金手指码。
- **多人联机**：Azahar 支持本地联机，两台设备之间可以一起玩支持联机的 3DS 游戏。

---

## 🔗 相关资源

- [azahar官网](https://azahar.gltd.quest)
- [azahar文档](https://azahar-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/azahar)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/azahar)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/azahar)

## 📚 相关文章

- [用了一年 Azahar，我把 3DS 模拟器里里外外摸了一遍](https://laoguosofts.github.io/posts/azahar-docs.html)
- [Azahar 用了八个月：从"又一个模拟器"到 3DS 怀旧日常](https://alipymanbu.github.io/posts/azahar-docs.html)
