## ✨ Kikoeru 是什么：把音声库搬进自家服务器的轻量方案

Kikoeru 是一款面向音声爱好者的自托管音乐管理系统。它的核心定位很清晰——把你散落在硬盘、NAS、不同目录下的音声文件统一收拢，通过 RJ 番号自动抓取 DLSite 元数据（封面、标签、分类、发售日期），提供一个带搜索、筛选、在线播放的 Web 界面。软件本身不托管任何资源，所有文件都在你自己的存储上，服务端只做索引和元数据管理。

它适合哪些场景？如果你手头有几百到几千个 DLSite 音声作品、同人专辑或 ASMR 合集，想告别文件管理器里翻来翻去找音频的原始体验，又不想用体积臃肿的通用媒体服务器，Kikoeru 是目前社区里最贴近这个 niche 的选择。它没有 Jellyfin 那样复杂的多媒体转码管线，也不需要数据库层面的重量级依赖——一个 Docker 容器加几个挂载卷就能跑起来，单机 NAS 甚至 OpenWRT 软路由都能扛住。

> [!NOTE]
> Kikoeru 的元数据抓取依赖 DLSite 的页面结构，如果官方改版或触发反爬策略，封面和标签可能抓取不全。遇到这种情况重跑一次扫描通常就能恢复，极端情况下需要手动补图。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://kikoeru.fewf.click/download.html) —— 下载页整理了当前可用的版本和安装说明
> - Docker Hub：社区维护的 number17/kikoeru 镜像（最活跃的发行版，支持 Docker Compose 一键部署）
> - GitHub：源码仓库，有能力的可以自行构建

---

## 🔧 安装部署：Docker Compose 一条命令搞定

Kikoeru 目前最主流、最省心的部署方式是 Docker Compose。社区维护的 `number17/kikoeru` 镜像持续更新，兼容 x86 和 ARM 架构，Linux、NAS、OpenWRT 软路由、树莓派都能直接用。

### 前置检查

开始之前确认三件事：

1. **Docker 环境就绪**：`docker --version` 和 `docker compose version` 都能正常输出
2. **音声文件已经就位**：提前整理好你想接入的音声目录（比如 `/mnt/media/VoiceWork`）
3. **主机端口可用**：默认暴露端口建议避开 8888（容器内端口是 8888，主机侧自由映射）

### 创建目录与 docker-compose.yml

先建四个持久化目录：

```bash
mkdir -p /opt/kikoeru/{sqlite,covers,config}
```

四个目录的分工：

| 目录 | 用途 | 说明 |
| --- | --- | --- |
| sqlite | SQLite 数据库文件 | 全部索引、收藏、播放进度都在这里 |
| covers | 专辑封面缓存 | DLSite 抓取的封面图片存这里 |
| config | 配置文件 | Web 界面修改的配置会写入此目录 |
| VoiceWork | 音声库 | 挂载自硬盘上的实际音声路径 |

然后写 `docker-compose.yml`：

```yaml
services:
  kikoeru:
    image: number17/kikoeru:latest
    container_name: kikoeru
    ports:
      - 48488:8888
    volumes:
      - ./sqlite:/usr/src/kikoeru/sqlite
      - ./covers:/usr/src/kikoeru/covers
      - ./config:/usr/src/kikoeru/config
      - /mnt/media/VoiceWork:/usr/src/kikoeru/VoiceWork
    restart: unless-stopped
```

> [!WARNING]
> 音声库卷的左边是宿主机上的真实路径，右边容器内路径建议保持 `/usr/src/kikoeru/VoiceWork` 不变——Web 界面添加根文件夹时会用到容器内路径。两边写反了容器启动正常但扫描找不到任何文件。

### 启动与验证

```bash
cd /opt/kikoeru
docker compose up -d
```

等几秒容器起来，浏览器访问 `http://<服务器IP>:48488` 就能看到 Web 界面。如果是云服务器，记得在安全组或防火墙里放行对应的主机端口。

### 日常维护命令

```bash
docker compose down        # 停掉并删除容器（数据卷保留，不会丢音声和索引）
docker compose up -d       # 重新拉起
docker compose pull && docker compose up -d   # 升级镜像
```

`restart: unless-stopped` 保证机器重启后容器自动恢复，不用手动再拉一次。升级前建议先把 `sqlite/` 目录备份，虽然理论上不会丢数据，但 SQLite 数据库在升级过程中有极罕见的兼容性风险。

---

## 📖 使用入门：从音声库接入到多端消费

### 添加音声根文件夹：支持多硬盘挂载

Kikoeru 没有"主库"概念——所有根文件夹平等对待，扫描时一视同仁。如果你的音声分散在多块硬盘或 NAS 的不同共享目录里，直接全加进来就行。

