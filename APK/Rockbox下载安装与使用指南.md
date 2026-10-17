## ✨ Rockbox 是什么

Rockbox 是一款开源固件项目，专门运行在各类便携式音乐播放器（DAP）上——它不是一个 App，而是**直接替换掉设备原厂固件**的操作系统层。从 2001 年启动至今，Rockbox 已经为上百款设备编写了完整的固件移植，涵盖 iPod Classic、SanDisk Sansa Clip 系列、iRiver H 系列、Cowon D2、Archos Jukebox、复古掌机 RG35XX/TrimUI 等。

它的核心价值可以用一句话概括：**让原厂固件做不到的事，变成默认行为**。原厂固件通常只支持少数几种音频格式、变速会跑调、录音只有 MP3、界面死板没得改——Rockbox 把这些限制一条条拆掉，补上无损格式、音高保持变速、无损录音、主题换肤、插件扩展等功能，让一台老 DAP 焕发出远超原厂设计的生命力。

Rockbox 的源代码以 GNU GPL v2 协议发布，项目完全由全球社区志愿者驱动，没有任何硬件厂商正式参与开发，所有设备端口都是逆向工程完成的。这种纯社区模式保证了 Rockbox 永远保持开源、不被商业化，但也意味着它**不提供任何官方保修**——刷机有风险，动手前要对自己的设备负责。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://rockbox.kook.yoga/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Rockbox 是固件级软件，不走应用商店分发。官方工具 Rockbox Utility 提供 Windows / macOS / Linux 三平台安装器，运行后自动识别连接的 DAP 并匹配对应固件版本。

---

## 🔧 刷机安装指南

Rockbox 的安装和普通软件不一样——它是**替换设备底层固件**，整个过程叫「刷机」。好消息是，Rockbox 团队为大多数设备准备了图形化的 Rockbox Utility 工具，把编译好的固件、引导加载程序和基础文件打包好了，用户只需要点几下就能完成。

### 刷机前的准备

1. **确认设备型号**：打开 Rockbox Utility，用数据线连接 DAP。Utility 会自动识别设备型号——如果识别失败，检查数据线是否支持数据传输（不是纯充电线），或尝试换 USB 口。
2. **备份原厂固件**：大部分设备在刷机前需要用 Rockbox Utility 备份一次原厂引导加载程序——这是救砖的最后保险，**别跳过**。
3. **电量准备**：刷机过程中设备不能断电——确保电量在 50% 以上，或直接接 USB 供电。

> [!WARNING]
> 刷机有风险。虽然 Rockbox 的引导加载程序设计了双启动模式（原厂固件和 Rockbox 共存），但操作失误仍可能导致设备变砖。如果你的设备还在保修期内，刷机可能失去原厂保修。

### 典型设备刷机流程

不同设备的具体步骤略有差异，但通用流程大致一致：

1. 运行 Rockbox Utility → 自动识别设备型号
2. 点击「安装 Rockbox」→ 工具会下载对应版本的引导加载程序和固件文件
3. 按提示在 DAP 上执行一次原厂固件的引导加载程序更新（不同设备按键不同）
4. 更新完成后，DAP 会重启进入 Rockbox 引导菜单
5. 选择 Rockbox 启动——第一次进入需要几十秒初始化文件系统

> [!TIP]
> 刷完 Rockbox 之后，原厂固件**并没有被删除**——引导菜单里始终保留原厂固件的启动选项。想回去就重启设备、在引导菜单里选原厂固件即可，不会丢失任何数据。

### 手动安装（高级模式）

如果 Rockbox Utility 无法识别设备，或你想使用开发版固件，可以手动操作：

1. 从 Rockbox 构建服务器下载对应设备的固件包（.zip）
2. 解压后得到 `.rockbox` 文件夹和引导加载程序文件
3. 把 `.rockbox` 文件夹完整复制到 DAP 的根目录
4. 用设备的引导加载程序工具安装 Rockbox 的引导程序
5. 重启设备进入 Rockbox

手动模式需要查阅 Rockbox 官方 Wiki 对应该设备的页面——**每款设备的引导加载程序安装方式都不同**，不能套模板。

---

## 📖 使用入门

Rockbox 的功能面非常广——30+ 种音频格式、无损录音、音高保持变速、主题换肤、游戏插件，还有 Gapless 播放、书签、播放列表等专业功能。下面从实际使用频率最高的几个方向拆解。

### 音频格式：原厂播不了的，Rockbox 通吃

Rockbox 最大的护城河之一就是**软件解码**——它用自己写的解码器替代了原厂固件的，理论上能播 Rockbox 项目实现的任何格式。这意味着：

- **FLAC / APE / ALAC / WavPack** 等无损格式在 iPod Classic 上直接就能播，原厂固件做不到
- **Opus / Musepack / Ogg Vorbis** 等小众但高质量的格式，在 SanDisk Clip 上原生支持
- 甚至连 **SID（Commodore 64）、NSF（NES）、VGM（世嘉）** 这类复古游戏音频也能直接播放

