## ✨ QPython 是什么

QPython 是一款专为 Android 手机打造的完整 Python 编程平台。它把 Python 解释器、代码编辑器（QEditor）、交互式 REPL 终端、文件浏览器、二维码扫描，以及 QSL4A 硬件控制扩展整合进一个 App，让你在手机上就能写、跑、调 Python 代码，不需要电脑也不需要 root 权限。

它的核心定位是「移动端 Python 开发运行时」——既适合初学者在手机上练语法、跑小脚本，也适合进阶用户利用手机自带的硬件（GPS、相机、蓝牙、传感器）做自动化和数据采集。QSL4A 扩展模块提供了大量 Android API 的 Python 封装，从定位、拍照、录音到网络请求、传感器监听都能直接调用。

**主要特性一览：**

- **内置 Python 3.x 解释器**，支持标准库和大部分第三方包（通过 QPYPI 管理安装）
- **QEditor 编辑器**，语法高亮、代码补全、运行按钮一体化
- **REPL 交互式终端**，即时执行代码片段并查看结果
- **QSL4A 硬件控制层**，封装了 GPS、相机、蓝牙、NFC、传感器等 Android API
- **悬浮窗与后台服务**，脚本可以在其他 App 之上运行和弹窗
- **二维码扫描触发**，扫码即可远程下发和执行脚本
- **QNotebook 支持**，可以在手机上跑 Jupyter 风格的笔记本

QPython 面向的用户群相当广泛：学生用它在手机上学 Python 基础；开发者用它快速验证想法或写轻量自动化脚本；硬件爱好者可以利用手机传感器做数据采集实验。只要你的 Android 手机版本在 8.1 以上，存储空间够 200 MB，就可以跑起来。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://qpython.fewz.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：国内用户在小米应用商店、华为应用市场、应用宝、OPPO/vivo 应用商店搜索「QPython」；海外用户可在 Google Play 搜「QPython - Python for Android」

---

## 🔧 安装步骤

### 系统前置检查

开始下载之前先确认手机满足几个基本条件：

| 项目 | 要求 |
| --- | --- |
| Android 版本 | 8.1（Oreo）或以上 |
| 存储空间 | 至少预留 200 MB |
| 网络 | 能正常访问外网（下载第三方包时需要） |
| 安装权限 | 通过 APK 安装时需允许未知来源应用 |

App 本身的体积大约 74 MB，加上 Python 运行时和后续通过 QPYPI 安装的第三方包，200 MB 是比较稳妥的预留量。

### 渠道一：应用商店安装（最省心）

打开手机自带的应用商店（小米、华为、应用宝、OPPO、vivo 都可以），在搜索框里输入 **QPython**，找到官方发布的那一条。

> [!TIP]
> 认准开发者名称「北京优趣天下信息技术有限公司」和包名 `org.qpython.qpy`。市面上有名称类似的 App（如 QPython 3、QPython OH），注意区分，避免下到非官方版本。

点击安装后，系统会自动处理权限申请弹窗，一路确认即可。安装完成后桌面上会出现 QPython 的图标。

### 渠道二：GitHub APK 安装

如果应用商店搜不到或者需要更及时的更新，可以从 GitHub 仓库下载 APK：

1. 在手机浏览器访问 `qpython-android/qpython3` 仓库的 Releases 页面
2. 找到最新版本的 `qpython3-app-release.apk`，下载到本地
3. 打开手机的文件管理器，找到下载的 APK 文件并点击安装
4. 如果系统提示「禁止安装未知来源应用」，进入设置 → 安全 → 开启对应开关后重试

> [!WARNING]
> APK 安装包没有经过应用商店的安全扫描，下载前务必确认来源是官方 GitHub 仓库，不要从第三方网盘或论坛下载来路不明的 APK。

### 渠道三：Google Play

海外用户直接在 Google Play 搜索「QPython - Python for Android」即可安装，版本功能与国内应用商店基本一致。

### Lite 版 vs 完整版

