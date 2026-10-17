## ✨ Bochs 是什么——一个能完整模拟 x86 PC 的开源虚拟机

Bochs 是一个开源的 x86 模拟器，它能在你当前的操作系统里，完整模拟出一台"从零开始的"PC——包括 CPU、主板、PCI 总线、VGA 显卡、磁盘控制器、网卡、声卡这些硬件，以及它们各自的 BIOS 和固件。和 VirtualBox、VMware 这类"虚拟机"不同，Bochs 不是让宿主机直接执行客户机的指令（那叫硬件加速），而是**纯软件一条指令一条指令地解码执行**。

这个设计带来的最大好处是**完全可控**：你可以在 CPU 指令级下断点、观察 PCI 配置空间枚举过程、逐跳调试 Bootloader——这正是操作系统开发者、逆向工程师、驱动写作者最爱它的原因。代价是慢，但做研究和调试时，慢根本不是问题。

除了硬核场景，Bochs 也能跑日常用途——装个 Windows 98、老版 Linux、DOS，把玩经典软件或怀旧游戏。Bochs 模拟的经典硬件（i440fx + PIIX3 芯片组 + Cirrus VGA）和当年的真机高度一致，很多只认特定硬件的老驱动反而在 Bochs 上跑得比现代模拟器好。

> [!NOTE] Bochs 不是 QEMU、不是 VirtualBox
> QEMU 有硬件加速的全虚拟化模式（KVM/HAXM），VirtualBox 也用 VTx/AMD-V——它们追求的是**和宿主机共享硬件、跑得快**。Bochs 是**纯软件模拟**，每条 CPU 指令都自己解码执行，精度最高、调试能力最强，但速度可能只有硬件加速方案的几分之一到几十分之一。如果你要跑重型系统做日常用，Bochs 不是首选；如果你要研究 PC 是怎么启动的、写 Bootloader、开发操作系统，Bochs 是目前最好的"虚拟硬件"平台。

Bochs 支持 Windows、Linux、macOS 三大平台，安装包里直接打包好 BIOS 镜像、VGA BIOS、示例配置和磁盘镜像工具，解压就能用。Windows 版本还额外带一个 `bochsdbg` 调试版，每次启动自动停在调试器提示符，对调试 Bootloader 和内核来说省了自己编译加 `--enable-debugger` 的事。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://bochs.fwen.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 项目主页：Bochs 官方 SourceForge 项目文件区（搜索 "Bochs x86 emulator"）下载 Windows EXE 安装包，文件名通常为 `bochs-X.X.X.exe`（当前最新 3.1）

---

## 🔧 安装步骤（Windows 平台）

### 1. 下载安装包

从官方渠道获取 `bochs-X.X.X.exe`，安装包体积在 10~20MB 左右（带示例配置、BIOS、VGA BIOS 和文档）。安装包里已经**自带了运行时需要的一切**——不需要额外下载 BIOS 镜像、不需要安装 VGA ROM，解压就能用。

### 2. 运行安装向导

双击 EXE 进入安装向导，一路默认即可。关键的一步是**选择安装类型**：

| 类型 | 包含内容 | 建议 |
|---|---|---|
| Minimal | bochs.exe + bochsdbg.exe + BIOS 镜像 | 只想跑起来、不需要示例和文档 |
| **Typical（推荐）** | Minimal + 示例 bochsrc + bximage + 文档 | 绝大多数场景 |
| Full | Typical + C++ 源码 | 需要改源码才选，体积大 |