在 Web 界面找到 **设置 → 音色库**（不同版本位置略有差异，可能在侧边栏或顶部菜单），点「添加新文件夹」，输入**容器内路径**而不是宿主机路径。举个例子，如果你的 `docker-compose.yml` 里写了：

```yaml
volumes:
  - /mnt/nas/audio/RJ:/mnt/audio/rj
  - /mnt/nas/audio/ASMR:/mnt/audio/asmr
```

Web 界面里就依次填 `/mnt/audio/rj` 和 `/mnt/audio/asmr`。填完点「添加」，路径就出现在根文件夹列表里。

> [!TIP]
> Docker 用户最容易踩的坑：Web 界面里填了宿主机路径（比如 `/mnt/nas/audio/RJ`），容器里根本找不到，扫描日志直接报"目录不存在"。记住一个原则——Web 界面里看到的路径 = 容器内的路径。

添加完根文件夹后立即跑一次扫描测试，日志输出"找到 N 个文件"就说明路径对了；报"目录不存在"就回去检查 `docker-compose.yml` 的挂载映射。

### Android 端选择：Web PWA vs KikoFlu 原生客户端

Kikoeru 没有官方 Android 原生客户端，但生态里有两个成熟方案可选。

**方案一：Web PWA（最简单，零安装）**

Chrome 打开你的 Kikoeru 地址，三点菜单 →「添加到主屏幕」，桌面上就出现了一个全屏启动的图标，体验接近原生 APP。优点是不用维护、和 Web 端完全一致；缺点是后台播放和离线下载不如原生。

**方案二：KikoFlu（第三方原生，功能最强）**

社区开发的 KikoFlu 是目前功能最完整的 Android 客户端，连接 Kikoeru-express 后端 API，基于 REST + Socket.io 实时推送。它的特色功能包括：

| 功能 | 说明 |
| --- | --- |
| 高级搜索 | 多标签 AND/OR/排除、评分/发售日期多维筛选 |
| 多账户 | 配置多个 Kikoeru 服务器随时切换 |
| 离线下载 | 完整的音频下载和离线播放 |
| 悬浮窗 | 切其他 APP 时播放器悬浮控制条 |
| 缓存管理 | 自定义缓存大小、智能清理策略 |
| UI 自定义 | 主题配色、简洁模式/详细模式切换 |

怎么选？只是想手机上听听、不想折腾 → Web PWA；需要悬浮窗、离线、多服务器切换 → KikoFlu。

### 封面缺失排查：分三级诊断

作品详情页封面不显示是最常见的问题，症状分三种：

**个别作品没封面** → DLSite 上这几个作品本身就没上传封面，不是你的问题。接受默认占位图或手动找图补都行。

**很多作品一起没封面** → 大概率是 DLSite 反爬导致批量下载失败，或者扫描器中途异常退出、容器的 covers 目录没有写权限。排查时先看容器里 covers/ 目录有多少文件：

```bash
docker exec -it kikoeru ls /usr/src/kikoeru/covers | wc -l
```

和数据库里的作品数量对比，差距大说明下载确实没完成。解决方案：修一下 covers/ 目录权限，然后重跑一次完整扫描——扫描器有"封面缺失就重新下载"的逻辑。

**所有封面都显示破碎** → 封面文件存在但打不开，说明下载到了半截（0KB 或极小文件）。直接清空 covers/ 缓存再扫描：

```bash
docker exec -it kikoeru rm -rf /usr/src/kikoeru/covers/*
```

正常封面图片应该在 20KB ~ 200KB 之间，如果 covers/ 里出现 0KB 文件就是下载不完整的铁证。

### 单作品封面刷新

有的版本在作品详情页有「刷新元数据」按钮——点一下会重新去 DLSite 抓该作品的全部信息（包括封面）。适合只想修几个作品的场景，不用跑全库扫描。

### 删除根文件夹的注意事项

在 Web 界面删除一个根文件夹时，数据库里已扫描的作品条目**不会自动删除**——只是文件路径失效。下次扫描时这些失效条目会被识别为"孤立条目"，系统会问你要不要清理。所以如果你真的卸掉了一整块硬盘，先删根文件夹再跑扫描，数据库会自动同步。

---

## 🔗 相关资源

- [kikoeru官网](https://kikoeru.fewf.click)
- [kikoeru文档](https://kikoeru.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/kikoeru)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/kikoeru)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/kikoeru)

---

## 📚 相关文章

- [把几千个 DLSite 音声作品收进自家服务器，kikoeru 上手一周记](https://laoguosofts.github.io/posts/kikoeru.html)
- [折腾了三天 Kikoeru，终于把我那堆 RJ 番号音声整理明白了](https://alipymanbu.github.io/posts/kikoeru.html)
