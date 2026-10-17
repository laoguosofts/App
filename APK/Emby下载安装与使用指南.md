## ✨ Emby 是什么

Emby 是一款开源的个人媒体服务器软件——简单说就是把你电脑、NAS 上零散的电影、电视剧、音乐整理成一个"私人 Netflix"，然后通过客户端或浏览器在手机、平板、电视上随时随地播放。

它的核心逻辑很朴素：**Server 端负责扫描你的媒体文件、自动抓海报和剧情、转码适配不同设备；Client 端只负责展示和播放**。Server 支持几乎所有主流操作系统（Windows、Linux、macOS、Docker、群晖/威联通 NAS 套件），客户端覆盖了 Android TV、Apple TV、Fire TV、iPhone、Android、浏览器等几乎所有消费级终端。

适合谁用？家里有一台 NAS 或常年不关机的电脑、硬盘里堆了几十部电影和电视剧、想让家人在各自的设备上方便地看片的人。不想折腾的话，群晖 NAS 上点几下就能装好；喜欢折腾的话，Docker 一条命令也能跑起来。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://emby.fewz.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 端在 Google Play 搜 Emby，iOS 端在 App Store 搜 Emby，Android TV 直接在应用商店装 Emby for Android TV；PC 软件建议从官网下载 exe 或 zip 便携版

---

## 🔧 Emby 安装指南

Emby Server 的安装路径因平台不同差异很大，这里按最常见的几种场景分别说明。

### Windows 安装

最简单的方式是下载 `.exe` 安装包，双击一路下一步即可。安装过程中会自动注册为 Windows 服务，开机自启，不需要手动配置。

> [!TIP]
> 如果不想让 Emby 以服务方式运行（比如临时试用或 U 盘环境），下载 zip 便携版解压后直接双击 `EmbyServer.exe` 即可——关闭窗口就停止，不留后台进程。

安装完成后，浏览器会自动弹出启动向导，按提示创建管理员账号、选媒体库路径、完成初始配置。

### NAS 安装（群晖 DSM）

群晖是 Emby 最受欢迎的部署平台。两种方式可选：

| 方式 | 优点 | 缺点 |
|------|------|------|
| **DSM 套件** | 点几下就装好，集成度高，自动配置权限 | 更新可能滞后于官方 |
| **Docker** | 随时升级最新版本，灵活挂载路径 | 需要手动写 docker-compose |

套件方式：打开群晖 DSM 的套件中心 → 搜 Emby → 安装。安装完成后同样会弹出启动向导。

Docker 方式更灵活，一条 compose 就能跑：

```yaml
version: "3"
services:
  emby:
    image: emby/embyserver:latest
    container_name: emby
    ports:
      - "8096:8096"
    volumes:
      - /volume1/docker/emby/config:/config
      - /volume1/video:/media
```

### 其他平台速查

- **macOS**：下载 `.dmg` 安装包，拖进 Applications 文件夹即可
- **Linux（Ubuntu/Debian）**：官方提供 `.deb` 包，`dpkg -i` 安装；也支持 snap 和 flatpak
- **Android TV 盒子**：侧载 Emby Server APK，适合媒体文件在盒子本地的场景

### 安装后核对

首次启动后，浏览器访问 `服务器IP:8096` 应该能看到 Emby 的启动向导页面。如果打不开，优先检查防火墙是否放行 8096 端口。

> [!WARNING]
> 首次使用务必创建一个管理员账号并记住密码——Emby 不提供密码找回功能，丢了只能删配置重建。

---

## 📖 Emby 使用入门

### 添加媒体库

启动向导里会让你选媒体库类型和路径，向导之后也能随时在 Dashboard 里添加。Emby 支持的库类型包括电影、电视剧、音乐、家庭视频、书籍、图片等。

添加库之后，Emby 会自动扫描路径下的文件，然后去元数据源拉海报、评分、剧情、分级等信息。

### 文件命名规范

**元数据扫描失败 90% 是因为命名不对**。Emby 靠解析文件名来搜索元数据源，格式差一点就搜不到：

- 电影：`星际穿越 (2014).mkv` 或 `Interstellar (2014).mkv`——年份必须加
- 电视剧：`绝命毒师/Season 01/s01e01.mkv`——按 Season/Episode 组织
- 音乐：`周杰伦/叶惠美/01-晴天.flac`——按艺人/专辑/曲目组织

> [!NOTE]
> 文件名被下载站广告污染是最常见的坑，比如 `[广告]星际穿越高清版[蓝光1080P].mkv` 这种，Emby 根本匹配不到。推荐用 FileBot 批量重命名。

