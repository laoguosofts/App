## ✨ SmartThings 是什么

SmartThings 是三星推出的智能家居平台，核心定位是把三星自家的 TV、冰箱、洗衣机、空调等家电与第三方智能设备统一纳入一个 App 管理，并通过 Routines 自动化引擎实现设备之间的联动。平台支持 Matter、Thread、Zigbee、Z-Wave、BLE 等多种协议，设备覆盖范围达到 380 多个品牌、4500+ 款产品。

对三星家电用户来说，SmartThings 的价值不可替代——HomeKit 和 Home Assistant 都无法调用三星家电的专属 API，比如 Family Hub 冰箱的屏幕推送、AI 洗衣机中途加衣物提醒、冰箱门未关报警等，这些功能只有 SmartThings 原生支持。2024 年 CES 发布的 3D Map View 功能允许用户扫描或上传房屋平面图后，设备以 3D 视图在空间中可视化排布，配合 AI Energy Mode 节能引擎可让三星洗衣机节水节电 60%、烘干机节能 30%、冰箱节能 10%。

技术门槛极低，普通消费者拿到手机就能上手。Hub 方面，SmartThings Hub V3 同时支持 Matter-over-WiFi 和 Matter-over-Thread，内置完整的 Thread Border Router 功能；如果只有老款 v2 Hub，Thread 硬件缺失，可以额外配一个 Google Nest Wifi Pro 或 Nanoleaf 控制器补足。语音控制支持 Bixby、Alexa 和 Google Assistant，iOS 用户也可以正常使用 App。

SmartThings 的劣势主要在本地控制能力偏弱（多数场景依赖云端），iOS 系统级集成不如 HomeKit 深入，社区插件生态远不如 Home Assistant 丰富。但对于拥有三星家电的用户来说，这是目前唯一能完整发挥硬件能力的选择。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://smartthings.fedq.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜索 SmartThings（Android 7.0+）；App Store 搜索 SmartThings（iOS 15+）；三星 Galaxy 手机自带预装

---

## 🔧 安装与首次配置

SmartThings App 本身不复杂，但首次安装涉及 Hub 和设备配对，建议按以下步骤顺序操作。

### 手机端安装

1. 在应用商店搜索 **SmartThings** 下载安装，Android 用户注意检查是否为官方开发发布。
2. 打开 App 后用三星账号登录（没有可以免费注册），或者选择 Google / Facebook 登录。
3. 授权位置、蓝牙、通知等必要权限——Geofencing 和 Quick Remote 依赖这些权限才能正常工作。

> [!TIP]
> Android 用户如果下载渠道非 Google Play，安装时需要在系统设置中允许「未知来源」或「安装未知应用」。

### Hub 配对（如有）

SmartThings Hub V3 是目前推荐型号，硬件上同时具备 Matter-over-WiFi 和 Matter-over-Thread 能力：

| 步骤 | 操作 |
|------|------|
| 1 | Hub 插电，等待 LED 指示灯变为蓝色闪烁 |
| 2 | App → Menu → Add devices → Scan QR code，扫描 Hub 底部二维码 |
| 3 | Hub 自动连接到你家的 WiFi 网络（2.4GHz） |
| 4 | 等待约 2 分钟完成固件更新 |

v2 Hub 硬件没有 Thread 天线，不能做 Thread Border Router，但 Matter-over-WiFi 设备仍可正常接入。Thread 设备需要额外配一个独立的 Thread Border Router（Google Nest Wifi Pro、Nanoleaf 控制器等都可以）。

### 设备添加

Hub 就绪后，开始逐个添加设备：

1. 让设备进入配对模式（通常是长按设备上的配对按钮 3~5 秒，直到指示灯快闪）。
2. App → Menu → Add devices，选择设备类别和品牌。
3. 也可以在首页顶部点击「Scan」直接扫描设备上的 QR 码，部分新设备支持 Matter 一键配对。

> [!NOTE]
> 三星自家的 TV、Family Hub 冰箱、Jet Bot 扫地机器人等，首次开机时会自动出现在 SmartThings App 的「Nearby devices」列表，点击确认即可完成配对，非常省心。

### Thread 网络自动启动

如果你使用的是 Hub V3，配完 Hub 后 Thread Border Router 会自动启动：

- 附近支持 Thread 的设备开机后自动加入 Thread Mesh 网络
- 不需要手动配置 Thread 网络参数
- Hub V3 为每个 Thread 设备分配独立的 IPv6 地址，数据通过 Mesh 多跳传输，覆盖面远超单一设备的 WiFi 范围

