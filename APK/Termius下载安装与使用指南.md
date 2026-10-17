## ✨ Termius 是什么

Termius 是一款面向运维工程师和开发者的跨平台 SSH 客户端。它解决的核心问题很朴素——当你手上有几十台甚至上百台需要经常连接的服务器时，传统 SSH 命令行工具（自带的 OpenSSH、PuTTY）很快就会让人疲于记忆 IP、端口、密钥密码。Termius 把「连接管理 + 命令片段 + 端口转发 + 跨设备同步」揉进了一个统一界面，让日常运维的重复动作少很多。

Vaults 架构是 Termius 的骨架。你可以把 Vault 理解为一个加密的远程主机容器——里面装着主机信息、密钥、Snippets、端口转发规则等一切运维资产。Personal Vault 存你自己的，Team Vault 可以在团队间共享。Vault 本身有端到端加密，即使同步到云端也不用太担心凭据泄漏。

跨平台是它的另一个卖点——Windows、macOS、Linux、iOS、Android 全覆盖，同一套 Vault 数据跨设备同步。用惯了桌面版，手机上打开移动端连台服务器跑条命令也能无缝衔接。对于需要在通勤途中处理线上告警的人来说，这一点比功能堆料更实际。

免费版（Starter）和付费版（Pro/Team）在功能上有明确分层，但最近几个版本的趋势是把批量执行等原本 Pro 专属的功能逐步下放给免费用户。安全投入方面，Termius 在 9.25.0 版本率先支持了后量子密钥交换算法，比大多数同类客户端早了一步。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://termius.bpfl.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：移动端在 Google Play 或 App Store 搜 Termius；桌面端通过上面的官方下载页获取各平台安装包

---

## 🔧 安装步骤

### 桌面端（Windows / macOS / Linux）

桌面端安装比较直接，各平台的注意事项不一样：

