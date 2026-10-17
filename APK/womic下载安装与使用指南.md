## ✨ WO Mic 是什么

WO Mic 是一款能把手机当成电脑麦克风用的工具。它由两部分组成：手机端的 WO Mic App 负责采集手机麦克风的声音，PC 端的 WO Mic Client 负责接收并把音频数据喂给一个虚拟驱动，让 Windows 把这块手机当成一块真实的 USB 麦克风。

核心就两个字：**虚拟**。装完之后，设备管理器里会多一个 "WO Mic Device"，任何能录音的程序——Zoom、腾讯会议、Skype、Audacity——都会把它当成一块独立的音频输入设备来选，跟插一块物理麦克风没区别。

支持的手机平台覆盖 Android 和 iOS，连接方式有 USB、Wi-Fi、蓝牙三种可选。对经常在家办公、上网课的人来说，手机麦克风的音质通常比笔记本自带的好不少，换一根数据线就能省掉买麦的钱。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://womic.xnyx.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 手机端：Google Play 或 App Store 搜 "WO Mic"
> - PC 端：Windows 64 位系统，直接下 setup 安装包

---

## 🔧 安装步骤

手机端和 PC 端都需要装，顺序没所谓，但 PC 端装完**必须重启电脑**。

### 手机端

1. 从官方下载页面或应用商店找到 WO Mic App，装到手机上
2. 首次打开时会请求麦克风权限，允许即可
3. 如果用 Wi-Fi 或蓝牙连接，还需要允许位置权限——这是 Android 扫描周边设备的系统要求，不是 WO Mic 故意索要

### PC 端

1. 去官方下载页面找 Windows 安装包，双击运行 setup
2. 安装过程中驱动签名可能会弹 Windows 安全提示，选 "始终信任此驱动" 或 "允许安装"
3. 安装完成后**立刻重启电脑**——驱动在内核层加载，不重启就不会生效

> [!WARNING]
> 重启之前打开 WO Mic Client 去看驱动状态，一定是 0（未加载）。别慌，重启完再打开就是 1 了。

### 验证驱动是否装好

重启后打开 WO Mic Client，菜单 **Help → Statistics**，弹出面板里看 **Driver state: 1** 就说明驱动挂载成功。也可以打开设备管理器（Win + X → 设备管理器 → 声音、视频和游戏控制器），能看到 "WO Mic Device" 条目且没有感叹号。

### 常见安装障碍

- **安全软件拦截**：360、火绒、Defender 可能把未签名驱动标记为可疑，选 "允许" 或 "信任" 就行
- **旧版本冲突**：之前装过 WO Mic 旧版或 VB-Cable 这类虚拟音频驱动的，先用自带 uninstall 卸掉，设备管理器里手动删旧驱动（右键卸载 + 勾选"删除此设备的驱动程序软件"），重启再装
- **装完驱动状态始终是 0**：九成是没重启，剩下一成是安全软件把驱动文件隔离了

---

## 📖 四种连接方式怎么选

WO Mic 提供 USB、Wi-Fi、蓝牙、Wi-Fi Direct 四种连接。实际用起来主要是前三种，选哪种取决于你的场景。

### USB：最稳、延迟最低

两端插数据线，手机端选 USB 传输模式（Android 需要开启 USB 调试）。延迟最低、抗干扰最强，但需要一根线拉着。适合需要低延迟的场景——比如游戏语音、录屏配音。

### Wi-Fi：最常用的方式

手机和电脑连同一个路由器。手机 App 打开后会显示本机 IP，PC 端 Connection → Connect，Transport 选 Wi-Fi，把手机 IP 填进去就行。比 USB 稍延迟但胜在无线自由，语音聊天、线上会议完全够用。

> [!TIP]
> 如果公司或学校网络有 AP 隔离（同网段设备互相看不见），Wi-Fi 就连不上，换 USB 或蓝牙。

### 蓝牙：省事但有代价

电脑自带蓝牙适配器就能用，不用插线也不用看 IP，配对成功后 PC 客户端 Transport 选 Bluetooth、Phone 下拉框选你手机名字就行。但蓝牙和 Wi-Fi 都跑 2.4GHz，同时开会严重互相干扰——声音断断续续、连接频繁断。建议用蓝牙时关掉 Wi-Fi，反之亦然，或者把手机和电脑拉近到 1-2 米内。

| | USB | Wi-Fi | 蓝牙 |
| --- | --- | --- | --- |
| 延迟 | 最低 | 中 | 较高 |
| 抗干扰 | 最强 | 中 | 弱 |
| iPhone 支持 | ✘ | ✔ | ✘ |
| Android 支持 | ✔ | ✔ | ✔ |
| 额外硬件 | USB 线 | 路由器 | 蓝牙适配器 |

### 各会议软件里怎么选 WO Mic

大部分会议软件都有专门的麦克风下拉框，直接选 "WO Mic Device" 就行，Zoom、腾讯会议、飞书都是这样。有两个特殊点要注意：

**Skype** 把麦克风和扬声器绑成一个 "Audio device"，如果扬声器用电脑自带喇叭、麦克风用 WO Mic，默认设备选不到。解决办法是进 Skype 设置 → 音频和视频，找 "Custom device"，选完会弹出独立的麦克风和扬声器下拉框，麦克风那栏选 WO Mic Device、扬声器那栏选电脑自带的就行。

**网页版会议**（Google Meet 等）需要浏览器单独授权：Chrome 或 Edge 打开会议页面后，点地址栏右边的锁图标 → 网站设置 → 麦克风 → 允许，然后下拉选 WO Mic Device。

> [!NOTE]
> 某个会议软件下拉框找不到 WO Mic？先在 Windows 声音设置里把 WO Mic 设为默认麦克风，然后**重启这个软件**——启动时它会重新读取默认设备列表。

会议软件自己的静音按钮只是让软件不输出声音，WO Mic 端还在正常传。真想停传输，按手机 App 的 Mute 或直接断连。

### 权限和隐私

WO Mic 手机端请求的权限很克制：麦克风是必须的，位置权限是 Android 做 Wi-Fi Direct / 蓝牙扫描的系统要求（Google Play 政策强制），前台服务是让 App 在锁屏或切出去后还能继续传音频（否则 Android 会杀掉后台进程）。

它不做的事反而更重要：**音频流只在手机和 PC 之间点对点传输**，不经过 WO Mic 任何服务器；不收集通讯录、短信、通话记录；不读位置历史；不跟踪设备。Windows 虚拟驱动只干一件事——把客户端收到的音频注册为麦克风输入设备，不碰其他系统资源。

---

## 🔗 相关资源

- [womic官网](https://womic.xnyx.quest)
- [womic文档](https://womic.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/womic)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/womic)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/womic)

---

## 📚 相关文章

- [WO Mic 到底值不值得掏钱？一笔虚拟麦克风的时间账](https://laoguosofts.github.io/posts/womic.html)
- [WO Mic 到底值不值得掏钱？——把手机当电脑麦克风这笔账算明白](https://alipymanbu.github.io/posts/womic.html)
