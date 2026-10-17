## ✨ Home Assistant 是什么

Home Assistant 是一个开源的智能家居控制平台——把你家里所有智能设备（灯泡、传感器、插座、门锁、摄像头）统一接入一个控制面板，然后用自动化规则把它们串起来。它的核心优势在于**开放性**：不像米家或 HomeKit 那样是封闭生态，Home Assistant 可以接入几百种品牌的设备，从小米 Aqara、飞利浦 Hue、SONOFF 到宜家 Trådfri 都原生支持，而且接入之后你可以用 YAML 或者可视化编辑器写自动化，想让灯什么时候亮、空调什么时候开、门锁什么时候触发通知，全由你自己定。

对技术用户来说，Home Assistant 的吸引力还在于它的**自托管**属性——你可以把它跑在树莓派上、跑在 NAS 上、甚至跑在 Docker 容器里，数据全部存在自己的硬件上，不经过任何云厂商。配合 HACS（Home Assistant Community Store），还能安装社区开发的自定义集成和仪表盘卡片，功能边界几乎没有上限。入门门槛并不算高：Home Assistant OS 把系统层的复杂度都封装好了，插上树莓派、刷好系统、浏览器打开就能用。

适合谁用？家里有三件以上不同品牌的智能设备、觉得每个 App 单独控制太分散、想搞点自动化但又不想被某个厂商绑定生态的用户——Home Assistant 刚好踩在"比专业平台简单、比消费级 App 灵活"的那个甜点区。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://home-assistant.gltd.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Home Assistant Companion App（iOS / Android）—— 用来远程控制和接收推送通知；PC 端无原生客户端，用浏览器访问 Web 界面即可

---

## 🔧 Home Assistant 安装实操

Home Assistant 的安装路径不止一种，选择哪个取决于你手上有什么硬件、以及你愿意花多少时间折腾。下面按上手难度从低到高排序。

### 路径一：Home Assistant OS（最推荐新手）

Home Assistant OS 是官方提供的完整操作系统镜像，把整个系统栈（HassOS + Supervisor + Home Assistant Core）打包好，刷到硬件上直接用。

1. **准备硬件**：树莓派 4B（2GB RAM 起步，推荐 4GB）、16GB 以上 microSD 卡（Class 10 以上）、网线一根
2. **刷写镜像**：去官网下载对应型号的 Home Assistant OS 镜像文件，用 BalenaEtcher 或 Raspberry Pi Imager 写入 SD 卡
3. **首次启动**：SD 卡插树莓派、接网线、通电。等待 2~5 分钟，浏览器打开 `http://homeassistant.local:8123`
4. **创建管理员账号**：按向导填写用户名和密码，选择时区和位置，完成初始化

> [!TIP]
> SD 卡寿命有限，Home Assistant 数据库每天都在写（记录每个实体的状态变化历史）。建议用高质量的工业级 SD 卡，或者直接上 SSD（树莓派 4 支持 USB 启动）。

### 路径二：Docker 部署（有现成服务器或 NAS）

如果家里有一台常开的 Linux 机器、或者群晖 / 威联通 NAS，用 Docker 跑 Home Assistant 很方便：

1. **拉取官方镜像**：`docker pull homeassistant/home-assistant:stable`
2. **创建容器**：把宿主机的一个目录映射到容器内的 `/config`，这个目录就是你的数据持久化位置
3. **启动访问**：`docker run -d --name homeassistant -v /path/to/config:/config --net=host homeassistant/home-assistant:stable`，然后浏览器打开 `http://<主机IP>:8123`

群晖 NAS 用户可以直接在 DSM 的套件中心搜索 "Home Assistant"，有社区维护的套件包，点点鼠标就装好了。

### 路径三：Python venv（极客路径）

如果你已经有一台配置好 Python 环境的 Linux 主机，可以用虚拟环境方式安装——最灵活但需要处理更多依赖细节，适合想二次开发 Home Assistant Core 的用户。

### 安装后的首次检查

不管走哪条路径，装好之后先在浏览器里确认三件事：

1. **能打开 Web 界面**：`http://<设备IP>:8123` 能看到仪表盘
2. **能进入 Settings**：左侧边栏点 Settings → System，确认系统信息页面能正常加载
3. **能发现设备**：Settings → Devices & services，Home Assistant 会自动扫描局域网内可发现的设备（路由器、打印机、其他 IoT 设备）

> [!WARNING]
> 首次启动后立刻做两件事：启用管理员账号的双因素认证（Settings → People → 你的用户 → Security）、以及做一次快照备份（Settings → System → Backups → Create backup）。这两件事能帮你避开绝大多数新手踩坑场景。

---

## 📖 设备接入与自动化入门

Home Assistant 的真正价值不在于它本身，而在于它能把各种设备接进来、然后用自动化规则串起来。这一节从"怎么把设备接进来"开始，讲到"怎么写第一条自动化"。

