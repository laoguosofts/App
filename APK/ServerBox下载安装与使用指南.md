## ✨ ServerBox 是什么

ServerBox 是一款开源的跨平台服务器管理工具，最早以 SSH 远程终端起家，后来逐步演进为覆盖进程监控、跳板机链式连接、批量运维等场景的轻量级运维工作台。它基于 Flutter 开发，同时发布 Android、Windows、macOS 和 Linux 版本，让你在手机上也能随时连上服务器跑命令、看进程、杀卡死的服务。

对于手上有好几台 VPS、树莓派或者公司内网机器的个人开发者和小团队来说，ServerBox 解决了一个很实际的痛点：不用每次打开终端敲 `ssh`，也不用记住那一堆 `-i`、`-p`、`-J` 参数——所有服务器配置图形化管理，跳板机自动链式跳转，进程列表一键展开，信号发送点按钮就够了。

开源社区是 ServerBox 更新迭代的主要动力，GitHub 上的 Releases 始终是最新版本首发渠道，F-Droid 社区也会跟进编译发布。无论你是想在手机上应急连个终端，还是需要在外出时远程重启一个卡死的服务，ServerBox 都能覆盖到。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://serverbox.fewz.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - F-Droid：开源社区认证渠道，搜索 "ServerBox" 或 "flutter_server_box"，支持自动更新
> - GitHub Releases：各平台 APK/安装包最新版本第一时间发布，适合追求更新节奏的用户

---

## 🔧 Android 平台安装步骤

ServerBox 目前没有上架 Google Play，Android 用户主要通过 GitHub Releases、F-Droid 和 CDN 镜像获取安装包。

### 下载渠道对比

| 渠道 | 优点 | 注意点 |
|------|------|--------|
| GitHub Releases | 最新版本第一时间发布，有完整变更日志 | 需要手动下载更新 |
| F-Droid | 开源社区认证，支持自动更新 | 版本更新略慢于 GitHub |
| CDN 镜像 | 下载速度快，适合国内用户 | 更新节奏跟 GitHub |

### 推荐：从 GitHub 下载

1. 打开 ServerBox 项目的 Releases 页面
2. 找到最新版本，向下滚动到 Assets 区域
3. 现代安卓手机（arm64 架构）下载 `serverbox-android-arm64.apk`
4. 2018 年以前的老机型（armeabi-v7a 架构）选对应的 `serverbox-android-armeabi-v7a.apk`

### 安装流程

下载完成后在手机文件管理器里找到 APK 文件，点击即可安装。如果系统弹出「未知来源」提示，进入系统设置允许该来源安装即可——这是 Android 的安全机制，不是 ServerBox 本身的问题。

> [!TIP]
> 安装前确认两个细节：APK 大小约 25~30MB，安装后内部存储占用约 80MB；直接下载新版 APK 覆盖安装不会丢失已保存的服务器配置，放心升级。

首次启动时 ServerBox 会请求通知、存储等权限，全部允许即可正常使用。国内网络访问 GitHub 较慢时，可以改用 CDN 镜像或项目 README 里的备用下载链接。

---

## 📖 使用入门：从单台连接到跳板机集群

### 添加第一台服务器

打开 ServerBox 主界面，点击「添加服务器」开始录入你的第一台机器。必填项很简单：服务器主机名或 IP、SSH 端口（默认 22）、登录用户名，以及密钥或密码二选一的认证方式。填完后点「测试连接」，看到成功提示就可以保存了。

保存后的服务器会以卡片形式出现在主界面，点击卡片直接进入 SSH 终端。ServerBox 的终端支持手势缩放、分屏、后台运行，手机上临时跑个 `top`、`tail -f` 监控日志都很顺手。

### 批量导入已有 SSH 配置

如果你电脑上已经维护了一份 `~/.ssh/config` 文件，ServerBox 可以一键解析里面的所有 Host 条目，自动填充服务器地址、端口、用户名和密钥配置。

桌面端操作路径：设置 → 服务器管理 → 从 SSH config 导入，默认读取 `~/.ssh/config`，也可以手动指定其他路径。移动端则需要先用 AirDrop 或云盘把 config 文件传到手机，再在「添加服务器」页面选择「从文件导入」。

