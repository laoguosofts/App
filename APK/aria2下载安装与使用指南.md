## ✨ aria2 是什么

aria2 是一个用 C++ 写的轻量级命令行下载工具，主打**多协议支持**和**多线程并行**。HTTP、FTP、BT、磁力、Metalink 通吃，一个二进制文件就能搞定大多数下载场景。它不是图形界面软件，所有操作都在终端里敲命令完成，适合喜欢折腾、追求效率的用户。

aria2 的核心卖点在于**多源连接**——同一个文件可以从多个镜像同时拉取，把聚合带宽用到极致。BT 协议上它支持 DHT、PEX、加密、选择性下载等全栈特性，磁力链也能直接解析。另外它体积小（Windows 版单 exe 不到 10MB）、绿色解压即用，写进脚本里做批量下载也很顺手。

适合谁用：开发者、经常要下大文件的用户、有脚本自动化需求的人、或者受够了下载器广告和限速的所有人。不适合只想"双击就能下"的纯小白——命令行门槛是真实存在的，但跨过去之后就是另一番天地。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://aria2.rggd.pw/download.html) —— 下载页整理了当前可用的版本和安装说明
> - Windows：可用 winget（`winget install aria2.aria2`）、scoop（`scoop install aria2`）或手动下载 zip 解压
> - Linux：多数发行版自带仓库可直接 `apt install aria2` / `yum install aria2`
> - macOS：`brew install aria2c`

---

## 🔧 Windows 安装全攻略

Windows 上装 aria2 有四条路，推荐程度从上到下递减。

### 方式一：winget（最省心）

Windows 10/11 自带包管理器，管理员 PowerShell 里一行搞定：

```bash
winget install aria2.aria2
```

装完**关掉旧终端重新开一个**（旧窗口不会读到新的 PATH），输入 `aria2c --version` 能打印版本号即成功。此方式不需要管理员权限。

### 方式二：scoop

Scoop 是用户级包管理器，装在当前用户目录下，自动配好 PATH：

```bash
scoop install aria2
```

### 方式三：Chocolatey

需要管理员权限的 PowerShell：

```bash
choco install aria2
```

### 方式四：手动下载 zip

完全不依赖包管理器：

1. 打开官方 GitHub 发布页（Releases），找到最新版本；
2. 下载对应位数的 zip 包（32 位或 64 位 build1）；
3. 解压到固定目录，比如 `C:\aria2`，里面就是 aria2c.exe；
4. **配置 PATH 环境变量**，否则只能在解压目录里运行。

### 手动版怎么配 PATH

配 PATH 是手动解压用户最容易踩坑的步骤，写清楚：

1. 把 zip 解压到固定目录，例如 `C:\aria2`，确认里面有 aria2c.exe；
2. 开始菜单搜索"环境变量"，打开"编辑系统环境变量 → 环境变量"；
3. 在用户变量的 `Path` 里新增一条 `C:\aria2`（**注意是目录，不是 exe 文件路径**）；
4. 一路确定保存，**关掉旧终端重新开一个**；
5. 验证：`aria2c --version` 或 PowerShell 里 `Get-Command aria2c`。

> [!WARNING] 常见坑
> - 配完 PATH 没重开终端，怎么敲都是"找不到命令"——重开即可；
> - 目录写错层级（把 exe 文件路径而不是它所在目录写进了 Path）；
> - Windows 商店版终端继承的是启动时的环境，同样需要重开。

### 安装验证

不管哪种方式装的，跑这一条就够：

```bash
aria2c --version
```

正常会打印版本号和一串编译信息，其中 `HashAlgorithms` 一行列出了当前版本支持的哈希算法（校验文件时会用到）。

---

## 📖 从第一条命令到进阶玩法

### 基础下载命令

最简单的用法就是把直链跟在命令后面：

```bash
aria2c "https://example.com/files/big-file.zip"
```

aria2 默认开 5 个连接同时拉一个文件（`--max-connection-per-server=5`），比单线程下载快得多。

### 指定下载目录和文件名

两个参数控制文件落到哪里、叫什么：

```bash
aria2c -d "D:\Downloads" -o "mirror.iso" "下载直链"
```