### 接入 Zigbee 设备（ZHA）

Zigbee 是智能家居领域最常见的无线协议，小米 Aqara 传感器、飞利浦 Hue 灯泡、宜家 Trådfri 都是 Zigbee 设备。Home Assistant 内置了 **ZHA（Zigbee Home Automation）** 集成——Home Assistant OS 上是零配置的，插个 USB 适配器就能用。

**第一步：选适配器。** ZHA 支持绝大多数 Zigbee 适配器，但兼容性有差异：

| 适配器 | 芯片 | 适合场景 |
| --- | --- | --- |
| Home Assistant Connect ZBT-1 | CC2531 | 官方推荐，未来可刷 Thread 固件 |
| SONOFF ZBDongle-P | CC2652P | 性价比高，信号好，最常用 |
| SONOFF ZBDongle-E | EFR32MG21 | 支持 Thread / Matter，面向未来 |

> [!NOTE]
> 适配器务必用 USB 延长线插在主机外面，不要直接插树莓派——USB 3.0 会干扰 Zigbee 的 2.4GHz 信号，表现就是设备配对后频繁掉线。

**第二步：配对设备。** 插上适配器后，Settings → Devices & services 会自动弹出 ZHA 的配置提示，点 Configure → 选适配器串口路径 → 选芯片类型 → 创建网络。之后进入 ZHA 集成页，点 + Add device → Search for new devices，把你的 Zigbee 设备（灯泡、传感器等）恢复出厂设置（长按重置按钮 5~10 秒），设备就会自动被 ZHA 发现、命名、分配区域。

ZHA 接入后会自动为每种设备类型创建对应的实体：灯（`light.xxx`）、开关（`switch.xxx`）、传感器（`sensor.xxx` / `binary_sensor.xxx`）、门锁（`lock.xxx`）、窗帘（`cover.xxx`）、温控器（`climate.xxx`）。配对越近越好（首次配对建议不超过 3 米），之后 Zigbee Mesh 网络会通过其他充当中继的设备自动扩展覆盖范围。

> [!TIP]
> ZHA 和 Zigbee2MQTT（Z2M）是接入 Zigbee 设备的两大方案。Z2M 支持的设备范围更全（上千种 vs ZHA 的几百种），但需要额外装 MQTT Broker 和 Z2M 容器。新手从 ZHA 开始就够了，等你发现某台设备 ZHA 认不出来时再考虑换 Z2M。

### 理解事件总线

Home Assistant 的**事件总线（Event Bus）**是整个系统的心脏——所有组件之间的通信都通过事件流转。实体状态变化、自动化触发、服务调用，全都是事件。理解事件总线之后，你写自动化时就不会只停留在"点点点"的可视化层面。

事件总线采用发布-订阅模式：任何组件都可以发布事件，任何组件都可以订阅事件，发布者和订阅者互相不知道对方的存在。核心事件类型包括：

- **state_changed**（最重要）：任何实体状态变化时发出，包含变化前后的完整状态和属性。自动化里用得最多的 `state` 触发器，底层就是监听这个事件
- **automation_triggered**：每次自动化触发时发出，适合做"自动化的自动化"——比如统计某个自动化 1 小时内触发了多少次，超过阈值就发通知
- **homeassistant_started / homeassistant_stopped**：系统启动和停止时发出，适合做"启动后恢复某个状态"这类场景
- **custom**：社区集成可以发出自定义事件，比如小米人体传感器 2 分钟没检测到人时会发出 `xiaomi_aqara.movement_timeout`

在 Developer Tools → Events 页面可以实时监听事件——选 `state_changed` 点 Start Listening，然后手动开个灯或移动一下传感器，右边就会实时显示事件的完整 JSON。**这是调试自动化最有用的工具**——写自动化之前先监听一下，看实体实际发出的事件长什么样。

你甚至可以在自动化里**主动发出自定义事件**，让其他自动化监听：比如自动化 A 检测到主人回家，发出一个 `my_home_arrival` 事件；自动化 B 监听这个事件，然后触发语音播报"欢迎回家"。这样做的好处是自动化之间解耦，改任何一个都不影响另一个。

### 备份与恢复

Home Assistant 的配置和自动化会随着使用越积越多——一次 SD 卡损坏、一次大版本升级报错、一次误改配置文件，都可能让你一切从头再来。**定期备份是使用 Home Assistant 的基本纪律**。

Home Assistant OS 上最方便的方式是用系统自带的**快照（Snapshot）**功能：Settings → System → Backups → Create backup。快照会把整个实例打包成一个 `.tar.gz`，包括 config 目录（所有 YAML、自动化、仪表盘）、已安装的 App（Node-RED、ESPHome、Mosquitto 等）、Zigbee / Z-Wave 网络密钥、数据库（历史状态和事件日志）。

备份策略可以参考这个节奏：