> [!NOTE]
> ServerBox 能正确解析 `ProxyJump`、`IdentityFile`、`Port` 等常见字段，如果你在 config 里已经写好了跳板机链，导入后跳板机设置会自动填好，不用手动重复配置。

### 通过跳板机链式连接内网服务器

跳板机（Bastion Host）是内网运维的常见架构——公网只暴露一台入口机器，所有运维操作通过它中转到内网服务器。传统命令行的写法是 `ssh -J user@jump user@internal`，ServerBox 把这个流程图形化了。

**配置跳板机的两步走：**

第一步，把跳板机本身当作一台普通服务器添加到 ServerBox，填入它的公网地址和登录信息，测试连接成功后保存。

第二步，添加内网服务器时，填完内网地址（比如 `10.0.0.11`——这个地址只有跳板机能访问）后，下拉找到「跳板机 / ProxyJump」选项，选刚才添加的跳板机即可。ServerBox 会自动执行等价的链式 SSH 操作，后续你只需要点目标服务器的卡片，它会先连跳板机、再从跳板机跳进去，整条链路对用户完全透明。

**多层跳板也支持：** 有些生产环境不止一层入口，ServerBox 可以逐层配置——第一台跳板机连公网，第二台跳板机指定 ProxyJump 第一台，目标服务器再指定 ProxyJump 第二台，完整链路自动串联。跳板机本身不需要特殊权限，只要能通过 SSH 访问内网目标服务器、装了 OpenSSH client（默认都有）就行。

> [!WARNING]
> 跳板机链路里的每一跳主机密钥都会被 ServerBox 校验，任何一跳的 host key 发生变化都会被拦截——这是安全设计，不要随便跳过确认。

### 进程管理：发信号与批量操作

ServerBox 的进程管理面板能让你像在系统监视器里一样查看服务器进程列表，并向进程发送不同的控制信号。这在远程应急时特别有用——不用敲 `kill -9`，点按钮就行。

**五种常用信号对应关系：**

| 信号 | ServerBox 按钮 | 适用场景 |
|------|---------------|---------|
| SIGTERM | 终止（默认） | 优雅停止，让进程做清理工作 |
| SIGKILL | 强制终止 | 进程卡死、不理 SIGTERM 时使用 |
| SIGSTOP | 暂停 | 冻结进程，暂时不占 CPU |
| SIGCONT | 继续 | 恢复被暂停的进程 |
| SIGHUP | 重载配置 | nginx、apache 等服务收到后重载配置 |

日常操作优先使用「终止」按钮发送 SIGTERM——大多数服务收到后会主动关闭连接、保存数据、写日志再退出，是最安全的停止方式。等 3~5 秒进程还在，再换「强制终止」的 SIGKILL。

**批量杀进程：** 进程面板支持按关键词过滤，比如在搜索框里输 `python` 筛出所有 Python 进程，全选后批量发送终止信号。这个功能在某个服务 spawn 了一堆失控子进程时特别实用，比敲 `pkill` 还直观。

> [!WARNING]
> ServerBox 能杀掉你有权限操作的**任何进程**，包括 sshd、systemd、Docker 守护进程这类关键服务。生产服务器上操作前，先用 `ps -fp <PID>` 或 `strace -p <PID>` 确认进程在做什么，别盲目 kill。

### 设备间迁移

换手机或换电脑时，ServerBox 的服务器配置可以无损迁移。旧设备上导出：设置 → 服务器管理 → 导出服务器列表，推荐选 JSON 格式（包含密钥文件引用）。新设备上导入：设置 → 服务器管理 → 从文件导入，选择刚导出的 JSON，同时记得把用到的 SSH 私钥文件也一并迁过来。

导出的 JSON 文件里存的是密钥文件的路径引用，不是密钥内容本身——迁移时需要手动搬运私钥文件。也不要把导出文件上传到不安全的云存储，毕竟里面包含了你所有服务器的连接信息。

---

## 🔗 相关资源

- [ServerBox官网](https://serverbox.fewz.tech)
- [ServerBox文档](https://serverbox-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/ServerBox)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/ServerBox)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/ServerBox)

## 📚 相关文章

- [ServerBox 这几年是怎么长大的：从 SSH 玩具到运维工作台](https://laoguosofts.github.io/posts/serverbox-docs.html)
- [ServerBox 这一路：从单服务器面板到集群运维控制台](https://alipymanbu.github.io/posts/serverbox-docs.html)