**Windows：**
1. 下载 .exe 安装包后双击运行
2. 安装向导会让你选择安装路径，默认路径即可（`C:\Program Files\Termius\`）
3. 安装过程中 Windows Defender 可能会弹出安全提示，选择「仍要运行」即可——这是因为签名机制被国内环境误报，不是真的有问题
4. 装完后桌面和开始菜单会出现 Termius 图标

**macOS：**
1. 下载 .dmg 文件，双击打开后把 Termius 拖进「应用程序」文件夹
2. 首次打开时 macOS 可能提示「无法验证开发者」，右键应用程序 → 打开 → 在弹窗里再次点「打开」就能绕过 Gatekeeper
3. 移动硬盘或 NAS 上复制过来的 dmg 要注意扩展属性隔离：如果双击无法打开终端，打开「终端」执行 `xattr -cr /Applications/Termius.app` 即可

**Linux：**
1. 官方提供 deb / rpm / AppImage 三种格式
2. Ubuntu/Debian 系用 `sudo dpkg -i termius_*.deb` 安装，可能需要补依赖 `sudo apt-get install -f`
3. CentOS/RHEL 系用 `sudo rpm -i termius-*.rpm`
4. AppImage 格式无需安装，`chmod +x Termius*.AppImage` 后直接双击运行

### 移动端（Android / iOS）

**Android：**
1. 从 Google Play 搜索安装，或者从官方下载 .apk 后手动安装
2. 手动安装时需要在系统设置里开启「允许未知来源应用」或「安装未知应用」的权限
3. 如果之前装过旧版本，安装前先卸载旧版再装新的，避免签名冲突

**iOS：**
1. 直接从 App Store 搜索 Termius 安装即可，无需特殊设置

### 安装后的核对

装完启动后先做三件事：

1. **确认版本号**：菜单栏（桌面端）或设置里能看到当前版本，建议保持在最新稳定版以上
2. **登录或跳过**：Termius 可以匿名使用本地功能，但要用到 Vault 同步、团队协作等云特性需要注册账号登录
3. **导入凭据**：如果之前用其他 SSH 客户端保存过主机信息，可以通过「导入」功能把 PuTTY 的 .ppk 或 OpenSSH 的 known_hosts 导入进来

> [!TIP]
> 安装后建议立即开一次「检查更新」。Termius 的功能迭代比较快，Snippets 批量执行、后量子加密这些实用特性都是近几个版本陆续加上的，错过更新等于错过新能力。

---

## 📖 使用入门

### 添加第一台主机

启动 Termius 后，左侧导航栏默认显示 Vault 列表。点击某个 Vault 下的「+」或者在顶部菜单选「New Host」就能添加一台远程主机。

需要填的核心字段：
- **地址**：IP 或域名
- **端口**：默认 22，如果服务器改了端口就填实际端口
- **用户名**：SSH 登录用的用户名
- **认证方式**：密码 or 密钥对

密钥对的情况，点击认证方式下拉选「Key」，然后导入私钥文件（支持 OpenSSH 格式的私钥，PuTTY 的 .ppk 可以通过 Termius 的工具转换）。Termius 也支持直接生成新密钥对并自动把公钥复制到服务器上——如果你的服务器还没配置密钥登录，这个功能能省掉手动 SSH 公钥分发的麻烦。

填完点「保存」，Vault 里就多了一个主机条目。双击条目，Termius 会建立 SSH 连接并打开终端窗口。第一次连接某台主机时会问你是否信任它的指纹，确认无误后选「接受」，后续再连就不会重复问了。

### Snippets：把常用命令存起来

Snippets 是 Termius 里最实用的功能之一——说白了就是命令片段的管理器。但它比简单的命令历史记录好用得多，因为 Snippets 有专门的组织方式和高级玩法。

**创建 Snippet 有三种方式：**

1. **Snippets 界面创建**（推荐，最干净）：左侧导航 → Snippets → 顶部「New Snippet」，填名称、描述、脚本内容、选存哪个 Vault 后保存。脚本内容可以是多行 Shell，Termius 会一行一行发到终端
2. **终端侧面板快速创建**：在已打开的终端里，右侧展开侧面板的 Snippets 标签，点「+ New」就能随手把当前正在跑的长命令存下来，适合排查问题时临时存档
3. **从命令历史里转 Snippet**：侧面板 → History，找到那条想要的历史命令，点击命令旁边的「存为 Snippet」图标即可

Snippet 太多了可以打**包管理**。典型的打包方式是按运维场景分：`debian-ops` 放 apt 升级、systemctl 操作、dpkg 查询等 Debian/Ubuntu 相关命令；`nginx-ops` 放 reload、restart、检查配置、tail 日志；`docker-ops` 放容器状态、日志查看、重建等。包本身跟着 Vault 同步，团队共享 Snippets 时包结构也一起带过去。

> [!NOTE]
> 某些场景希望每次连接到某台主机后自动执行一条命令——比如连到 MySQL 主库后自动跑 `mysql -e "SHOW SLAVE STATUS\G"` 确认从库同步状态。Termius 支持「Startup Snippet」配置：打开主机编辑面板，找到 Startup Snippet 选项，选一条 Snippet 绑定即可。下次双击这台主机连接，SSH 成功后会自动执行这条脚本。

**Snippets 的导入导出**使用 JSON 格式，便于团队间共享或跨设备迁移。右键包或 Vault → Export 导出；Snippets 界面 → Import 导入。格式很简单，就是 Snippet 的名称、描述、内容数组。

### 端口转发：三层排查法

Termius 的 Port Forwarding 功能把本地端口映射到远程服务器可达的目标服务上。典型用例：本地 MySQL 客户端访问服务器内网的 MySQL、本地浏览器访问仅内网开放的 Web 服务。

端口转发失败时，按「客户端 → SSH 服务器 → 目标服务」三层顺序排查能最快定位问题：

**第一层——Termius 客户端：**
- 主机连接正常吗？SSH 终端能跑基本命令才行，Jump Host 链路任何一层断了转发都不生效
- Port Forwarding 页面规则开关是高亮状态（启用）吗？
- 本地端口被占用了吗？Windows 用 `netstat -ano | findstr :端口号` 查，macOS/Linux 用 `lsof -i :端口号`

**第二层——SSH 服务器：**
这是最常见的坑。Termius 只负责建 SSH 隧道，服务器能不能连到目标是服务器自己的事。在服务器的 SSH 终端里跑 `nc -zv 目标IP 目标端口`，如果超时就是服务器到目标不通，排查服务器和目标之间的防火墙或目标主机的可用性。

远程转发（-R）需要服务器端允许外部连接：检查 `/etc/ssh/sshd_config` 里的 `GatewayPorts` 是否设为 `yes`，否则外部用户无法通过服务器端口访问到你本地的服务。极少数安全严苛的服务器会在 sshd_config 里把 `AllowTcpForwarding` 设为 `no`，这种情况只能找管理员改。

**第三层——目标服务：**
- 目标端口在监听吗？服务器上跑 `ss -tlnp | grep 端口号` 确认
- 很多数据库默认只绑定 `127.0.0.1`，转发过来的连接会被拒。改成 `0.0.0.0` 或服务器内网 IP 后重启服务
- 数据库的用户权限（比如 MySQL 的 `user@host`）也要允许来自服务器 IP 的连接，`root@localhost` 不够

> [!WARNING]
> Windows Defender 或 macOS 防火墙偶尔会拦截 Termius 绑定本地端口。如果本地端口确认空闲但转发规则就是启用不了，临时关防火墙试试——能用了就给 Termius 加白名单。

### AI 生成与批量执行 Snippets

AI 生成 Snippets 是 Termius 相对新的功能。用法很直接：在 Snippets 界面或终端侧面板点「AI Generate」，用自然语言描述想做什么——比如「Ubuntu 服务器 apt 更新所有包并清理缓存」或「一键检查服务器 CPU/内存/磁盘」，AI 返回候选脚本，确认无误后保存为 Snippet。

AI 生成的内容需要你自己校验：可能有语法错误，也可能不适合你的系统环境。生成前告诉 AI 你的系统类型（Ubuntu / CentOS / Debian）能提高准确率。隐私方面，AI 只接收你的**描述文字**，不会把服务器上的命令历史或数据发出去。

**批量执行 Snippets**是另一个高频场景。在 Snippets 界面选中一条脚本，点「Run on multiple hosts」，从主机列表里勾选目标服务器（可以跨 Group / Vault），Termius 就同时在所有选中的主机上执行。完成后会打开汇总面板，每台服务器一个输出区域，执行失败的标红，方便对比排查。

从 9.25.0 版本起，跨主机批量执行 Snippets 对免费用户也开放了——之前这是 Pro 专属功能，Starter 用户现在也能用。

批量执行的安全建议不能省：
1. 先在一台测试机上跑通，确认脚本正确再批量
2. 批量前再核对一遍目标服务器清单，别选到不该操作的机器
3. 生产环境分批操作——先 2 台 → 观察 5 分钟 → 再操作剩下的
4. 破坏性命令（重启、删数据）在脚本开头加 `read -p "Confirm? " answer` 让每台服务器弹窗等你确认

### 后量子加密：默默加上的安全锁

9.25.0 版本还有一个低调但重要的变化——Termius 新增了密钥交换算法 `mlkem768x25519-sha256`，应对量子计算的威胁。

这个算法是混合方案：经典椭圆曲线 X25519 加后量子格密码 ML-KEM-768 同时跑，得到的密钥混合在一起。X25519 提供今天的安全，ML-KEM-768 提供未来的安全。这样建立的 SSH 会话，即使 10 年后量子计算机成熟了也解不开——直接防御了「现在收集、以后解密」的长期情报攻击。

对普通用户来说这是完全透明的：连接支持 PQC 的服务器时自动使用新算法，服务器不支持就自动降级回普通 X25519，不需要任何配置。速度上会多几毫秒计算开销，但对 SSH 这种长连接完全无感。

有意思的是，Termius 在这件事上比 PuTTY、SecureCRT、Xshell 等同类客户端行动得快得多，说明安全投入在它的产品优先级里排得靠前。

---

## 🔗 相关资源

- [Termius官网](https://termius.bpfl.quest)
- [Termius文档](https://termius.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/Termius)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/Termius)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/Termius)

## 📚 相关文章

- [为什么Termius不做文件夹？Vaults架构背后的设计取舍](https://laoguosofts.github.io/posts/termius.html)

- [Termius 凭什么把 SSH 客户端做成付费产品？拆解它的 Vaults 架构](https://alipymanbu.github.io/posts/termius.html)