---

## 📖 使用入门

SmartThings 的能力涵盖设备控制、场景自动化、语音交互、协议桥接等多个层面。下面从最常用的几个场景切入，逐步深入。

### Routines 自动化：平台的核心引擎

Routine 是 SmartThings 自动化的基础单位，逻辑结构是 **IF（触发条件）+ THEN（执行动作）**。一个 Routine 可以同时触发设备控制、发送通知、甚至调用其他 Routine，通过简单的条件组合就能实现复杂的家庭自动化。

#### IF 条件组合

Routine 支持多条件同时判断，关系由用户选择：

- **All conditions（AND）**：所有条件同时满足才触发——比如「晚上 10 点 且 所有人在家 → 执行 Good night」，两个条件缺一不可，避免白天误触发睡眠场景。
- **Any condition（OR）**：任一条件满足即触发——比如「主卧门开 或 门厅运动传感器触发 → 开主卧灯」，两个信号都能触发同一套动作。

可用的条件类型包括：

| 类型 | 说明 |
|------|------|
| 时间 | 指定时间、每天/每周重复、日出、日落、日出/日落前 X 分钟 |
| Geofencing | 到家、离家、到达特定地理围栏（需要后台位置权限） |
| 设备状态 | 开关变化、数值变化（温度、亮度）、电池电量、传感器触发 |
| 手动 | 手动运行按钮、语音命令 |
| 存在 | 某人不在家、某设备在线/离线 |

**延迟条件（Grace Period）** 是缓解传感器误触发的关键机制。比如「冰箱门开启 → 延迟 2 分钟 → 推送通知」，你打开冰箱拿东西的短暂过程不会报警，只有门开了超过缓冲期才触发。对门窗传感器、运动传感器这类容易被短暂触发的设备特别有用。

#### THEN 动作设计

动作端支持的类型包括：

- **设备控制**：开关设备、调节温度/亮度/风速/音量、调用预设场景
- **通知类**：推送通知（手机）、Family Hub 屏幕提示（三星冰箱专属）、三星 TV/音箱语音播报
- **控制流**：延迟等待、嵌套调用其他 Routine

嵌套调用是构建复杂场景的有力工具——你可以把常用的动作组合做成一个手动 Routine（比如「Secure Home」负责关所有窗、锁门、关灯），然后在其他 Routine 的 THEN 里调用它。需要注意的是，自动触发的 Routine 之间不要直接嵌套，容易造成死循环，推荐自动 Routine 只调用手动 Routine。

#### 实战举例

**冰箱门未关报警**：

```
IF：
  冰箱门传感器 → 开启
  延迟 2 分钟
THEN：
  推送通知 "冰箱门未关，请检查"
  Family Hub 屏幕提示
```

**雨天提醒关窗**：

```
IF：
  气象服务检测到降雨
THEN：
  推送通知 "今天有雨，记得关窗"
  调用 Secure Home Routine（关所有窗、锁门）
```

**回家自动开空调**：

```
IF：
  Geofencing：我到家了
  当前温度 > 30°C
THEN：
  开客厅空调 → 制冷 → 26°C
  开玄关灯
```

#### 管理技巧

- **临时禁用 vs 删除**：Routine 详情页底部有 Enabled/Disabled 开关，过年离家时把日常 Routines 禁用避免异常触发，比删除好。
- **复制快速创建**：在 Routines 列表页长按 Routine → 复制 → 改几个条件就能快速创建新的。Good morning 复制一份改成 Good afternoon 就是现成的两个场景。
- **命名规范**：用描述性强的名字如「Good night」「Arriving home」，不要用「Routine 1」「Test」这类模糊命名。推荐用前缀分组，比如 `Light-` `Lock-` `Energy-` 方便在列表中快速定位。
- **数量控制**：SmartThings 本身不限 Routine 数量，但 200 条以内比较合理，太多会让列表难以管理。
- **排查触发**：Routine 详情页可以查看最近运行记录，看有没有触发日志、触发时间、触发原因。有时候你以为 Routine 在运行，其实某个条件没满足。

### Galaxy Quick Remote：拿着手机靠近设备自动弹出遥控

这是 Galaxy 手机（S21 及以后 / Z Flip/Fold 系列）独有的系统级黑科技。当你拿着手机靠近三星 TV、空调、Family Hub 冰箱或 Jet Bot 扫地机器人时，屏幕底部会自动弹出对应设备的控制面板——不需要打开 App、不需要解锁屏幕、也不需要手动配对（配对只做一次）。