- `-d`（--dir）：文件存到哪个目录，目录不存在时需要预先建好；
- `-o`（--out）：保存成什么名字，相对 `-d` 目录解析。

只改名字就写 `-o 新文件名`，只改位置就写 `-d 目标目录`。

> [!TIP] 多镜像统一下载
> `-o` 在多源场景特别有用——不同镜像的文件名五花八门，指定一个统一输出名就能并行写进同一个文件：
> ```bash
> aria2c -o 文件.zip "镜像A直链" "镜像B直链"
> ```

### 路径变量

配置文件与命令行里的这些参数支持 `${HOME}` 展开为用户主目录：`dir`、`out`、`input-file`、`save-session`、`load-cookies`、`log` 等。写配置时用它比写死绝对路径好迁移。

### 已有同名文件怎么办

aria2 默认开启 `--auto-file-renaming`，遇到同名会自动改名保存。确认要覆盖则用 `--allow-overwrite=true`。两者的取舍取决于你是想保留旧文件还是强制刷新。

### BT 与磁力下载

BT 任务的种子内文件名和目录结构由种子文件决定，**不能用 `-o` 指定内容文件名**。要重排种子内文件的落盘位置，用 `--index-out`（按文件编号指定新路径，编号先用 `-S` 查看）。

磁力链直接喂给 aria2c 就行：

```bash
aria2c "magnet:?xt=urn:btih:..."
```

磁力链第一次启动需要先从 DHT 或 tracker 拉取元数据，这一步可能要等 1～2 分钟，进度行显示 metadata 相关而没进入下载是正常的，别急着 Ctrl+C。

### BT 没速度怎么办：peer 数为 0 的排查清单

BT 任务 `CN:0`、`SD:0` 连不上人时，按清单逐条过：

**1. 确认 DHT 开着**

`--enable-dht` 默认开启，但显式确认一遍不亏；IPv6 网络再开 `--enable-dht6`。

**2. 放行端口**

默认 TCP 6881-6999（peer 数据）+ UDP 6881-6999（DHT 与 UDP tracker）：

- 本机防火墙放行；
- 路由器把端口转发到下载机——aria2 **不会**自动配置转发，官方手册明确要手动做；
- 端口不通是"明明有 peer 却连不上"的最常见原因。

**3. 追加活跃 tracker**

种子自带 tracker 可能半死不活，补一批公开列表里的 announce 地址：

```bash
aria2c --bt-tracker="tracker1/announce,tracker2/announce" "文件名.torrent"
```

**4. 私有种子的限制**

种子带私有标记时，DHT、PEX、局域网发现**都不参与**，只能靠种子内 tracker。这是协议规则不是故障，此时第 3 步是唯一抓手。

**5. 局域网场景**

同一内网有其他 BT 客户端时开 `--bt-enable-lpd=true` 直连互传。

**6. 冷门资源**

peer 本来就少：加 tracker、放行端口、提高 `--bt-request-peer-speed-limit`（默认 50K）让它更积极找 peer，然后交给时间。

> [!NOTE] 还是不行怎么办
> 换个明显热门的种子测试，能跑说明环境没问题，是原资源没人。也可以检查网络运营商对 BT 的限速或封锁。

### 批量下载

`-i` 参数可以读一个清单文件批量下载，每行一个直链，也可以在每行后面追加参数覆盖全局设置：

```bash
aria2c -i urls.txt
```

批量模式下，命令行的 `-o` 不生效，把 `out=新文件名` 写进清单里对应行的尾部。

### 选择性下载 BT 种子内的文件

用 `-S` 先列出种子里所有文件的编号，再用 `--select-file` 指定只下哪些：

```bash
aria2c -S "文件.torrent"          # 列出文件和编号
aria2c --select-file=1,3-5 "文件.torrent"  # 只下第 1、3 到 5 号文件
```

---

## 🔗 相关资源

- [aria2官网](https://aria2.rggd.pw)
- [aria2文档](https://aria2-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/aria2)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/aria2)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/aria2)

---

## 📚 相关文章

- [命令行下载工具的实力派——aria2 上手体验](https://laoguosofts.github.io/posts/aria2-docs.html)
- [aria2 上手测评：命令行下载工具凭什么让我把迅雷卸了](https://alipymanbu.github.io/posts/aria2-docs.html)
