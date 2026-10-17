## ✨ QEMU 是什么

QEMU（Quick Emulator）是一个开源的通用机器模拟器和虚拟化器。它能干两件事：**跨架构模拟**——在 x86 上跑 ARM、RISC-V、MIPS 等其他架构的系统和程序；**同架构硬件虚拟化**——配合 KVM、HVF、WHPX 等系统级加速器，在宿主架构上跑出接近物理机的性能。

它的定位介于"模拟器"和"虚拟机管理器"之间。纯软件模拟时，QEMU 通过内置的 TCG（Tiny Code Generator）JIT 编译器，把 Guest CPU 的指令实时翻译成宿主 CPU 能执行的机器码；有硬件加速时，它退化为设备管理器和状态协调器，把 CPU 指令直接交给硬件跑。

QEMU 支持的架构跨度极大：x86/x86_64、ARM/AArch64、MIPS、PPC、SPARC、RISC-V、LoongArch……几乎覆盖了市面上所有主流 CPU 家族。对开发者、嵌入式工程师、逆向分析人员、OS 课程学生来说，它是搭建多架构实验环境最顺手的工具之一。

性能方面，纯 TCG 模拟通常能达到原生的 10%~30%，足够跑轻量系统和用户态程序；配合硬件加速器（Linux KVM、macOS HVF、Windows WHPX），同架构虚拟化可以达到物理机的 85%~95%，大部分计算密集型工作负载几乎感觉不到差异。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://qemu.ggcd.online/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：QEMU 本身不在应用商店分发，Windows 用户建议选择预编译安装包或 MSYS2 方式，macOS 用户推荐通过 Homebrew 获取

---

## 🔧 三平台安装速查

QEMU 对硬件的最低门槛不高——纯软件模拟时几乎没要求。但要发挥最佳性能，需要 CPU 支持硬件虚拟化（Intel VT-x 或 AMD-V），并且在 BIOS/固件中已开启。宿主机内存建议至少 4GB 起步，分给虚拟机留 2GB 以上。

### Linux 安装

Linux 是 QEMU 的原生平台，包管理器直接装。

**Debian / Ubuntu：**

1. 更新软件源：`sudo apt update`
2. 安装核心组件：`sudo apt install qemu-system qemu-user qemu-utils`
3. 启用 KVM 加速：`sudo apt install qemu-kvm libvirt-daemon-system virt-manager`
4. 将当前用户加入 kvm 和 libvirt 组：`sudo usermod -aG kvm $USER && sudo usermod -aG libvirt $USER`
5. **注销并重新登录**，让组权限生效
6. 验证 KVM：运行 `kvm-ok`，输出 "KVM acceleration can be used" 即为成功

**RHEL / CentOS / Rocky：**

```
sudo dnf install qemu-system-x86 qemu-img qemu-kvm libvirt virt-manager
sudo systemctl enable --now libvirtd
```

**Arch Linux：**

```
sudo pacman -S qemu qemu-arch-extra libvirt virt-manager
sudo systemctl enable --now libvirtd
```

> [!TIP]
> KVM 用不了时先查 `/dev/kvm` 设备文件。`ls -la /dev/kvm` 看是否存在、权限是否为 `crw-rw----`、当前用户是否在 kvm 组。任何一项不对都会导致 KVM 静默回退到 TCG，性能差一个数量级。

### Windows 安装

Windows 没有系统包管理器，两条路可选。

**方式一：Stefan Weil 的预编译版（推荐）**

访问 `qemu.weilnetz.de` 下载 `.exe` 安装包。作者 Stefan Weil 是 QEMU 核心贡献者之一，Windows 版由他维护，版本更新及时。安装完成后将安装目录（默认 `C:\Program Files\qemu`）加入系统 PATH 环境变量。

**方式二：MSYS2**

在 MSYS2 MinGW64 终端里：`pacman -S mingw-w64-x86_64-qemu`

**启用 WHPX 硬件加速：**

1. 控制面板 → 程序和功能 → 启用或关闭 Windows 功能
2. 勾选「虚拟机平台」和「Windows Hypervisor Platform」
3. 重启系统
4. 启动 QEMU 时加 `-accel whpx` 参数即可

> [!NOTE]
> Windows 家庭版也支持虚拟机平台和 WHPX，只是没有 Hyper-V 管理界面。加速效果与专业版一致。

### macOS 安装

**Homebrew（推荐）：** `brew install qemu`

Apple Silicon（M1/M2/M3/M4）和 Intel Mac 都支持。macOS 10.15（Catalina）起自带 Hypervisor.framework，QEMU 6.1+ 原生支持 HVF 加速——不需要额外安装任何东西，启动 QEMU 时加 `-accel hvf` 即可获得硬件加速。