早期 QPython 存在 Lite（轻量版）和完整版两个分支：

| 版本 | Python 版本 | 编辑器 | 扩展 | 体积 |
| --- | --- | --- | --- | --- |
| Lite | 3.6.6 | 简易编辑器 | 无 | 更小 |
| 完整版 | 3.x | QEditor | QSL4A、AI、Web 等 | 较大 |

目前 4.0 版本起官方主要维护完整版一个渠道，功能比旧版 Lite 丰富得多。即使只是学基础 Python，完整版也完全够用，不需要特意找 Lite。

### 安装后首次打开

首次启动 App 后，底部可以看到几个核心入口按钮：

- **终端**：REPL 交互式控制台，直接写代码执行
- **编辑器**：QEditor，可视化编辑 `.py` 文件
- **文件**：浏览手机文件系统
- **二维码**：扫描二维码执行脚本
- **扩展**：QSL4A / AI / Web 等扩展模块入口

进入主界面后，系统会陆续弹出权限申请，建议优先授予**存储、互联网、前台服务、悬浮窗、位置、相机**这几类核心权限，其他的按需来（具体每条权限的用途可以参考「使用入门」部分）。

---

## 📖 使用入门

### 权限配置：哪些必须给，哪些按需给

QPython 申请了 30+ 条 Android 权限，每条都对应实际功能，不是乱要的。建议优先授予的核心权限包括：

**存储类：** 写入/读取文件（必须给，脚本和第三方包都要存）、管理所有文件（Android 11+，需要跨 App 读数据时用）

**硬件类：** 精确位置（GPS 定位 API）、相机（拍照 + 二维码扫描），蓝牙/NFC/麦克风/闪光灯按需授予

**网络类：** 完全互联网访问（必须给，下载第三方包和访问在线 API 都要）、查看网络状态（建议给）

**系统类：** 悬浮窗（后台脚本弹窗必备）、前台服务（防止后台脚本被系统杀掉，长任务必须）、防止手机休眠（长任务建议给），敏感的「写入设置」「修改手机状态」「获取手机信息」能不给就不给

> [!NOTE]
> 如果某个功能调用报错 `Permission denied`，直接去系统设置 → 应用管理 → QPython → 权限管理里开就行，不需要重启 App——Android 权限是运行时生效的。

### 模块导入与 sys.path

Python 的 `import` 语句靠 `sys.path` 这个路径列表来找模块。QPython 启动时预设好了内置标准库、第三方包目录和当前工作目录：

```python
>>> import sys
>>> print(sys.path)
['', '/data/data/org.qpython.qpy/files/lib/python3.x', ...]
```

列表里的空字符串 `''` 就是当前目录，所以**当前目录下的 `.py` 文件可以直接 import**。但如果你的主脚本和自定义模块不在同一个目录，就需要手动把模块目录加进 `sys.path`：

```python
import sys, os
# 通用写法：定位到项目根目录后加入搜索路径
sys.path.insert(0, os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
from modules import utils
```

`sys.path.insert(0, ...)` 把路径插到最前面实现优先搜索，`sys.path.append(...)` 则是追加到末尾。修改只在当前会话生效——下次打开 QPython 需要重新加。如果目录里放一个空的 `__init__.py`，Python 会把它识别为包，之后可以用 `from modules.utils import some_function` 的方式导入。

> [!TIP]
> 早期 QPython 版本不支持虚拟环境（venv / virtualenv），4.0 之后虽然可以运行 `python -m venv`，但手机算力有限实际意义不大。移动端推荐的做法是在 QPYPI 里统一管理包，对项目隔离有强需求时用不同目录存放各自的 `requirements.txt` 记录依赖。

### 悬浮窗：后台脚本也能弹窗

悬浮窗让 QPython 可以在任何 App 之上显示 UI——包括 Toast 弹窗、Dialog 对话框、FSW2 Activity 浮窗形式，甚至自定义悬浮小球。实际用途有：后台脚本运行时弹出传感器触发报警、在任何 App 上显示一个一键触发脚本的控制按钮、在其他 App 使用过程中显示实时调试信息。

