## ✨ ShizukuRunner —— 在手机上跑 shell 命令的无 root 方案

ShizukuRunner 是一个让 Android 手机在**不需要 root** 的前提下执行 Linux shell 命令的轻量工具。它的核心架构很简洁：通过 Shizuku 提供的 Binder 通道，把你输入的命令转发给系统级 shell 进程去执行，再把结果返回给 APP 界面。整个过程不需要刷机、不需要改系统分区，只要装好 Shizuku 就能用。

Runner 本身不做任何复杂的功能——它只是一个命令管道。但正是这条管道打通了 Android 的 shell 权限，让你能做很多平时需要 root 才能干的事：从 `/data/app/` 目录直接提取 APK、批量备份手机上所有应用的安装包、清理缓存、查看系统底层信息……一切都通过 shell 命令完成，Runner 负责的只是「送命令过去」和「把结果拿回来」。

> [!NOTE]
> Runner 拿到的是 shell 权限（uid=2000），不是 root。shell 权限能读 `/data/app/`、能读写 `/sdcard/` 和 `/data/local/tmp/`，但写不了 `/system/`、访问不了其他 App 的私有数据目录。够用，但不是万能。

适合谁用？折腾党需要从 APK 提取做逆向分析、开发者要排查 App 在手机上的实际行为、普通用户想清理缓存但不想一个个点开设置——这些场景 Runner 都能派上用场。如果你只是想装个 APP 然后日常用，那 Runner 对你来说可能太重了。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://shizukurunner.qjxz.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：ShizukuRunner 在 Google Play 或各大 Android 应用商店可搜；Shizuku 服务端需单独安装（Shizuku）

---

## 🔧 安装步骤

Runner 的安装分两部分：先装 Shizuku（提供 Binder 通道的服务端），再装 Runner（命令执行界面）。Runner 自己跑不起来，必须有 Shizuku 在后台提供通道。

**第一步：安装 Shizuku 并启动服务**

1. 安装 Shizuku 应用到手机
2. 用 adb 在电脑上执行：`adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/start.sh`
3. 手机上打开 Shizuku，确认授权状态显示「已授权」
4. 授权方式还有一种：用 root 模式启动 Shizuku 服务（如果手机已经 root），或者通过无线调试授权

> [!TIP]
> Shizuku 授权容易过期，重启手机后需要重新走一遍 adb 启动流程。如果你发现 Runner 执行命令返回权限错误，第一反应就是去 Shizuku 看授权状态。

**第二步：安装 Runner 并连接 Shizuku**

1. 安装 Runner APK，允许安装未知来源应用（Android 8.0+ 需要在设置里对当前浏览器/文件管理器开启「允许安装未知应用」）
2. 打开 Runner，首次启动会弹出 Shizuku 授权请求，点允许
3. 界面上显示「已连接 Shizuku」就说明通道打通了
4. 随便执行一条 `echo hello` 验证一下，能看到输出就说明一切正常

**版本核对注意点**

- Runner 和 Shizuku 的版本要匹配，版本号差太多可能连接不上
- Android 版本在 7.0 以下的设备不支持 Shizuku，也就跑不了 Runner
- 部分国产 ROM（MIUI、ColorOS、OneUI 等）对后台进程管理较激进，建议把 Shizuku 和 Runner 都加入电池优化白名单

---

## 📖 使用入门

Runner 的界面就是一个命令输入框加一个结果展示区。你敲命令，Runner 帮你执行，然后把输出贴回来。听起来简单，但想用好它需要先摸清楚 Runner 的能力边界和使用技巧。

### Runner 能做什么

Runner 本身不提供反编译、不提供文件管理界面——它只是送命令。真正干活的是 shell 命令本身和你自己的工具链。Runner 的核心价值是**把 shell 权限交给你**，让你在手机上直接操作，不用每次都 adb 连电脑。

最常用的几个场景：

**提取 APK 做逆向分析**

shell 权限可以直接读 `/data/app/` 目录，所以提取 APK 根本不需要 root。整个流程分三步：先找路径，再复制出来，最后用电脑上的工具反编译。

```bash
# 找到目标 App 的安装路径
pm path com.example.app
# 返回: package:/data/app/com.example.app-abcxyz==/base.apk

# 把 APK 复制到 sdcard
cp /data/app/com.example.app-abcxyz==/base.apk /sdcard/runner-extract/app.apk

# 或者更简洁的一行命令
cp $(pm path com.example.app | cut -d: -f2) /sdcard/runner-extract/
```

拿到 APK 后传到电脑，用 jadx 看 Java 源码、用 apktool 看 smali 和 XML、用 aapt2 查资源清单。jadx 的 GUI 版直接拖 APK 进去就能看代码，命令行版用 `jadx -d output-folder base.apk` 指定输出目录。

> [!NOTE]
> `pm path` 只能拿到主 APK（base.apk）。如果 App 用了 split APK（动态特性模块），需要额外找 `split_config.*.apk` 文件，一般在同一个目录下。

**批量备份所有已安装 APK**

想保留一份当前手机上所有 App 的安装包？在 Runner 里逐行敲命令效率太低，正确做法是把脚本写好推到手机上执行：