安装路径默认在 `C:\Program Files\Bochs-X.X.X\`，可以改到非系统盘。

### 3. 安装后目录一览

安装完成后，安装目录下会有这些关键文件：

```
Bochs-3.1/
├── bochs.exe                       # 无调试器版本（快速启动）
├── bochsdbg.exe                    # 带调试器版本（OS 开发必用）
├── bximage.exe                     # 磁盘镜像创建/转换工具
├── BIOS-bochs-latest               # 系统 BIOS 镜像（128KB）
├── VGABIOS-lgpl-latest.bin         # 默认 Bochs VGA BIOS
├── VGABIOS-lgpl-latest-cirrus.bin  # Cirrus GD5446 VGA BIOS
├── bochsrc.bxrc                    # 示例配置文件（带注释）
└── docs/                           # 官方文档
```

> [!TIP] bochsrc 是什么
> `bochsrc`（扩展名通常是 `.bxrc` 或 `.bochsrc`）是 Bochs 的**配置文件**——一切硬件模拟都靠它声明：CPU 型号、内存大小、装什么 BIOS、显卡用什么 VGA、磁盘镜像在哪里、PCI 槽上插什么设备……没有 bochsrc，Bochs 不知道要模拟什么样的硬件组合。示例文件 `bochsrc.bxrc` 里有详细注释，是新手最好的参考模板。

### 4. 验证安装

打开命令提示符，切到安装目录，分别试两个版本：

- **无调试器版本**：`bochs.exe -q`（`-q` 跳过启动对话框），应该弹出 Bochs 窗口，会报找不到配置文件——这是正常的，说明程序能跑起来。按 Ctrl+C 退出。
- **带调试器版本**：`bochsdbg.exe -q`，会进入调试器提示符 `bochs:1>`，输入 `c`（continue）让它继续跑，或 `q` 退出。能看到调试提示符就说明安装完整，调试器编译进去了。

### 5. 常见安装问题

| 问题 | 原因 | 解决 |
|---|---|---|
| 杀毒软件误报 | Bochs 底层模拟行为和某些恶意软件相似 | 官方下载的是误报，加白名单 |
| 运行提示缺少 MSVCP140.dll | C++ 运行库缺失 | 安装 Visual C++ Redistributable 2015-2022 |
| bochsrc 加载时路径错 | Windows 路径用了反斜杠 `\` | bochsrc 里统一用正斜杠 `/` |

---

## 📖 配置与使用入门

安装完 Bochs，接下来要做的事可以分成两条路线：**日常跑经典系统**（用预做好的 bochsrc + 现成磁盘镜像）和 **OS 开发/调试**（自己写 bochsrc、自己造磁盘镜像、用 bochsdbg 下断点）。两条路线的核心都是**bochsrc 配置文件**——理解了它，就掌握了 Bochs 的使用。

### bochsrc 的基本语法

bochsrc 是一行一条声明的纯文本文件，格式固定：

```
关键字: value1=xxx, value2=yyy
```

注意冒号**后面必须有空格**，语法严格到多打一个空格或少打一个冒号，Bochs 可能静默跳过这行。

### 最小可用配置（跑通 BIOS POST）

如果你只是想验证 Bochs 能启动，先写一个**极简 bochsrc**——没有磁盘、没有网卡、没有 PCI，就让 BIOS 上电自检然后停住：

```
romimage: file=BIOS-bochs-latest
vgaromimage: file=VGABIOS-lgpl-latest.bin
cpu: model=pentium, count=1
memory: guest=16, host=16
boot: none
mouse: enabled=0
display_library: wx
panic: action=ask
```

保存为 `minimal.bxrc`，运行 `bochs.exe -f minimal.bxrc`，应该能看到 Bochs 窗口里打印出 BIOS POST 界面：

```
Bochs Bios v1.0.9
00000MB RAM
Booting from Floppy...
Boot failed: could not read the boot disk
```

如果**一片黑屏**——这是新手最常遇到的问题，按以下清单逐项排查：

| 检查项 | 最常见原因 | 修复 |
|---|---|---|
| VGA BIOS 文件路径 | 路径写错、文件不存在、反斜杠被当转义 | 用正斜杠、确认文件在安装目录 |
| VGA extension 和 vgaromimage 不匹配 | bochsrc 声明 `extension=cirrus` 但 vgaromimage 指向默认版 | 配好对应组合（见下方表格） |
| BIOS 加载地址 | 老式 64KB BIOS 和新式 128KB BIOS 地址不同 | 不写 address，让 Bochs 自动推断 |
| display_library 设成了 term/nogui | 纯终端模式本来就不弹窗口 | 改成 `display_library: wx` |

> [!WARNING] VGA extension 必须和 vgaromimage 配对
> | vga extension | 对应 vgaromimage |
> |---|---|
> | `std`（默认） | `VGABIOS-lgpl-latest.bin` |
> | `cirrus` | `VGABIOS-lgpl-latest-cirrus.bin` |
> 混用了 Bochs 启动后一定黑屏，且不会报错。

### 核心硬件配置：PCI 芯片组与槽位

跑真实系统（比如 Windows 98/XP/Linux）时，bochsrc 里需要启用 PCI 总线——它是连接 CPU、显卡、网卡、声卡、磁盘控制器的"主干道"。最经典的组合是 **i440fx 芯片组（北桥）+ PIIX3 南桥**，覆盖 Pentium II/III/4 时代几乎所有硬件场景，也是 Bochs 的默认推荐：

```
pci: enabled=1, chipset=i440fx, slot1=cirrus, slot2=usb_uhci, slot3=e1000
```

| slot 位置 | 常用设备 | 说明 |
|---|---|---|
| slot1（VGA 槽位） | `cirrus`（Cirrus GD5446） | **最优先**，操作系统最先初始化的显卡 |
| slot2 | `usb_uhci` / `usb_ehci` / `usb_xhci` | USB 控制器 |
| slot3 | `e1000` / `ne2k` | 网卡（Intel 82540EM 千兆 / NE2000） |
| slot4 | `es1370` | 声卡（Ensoniq ES1370） |
| slot5-6 | 留空 | — |

> [!NOTE] slot0 去哪了
> slot0 被 PCI Host Bridge（北桥本身）自动占用，不需要显式声明。启用 PCI 后它就存在。

### 创建磁盘镜像：bximage 工具

有了硬件配置，下一步需要给模拟 PC 安"硬盘"——就是一个镜像文件。Bochs 安装包里自带的 `bximage.exe` 是管理磁盘镜像的一站式工具，能创建、转换、调整大小、合并 undoable 日志。

**创建 flat 格式磁盘**（OS 开发和经典系统最常用的格式）：

```
bximage -q -create -mode flat -size 60M -filename hd60M.img
```

运行后它会输出一行关键信息——**C/H/S 三参数必须抄到 bochsrc 里**：

```
hd60M.img:
  CHS=121/16/63
