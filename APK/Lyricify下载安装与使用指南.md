## ✨ Lyricify 是什么

Lyricify 是一款专为 Spotify 用户打造的 Windows 桌面歌词增强工具。它解决了 Spotify 桌面客户端长期以来缺少原生逐字卡拉 OK 歌词体验的痛点，通过读取 Spotify 的播放进度，从 QQ 音乐、网易云音乐等词源自动搜索并同步展示歌词，配合灵动的词岛、桌面歌词浮窗、Apple Music 风格逐字界面等多种展示形态，让听歌体验提升一个档次。

除了歌词展示，Lyricify 还内置了时间差校准、全局快捷键、歌词标记与管理、智能引擎等实用功能。后续推出的 Fusion 版本进一步突破了 Spotify 绑定，改为走 Windows SMTC 系统接口，让 QQ 音乐、网易云、Apple Music for Windows 等主流播放器都能享受到同样的歌词增强层。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://lyricify.fewz.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - GitHub Releases：开源项目，源码与二进制同仓库发布

---

## 🔧 安装步骤

Lyricify 4 和 Lyricify Fusion 共享同一套安装流程，运行要求略有不同。

### 前置组件检查

安装前先确认系统已安装对应的 .NET 运行时：

> [!WARNING] 版本对应关系
> - **Lyricify 4** 依赖 **.NET Desktop Runtime 6.0**
> - **Lyricify Fusion** 依赖 **.NET Desktop Runtime 8.0**
> - 两个 Runtime 可以共存，不互相冲突
> - 未安装时安装程序会自动提示下载

### 安装过程

1. 从下载入口获取最新版本的安装程序（`.exe` 或 `.msixbundle`）
2. 双击运行安装程序，按向导选择安装路径（默认 `C:\Program Files\Lyricify`）
3. 首次启动时进入欢迎界面，选择预设配置：
   - **默认配置**：均衡效果与性能，推荐绝大多数用户
   - **更好的性能**：降低 Apple Music 界面渲染细节，适合集显或低配电脑
   - **更好的体验**：启用全部动画与动态背景，适合高性能独显
4. 如果选择 Spotify 版（Lyricify 4），还需要在首次启动时完成 Spotify 账户的 Web API 授权跳转

### 装完核对

打开 Lyricify 后，先确认三件事：

1. **版本号**：主菜单 → 关于，对比下载页标注的最新版本
2. **歌词浮窗是否可用**：按 `Ctrl + Shift + W` 测试桌面歌词开关
3. **Spotify 是否连通**（Lyricify 4）：主菜单 → 账户，看到你的 Spotify 用户名即表示授权成功

---

## 📖 使用入门

Lyricify 的核心能力可以归为三块：**歌词展示形态、校准与标记、跨播放器方案**。以下从这三个维度展开日常使用中最实用的功能点。

### 歌词展示：从灵动词岛到 Apple Music 风格

Lyricify 提供了多种歌词展示形态，默认的灵动词岛会在屏幕底部显示一行当前高亮的歌词，简洁不遮挡。常见的切换方式包括：

- **桌面歌词浮窗**：`Ctrl + Shift + W` 开关传统桌面歌词，`Ctrl + Shift + M` 切换到 Spotify 移动端 UI 风格
- **全屏歌词**：按 `F11` 进入 Lyricify 全屏，`Ctrl + F11` 进入移动端风格全屏
- **移动端滚动窗口**：`Ctrl + E` 打开类似手机 App 界面的独立歌词窗口

> [!TIP] 字号快速调整
> 桌面歌词和主界面都支持 `Ctrl + 滚轮上/下` 快速缩放字号，按 `Ctrl + 0` 恢复默认值（默认 10pt，可自定义）。

其中 Apple Music 风格界面是最惊艳的展示形态——逐字卡拉 OK 动画配合专辑封面渐变和动态背景，视觉效果拉满。但它也是 Lyricify 里 GPU 渲染压力最大的管线，集显或 4K 显示器上可能出现掉帧和延迟。

**Apple Music 界面卡顿怎么办？**

按从轻到重的顺序排查，通常前两步就能解决：

1. **切换预设配置**：主菜单 → 设置 → 预设配置，改成「更好的性能」——这是最有效的单步优化，Lyricify 会自动降低渲染细节、关闭部分动画过渡、降低动态背景刷新频率
2. **禁用动态背景**：主菜单 → 窗口 → 勾选「禁用动态背景」，背景变成纯色、专辑封面不再参与渲染，完全消除 GPU 压力
3. **检查 GPU 驱动**：去显卡厂商官方下载页更新最新版驱动，老驱动在 Win10/11 硬件加速场景下经常有兼容性问题
4. **关掉其他 GPU 密集应用**：Chrome 多标签视频、视频播放器、游戏、网易云桌面版等都会抢 GPU 资源
5. **降低显示器分辨率**：4K + 集显的渲染压力是 1080p 的 4 倍，临时降到 1080p 验证显卡是否扛得住
6. **退回普通歌词界面**：以上都试了还卡，换回麦克风图标那个普通界面，渲染压力极低，任何电脑都能流畅跑

### 时间差校准：让歌词和歌声完美对齐

Lyricify 读取的播放进度来自 Spotify 服务器 API 的返回值，服务器看到的客户端进度和你本地实际听到的声音之间，通常会差几百毫秒。网络条件越好偏差越小，不同访问方式的典型偏差范围：

| 访问方式 | 典型偏差范围 |
|---|---|
| Spotify 桌面客户端 + 稳定网络 | ±300ms 以内 |
| Web Player（网页版） | +500ms ~ +800ms |
| 手机 App | 取决于网络延迟 |