| 格式类别 | 代表格式 | 原厂固件支持 | Rockbox 支持 |
|---------|---------|------------|-------------|
| 无损 | FLAC / APE / ALAC | 部分缺失 | 全覆盖 |
| 有损高质量 | Opus / Musepack / Ogg Vorbis | 普遍缺失 | 全覆盖 |
| 杂项游戏音频 | SID / NSF / VGM / MOD | 不支持 | 通过 MikMod 插件支持 |
| DRM 保护 | FairPlay / WMA DRM | 部分支持 | **不支持**（项目明确立场） |

> [!NOTE]
> 带 DRM 的音频文件在 Rockbox 上**不能播放**——这是 Rockbox 项目的明确立场，不是技术限制。iTunes Match 导出的无 DRM m4a、Bandcamp 下载的 FLAC、Qobuz 的无损格式全部支持。Monkey's Audio（APE）的最高压缩比 profile 在老设备上可能不是实时解码——播放时会卡顿或跳帧，新设备（iPod Classic 之后）没有这个问题。

### 变速播放：倍速不变调

原厂固件的变速功能大多是「直接加速/减速」——2x 速听起来所有人都变成花栗鼠。Rockbox 用 **SOLA（Synchronous Overlap Add）** 算法，在保持音高不变的前提下改变播放速度：

- **0.5x ~ 2.0x** 连续可调，步长可选 0.05x / 0.1x / 0.2x
- **Preserve Pitch** 默认开启——关掉就变回原厂那种变调模式，别关
- 支持「始终固定速度」和「按需按键切换」两种模式

按需切换的典型场景：平时听音乐用正常速度，听播客/有声书时长按 Play + 方向键快速调到 1.25x ~ 1.5x。不同设备的按键组合不同，但都可以在 Settings → General Settings → Keymap 里自定义。

> [!TIP]
> 1.25x ~ 1.5x 是播客和有声书的黄金区间——理解度基本不下降，但能节省 20%~33% 的时间。语言学习时 0.5x ~ 0.8x 能听清每个音节，1.2x ~ 1.5x 可以模拟母语者语速训练理解力。乐器练习时用 0.7x 速听吉他 riff，保持音高不变意味着可以直接跟着弹而不用调弦。

### 麦克风录音：无损多格式支持

Rockbox 的录音功能**硬依赖设备有没有内置麦克风**——iPod Classic 没有，录音菜单会显示为灰色；SanDisk Sansa Clip/Clip Zip、iRiver H140、Cowon D2、Archos Jukebox Recorder 系列、复古掌机 RG35XX/TrimUI 等都有内置麦克风，可以直接用。

| 维度 | 原厂固件录音 | Rockbox 录音 |
|------|-------------|-------------|
| 格式 | 通常只有 MP3 | WAV / WavPack / AIFF / MP3 |
| 采样率 | 通常固定 | 可选 44.1kHz / 48kHz / 96kHz |
| 位深 | 通常固定 | 可选 16-bit / 24-bit |
| 声道 | 单声道 | 单声道 / 立体声 |
| 增益控制 | 自动 | 自动 + 手动 |
| 定时录音 | 无或简陋 | 完整支持 |

进入 Settings → Recording → Recording Settings 可以配置全部录音参数。高质量录音建议：格式选 WAV 或 WavPack（无损），采样率 48kHz/96kHz，24-bit，手动增益时对着麦克风说话看电平表——峰值不要超过 0dBFS（红色区域），留 -6dB 余量防削波。

录音文件默认按时间戳命名 `YYYYMMDD-HHMMSS.wav`，可以自定义命名格式（支持年/月/日/时/分/秒/序号占位符），自动保存到 `/Recordings/` 目录。录完的文件直接拷到电脑上，Audacity、Adobe Audition、GarageBand 等软件都能直接打开编辑。

> [!WARNING]
> 录音时会暂停正在播放的音乐——这是硬件限制，Rockbox 无法绕过。FM 收音机录音例外。

### 社区与支持

Rockbox 没有商业公司，项目由全球志愿者社区维护——核心开发者约 5~10 名长期维护者，加上提交 bug 报告、编写代码、制作主题、翻译文档的贡献者。官方沟通渠道主要是 Libera.Chat 的 #rockbox IRC 频道和 rockbox-dev 邮件列表，Wiki 和帮助手册是官方文档的主要载体。

项目至今没有任何硬件厂商正式提供代码或开发硬件支持，所有设备端口都是逆向工程完成的——开发者靠硬件调试器、反汇编原厂固件、阅读公开的硬件规格书来搞懂每款设备。如果你喜欢 Rockbox，可以通过写代码、制作主题或插件、捐赠硬件、翻译文档、提交 bug 报告等方式支持社区。

---

## 🔗 相关资源

- [Rockbox官网](https://rockbox.kook.yoga)
- [Rockbox文档](https://rockbox-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/Rockbox)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/Rockbox)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/Rockbox)

---

## 📚 相关文章

- [Rockbox 为什么能在 160MHz 的 iPod 上跑 Doom？](https://laoguosofts.github.io/posts/rockbox-docs.html)

- [一台 DAP 的"软件越狱"——Rockbox 凭什么让原厂固件自惭形秽](https://alipymanbu.github.io/posts/rockbox-docs.html)
