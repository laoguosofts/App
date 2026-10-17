## ✨ FluxDown 是什么：一个引擎五种形态的全能下载器

FluxDown 是一款用 Rust 编写的现代下载引擎，主打「一套引擎、五种形态」的架构——桌面端、Headless Server、NAS 套件、CLI 命令行、Android 原生应用，全部共用同一个 fluxdown_engine，协议解析、分段下载、Work Stealing 调度、BT/eD2K 客户端、SQLite 持久化、令牌桶限速这些底层能力完全一致，只是暴露给用户的界面不同。

协议覆盖方面，FluxDown 支持 HTTP/HTTPS、FTP、BT、eD2K、Magnet、HLS 六种协议，日常能遇到的下载场景基本都能吃下。桌面端永久免费、不需要注册账号，所有下载功能不登录即可使用；可选登录开通云同步（只同步设置偏好，永远不同步下载内容和历史任务）。

它的调度器用令牌桶做全局限速、Work Stealing 做并发分配，默认公平对待所有队列里的任务。对单个紧急任务可以点 Boost 按钮临时提升优先级——引擎给它分配额外工作线程、在令牌桶里开快速通道，其他任务只是暂时让道不会被暂停或取消。

SQLite 数据库是整个应用的真相源——下载记录、队列配置、RSS 订阅、Webhook 端点、主题偏好全部集中在一个 fluxdown.db 文件里，覆盖安装或迁移到新机器时只要保住这个文件，一切设置和历史就不会丢。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://fluxdown.boop.uno/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 可到应用市场搜索 FluxDown；桌面端安装包见上方下载页，Windows / macOS / Linux 均有对应安装版与便携版

---

## 🔧 安装步骤

FluxDown 提供桌面端安装版、便携版、Docker 镜像、DSM .spk 套件等多种分发形态，安装方式因平台而异。

### Windows 安装版

下载 setup.exe 后双击运行，安装器会引导你选择安装路径（默认 `C:\Program Files\FluxDown`），并自动注册 Native Messaging、`fluxdown://` 协议、`.torrent` 文件关联。如果系统弹出 SmartScreen 提示，选择「更多信息 → 仍要运行」即可。

> [!WARNING]
> 安装路径尽量不要放到系统盘根目录或受 UAC 严格管控的位置，否则便携版写入 data/ 目录时可能遇到权限问题。安装版自带服务权限，通常不受影响。

### Windows 便携版

解压 zip 到任意目录即可，不需要管理员权限。注意**不要手动删除 data/ 文件夹**——那是 SQLite 数据库所在，删了历史任务和设置就没了。

### macOS

下载 DMG 后打开，把 FluxDown.app 拖进 Applications 文件夹覆盖安装即可。SQLite 数据库位于 `~/Library/Application Support/FluxDown/fluxdown.db`，迁移到新 Mac 时记得一并带走。

### Linux

deb 包用 `sudo apt install ./FluxDown-<版本>.deb`，Arch 用户也可以用 `sudo pacman -U FluxDown-<版本>.pkg.tar.zst`。SQLite 在 `~/.local/share/fluxdown/fluxdown.db`。

### Docker Server（Headless 形态）

```
docker run -d --name fluxdown \
  -p 17800:17800 \
  -v fluxdown-data:/data \
  -v /你的下载目录:/downloads \
  zerx-lab/fluxdown:latest
```

启动后访问 `http://<容器IP>:17800` 即可打开 Web UI。数据卷 `fluxdown-data` 里存着 SQLite 和配置，升级时 `docker pull` 新镜像、停旧容器、用同一个数据卷起新容器就行。

### 安装后核对

启动 FluxDown 后看版本号和引擎状态是否正常——桌面端在「设置 → 关于」里能看到，Server 形态在 Web UI 首页右上角。首次启动会自动生成 SQLite 数据库，不需要手动初始化。

---

## 📖 使用入门：从单次下载到自动化流水线

FluxDown 的能力不止是点一下开始下载——它的统一引擎 + 统一 API 协议设计让跨形态协作和自动化变得很顺。下面分几个常见场景来讲。

### 基础下载：桌面端点一下就行

桌面端界面和常见下载器类似——工具栏有「添加任务」按钮，粘贴 URL 或拖入 `.torrent` 文件即可。HTTP 链接可以一次贴多条，BT 和 Magnet 会自动解析种子元数据。任务跑起来后看速度、进度、线程数，右上角实时显示总带宽。

下载过程中引擎用令牌桶做限速——默认不限制，你可以在「设置 → 下载偏好」里给全局或单个队列设上限。如果某个任务卡了或下完了，右键菜单里有暂停、继续、删除、打开文件所在位置等标准操作。

### Boost 让紧急任务先跑完

默认情况下调度器公平对待所有队列里的任务，共享同一个令牌桶和并发池。遇到其他下载都在跑、但有一个小文件或关键文件想先下完的情况，对那个任务点「Boost」就行——桌面端在任务列表右键，Web UI 在操作按钮里，CLI 用 `fluxdown boost <task-id>`，API 调 `POST /api/v1/tasks/<task-id>/boost`。

Boost 做两件事：临时给这个任务分配更多工作线程（突破它所在队列的正常并发上限），在全局令牌桶里给它开一条快速通道优先取令牌。其他任务不会被暂停或取消，只是暂时让道。Boost 效果持续到该任务跑完、你手动取消、或暂停/删除它。

> [!TIP]
> 一次只能 Boost 一个任务。如果同时有多个紧急任务，不如把它们挪到一个独立的加急队列、给加急队列设更高并发数，效果比同时 Boost 多个更好。