**校准步骤**：

1. 播放一首你熟悉的歌
2. 主菜单 → 账户 → 找到「设置整体时间差」区域
3. 四个箭头按钮（步进值可改，默认每个调整 100ms）：

| 按钮 | 效果 |
|---|---|
| `←` | 整体提前 100ms（歌词往更早的方向挪） |
| `→` | 整体延后 100ms |
| `↑` | 大步长提前 |
| `↓` | 大步长延后 |

4. 边听边调，对齐后数值是**全局生效**的——所有歌曲共用一个时间差值

> [!NOTE] Web Player 优化会锁定时间差
> 如果你在使用 Spotify Web Player，Lyricify 有一个「Web Player 优化」开关——打开后会自动用 Media Session 读取 Web Player 的播放进度（更准），同时**锁定时间差设置**不允许手动修改。如果开了优化还是有明显偏差，可以先关掉优化、手动校准满意后再决定是否开回去。

### 全局快捷键：系统级响应，无需切窗

Lyricify 的快捷键是通过 Windows 的 `RegisterHotKey` API 实现的**系统级全局快捷键**——不管你当前在哪个软件（浏览器、文档编辑器、游戏窗口），只要按下快捷键就能响应。这也是为什么「歌词标记」功能被设计成全局快捷键——听到一句对的歌词随手按一下就行，不用切窗口。

默认快捷键一览：

| 功能 | 默认快捷键 | 备注 |
|---|---|---|
| 桌面歌词开关 | `Ctrl + Shift + W` | 传统版桌面歌词 |
| 移动端 UI 桌面歌词 | `Ctrl + Shift + M` | 移动端风格 |
| 移动端滚动窗口 | `Ctrl + E` 或 `Ctrl + Shift + E` | 类似手机 App 界面 |
| 全屏歌词 | `F11` | Lyricify 全屏 |
| 移动端 UI 全屏 | `Ctrl + F11` | 右键全屏按钮也能进 |
| 放大字号 | `Ctrl + 滚轮上` | 桌面歌词和主界面都有效 |
| 缩小字号 | `Ctrl + 滚轮下` | 同上 |
| 恢复默认字号 | `Ctrl + 0` | 10pt（可自定义） |
| 保存当前歌词 | `Ctrl + S` | 存为 .txt 或 .png 图片 |
| 歌词标记 | 自定义（建议 `Alt + M`） | 默认没设，需手动配置 |

**和别的软件冲突怎么办？** 常见冲突有 `Ctrl + E`（Windows 资源管理器搜索）、`Ctrl + Shift + W`（Chrome 关闭标签页）、`F11`（浏览器全屏）。冲突时在 Lyricify 设置 → 快捷键里改成组合键（比如加个 Alt）即可，全局快捷键支持 Ctrl、Alt、Shift、Win 的任意组合 + 字母/数字/F 键。高级用户也可以直接编辑设置 JSON 文件（主菜单 → 设置 → 手动（高级）打开），在 `keybindings` 数组里直接修改，改完保存后 Lyricify 自动热重载。

### Lyricify Fusion：跨播放器新方案

Lyricify 4 深度绑定 Spotify，依赖 Web API 授权。Fusion 版本则完全不同——它走 Windows 的 System Media Transport Controls（SMTC）接口，只要你的播放器正确接入了 SMTC，Fusion 就能读取播放进度、专辑封面、曲目信息，然后提供灵动词岛、桌面歌词、Apple Music 风格界面、歌词管理等一整套功能。

**Fusion 支持的播放器**：Apple Music for Windows、QQ 音乐桌面版、网易云音乐桌面版、酷狗音乐桌面版，以及其他任何正确注册 SMTC 的媒体应用。

两个版本的核心差异对比：

| 维度 | Lyricify 4 | Lyricify Fusion |
|---|---|---|
| 播放器绑定 | 仅 Spotify（Web API 授权） | 任何接入 SMTC 的播放器 |
| 歌词源 | QQ 音乐 / 网易云 | QQ 音乐 / 网易云 / 酷狗 |
| 运行依赖 | .NET Runtime 6.0 | .NET Runtime 8.0 |
| 内置播放 | 有 | 没有（它是增强层，不做播放器） |
| 智能引擎 | 有（Premium 独占） | 没有 |
| 安装包大小 | 差不多 | 差不多 |

> [!TIP] 两个版本可以共存
> Lyricify 4 和 Fusion 可以同时装在一台电脑上互不冲突。如果你有时候用 Spotify、有时候用 QQ 音乐，可以分别开两个来用——但两个同时打开会有 GPU 渲染开销叠加，建议按需启动。

**怎么选？**

- **选 Fusion**：主要用 QQ 音乐 / 网易云 / Apple Music 听歌，想要 Lyricify 风格的桌面歌词但不想切到 Spotify；多播放器混用时，Fusion 能提供统一的歌词增强层
- **继续用 Lyricify 4**：主要用 Spotify，需要内置播放功能或 Premium 订阅的智能引擎

---

## 🔗 相关资源

- [Lyricify官网](https://lyricify.fewz.tech)
- [Lyricify文档](https://lyricify-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/Lyricify)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/Lyricify)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/Lyricify)

## 📚 相关文章

- [Lyricify 快问快答：把 Spotify 歌词这件事讲透](https://laoguosofts.github.io/posts/lyricify-docs.html)

- [Lyricify 深度盘点：把 Spotify 歌词体验做到极致的开源项目](https://alipymanbu.github.io/posts/lyricify-docs.html)
