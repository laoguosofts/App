## ✨ GoodSync 是什么

GoodSync 是一款老牌的专业级文件同步与备份工具，主打"一次配置、长期省心"的自动化数据守护。它的核心定位不是简单的文件复制，而是在两台或多台设备之间建立可靠的同步关系——无论这些设备是在同一局域网、跨互联网、还是分布在云存储上，GoodSync 都能保持文件的一致性和可追溯性。

从早期的本地目录对同步，到如今支持 GoodSync Connect 全球点对点网络、S3 兼容对象存储、各大主流云盘，GoodSync 已经演化成一套覆盖个人用户、小团队、企业级服务器的完整数据同步方案。它在同步领域的几个标志性特性包括：**双向同步**（双方变更都能合并）、**块级传输**（大文件只传变动片段）、**版本历史**（误删误改可回滚）、**后台服务模式**（无人值守 7x24 运行）。

适合 GoodSync 的典型场景很广：个人用户的电脑 ↔ 移动硬盘 ↔ 云盘三方备份、摄影团队的素材集中归档、小公司的服务器 ↔ 各工作电脑的自动同步、NAS 玩家的多节点数据镜像。如果你用过免费同步工具但被"单向覆盖""冲突丢失""大文件卡死"等问题折磨过，GoodSync 的专业性会立竿见影。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://goodsync.ggcd.store/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：桌面平台通过官网下载安装包；NAS 版在对应设备的应用中心（Synology Package Center / QNAP App Center / WD My Cloud App Center 等）搜索 GoodSync 安装

---

## 🔧 多平台安装指南

GoodSync 支持的平台跨度很大，从 Windows/macOS/Linux 桌面到各种品牌的 NAS，甚至 Docker 容器。以下按常见平台分别说明。

### Windows 桌面版安装

1. 从下载页获取安装包（通常是 `.exe` 格式，体积约 50-80MB）
2. 双击运行安装程序，按向导选择安装路径（默认 `C:\Program Files\GoodSync`）
3. 安装完成后首次启动，会提示创建或登录 GoodSync Connect 账户（用于跨设备发现和通信）
4. 进入主界面即可开始创建同步作业

> [!NOTE]
> 杀毒软件可能对 GoodSync 的实时同步监控行为误报。如果安装时被拦截，将 GoodSync 加入杀毒软件的信任列表即可。GoodSync 是正规商业软件，不存在恶意行为。

### Linux 桌面与服务器安装

Linux 版以 tar.gz 归档分发，安装方式灵活：

```bash
# x86_64 架构
curl -O https://www.goodsync.com/download/goodsync-v12-linux-x86_64.tar.gz
tar -xzf goodsync-v12-linux-x86_64.tar.gz
cd goodsync-v12-linux-x86_64
sudo ./install-gssh

# ARM aarch64 架构（如树莓派、部分 NAS）
curl -O https://www.goodsync.com/download/goodsync-v12-linux-aarch64.tar.gz
tar -xzf goodsync-v12-linux-aarch64.tar.gz
cd goodsync-v12-linux-aarch64
sudo ./install-gssh
```

安装脚本会把二进制放到 `/usr/local/bin/`，并自动处理依赖。安装完成后可以直接运行 `goodsync` 启动图形界面，也可以注册为后台服务（见后文"后台服务模式"）。

### NAS 设备安装

GoodSync 对 NAS 生态的支持非常深入，几乎所有主流品牌都有原生包。

**Western Digital My Cloud OS5**：进入应用中心，搜索 GoodSync 直接安装，安装完成后 GoodSync Server 自动启动。注意 My Cloud 系列 CPU 通常较弱，块级传输计算可能成为瓶颈，建议在客户端降低并行线程数。

**ASUSTOR ADM**：App Central 中可直接找到 GoodSync 包。安装后需要在 ADM 防火墙规则中手动放行 GoodSync 端口。

**TerraMaster 铁威马**：新型号的 TOS 4.x 应用商店有 GoodSync；所有型号都可以通过 Docker 部署，搜索 GoodSync 镜像创建容器，配置端口 11000 和卷挂载即可。

**通用 DIY NAS（OpenMediaVault / Unraid / TrueNAS）**：这三个热门 DIY 系统都可以通过 Docker 容器部署 GoodSync。以 Unraid 为例，进入 Docker → 容器模板社区 → 搜索 GoodSync，配置端口 11000 和存储目录挂载后启动容器。

> [!WARNING]
> NAS 上的 Linux 权限管理需要格外注意。GoodSync 运行账户必须对同步目录有读写权限，建议创建专用的 `goodsync` 用户组，避免用 root 直接运行。DIY NAS 的防火墙默认策略较严格，需要手动放行 GoodSync 端口（11000/tcp Web 界面、443/tcp Connect HTTPS）。