```bash
# 电脑上写好脚本，推到 /data/local/tmp/
adb push backup.sh /data/local/tmp/

# Runner 里执行
sh /data/local/tmp/backup.sh
```

脚本内容很简单——遍历 `/data/app/*/base.apk`，一个个复制到 `/sdcard/apk-backup/`。不过要注意，Runner 里执行 shell 脚本时，for 循环这类语法的兼容性取决于 Android 内置的 toybox/busybox 版本，遇到问题可以加一层 `sh -c` 执行。

**清理应用缓存和数据**

Runner 可以直接用 pm 命令操作应用：

```bash
# 清理指定 App 的所有数据（包括登录状态，谨慎使用）
pm clear com.example.app

# 禁用一个应用（可恢复）
pm disable-user --user 0 com.example.app

# 强制停止应用
am force-stop com.example.app
```

如果只是想清缓存而不想丢登录状态，可以直接删缓存目录——但 shell 权限碰不到某些 App 的私有 `/data/data/` 子目录，这时候就得退而求其次清 `/sdcard/Android/data/` 下的临时文件。

**查看系统底层信息**

Runner 能读 `/system/`、`/vendor/`、`/proc/`，所以系统版本、硬件配置、CPU 架构都能直接查：

```bash
# ROM 版本
getprop ro.build.display.id

# Android 版本
getprop ro.build.version.release

# 设备型号
getprop ro.product.model

# CPU 信息
cat /proc/cpuinfo
```

### Runner 的硬限制和安全禁区

Runner 有几个硬性限制，不是 bug，是开发者为了防止 APP 卡死做的安全设计：

| 限制项 | 阈值 | 原因 |
|---|---|---|
| 单次输出上限 | 约 1000 字符 | 超过后 Runner 强行关闭输出管道，返回值 141（SIGPIPE） |
| 无限输出命令 | 不支持 | Runner 无法给子进程发送 Ctrl+C，一旦跑起来会一直到手机重启 |
| 交互式命令 | 不支持 | Runner 不会弹出输入框，命令会一直阻塞在等待输入上 |

**无限输出是头号禁区**——`logcat` 持续输出、`dumpsys --watch` 实时监控、`top -b -d 1` 持续刷新，这些命令绝对不能在 Runner 里直接跑。正确做法是加时间限制或重定向到文件：

```bash
# 只捕获 10 秒 logcat，重定向到 sdcard
timeout 10 logcat -d -v time > /sdcard/log.txt

# dumpsys 单次执行，不 watch
dumpsys activity activities > /sdcard/activity.txt
```

输出被截断返回值 141 其实是 Runner 的保护机制生效了——它主动关闭了输出管道，shell 子进程收到 SIGPIPE 后自动终止，输出不会全部丢失。遇到 141 不要慌，换成重定向到文件的方式就能拿到完整结果。

### 命令卡死了怎么办

Runner 通过 Shizuku Binder fork shell 子进程执行命令，但有个结构性盲区：**Runner 无法给子进程发送终止信号**。一旦命令进入无限输出、等待输入、等待慢速网络这三种状态之一，Runner 就管不了它了——界面卡在「执行中」转圈圈，强关也没用。

自救按优先级来：

1. **等 141 保护**——无限输出的命令跑到输出超限时会自动被截断终止，返回 141
2. **adb 手动杀进程**——电脑端 `adb shell ps | grep sh` 找到 Runner fork 的 shell 进程 PID，然后 `adb shell kill -9 <PID>`
3. **重启手机**——终极手段，因为卡住的 shell 子进程是独立后台进程，杀 Runner APP 不会连带杀掉它

预防比自救重要：批量脚本里每条命令独立执行，给需要时间的命令加 `timeout` 包装，不要把太多命令串在一条长链里。

### shell 和 root 的边界

Runner 拿到的是 shell 权限，不是 root。两者能操作的目录有明确差别：

| 目录 | shell 权限 | root 权限 |
|---|---|---|
| `/sdcard/` | 完整读写 | 完整读写 |
| `/data/app/` | 只读 | 完整读写 |
| `/data/local/tmp/` | 读写 | 读写 |
| `/system/` | 只读 | 读写（需 remount） |
| `/data/data/<其他包>/` | 基本碰不到 | 完整读写 |

Runner 有个「降权开关」——默认开启，会挡掉 shell 权限下做不到的 root 破坏性操作。但 shell 权限下本身就能做的危险操作（比如 `pm clear --all` 清空所有应用数据、`pm disable --all` 禁用所有应用）降权开关挡不住，执行前一定要想清楚后果。

---

## 🔗 相关资源

- [shizukurunner官网](https://shizukurunner.qjxz.skin)
- [shizukurunner文档](https://shizukurunner.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/shizukurunner)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/shizukurunner)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/shizukurunner)

---

## 📚 相关文章

- [ShizukuRunner 深度解析：为什么手机上跑 shell 命令这件事，值得一套专门的架构](https://laoguosofts.github.io/posts/shizukurunner.html)
- [ShizukuRunner 深度解析：一个无 root 应用如何拿到 shell 权限](https://alipymanbu.github.io/posts/shizukurunner.html)
