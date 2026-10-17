## ✨ Cemu 是什么：Wii U 玩家在 PC 上的归宿

Cemu 是目前 PC 上**功能最完整的 Wii U 模拟器**，没有之一。它的核心工作是在 x86 架构的 Windows / Linux / macOS 上，把 Wii U 原生的 OpenGL 图形调用翻译成 PC GPU 能理解的 Vulkan 或 OpenGL，同时模拟 Wii U 的 CPU、系统菜单、手柄输入和存档读写。

从 2017 年第一个能跑商业游戏的版本算起，Cemu 走过了 Vulkan 后端从实验到稳定、异步着色器编译从 0 到有、Pretendo 联机从无到有的七年。今天你在 PC 上用 Cemu 跑《马里奥赛车 8》《Splatoon》《塞尔达传说：旷野之息》Wii U 原版，帧率、分辨率、画面质量都能超过原版主机。

它适合三类人：

- **有 Wii U 但想换大屏幕**的玩家：把自己的正版光盘转储到 PC，用显示器或电视接着玩
- **错过 Wii U 世代**的新玩家：Wii U 停产后正版主机和光盘越来越难买，Cemu 是体验 Wii U 独占游戏的现实选择
- **追求画质升级**的折腾党：Cemu 内置图形包系统，社区有大量 4K 分辨率包、60fps 补丁、画质增强滤镜

Cemu 本身不随模拟器分发游戏，游戏文件需要玩家自己从合法途径获取。模拟器本体是开源免费的，支持 Windows 10 / 11（64 位）、Linux 和 macOS（macOS 仅 OpenGL 后端）。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://cemu.gerw.hair/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 官方分发渠道：访问 Cemu 官方网站获取最新版本；Windows 用户下载 zip 压缩包解压即用，无需安装程序
> - macOS 用户：下载 .dmg 文件拖拽到 Applications 文件夹；注意 macOS 上 Cemu 仅支持 OpenGL 后端

---

## 🔧 Cemu 安装与首次启动

Cemu 的安装比一般模拟器简单很多——**Windows 上根本没有安装程序，就是一个 zip 包解压**。但首次启动前有几个必须注意的准备工作，跳过的话后面大概率踩坑。

### 系统要求速查

| 项目 | 最低配置 | 推荐配置 |
| --- | --- | --- |
| 操作系统 | Windows 10 64 位 / Linux / macOS 10.15 | Windows 11 64 位 |
| CPU | Intel i5-3570K / AMD FX-8350 | Intel i5-10400 / AMD Ryzen 5 3600 及以上 |
| 显卡 | NVIDIA GTX 750 Ti / AMD R7 260X（支持 Vulkan 1.2） | NVIDIA GTX 1660 / AMD RX 5700 XT 及以上 |
| 显存 | 2GB | 4GB 及以上 |
| 内存 | 4GB | 8GB 及以上 |

> [!WARNING]
> macOS 用户注意：Cemu 在 macOS 上只能使用 OpenGL 后端（通过 MoltenGL 转译到 Metal），性能比 Windows 上的 Vulkan 有明显差距。MacBook Air M1/M2 可以跑，但帧率不会太高。

### 解压到正确的位置