触发原理是多种近距离通信技术协同工作：BLE 蓝牙低功耗负责设备广播存在、WiFi Direct 建立点对点连接传输控制面板数据，部分支持 UWB 的型号（Galaxy Z Fold4/S22 Ultra 等）还能用超宽带做精确定位，知道你在设备正前方 1 米内还是侧面，决定什么时候弹面板。

支持的功能面板包括：

| 设备 | Quick Remote 功能 |
|------|------------------|
| 三星 TV | 电源开关、音量 ±/静音、输入源切换、频道切换、快捷启动 Netflix/YouTube |
| 三星空调 | 电源开关、温度调节（1°C 步进）、模式切换（Cool/Heat/Dry/Auto）、风速调节 |
| Family Hub 冰箱 | 冰箱温度、门状态、Family Hub 屏幕快捷入口（显示菜谱、播放音乐） |
| Jet Bot 扫地机器人 | 清扫模式选择（静音/标准/强力/局部）、回充按钮、实时清扫地图 |

非三星 Android 手机和 iPhone 不支持这个功能，Quick Remote 依赖 One UI 与 SmartThings App、BLE 栈的深度集成，是三星硬件和软件协同的专属体验。

### Thread 协议与 Matter：低功耗 Mesh 网络的基础

Thread 是基于 IEEE 802.15.4 的低功耗 Mesh 网络协议，与 Zigbee 同频段但设计理念完全不同：

| 维度 | Thread | Zigbee |
|------|--------|--------|
| IP 原生 | ✅ IPv6 | ❌ 封闭协议，Hub 转译 |
| 网络规模 | 上百台 | 通常 < 30 台 |
| Mesh 中继 | 每个设备都是中继 | 仅部分设备（路由设备） |
| 数据安全 | AES-128 + Thread 自有加密 | AES-128，加密较弱 |
| 与 Matter 关系 | Matter 首选底层 | Matter 可以跑 Zigbee 但非首选 |

Thread 的核心优势在于 IP 原生——每个 Thread 设备都有独立的 IPv6 地址，Mesh 中继让数据可以多跳传输，覆盖范围远超 WiFi 单跳。Thread Group 的成员包括 Google、Apple、Samsung、Amazon 等，是 Matter 协议的理想底层传输。

在 SmartThings 体系里，Thread 的角色可以概括为：Thread 设备通过 Thread Mesh 网络相互通信，Thread Border Router（Hub V3 就是）负责把 Thread 网络桥接到家里的 WiFi/IP 网络，SmartThings App 通过 IP 网络与设备通信。Thread 设备之间可以自行中继，不需要 Hub 直接射频通信，这让 Mesh 网络的覆盖能力大幅提升。

### 三大平台定位对比

| 维度 | SmartThings | Apple HomeKit | Home Assistant |
|------|-------------|---------------|----------------|
| 目标用户 | 三星家电用户 + 普通消费者 | iPhone/iPad 全家桶用户 | 极客 / DIY 爱好者 |
| 技术门槛 | 零 | 低 | 高（配置文件、Docker） |
| 生态锁定 | 弱（三星 + Matter 开放） | 强（Apple 认证设备） | 无（几乎任何设备） |
| 本地控制 | 有限 | 有限 | 全本地 |
| 设备支持 | 4500+（380+ 品牌） | ~400（Apple 认证） | 上万 |
| Hub 必须？ | 多数场景需要 | 多数场景需要 | 不需要 |

选型建议很直接：

- 有三星 TV / 冰箱 / 洗衣机 → SmartThings 是必须的，没有其他平台能完整调用三星家电的专属 API
- 全家 iPhone/iPad/Mac → HomeKit 系统级集成最好
- 爱折腾、会装树莓派/NAS、追求全本地控制 → Home Assistant
- 以上都沾一点 → SmartThings + Home Assistant 共存，通过 Matter 协议互通

---

## 🔗 相关资源

- [SmartThings官网](https://smartthings.fedq.ren)
- [SmartThings文档](https://smartthings.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/SmartThings)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/SmartThings)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/SmartThings)

---

## 📚 相关文章

- [SmartThings 上手一周，说说三星把智能家居这件事做对了什么](https://laoguosofts.github.io/posts/smartthings.html)
- [SmartThings 上手两周：从一脸懵到全屋自动的真实体验](https://alipymanbu.github.io/posts/smartthings.html)