```

bochsrc 里对应写：

```
ata0-master: type=disk, path="hd60M.img", mode=flat, cylinders=121, heads=16, spt=63
```

三参数写错了，BIOS 报告的磁盘容量会和真实大小不符，某些老系统会因此分区错误。

> [!TIP] 为什么选 flat
> flat 格式就是一块纯线性数据，没有元数据头，文件大小 = 磁盘大小。相比之下 sparse 格式有可写日志层（undoable 模式，能撤销修改），适合临时实验，但 OS 开发里直接写 flat 最省事。

bximage 的命令行模式参数速查：

| 操作 | 命令示例 |
|---|---|
| 创建 flat 硬盘 | `bximage -q -create -mode flat -size 60M -filename hd.img` |
| 创建 sparse 硬盘 | `bximage -q -create -mode sparse -size 1G -filename sys.img` |
| 创建 1.44MB 软盘 | `bximage -q -create -fd -size 1_44M -filename boot.img` |
| flat 转 VMDK | `bximage -q -convert flat-to-vmdk -filename src.img -target out.vmdk` |
| 调整大小 | `bximage -q -resize -filename hd.img -size 120M` |

### 调试 Bootloader 和内核：bochsdbg

Bochs 的调试器是它最核心的价值所在。`bochsdbg.exe` 每次启动会自动停在调试器提示符，你可以在 CPU 指令级观察程序的每一步：

```
bochs:1> b 0x7c00          # 在 Bootloader 入口下断点
bochs:2> c                  # 继续执行（遇到断点自动停住）
bochs:3> info registers     # 查看所有寄存器状态
bochs:4> xp /16bx 0x7c00    # 以 hex 格式查看内存 0x7c00 处的 16 个字节
bochs:5> step               # 单步执行一条指令
```

OS 开发者最常用的场景：

- **枚举 PCI 设备**：在 I/O 端口 0xCF8 下断点，观察内核写 PCI 地址 → 读 0xCFC → 得到设备 ID 的完整过程
- **调试 PCI BAR**：每个 PCI 设备有 6 个 Base Address Register，告诉内核它的 I/O 端口或内存窗口位置——在 Bochs 里 BAR 分配和真实硬件完全一致
- **BIOS 加载验证**：黑屏时用 `xp /16bx 0xfffc0000` 检查 128KB BIOS 镜像是否真的加载到了 ROM 区域，全是 0 就说明 bochsrc 的 romimage 路径有问题
- **IRQ 路由**：用 `info pci` 查看 PCI IRQ 路由表，验证中断是否正确映射到 8259 PIC

### 典型场景：跑 Windows 98

把上面几块组合起来，一个能装 Windows 98 的完整 bochsrc 大概长这样：

```
romimage: file=BIOS-bochs-latest
vgaromimage: file=VGABIOS-lgpl-latest-cirrus.bin
cpu: model=pentium, count=1
memory: guest=64, host=64

pci: enabled=1, chipset=i440fx, slot1=cirrus, slot2=usb_uhci, slot3=ne2k
vga: extension=cirrus, update_freq=5

ata0-master: type=disk, path="win98.img", mode=flat, cylinders=406, heads=16, spt=63
ata0-slave: type=cdrom, path="win98.iso", status=inserted
floppyA: type=1_44, status=ejected

e1000: enabled=0
ne2k: enabled=1, mac=00:40:3c:00:00:02, ethmod=slirp
mouse: enabled=1

boot: cdrom
display_library: wx
```

装好系统后把 `boot: cdrom` 改成 `boot: disk`，下次就直接从硬盘启动。

### 性能预期

Bochs 是纯软件模拟，速度肯定慢，但慢到什么程度取决于你跑什么：

- 跑 DOS、Windows 98 这类老系统：在现代电脑上速度可接受
- 跑 Windows XP、Linux：明显慢，但做功能验证和调试完全够用
- 跑现代操作系统（Windows 10+/现代 Linux 发行版）：不推荐，太卡

如果需要更快的模拟，QEMU + KVM 是更好的选择——但 Bochs 的调试精度是 QEMU/GDB 组合也无法替代的。

---

## 🔗 相关资源

- [bochs官网](https://bochs.fwen.ren)
- [bochs文档](https://bochs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/bochs)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/bochs)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/bochs)

---

## 📚 相关文章

- [用 Bochs 从零跑起 Windows 98——手把手搭一个 x86 模拟器环境](https://laoguosofts.github.io/posts/bochs.html)
- [从零用 Bochs 调试你的第一个 Bootloader](https://alipymanbu.github.io/posts/bochs.html)