Boost 没有绕过统一队列和 SQLite 持久化——引擎只是在调度器内部把这个任务的优先级权重调高，让 Work Stealing 更快把空闲 worker 分配过去。总限速仍然生效，Boost 任务可以拿更多带宽但不会突破你设的全局限速。

### 跨形态协作：桌面端盯即时下载，NAS Server 跑大文件

这是 FluxDown 全家桶最有价值的用法之一——桌面端负责你盯着时的即时下载（比如临时要一个文件、顺手拖个链接），NAS 上起一个 Headless Server 负责 7×24 大文件、RSS 订阅自动下发、夜间计划任务。

所有形态对外暴露的 API 完全一样：HTTP REST（`/api/v1/tasks` 等）、aria2 RPC 兼容层（`/jsonrpc`）、MCP（给 AI agent 用）。这意味着你写了一个脚本给桌面端发任务，把 `127.0.0.1:17800` 换成 NAS 的 `<nas-ip>:17800` 就能远程下发。CLI 也支持远程：`fluxdown --server http://nas:17800 add https://...`，桌面端没开但 Server 在跑时照样能用。

出门在外想加个下载任务？Android App 里填 NAS 的 API 地址，手机上贴 URL、选保存目录、点下发，回家就能接着跑。甚至可以配置 MCP server，让 AI agent 用自然语言让 FluxDown 下载、查状态、暂停任务。

### 云同步：免费就能用，只同步设置不同步内容

桌面端永久免费、不需要账号——不登录的情况下六协议下载、插件、RSS、Webhook 全部 100% 可用。登录只是用来开通云同步的可选功能。

云同步只同步这些：外观主题和 accent 色、下载偏好（默认线程/限速/代理）、BT 偏好（端口/Tracker/Kad）、eD2K 偏好（Server 列表/Kad/UPnP）、插件列表。**下载任务、队列、历史记录、已下载的文件永远只在本地**——不上云、不做云存储，这是设计决策。

登录方式：设置 → 账号 → 邮箱或 OAuth（GitHub/Google）登录，当前设备自动标记为受信任设备。新设备上装 FluxDown 后同一账号登录，选「同步设置到本设备」或「把本设备设置推到云端」即可。

| 功能 | 不登录 | 免费版登录 | 专业版（一次买断） | 创始会员（一次买断） |
| --- | --- | --- | --- | --- |
| 六协议下载 | ✅ | ✅ | ✅ | ✅ |
| 插件/RSS/Webhook | ✅ | ✅ | ✅ | ✅ |
| 云同步设置 | ❌ | ✅ | ✅ | ✅ |
| 跨设备远程下发 | ❌ | ❌（2 设备） | ✅（无限） | ✅（无限 + 早鸟） |
| 社区加速 CDN | ❌ | ✅ | ✅ | ✅ |
| 新功能抢先 | ❌ | ❌ | ✅ | ✅ |

免费版登录后 2 台受信任设备，一次买断的专业版和创始会员解锁全部高级能力。云服务边际成本接近零，所以买断制可以无限期使用。

### 重装或迁移到新机器

SQLite 数据库是真相源——覆盖安装或迁移时只要保住 `fluxdown.db` 文件，设置、历史任务、队列、RSS 订阅、Tracker 列表、Webhook 端点就都在。

最推荐的做法是**覆盖安装**：不要先卸载，直接用新版安装。安装版会自动检测旧版本覆盖；便携版解压到同一目录但**不要删原来的 data/ 文件夹**；macOS 拖 `.app` 进 Applications 覆盖；Linux 用包管理器直接升级；Docker 拉新镜像、停旧容器、用同一个数据卷起新容器。

> [!NOTE]
> 如果新版改了数据库 schema，首次启动会自动做 migration，不需要手动处理。直接覆盖安装就行。

非要卸载后重装（比如旧版本坏了），先备份 `fluxdown.db`。路径如下：

| 安装方式 | SQLite 位置 |
| --- | --- |
| Windows 安装版 | `%APPDATA%\FluxDown\fluxdown.db` |
| Windows 便携版 | 解压目录旁边的 `data\fluxdown.db` |
| macOS | `~/Library/Application Support/FluxDown/fluxdown.db` |
| Linux | `~/.local/share/fluxdown/fluxdown.db` |
| Docker Server | 容器内 `/data/fluxdown.db`（host 上是数据卷） |

卸载 → 装新版 → 把备份的 db 放回对应位置 → 启动即可。

迁移到新机器同理：旧机器备份 db → 新机器装 FluxDown 启动一次让它生成初始数据库 → 关掉 → 用备份的 db 替换 → 启动。下载文件的落盘路径如果和旧机器不一致（比如旧机器在 `D:\Downloads` 新机器没有 D 盘），FluxDown 会在启动时提示路径丢失，手动改下载目录设置即可。

建议定期手动备份 SQLite 数据库——虽然引擎用原子写入，损坏概率极低，但磁盘故障或系统崩溃后电源突然断开都有可能影响。FluxDown 启动时检测到损坏会自动尝试修复，修复失败提示恢复备份。

---

## 🔗 相关资源

- [fluxdown官网](https://fluxdown.boop.uno)
- [fluxdown文档](https://fluxdown.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/fluxdown)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/fluxdown)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/fluxdown)

## 📚 相关文章

- [FluxDown 从零到自动下载：一个下载器能帮你省多少手动活](https://laoguosofts.github.io/posts/fluxdown.html)
- [FluxDown 从零上手实战：浏览器下载、流媒体抓取、断点续传一条龙搞定](https://alipymanbu.github.io/posts/fluxdown.html)