**权限前置：** 悬浮窗权限（`SYSTEM_ALERT_WINDOW`）必须授予，没有这条权限所有弹窗都会静默失败。检查方式：系统设置 → 应用管理 → QPython → 权限 → 悬浮窗 → 允许。MIUI、ColorOS 等 ROM 需要在「显示在其他应用上方」里单独打开。

最简单的悬浮窗调用是 Toast 通知：

```python
from androidhelper import Android
droid = Android()
# 在任何 App 上弹出 Toast，几秒后自动消失
droid.makeToast("来自 QPython 的后台通知")
```

Toast 最轻量、不打断用户，适合状态提示。需要用户交互时可以用 Dialog，或者部分版本支持的自定义悬浮小球——在屏幕上画一个半透明按钮，用户点一下就触发后台脚本的某个动作。

### 二维码扫描：远程下发脚本

QPython 内置了摄像头二维码扫描功能，调用后拉起相机界面，对准二维码扫描即可返回解码后的字符串。核心用途有两个：

**场景一：扫码运行脚本。** 把 Python 脚本的下载链接编码成二维码，QPython 扫码后自动下载并执行。这个功能在团队协作时特别好用——一个人写好脚本放到网上，其他人扫码就能拿到并运行。

**场景二：扫码传数据。** 两台 QPython 手机之间可以用二维码传递指令。A 手机用 `qrcode` 库生成包含指令的二维码图片，B 手机扫描后解析内容，根据指令启动对应的自动化流程。

二维码生成不是 QPython 内置功能，需要通过 QPYPI 安装 `qrcode` 库（纯 Python，可以装成功），生成图片后可以用 Android 的文件 API 保存或展示。

### 悬浮窗 + 二维码的组合玩法

这两个功能组合起来能做不少有意思的事情：

**远程控制脚本：** 手机 A 后台运行脚本并显示悬浮控制球，手机 B 扫 A 的屏幕二维码（包含远程控制 URL），扫码结果通过网络传到 A，A 根据内容执行对应操作。

**扫码配置自动化：** 在电脑上生成一个包含自动化参数的二维码（比如开启传感器监听、采样频率 500ms、触发阈值 20），手机 QPython 扫码后解析参数，脚本根据参数启动对应的自动化流程——不用手动敲代码，扫一下就能配置。

### 硬件控制入门：QSL4A API 速览

QPython 的 QSL4A 扩展模块是移动端自动化的利器，封装了大量 Android API。几个常用的硬件调用示例：

```python
from androidhelper import Android
droid = Android()

# GPS 定位
location = droid.getLastKnownLocation()

# 拍照
droid.takePicture("/sdcard/photo.jpg")

# 传感器监听（加速度计，采样间隔 1000ms）
droid.startSensingTimed(1, 1000)

# 蓝牙发现设备
devices = droid.bluetoothDiscover()
```

这些 API 的完整列表可以在 App 内的「扩展」入口里找到，也可以参考 QPython 文档站的 QSL4A 部分。使用前记得先在权限设置里给对应硬件权限。

### 常见问题排查

脚本报错 `Permission denied` → 权限没开，去系统设置里找对应权限打开即可，不需要重启 App。

导入自定义模块报 `ModuleNotFoundError` → 检查 `sys.path` 里有没有包含模块所在目录，用 `import os; print(os.getcwd())` 确认当前工作目录。

后台脚本被系统杀掉 → 确认前台服务权限已授予，同时可以在脚本里调用 `droid.wakeLockAcquire()` 防止手机休眠。

二维码扫描后没有返回结果 → 检查相机权限，部分手机需要在系统设置里单独给 QPython 相机权限。

---

## 🔗 相关资源

- [QPython官网](https://qpython.fewz.tech)
- [QPython文档](https://qpython.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/QPython)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/QPython)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/QPython)