1. 下载 zip 包后，解压到一个**非系统盘、路径不含中文和空格**的位置，例如 `D:\Emulators\Cemu\`
2. 解压后目录结构应该是：

```
Cemu/
├── Cemu.exe
├── keys.txt          ← 这个文件稍后要放
├── mlc01/            ← 系统文件目录，首次启动自动创建
├── resources/
└── ...
```

> [!NOTE]
> 路径不能有中文、不能有空格、不能放在 Program Files 下——这些都会导致 Cemu 找不到文件或写权限不足。最稳妥的做法就是直接放 D 盘根目录的英文文件夹里。

### keys.txt：最容易被忽略但最关键的一步

这是 Cemu 新手踩坑频率最高的环节。如果你打算玩 WUD / WUX 格式的游戏（Wii U 光盘原始转储格式），**必须有一个正确的 keys.txt 文件放在 Cemu.exe 同级目录**。

关于 keys.txt 你只需要记住：

- 文件名**必须精确**叫 `keys.txt`（全小写），不能是 `Keys.txt`、`key.txt` 或 `keys.txt.txt`
- 文件位置**必须和 Cemu.exe 在同一目录**，不能在 `mlc01/` 或 `resources/` 里
- 里面的密钥是你从自己的 Wii U 上用 Dumpling 工具提取的，每张光盘对应唯一密钥

如果你觉得 keys.txt 太麻烦，有一个更简单的选择——**直接用 WUA 格式**。WUA 是 Dumpling 转储时就解密好的格式，**完全不需要 keys.txt**，体积更小（2~8GB vs WUD 的 25GB），Cemu 扫描到直接显示在游戏列表里。这也是社区推荐的首选格式。

### 首次启动

1. 双击 `Cemu.exe`，第一次启动会弹出一个对话框让你选择 mlc01 目录——就选解压目录里自带的那个
2. 进入主界面后，顶部菜单 **File → Add game path**，选择你放游戏文件的文件夹
3. 回到主界面，游戏列表里应该会出现你路径下的 Wii U 游戏
4. 双击任意一个游戏，开始自动编译着色器——第一次会比较慢，之后就快了

---

## 📖 Cemu 核心设置与进阶玩法

首次启动跑通之后，真正影响体验的就是图形后端选择、着色器缓存、显卡驱动和联机配置这几件事。下面按影响程度从大到小讲。

### Vulkan vs OpenGL：几乎所有人都该选 Vulkan

Wii U 原生使用 OpenGL 4.x 接口，Cemu 1.0.0 起一直用 OpenGL 后端模拟。但 2021 年 Cemu 1.22.7 引入 Vulkan 后端后，情况彻底变了：

| 对比项 | Vulkan | OpenGL |
| --- | --- | --- |
| 性能 | **更高**，着色器编译快 | 略低 |
| 异步着色器编译 | ✅ 可用 | ❌ 没有 |
| VSync | ✅ 实验性 "Match emulated display" | Double buffering |
| 着色器卡顿 | 大幅减少 | 首次必卡 |
| 显存占用 | 较高 | 较低 |
| macOS | ❌ 不支持 | ✅ 唯一选项 |
| 驱动要求 | Vulkan 1.2 | OpenGL 4.5+ |

**结论很简单**：能跑 Vulkan 就跑 Vulkan。99% 的现代 Windows 显卡（NVIDIA GTX 10 系列以后、AMD RX 5000 以后、Intel Arc）都支持 Vulkan 1.2。

切换路径：顶部 **Options → General settings → Graphics → Graphics API** 下拉菜单选 Vulkan。切换后**Async shader compiler（异步着色器编译）**这个开关会出现，**强烈建议开启**——它让着色器编译在后台线程做，游戏不会在等待新着色器时卡死，只会短暂跳过渲染。

> [!TIP]
> Vulkan 上着色器缓存需要**两个文件**：shader cache + pipeline cache。pipeline cache 是 Vulkan 特有的，按游戏区域锁定（同一游戏不同区域的 pipeline cache 不通用）。如果你从社区下载着色器缓存，记得两个都下。

只有三种情况用 OpenGL：macOS 用户（没得选）、极老显卡不支持 Vulkan、显存只有 2GB 以下（Vulkan 多占几百 MB 显存）。

### 显卡驱动：Vulkan 崩溃的头号元凶

Cemu 把 Wii U 的图形调用翻译成 PC GPU 的调用，这个过程的质量高度依赖**显卡驱动**——Vulkan 后端尤其吃驱动。很多人遇到 Vulkan 一启动就崩、着色器编译失败、画面闪烁，本质都是驱动太旧。

各厂商驱动更新渠道：

| 厂商 | 官方渠道 | 推荐驱动类型 |
| --- | --- | --- |
| NVIDIA（GeForce） | 国内 nvidia.cn/drivers / 国际 nvidia.com/drivers | **Game Ready**（性能优先） |
| AMD（Radeon） | amd.cn/support | Adrenalin 稳定版 |
| Intel（Arc / UHD） | intel.com 显卡支持页 | 最新稳定版 |

什么时候必须立刻更新驱动：

- 切到 Vulkan 就崩溃
- 异步着色器编译开关找不到或勾了没效果
- Shader Cache 导入后还是重新编译（Vulkan Pipeline Cache 和驱动版本绑定，换大版本驱动后缓存可能失效）
- 游戏内画面异常（模型缺失、色块、光效不对）

> [!WARNING]
> 更新驱动后**记得清 Shader Cache**。驱动大版本更迭会让 Vulkan Pipeline Cache 全部失效，Cemu 会重新编译，但不清的话可能产生冲突。清理路径：Cemu 目录下 `resources/shadercache/`，删掉里面所有文件即可。

更新策略：NVIDIA 和 AMD 每月至少检查一次，有 Game Ready / Adrenalin 稳定版就更。Intel Arc 系列驱动更新更频繁，Vulkan 实现还在快速迭代，更要跟上。

### 着色器缓存：跳过首次卡顿的关键

Wii U 游戏第一次在 Cemu 里跑的时候会**疯狂卡顿**——不是性能不够，是 Cemu 正在把 Wii U 的着色器翻译成 PC 显卡能理解的格式，每遇到一个新着色器组合就卡几秒。这个过程可能持续几分钟甚至更久，取决于游戏的着色器数量和你的显卡编译速度。

解决方法有两个：

1. **自己跑一遍**：慢慢等 Cemu 编译完，之后进同一游戏的同一区域就不卡了。缓存会自动保存在 `resources/shadercache/` 里。
2. **导入社区缓存**：Cemu 社区有大量玩家分享的着色器缓存文件，下载后丢进 `resources/shadercache/` 就能跳过首次编译——这是老玩家的常规操作。注意 Vulkan 缓存需要 shader cache + pipeline cache 两个文件都下。

### Pretendo：任天堂关服后的在线联机方案

2023 年 4 月任天堂正式关闭了 Wii U 和 3DS 的在线服务——在线多人对战、排行榜、EShop 全部停了。马里奥赛车 8、Splatoon、Super Smash Bros. for Wii U 这些强依赖在线的游戏瞬间变成了纯单机。

**Pretendo**（pretendo.network）是社区搭建的替代网络，2022 年起 Cemu 2.0 就原生支持它。Pretendo 的核心特点：

- **非商业、开源**：社区志愿者维护，接受捐赠但不卖服务
- **独立账号体系**：在 Pretendo 网站注册 PNID（Pretendo Network ID），不需要任天堂账号
- **和官方服务器不互通**：Pretendo 用户只能和 Pretendo 用户联机，但用户量已经够匹配

要在 Cemu 里开 Pretendo 联机，需要四件事：

1. Pretendo PNID 账号（网站免费注册）
2. Cemu 设置里切到 Pretendo：**Options → General settings → Account → Network service** 下拉选 Pretendo
3. 在线系统文件：从 Wii U 用 Dumpling 导出的 `otp.bin` 和 `seeprom.bin`
4. 系统应用转储：从 Wii U 转储 Friends List 等系统应用

社区已验证能在 Pretendo 上联机的游戏：马里奥赛车 8、Splatoon、Super Smash Bros. for Wii U、Wii Sports Club、Monster Hunter 3 Ultimate、Hyrule Warriors 等。

> [!NOTE]
> 不玩联机的话**完全可以跳过 Pretendo 的所有配置**——Cemu 单机游戏和 Pretendo 没有任何关系。Pretendo 整套东西只对想玩在线的用户有意义。

### 图形包：让 Wii U 游戏看起来不像 Wii U

Cemu 的图形包系统（Graphic Packs）是它和其他 Wii U 模拟器拉开差距的重要功能。简单说就是社区制作的补丁包，能给原版游戏加上：

- **4K 分辨率**：Wii U 原生 1080p 渲染，Cemu 能开到 4K 甚至更高
- **60fps 补丁**：很多原版锁 30fps 的游戏能解锁到 60fps
- **画质增强滤镜**：抗锯齿、各向异性过滤、亮度对比度调整
- **宽屏补丁**：部分原版有黑边的游戏能全屏显示

Cemu 内置了图形包管理器：**Options → General settings → Graphic Packs**，里面有官方维护的一批图形包。更多图形包可以从社区仓库下载，解压到 Cemu 目录的 `graphicPacks/` 下，在管理器里勾选启用即可。

### 完整性能优化清单

| 优化项 | 设置 |
| --- | --- |
| Graphics API | **Vulkan** |
| Async shader compiler | **开启** |
| VSync | Match emulated display（G-SYNC 显示器可关 VSync 用硬件同步） |
| 显卡驱动 | 最新 Game Ready / Adrenalin 稳定版 |
| 图形设备 | 选最强的独显（笔记本别选核显） |
| Audio API | XAudio2 |
| Accurate barriers (Debug) | **关闭**（性能明显下降） |
| Shader Cache | 导入社区缓存跳过首次编译 |

---

## 🔗 相关资源

- [Cemu官网](https://cemu.gerw.hair)
- [Cemu文档](https://cemu-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/Cemu)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/Cemu)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/Cemu)

## 📚 相关文章

- [Cemu 三年记：从 Vulkan 还没出的 1.22 用到 Pretendo 联机的 2.6](https://laoguosofts.github.io/posts/cemu-docs.html)
- [Cemu 用了两年，我把 Wii U 的那些事想清楚了](https://alipymanbu.github.io/posts/cemu-docs.html)