### 验证安装

无论哪个平台，装完跑这两条命令确认：

```
qemu-system-x86_64 --version
qemu-img --version
```

都输出版本号就说明安装成功。

---

## 📖 使用入门：从核心概念到跨架构实战

### 两种运行模式：TCG 和硬件加速

QEMU 有两条执行路径，理解它们的差异是用好 QEMU 的第一步。

**TCG（Tiny Code Generator）** 是纯软件 JIT 翻译引擎，也是 QEMU 跨架构能力的根基。当你在 x86 主机上模拟 ARM 或 RISC-V 时，Guest 指令不会被宿主 CPU 直接识别——TCG 就是那个实时翻译官。它不是逐条翻译，而是按翻译块（Translation Block, TB）批量处理，一个 TB 的边界在分支指令、系统调用或 1000 条指令上限处。翻译完成后，TCG 用直接块链接（DBC）把相邻 TB 的执行串起来，下次运行同一 TB 时直接跳转，不再回 QEMU 主循环。

TCG 的性能瓶颈来自四个方面：每条指令都要经过"解码 → 中间表示 → 宿主指令"三级翻译链；运行时需要检查特权指令和内存访问有效性；Guest 的 MMU 和页表需要软件模拟；翻译产物常驻内存会挤占宿主 CPU 缓存。实测下来，TCG 模拟通常只能达到原生性能的 10%~30%。10.x 版本引入了多线程 TCG（`-accel tcg,thread=multi`），在多核 CPU 上首次启动虚拟机时能显著加速翻译过程，但根本限制绕不开——它要模拟的是另一套 CPU，本质上就需要翻译。

**硬件加速模式** 下，QEMU 的角色发生了根本性变化：它不再模拟 CPU，而是退化为设备管理器 + 状态协调器。Guest 的普通 CPU 指令（算术、内存访问、函数调用）直接跑在物理核心上，只有涉及 I/O 和特权操作时才需要 QEMU 介入。性能可以达到物理机的 85%~95%。

三个加速器原理相同，都是操作系统提供的硬件虚拟化接口：

| 平台 | 加速器 | 内核模块 | 说明 |
| --- | --- | --- | --- |
| Linux | KVM | kvm.ko + kvm-intel.ko / kvm-amd.ko | 最成熟，功能最全 |
| macOS | HVF | 内置于系统 | Apple Silicon 和 Intel 都支持 |
| Windows | WHPX | Windows 虚拟机平台功能 | 需要 1803+ 版本 |

> [!WARNING]
> 以下场景硬件加速不可用，QEMU 会静默回退到 TCG：跨架构模拟（x86 主机模拟 ARM64）、老 CPU 没有 VT-x/AMD-V 扩展、虚拟机里跑虚拟机（嵌套虚拟化需要额外配置）、Docker 容器里默认没有 KVM 权限、BIOS 没开虚拟化选项。回退不会报错，但性能会骤降。

### 硬件加速的三层架构

以 Linux KVM 为例，虚拟化过程涉及三个协作组件：

1. **QEMU（用户态）**：创建设备、处理 I/O、管理虚拟机状态，通过 `ioctl` 与 KVM 内核模块交互
2. **KVM 内核模块**：暴露 `/dev/kvm` 设备文件，将 QEMU 的创建/运行请求传递给硬件
3. **CPU 硬件虚拟化扩展（VT-x/AMD-V）**：提供 VMXON、VMLAUNCH、VMEXIT 等指令，Guest 指令在 VMXON 模式下直接执行

虚拟 CPU 的运行流程：QEMU 通过 `KVM_CREATE_VM` 创建虚拟机 → `KVM_CREATE_VCPU` 创建虚拟 CPU → `KVM_RUN` 让 CPU 进入 VMXON 模式 → Guest 指令直接在物理核心上执行 → 遇到敏感操作（I/O、中断、页表修改）触发 VMEXIT → CPU 回到宿主模式，QEMU + KVM 处理事件 → 处理完重新进入 VMXON 继续执行。

### 创建第一个虚拟机

装好 QEMU 之后，创建一个最小化的 x86_64 虚拟机大概是这样：

```
qemu-system-x86_64 \
  -accel kvm \
  -cpu host \
  -smp 4 \
  -m 4G \
  -drive file=disk.qcow2,format=qcow2,if=virtio \
  -nic user \
  -vga virtio
```

几个关键参数说明：