| 场景 | 操作 |
| --- | --- |
| Home Assistant 大版本升级前 | **必须**先做一次完整快照 |
| 每周例行 | 自动快照调度（Settings → System → Backups → Schedule backups），选凌晨 3 点这种空闲时间 |
| 手动大改配置后 | 做一次快照再继续改，改坏了随时回滚 |

恢复快照有两种方式：全新安装 Home Assistant OS 时，浏览器向导里会提示上传快照文件，上传完自动恢复所有设备和自动化；运行中恢复则在 Backups 页面点 Restoring backups，Home Assistant 会覆盖当前配置并重启。**注意恢复快照是完全覆盖操作**，恢复前确认不需要保留当前状态。

进阶用户可以把 config 目录放进 Git 仓库做版本控制——改坏了用 `git checkout` 回退，换机器用 `git clone` 拉下来。记得把 `secrets.yaml`（存密码和 API Key）、`*.db`（数据库太大）、`deps/` 和 `.venv/` 放进 `.gitignore`，不要提交敏感信息。

### 远程访问方案

出门在外想控制家里的 Home Assistant、查看摄像头、让自动化知道"主人离家了"——这些都需要远程访问。四种主流方案各有优劣：

| 方案 | 难度 | 成本 | 隐私 | 推荐人群 |
| --- | --- | --- | --- | --- |
| Nabu Casa Cloud | 极低 | ~$5/月 | 高（端到端加密） | 不想折腾的新手 |
| Tailscale | 低 | 免费版够用 | 高（P2P 直连） | 有一定动手能力 |
| DuckDNS + Let's Encrypt | 中 | 免费 | 中 | 有公网 IP、愿意折腾 |
| 自建 WireGuard VPN | 高 | VPS 费用 | 最高 | 有 VPS + Linux 运维经验 |

**Nabu Casa Cloud** 是官方云服务——Settings → System → Nabu Casa → Join Home Assistant Cloud，选订阅、付款、自动配置完成。零端口转发、零 SSL 证书管理、自动 HTTPS，Home Assistant 给你分配一个 `xxx.nabu.casa` 域名，手机 Companion App 自动走 Cloud 访问。额外福利包括远程语音处理（比本地快）和 Alexa / Google Home 集成入口。费用是月付 ~$5 或年付 ~$50。

**Tailscale** 是 VPN 方案——在 Home Assistant OS 的 Apps 里装 Tailscale、登录账号、把 Home Assistant 节点加进你的 Tailnet，然后手机装同一个 Tailscale App 登录，**手机就可以用 Home Assistant 的内网 IP 直接访问了**（比如 `http://100.x.x.x:8123`）。优点是零端口转发、数据不经过第三方服务器（P2P 直连）、免费版支持 100 个设备；Companion App 的 External URL 也不用填，Tailscale 连上时自动走 VPN。

> [!WARNING]
> 无论用哪种远程访问方案，**一定要做两件事**：启用管理员账号的双因素认证（2FA）、设置一个足够长且复杂的管理员密码。如果用 DuckDNS + Let's Encrypt 把端口直接暴露到公网，登录页会面临爆破风险——弱密码的话几天内就可能被攻破。

### 写你的第一条自动化

Home Assistant 写自动化有两种方式：**可视化编辑器**（Settings → Automations & scenes → Create automation）和直接编辑 YAML。新手建议从可视化编辑器开始，等理解了触发器和动作的结构后再去看 YAML。

一个典型的自动化由三部分组成：

1. **触发器（Trigger）**：什么情况下启动这条自动化——实体状态变化、定时到点、事件发生、设备被操作等
2. **条件（Condition，可选）**：满足什么条件才执行动作——比如"只在夜间执行"、"只在有人在家时执行"
3. **动作（Action）**：做什么——开灯、关空调、发通知、调用某个服务等

举个例子："晚上 7 点，客厅人体传感器检测到人，自动打开客厅主灯"——这个场景对应的触发器是 `time`（每天 19:00）+ `state`（binary_sensor.motion_living_room → on），条件可以加一个 `time`（19:00 ~ 22:00 之间），动作是调用 `light.turn_on` 服务。

在可视化编辑器里把这些点出来，保存后 Home Assistant 会自动校验语法、激活自动化。之后你可以在 Developer Tools → Events 里监听 `state_changed`，手动触发一下看事件是不是按预期发出的——这就是事件总线帮你调试自动化的正确姿势。

---

## 🔗 相关资源

- [Home Assistant官网](https://home-assistant.gltd.quest)
- [Home Assistant文档](https://home-assistant-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/Home%20Assistant)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/Home%20Assistant)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/Home%20Assistant)

---

## 📚 相关文章

- [我用 Home Assistant 把回家那三分钟缩成了一个语音词](https://laoguosofts.github.io/posts/home-assistant-docs.html)

- [把 Home Assistant 嵌进我的每周设备巡检流程](https://alipymanbu.github.io/posts/home-assistant-docs.html)
