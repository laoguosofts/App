## ✨ Drastic 是什么

Drastic 是一款专注于 Android 平台的 Nintendo DS 模拟器，由开发者 Exophase 维护。它的定位清晰——把 NDS 掌机的双屏游戏体验完整搬到手机上，包括上下分屏渲染、触屏模拟下屏输入、实体手柄映射、存档管理等核心功能。

Drastic 的核心优势在于对 NDS 硬件的深度还原：双引擎（Engine A 2D+3D 主引擎 / Engine B 2D 子引擎）独立模拟、完整的 DS 卡带兼容性、支持 savestate 即时存档与游戏内存档两套机制。2014 年首次发布后长期保持付费模式，2024 年转为免费提供，目前仍是 Android 上体验最成熟的 NDS 模拟器之一。

适合谁用：想在手机上重温 NDS 经典游戏的玩家、对掌机模拟器有一定了解的折腾党、需要随时随地玩 DS 游戏的通勤族。如果你只玩 3DS 游戏，Drastic 不是你的选择——它专注 NDS 一个世代。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://drastic.gkhf.beer/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜索 "Drastic DS Emulator"；iOS 用户暂无（Drastic 无 iOS 版本）

---

## 🔧 安装与首次配置

### Android 平台安装步骤

1. **允许未知来源安装**：Android 8.0 及以上系统，需要在设置里单独给文件管理器或浏览器开启"允许安装未知应用"权限。路径：设置 → 应用 → 你的浏览器 → 安装未知应用 → 允许。

2. **下载并安装 APK**：从下载入口获取最新 APK 文件，点击安装。如果系统弹出安全警告，确认继续。

3. **首次启动授权**：Drastic 首次运行需要存储权限来创建游戏目录和存档目录，点击允许即可。

> [!NOTE] 文件大小核对
> 当前 Drastic APK 通常在 30MB 左右。如果你下载的文件明显偏小或偏大，建议重新下载，避免损坏。

### 游戏 ROM 放置

Drastic 不自带任何游戏，需要自行准备 .nds 格式的 ROM 文件。启动后在主界面点击"Load New Game"，通过内置文件浏览器找到 ROM 所在位置。建议在手机存储根目录建一个 `Drastic/ROMs/` 文件夹统一管理。

### 安装后的首次检查

- **版本号确认**：进入 Options → About，确认是最新版本
- **存档目录**：默认存储位置在 Drastic 安装目录下的 `savestates/`，建议首次启动后手动确认一下路径
- **Google Drive 账号绑定**：如果你有云端备份需求，在 Manage Savestates 里绑定 Google Drive

---

## 📖 使用入门与进阶技巧

### 双屏布局的灵活控制

NDS 的两个屏幕由独立硬件引擎驱动：Engine A（主引擎）负责 2D + 3D 画面，Engine B（子引擎）只负责 2D 画面。大多数游戏默认主引擎显示在上屏，但少数游戏（比如 Sonic Rush、Sonic Chronicles）会动态交换两个引擎的位置，导致主画面跑到下屏去。

如果遇到"主要的游戏画面在下面，得低头看"的情况，打开 **Options → Video → Keep Main Screen on Top Screen**，强制主引擎始终显示在上屏。

> [!WARNING] 副作用提示
> 不建议全局开启这个选项——大部分游戏不需要它。而且和多线程 3D 渲染同时开启时，更容易出现画面不同步或触屏位置错误。遇到触屏不准或画面闪烁时，先试试把这两个选项都关掉。

### 实体手柄的模拟扳机支持

部分高端手柄的 L/R 肩键是模拟量的——按下时返回 0-255 的值，而不是简单的"按下/松开"。Xbox Series X/S 的 LT/RT、PS4/PS5 的 L2/R2、Switch Pro 的 L/R 都支持模拟扳机。

在 **Options → External Controller → Enable analog triggers** 开启后，Drastic 会识别手柄 L/R 的模拟量输入：完全不按对应 NDS L/R 松开，按下一半对应轻按，按到底对应全按。

