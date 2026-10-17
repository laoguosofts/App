## ✨ Meccha Chameleon——躲猫猫也能玩出 PVP 对抗味

Meccha Chameleon 是一款多人在线躲猫猫游戏，核心玩法简单：一部分玩家 Hider 变身成场景里的任意道具——书架、木板、瓷砖、雪地块都行——用喷漆和姿态把自己伪装成"场景的一部分"；另一部分玩家 Seeker 则在没有 minimap、没有雷达的情况下，**纯靠眼睛看光照异常和耳朵听方向**把 Hider 找出来。

游戏的乐趣点在于伪装的精度要求极高——不是随便找个角落蹲住就行，喷漆要匹配表面的 base 色、shadow 接缝、甚至窗光 band 的 value 差；Seeker 侧则要反过来，按 prop 簇 → seam → shadow strip → open wall 的优先级扫，盯着光照异常走。两边都有东西练，朋友局凑一桌下来不知不觉就能耗一个下午。

支持 Windows 平台通过 Steam 启动，也有安卓手游版本，适合想在碎片时间来上一局的玩家。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://meccha-chameleon.fewz.tech/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Steam 搜「Meccha Chameleon」获取 Windows 正版；Google Play / 各大安卓应用市场也可找到手游版本

---

## 🔧 安装步骤与环境准备

### Windows（Steam 版）

1. **确认硬件门槛**：游戏对 GPU 有一定要求，喷漆纹理渲染依赖显卡性能；显卡驱动建议更新到最新稳定版，否则可能出现喷漆贴图丢失、材质闪烁等问题。
2. **Steam 客户端安装**：启动 Steam，在商店搜索 Meccha Chameleon，点击"立即购买"完成支付后即可下载。默认安装路径为 C:\Program Files (x86)\Steam\steamapps\common\，如果 C 盘空间紧张可以在 Steam 设置 → 存储中手动调整。
3. **启动前的首次检查**：下载完成后先**不要急着进 Lobby**，先跑一次 Practice 模式（单人热身地图）确认喷漆笔刷能正常出颜色、Whistle 声音方向听得准。Practice 模式可以反复进出，不会消耗任何资源。

> [!WARNING]
> Windows Spatial Audio（3D 音频、虚拟环绕声、Dolby Atmos for Headphones）必须关掉——Seeker 侧完全依赖方向感定位 Whistle 和脚步声，开了 Spatial Audio 之后方向会被打乱，本来从左边来的声音变成"四面八方都有"，三角定位直接失效。

### 安卓版

- 安装渠道优先选 Google Play 或国内正规应用商店，避免第三方渠道的修改版本。
- 首次启动会请求存储和麦克风权限，麦克风是游戏内语音用的，存储是缓存截图和设置，两者都允许即可。
- 手机端建议戴耳机玩——外放的方向感极差，Seeker 侧几乎废一半。

### 文件完整性核对

下载完成后可以在 Steam 库 → 右键 Meccha Chameleon → 属性 → 已安装文件 → 验证游戏文件完整性，Steam 会自动比对本地文件和云端清单，缺失或损坏的会重新下载。手游版在应用商店也有类似的"检查更新"或"修复"入口。

---

## 📖 使用入门

### 角色分工

每局玩家被分成两队——Hider 和 Seeker，通常 1-2 个 Seeker 对 4-10 个 Hider。Hider 的目标是在 Hunt 阶段结束前不被发现；Seeker 则要在时限内找到所有 Hider。朋友局里两队经常互换，所以两套玩法都要会一点。

### Hider 侧：伪装才是王道

Hider 的核心操作是**喷漆**和**摆姿态（pose）**。开局有一段 Prep 时间（默认 60 秒），足够你选好躲藏位、喷上匹配的颜色、调整姿态。

#### 选躲藏位的优先级

| 级别 | 典型位置 | 为什么 |
|---|---|---|
| S 级 | 书架背景板、整块纯色墙面 | 一整块纯色，喷漆完几乎无破绽 |
| A 级 | 花纹墙、木地板、瓷砖 | 纹理多，hue 误差 ±15% 也不容易被扫出来 |
| B 级 | 阴影条带、接缝处 | value 差自然，混进去不显眼 |
| C 级 | 开阔墙面、纯白表面 | 容易被 value mismatch 直接暴露 |

> [!TIP]
> **白色表面是绝对陷阱**——雪地、纯白石膏墙这些看似"安全"的地方，游戏光源会让纯白产生细微的 value patch，你一喷 Seeker 10 米外就能看到一个"不完全白"的 patch。策略很简单：**避开任何大面积纯白表面**，找旁边的 prop 簇或 seam 躲。

#### 不同表面的喷漆要点

| 表面类型 | 采色重点 | 喷漆技巧 |
|---|---|---|
| 书架 | 背景板纯色 + 架子木头 base 色 | **不要采书脊**——五颜六色不准 |
| 木地板 | 木板中间 base 色 + 接缝 shadow | 两 pass：先 base 后 shadow strip，喷漆方向和木纹一致 |
| 砖墙 | 砖本体中间色 + 砖缝 grout | 砖和缝分别采，墙角阴影单独处理 |
| 瓷砖 | Tile 中间色 + Grout 缝 | 和砖墙同理，两 pass 必须分开喷 |
| 花纹墙 | 花纹中**最常见**的颜色 | 不要采花纹线条（太细太杂），高纹理噪声能容 hue 误差 |