- `-accel kvm`：启用 KVM 加速。如果系统里 KVM 不可用，这里会报错，换 `-accel tcg` 即可
- `-cpu host`：把宿主 CPU 特性完整暴露给 Guest，兼容性最好。如果需要虚拟机可迁移，换成固定型号如 `-cpu Skylake-Server`
- `-smp 4`：虚拟 4 个 CPU 核心。可以扩展成 `-smp 4,cores=2,sockets=2,threads=1` 模拟真实多核拓扑
- `-m 4G`：分配 4GB 内存
- `-drive ...if=virtio`：用 virtio 磁盘控制器，比默认的 IDE 控制器性能好很多
- `-nic user`：用户态网络，最简单的联网方式。需要端口转发时用 `-nic user,hostfwd=tcp::2222-:22`

> [!TIP]
> VirtIO 系列设备是半虚拟化设备，减少了 I/O 模拟开销。磁盘、网卡、显卡都用 virtio 系列，性能比模拟真实硬件（如 Intel e1000）好不少。Guest 系统需要有 virtio 驱动——Linux 内核自带，Windows 需要额外安装驱动。

### 跨架构模拟实战：RISC-V

QEMU 对 RISC-V 的支持非常完整，系统级和用户态模拟都能跑，10.x+ 版本已经覆盖 RV64（64 位）、RV32（32 位）、IMAFDC 标准扩展以及虚拟化扩展（H 扩展）。

模拟 RISC-V 时有两种板卡可选：

| 板卡 | 说明 | 推荐场景 |
| --- | --- | --- |
| `virt` | QEMU 虚拟 RISC-V 服务器板 | 跑完整 Linux 发行版，通用推荐 |
| `sifive_e` | SiFive 虚拟开发板 | 嵌入式、RTOS 开发 |

大多数场景用 `-M virt`，设备覆盖最广。

**直接引导内核（推荐方式）：**

准备好 RISC-V 内核、initrd 和根文件系统 qcow2 后，一条命令启动：

```
qemu-system-riscv64 \
  -M virt \
  -m 4G \
  -cpu max \
  -kernel vmlinuz-riscv64 \
  -initrd initrd-riscv64 \
  -drive file=rootfs-riscv.qcow2,format=qcow2,if=virtio \
  -append "root=/dev/vda1 console=ttyS0" \
  -netdev user,id=n0,hostfwd=tcp::2223-:22 \
  -device virtio-net-pci,netdev=n0 \
  -nographic
```

关键差异对比 x86 模拟：CPU 型号是 `riscv64` 或 `riscv32`；内核参数里控制台是 `console=ttyS0`（ARM 是 `ttyAMA0`）；`-M virt` 在 RISC-V 上同样是通用板卡。

传统引导链走 OpenSBI → U-Boot → Linux Kernel 三阶段，QEMU 已经内置了 OpenSBI（`-bios default`），但直接用 `-kernel` 参数跳过最省事。

**用户态模拟：** 不需要完整系统，跑单个 RISC-V 编译的二进制。安装 `qemu-user` 包后，用 `qemu-riscv64 ./riscv-hello` 直接执行，速度是全系统模拟的 5~10 倍，适合快速验证编译产物。带根文件系统时用 `-L /opt/riscv-rootfs` 指定，能获得完整的用户环境。

**GDB 调试内核：** 启动 QEMU 时加 `-s -S`（监听 1234 端口、启动时暂停等待调试器），然后用 RISC-V 交叉编译版 GDB 连接：

```
qemu-system-riscv64 -M virt -m 4G -smp 4 -kernel vmlinuz -initrd initrd -drive file=rootfs.qcow2,format=qcow2 -append "root=/dev/vda1 nokaslr console=ttyS0" -s -S -nographic
riscv64-unknown-elf-gdb vmlinux
(gdb) target remote :1234
```

### 调优要点汇总

- **CPU 型号**：`-cpu host` 性能最好但不可迁移，生产环境建议用固定型号
- **CPU 拓扑**：`-smp` 参数对齐 NUMA 拓扑能减少跨 socket 通信开销
- **大页内存**：启用 hugepages 减少 TLB miss，计算密集型负载提升明显
- **VirtIO 设备**：磁盘、网卡、显卡统一用 virtio 系列
- **TCG 多线程**：跨架构模拟时加 `-accel tcg,thread=multi`
- **轻量 Guest OS**：Alpine、OpenWrt 比 Windows 或完整 Ubuntu 在 TCG 模式下快得多

---

## 🔗 相关资源

- [qemu官网](https://qemu.ggcd.online)
- [qemu文档](https://qemu-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/APP/qemu)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/APP/qemu)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/APP/qemu)

---

## 📚 相关文章

- [QEMU 版本进化史：从 TCG 翻译器到三平台硬件加速生态](https://laoguosofts.github.io/posts/qemu-docs.html)
- [QEMU 深度评测：跨架构虚拟化的唯一解，为什么开发者绕不开它](https://alipymanbu.github.io/posts/qemu-docs.html)