这个功能对赛车游戏（马里奥赛车、GT Racing）和动作射击游戏（生化危机 死寂）特别有用——油门可以精确控制，而不是只能全关或全开。不开这个选项时，所有 L/R 输入都是数字量，要么不按要么按到底。

| 手柄 | L/R 模拟扳机 | 备注 |
| --- | --- | --- |
| Xbox Series X/S | ✅ LT/RT | 直接用 |
| PS5 DualSense | ✅ L2/R2 | 直接用 |
| Switch Pro | ✅ L/R | ZL/ZR 是数字的 |
| 8BitDo Pro 2 | ✅ L/R | 支持 |
| MOGA / 普通蓝牙 | ❌ 数字按键 | 没有模拟扳机 |

如果你发现"RT 按到一半才开始有油门"，可能是 Deadzone 值太大，在右摇杆 Deadzone 设置里适当调小到 0.1-0.2 试试。

### 存档管理的正确姿势

Drastic 有两套独立的存档机制：**即时档（savestate）** 和 **游戏内存档（in-game save）**。即时档是 Drastic 自己的快照系统，可以随时存随时读；游戏内存档是游戏本身的存档功能，通过 .sav 文件保存在 saves/ 子目录。

### 为什么存档会突然消失

Drastic 官方明确表示：模拟器本身**不应该会主动删存档**。已报告的所有案例几乎都是其他 App 导致的。最常见的元凶是手机清理 / 加速软件——360、猎豹清理大师、部分 OEM 自带的系统清理功能，会把 savestates/ 目录里的 .dsv 文件当作"大垃圾"直接删掉。杀毒软件有时也会误删。

其他可能原因包括：手动在 Android 设置里清除了 Drastic 数据（这会同时清空所有存档、皮肤、Lua 脚本）、用文件管理器误删了 savestates/ 目录、或者开了高级设置里的"把存档嵌进即时档"导致口袋妖怪等游戏的存档损坏。

> [!TIP] 存档防消失五招
> 1. **定期手动备份**：把 savestates/ 整个目录复制到手机其他位置或电脑
> 2. **开 Google Drive 云同步**：重要存档手动 Upload 到云端
> 3. **给 Drastic 文件夹加清理白名单**：所有清理 / 安全 App 里都加上
> 4. **别手动清 Drastic 数据**：除非确定要重装
> 5. **Autosave 是最后防线**：开着可以防意外杀进程丢进度

### 关于 3DS 支持的真相

很多新用户会问 Drastic 能不能玩 3DS 游戏。官方 FAQ 的回答很明确：**不会，也永远不会**。3DS 和 NDS 是差了整整一代的不同硬件——NDS 用双 ARM 9 处理器、自研 2D+3D 双引擎；3DS 用双核 ARM 11、PICA200 GPU、128MB 主 RAM。工作量相当于重写一个模拟器，不如直接用专门的 3DS 模拟器 Citra。

Drastic + Citra 是手机玩任天堂掌机游戏的标准组合：Drastic 玩 NDS，Citra 玩 3DS。

### 常见问题速查

- **触屏位置不准怎么办？** 先关掉"Keep Main Screen on Top Screen"和"Multi-threaded 3D"，确认是不是这两个选项的副作用。
- **游戏加载很慢？** 把 ROM 文件放到手机内置存储而不是 microSD 卡，可以明显提升加载速度。
- **画质怎么调？** Options → Video 里可以调整分辨率缩放、全屏模式、插值算法。建议 4x 以上配合骁龙 855 或同级处理器使用。
- **Lua 脚本怎么用？** Drastic 支持 Lua 脚本扩展，把 .lua 文件放到脚本目录，在游戏内可以随时加载执行。

---

## 🔗 相关资源

- [drastic官网](https://drastic.gkhf.beer)
- [drastic文档](https://drastic.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/drastic)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/drastic)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/drastic)

## 📚 相关文章

- [Drastic 快问快答——Android 上玩 NDS 必装模拟器](https://laoguosofts.github.io/posts/drastic.html)