### 家长控制分级

Emby 的家长控制有三种独立过滤机制，可以叠加使用：

1. **按分级（Rating）**：自动匹配 MPAA（G/PG/PG-13/R/NC-17）和 TV Parental Guidelines（TV-Y 到 TV-MA），设一个最大级别，高于它的内容全部屏蔽
2. **按标签（Tags）**：手动给内容打标签（"成人向"、"暴力"、"适合全家"），然后屏蔽带特定标签的内容
3. **按访问时间表（Schedule）**：限制某个用户一周中哪些时间段能访问

配置位置：Dashboard → Users → 点击目标用户 → Parental Control。

| 分级 | 说明 | 典型内容 |
|------|------|----------|
| G / TV-Y | 全年龄适合 | 儿童动画 |
| PG / TV-G | 建议家长指导 | 家庭片、普通动画片 |
| PG-13 / TV-14 | 13 岁以下建议家长陪同 | 轻度暴力、语言、惊悚 |
| R / TV-MA | 17+ 才能看 | 强烈语言、暴力、裸体 |
| NC-17 | 仅限成人 | 成人向内容 |

推荐做法是**先分库粗粒度隔离，再用家长控制在主库上做精粒度过滤**——比如孩子的账号只能看到动漫库，动漫库里再用分级和标签过滤掉不适合的。

### 远程访问：Emby Connect

在家能用只是第一步，出门在外也想回家看片怎么办？Emby Connect 是最省心的方案。

核心逻辑很简单：你在服务器端和远程设备端都登录同一个 Connect 账号，Connect 云服务帮你做 NAT 穿透——优先尝试 P2P 直连，失败了走中继转发，不需要手动配置端口转发。

**服务器端一次性配置**：

1. 让每个要远程访问的用户去注册 Emby Connect 账号
2. Dashboard → Users → 每个用户 → 关联他的 Connect 用户名
3. Dashboard → Advanced → Host → Networking → 勾选 "Enable remote connections"
4. 确认 Emby Connect 页面显示 "Connected"

**远程设备上**：打开 Emby App → Sign in with Emby Connect → 登录后家里的 Server 自动出现在列表里，点进去直接播放。

> [!TIP]
> Emby 客户端会自动按优先级尝试连接：本地直连 → P2P 穿透 → 公网 IP 直连 → 中继。如果家里是运营商 CGNAT（公网 IP 是 100.64.x.x 私有段），Connect 和端口转发都可能没用，这时候可以用 Tailscale 或 ZeroTier 组网，两边加入同一个虚拟网络后直接用虚拟 IP 访问——这是终极方案。

### 媒体库不扫描元数据排查

扫描完库但海报没出来、名字匹配错了、或者进度条卡住不动？按这个顺序排查：

1. **文件命名对不对**——最常见原因，对照命名规范检查
2. **Server 能访问外网吗**——Emby 需要联网去 TheMovieDB / TheTVDB 拉元数据，Windows 防火墙、Docker 网络、群晖 DSM 防火墙都可能拦出站
3. **元数据语言设置对不对**——Dashboard → Library → 对应库 → Advanced Settings → Metadata language，想要中文就设成 Chinese Simplified
4. **IPv6-only 网络**——某些 NAS 默认只开 IPv6 出站，但元数据源很多只支持 IPv4，导致 Emby 连不上。需要在路由器或 DSM 里确保 IPv4 出站是开启的
5. **SMB/CIFS 共享不支持实时监控**——如果媒体路径是 NAS 上的 SMB 共享，Real-time monitoring 不工作，需要靠定时扫描或手动触发 Scan Library Files

### 自带 Web App 的便利性

值得单独提一下——Emby Server 自带一个完整的 Web 客户端，任何设备的浏览器打开 `服务器IP:8096` 或 `app.emby.media` 登录 Connect 后，就能浏览媒体库、播放、管理用户、配置转码。新设备临时想用的时候，什么客户端都不用装，浏览器直接搞定。

---

## 🔗 相关资源

- [Emby官网](https://emby.fewz.tech)
- [Emby文档](https://emby-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Emby)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Emby)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/Emby)

## 📚 相关文章

- [Emby 深度解析：转码判断链、硬件加速与四维度兼容策略](https://laoguosofts.github.io/posts/emby-docs.html)
- [Emby 串流播放的三种模式到底在耍什么花招](https://alipymanbu.github.io/posts/emby-docs.html)