### 安装后的版本核对

安装完成后，建议在 GoodSync 主界面 → Help → About 中核对版本号是否与下载页标注一致。同时可以检查已安装组件是否完整——桌面版应包含 GUI 和同步引擎，Server 版额外包含后台服务注册能力。

---

## 📖 使用入门

GoodSync 的能力边界很广，从基础的"选两个目录开始同步"到专业级的"7x24 无人值守备份集群"都能覆盖。这里按照实际使用频率从浅到深展开。

### 创建你的第一个同步作业

GoodSync 的操作核心是 **Job（作业）**。一个 Job 定义了"谁和谁同步""怎么同步"。

1. 主界面 → 点击 **New Job**
2. 输入作业名称（建议能看出同步关系，如 `Laptop-HDD-Backup`）
3. 选择 **同步模式**：
   - **Synchronize（双向同步）**：两端变更都保留，适合协作场景
   - **Backup（单向备份）**：源端 → 目标端，覆盖旧文件，适合备份场景
   - **Copy（单向复制）**：类似备份但不删除目标端多余文件
4. 选择 **左端点（源）** 和 **右端点（目标）**：本地文件夹、GoodSync Connect 远程设备、云存储等
5. 点击 **Analyze（分析）**——GoodSync 会对比两端差异，显示哪些文件需要传输
6. 确认无误后点击 **Sync（执行同步）**

> [!TIP]
> 第一次同步前一定要先 Analyze，不要直接 Sync。Analyze 会列出所有待操作的文件，包括删除操作，让你有机会发现误配置。如果 Analyze 结果中有大量红色的"文件将被删除"，多半是两端路径搞反了。

### 版本历史——误删误改的后悔药

普通备份工具每次同步会用新文件覆盖旧版本，这意味着如果某个文件被意外损坏后还没发现，下一次同步就会把损坏版备份过去。GoodSync 的版本历史功能正是为了解决这个痛点：

- 在同步文件的同时，自动把被删除、被覆盖的旧文件保存到版本历史目录
- 版本历史默认保存在**目标端的 `_gsdata_/_history/` 隐藏子目录**中，GoodSync 自动排除这个目录不会同步它
- 可按三种策略配置版本保留：**按版本数**（每个文件最多保留 N 个版本，建议 5-10 个）、**按时间窗口**（只保留最近 N 天，建议 30-90 天）、**按空间上限**（总大小不超过 X GB）

恢复历史版本有两种方式：通过 GoodSync Explorer 手动定位 `_gsdata_/_history/` 目录找到需要的版本复制回原位置；或者创建一个专门的恢复作业，批量还原指定文件的某个历史版本。当目标端是云存储时，GoodSync 自身的版本历史仍然生效，而且可以和云服务商（如 OneDrive、Google Drive）自带的版本历史功能叠加双保险。

### 云存储同步的正确姿势

把文件同步到云存储是 GoodSync 的高频场景，但云存储的 API 限制比本地文件系统多得多。

**OAuth Token 过期**：GoodSync 用 OAuth 授权访问云存储（Google Drive、Dropbox、OneDrive 等）。Access Token 有效期短（通常 1 小时），GoodSync 会自动用 Refresh Token 刷新。但 Refresh Token 也可能过期或被云服务商主动撤销（比如修改了账户密码或触发安全事件），此时 GoodSync 日志会出现"Need to re-authorize"。解决方法是：主界面 → Job → 右键端点 → Account → Re-Authorize，浏览器会弹出云服务商的授权页面，重新允许即可。各平台 Refresh Token 稳定性不同——Google 长期有效最稳定，Dropbox 和 OneDrive 与账户安全策略相关可能定期失效。

**API 限流**：同步海量文件时容易触发云服务商的 API 限流（HTTP 429）。GoodSync 内置了指数退避重试机制，收到 429 后等待 N 秒再重试，每次失败等待时间加倍。用户侧可以做几件事缓解：降低并行线程数（云存储建议不超过 4）、把实时同步改为每 15-30 分钟的计划任务、把包含数万文件的大目录拆分成多个小目录分别同步。各服务商限流阈值差异较大：

| 云存储 | 默认请求率 | 备注 |
| --- | --- | --- |
| AWS S3 | 每桶 3500 PUT + 5500 GET /秒 | 可申请提高到 10000+ |
| Google Drive | 1000 万请求/天 | 通常够用 |
| Dropbox | 500 请求/秒/应用 | 商业 API 应用可能紧张 |
| OneDrive | 4-8 请求/秒/应用 | 较严格，需要保守配置 |