#### 姿态（Pose）要点

pose 不是随便站一个就行——道具本身有固定的轮廓。比如一个 60cm 高的道具，旁边出现一个 30cm 高的 curl（你蹲下的轮廓）就很可疑。Seeker 会盯着肩膀线、pose curl 不对、站立 blob（家具上方不该有人）这些线索扫你。选好 prop 之后记得调整 pose 让轮廓尽量贴合。

### Seeker 侧：没有雷达，只有眼睛和耳朵

Seeker 是这款游戏里最吃经验的角色——没有 minimap、没有 live radar，全靠眼睛扫光照异常和耳朵听方向。Intel Pulse（HUD 上的密度提示）是唯一的粗粒度辅助，在 Hunt 中间隔触发，帮你锁定密度最高的区域。

#### 扫描顺序（按 Tier）

```
Hunt 0-30s：先冲 S 级 prop 簇 + seam（prop 密度最高的 wing）
Hunt 30-60s：扫完 S/A 级后再看 B 级 shadow strip
Hunt 中后段：prop 簇没人 → 扫 C 级 open wall（看 value mismatch）
Intel Pulse 时：立刻朝 Pulse 方向 sweep 那个 wing，不要等 Pulse 消失
```

> [!WARNING]
> **绝对不要蹲 spawn center**——新手 Seeker 常犯的错，Hunt 一开始就蹲在出生点正中央扫开阔区，Hider 连 Prep 都不会站那里。浪费 30 秒等于给 Hider 多 30 秒巩固伪装。

#### 光照异常（最可靠的线索）

Seeker 扫伪装时眼睛要盯着这些光照异常：

| 异常表现 | 什么情况 | 你的反应 |
|---|---|---|
| 哑光墙面上出现亮斑 | Hider 的喷漆 glossy 没调好 | 立刻开枪 |
| 顶灯照射的墙面有亮度差异 | value mismatch 被 ceiling cone 照出来 | 扫 cone 覆盖区域 |
| 金属道具旁边多了块反光 | Hider metallic 值调错 | 扫 prop 周边墙面 |
| 窗光 band 被"切断" | Hider 刚好卡在 band 上 | 扫断裂点 |

#### 音频配置

Seeker 对音频的依赖度超过 Hider——Whistle（Hider 主动发出的声音提示）、paint menu 点击声、脚步声的 material 变化，方向听对了可以直接定位 prop 簇，听反了浪费 10 秒。

| 设置 | 推荐值 | 原因 |
|---|---|---|
| Windows Spatial Audio | **必须关** | 方向感不能乱 |
| Master Volume | 70-80% | Whistle 和 footsteps 要听得清 |
| Gameplay Cues | **拉满** | Seeker 核心线索 |
| 输出设备 | 耳机 | 音箱方向感极差 |
| Discord | Push-to-Talk | Hunt 中后段尽量不说话，脚步声和 Whistle 会被盖过 |

### 房间规则配置

如果你是 Host，在 Lobby 里可以调整几组关键规则，不同规则组合决定了整局的节奏和强度：

| 规则 | 新手推荐 | 朋友局常规 | 高强度局 |
|---|---|---|---|
| Infection | OFF | OFF（5 局后开） | ON |
| Forced Taunt 间隔 | OFF | 45 秒 | 20 秒 |
| Whistle Hints | ON | 可选 | OFF |
| Player Cap | 4-6 | 6-10 | 10-12 |
| Prep Timer | 60s | 60s | 45s |

**Infection** 是被抓的 Hider 直接加入 Seeker 队——小房间（6 人以下）别开，Seeker 滚雪球太快 Hider 会绝望。**Forced Taunt** 是给 Hider 定时强制发噪音，OFF 就是纯 painting + pose 练习，新手教学局建议先关掉。**Whistle Hints** 控制 Seeker HUD 上能不能看到 Whistle 的方向提示，教学局开着让 Seeker 练三角定位，老手局关掉增加难度。

Host 的上行带宽很关键——Wi-Fi host 超过 4 人就容易 rubber-band（橡皮筋效应），有线网络 + Steam 官方配置的 private lobby 6 人流畅，中端 PC 能撑 8-10 人，12 人以上 home upload 基本扛不住。

---

## 🔗 相关资源

- [meccha chameleon官网](https://meccha-chameleon.fewz.tech)
- [meccha chameleon文档](https://meccha-chameleon-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/meccha%20chameleon)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/meccha%20chameleon)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/meccha%20chameleon)

## 📚 相关文章

- [周末跟朋友躲猫猫，我蹲在书架后面听着搜索者的脚步声离我三厘米又走远](https://laoguosofts.github.io/posts/meccha-chameleon-docs.html)
- [被朋友硬拉进躲猫猫局，我在一堵墙后面冻了整整六分钟](https://alipymanbu.github.io/posts/meccha-chameleon-docs.html)