**对象存储重命名的临时残留**：S3/Azure/B2 等对象存储不支持原地重命名，GoodSync 执行"复制到新键名 + 删除旧键名"。如果在复制成功、删除旧键之前同步中断了，会留下新旧两个对象。GoodSync 有专门的孤儿对象清理机制，下次 Analyze 时检测到同一份内容的新旧两个对象会自动删除旧对象。

**时间戳兼容性问题**：偶尔会遇到"文件内容没变但每次同步都要重新上传"的情况，检查 Analyze 结果显示时间戳不一致。这是因为某些云存储在上传时自动把时间戳刷新为上传时间。解决方法：在作业中确认"时间戳翻译"已开启（默认已开启），把 Analysis Mode 切换为"MD5 + Quick Hybrid"，全量重新 Analyze 一次让 GoodSync 重建内部状态。

### NAS 作为 GoodSync Connect 节点

把 NAS 接入 GoodSync Connect 网络有几个独特的价值：NAS 全天候在线不像电脑需要关机，硬盘每 TB 成本远低于云存储，而且 NAS 同时提供 SMB、WebDAV、Connect 三种接入方式。两台电脑可以通过 NAS 间接同步（电脑 A → NAS → 电脑 B），相当于有了一个低成本的同步中继站。

### 后台服务模式——无人值守运行

普通 GoodSync 需要在桌面环境运行，用户登录打开程序才执行同步。但服务器备份、NAS 被动接收同步、团队共享目录自动同步这些场景需要 7x24 小时无人值守运行，GoodSync 的 Server 版本提供了后台服务模式。

**Windows 服务注册**：前置条件是 Server 版许可证（Workstation 版不支持）和管理员权限。步骤：主界面 → Tools → Options → Server 标签 → Install Server → 配置运行账户（建议用专用本地账户，不要用当前登录账户）→ 启动类型设为 Automatic。注册完成后可以在 `services.msc` 中看到 GoodSync Server 服务，用 PowerShell 管理状态：

```powershell
Get-Service -Name "GoodSync Server"   # 查看状态
Start-Service -Name "GoodSync Server"  # 启动
Stop-Service -Name "GoodSync Server"   # 停止
```

**Linux systemd 配置**：通过创建 systemd 单元文件注册为系统服务：

```bash
sudo tee /etc/systemd/system/goodsync.service << 'EOF'
[Unit]
Description=GoodSync Sync Service
After=network-online.target
Wants=network-online.target

[Service]
Type=forking
ExecStart=/usr/local/bin/goodsync --daemon
ExecStop=/usr/local/bin/goodsync --shutdown
User=goodsync
Group=goodsync
Restart=on-failure
RestartSec=10
MemoryMax=512M

[Install]
WantedBy=multi-user.target
EOF

# 重载配置并设置开机自启
sudo systemctl daemon-reload
sudo systemctl enable goodsync
sudo systemctl start goodsync
```

后台服务模式有几项安全注意事项：必须用专用账户运行（不给管理员权限，只授予同步目录的读写权限），GoodSync Server 监听的端口（默认 11000）需要在防火墙中限制只允许可信 IP 连接，GoodSync Connect 中确保只有你信任的账户能访问这台设备。升级 Server 版时需要先停止服务、运行安装程序、再重新启动，作业配置和 Connect 账户登记信息都会被保留。

### 进一步的进阶特性

除了上面这些高频功能，GoodSync 还有一些值得了解的进阶选项：

- **过滤器（Filters）**：在作业中设置文件类型白名单/黑名单、路径排除规则（如临时文件 `.tmp`、缓存目录 `cache/`），避免无关文件占用同步带宽
- **自动计划（Auto Scheduling）**：在 Job → Auto 中设置实时同步、按固定间隔（每 N 分钟）、按定时任务（每天几点）三种触发方式
- **端到端加密**：GoodSync 传输层默认加密，配合 GoodSync Storage 云盘可实现完整的端到端加密方案
- **日志与报告**：每个作业都有详细的同步日志，可以导出为 CSV 或通过邮件发送定期报告，适合团队场景的审计需求

---

## 🔗 相关资源

- [GoodSync官网](https://goodsync.ggcd.store)
- [GoodSync文档](https://goodsync.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/GoodSync)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/GoodSync)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/GoodSync)

---

## 📚 相关文章

- [GoodSync 藏着这 5 个没人讲透的功能，用完才知道什么叫专业级同步](https://laoguosofts.github.io/posts/goodsync.html)

- [用了三年GoodSync才摸到这五个暗门，藏得真深](https://alipymanbu.github.io/posts/goodsync.html)
