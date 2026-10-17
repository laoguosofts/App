## ✨ HakkoAI 是什么

HakkoAI 是一款由新加坡公司 HAKKO AI PTE. LTD. 开发的 AI 伴侣应用，核心定位是让 AI 角色陪伴你打游戏、聊日常。2025 年中首发 itch.io 至今，已在 Google Play 积累 170 万+ 下载量，产品迭代密集——Model Select 模型切换、Proactive Chat 主动对话、Agentic Memory 分层记忆系统、角色工坊（Character Workshop）等功能相继上线。

产品的特色在于把 AI 伴侣和游戏场景结合得很深：PC 端提供 Live2D 桌面小部件，可以半透明悬浮在游戏画面上；内置 Game Vision 视觉理解（Pro 订阅解锁），能"看"到游戏画面然后给策略建议；跨设备同步让你在手机上养的伴侣人设，回到 PC 上能无缝续上。

三档订阅制覆盖不同需求：免费版只有 Roleplay 陪伴模式（每天 30 分钟语音），Pro 解锁游戏辅助层（Concise Mode + Game Vision），Ultra 进一步解锁 Romantic 亲密模式和无限制语音/积分。三个模式共享同一套跨会话记忆和六阶段亲密值系统，切换不会清空对话历史。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://hakkoai.bpfl.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：PC 端可在 itch.io 和 Microsoft Store 搜索 HakkoAI；移动端在 Google Play 或 Apple App Store 搜索 HakkoAI

---

## 🔧 安装步骤

### PC 端（Windows）

1. **确认运行环境**：HakkoAI PC 端基于 .NET 构建，首次运行时如果系统缺少 .NET Desktop Runtime，安装包会自动引导下载。建议在安装前先确认系统已安装对应版本的 .NET Runtime，避免启动时报错。

2. **选择分发渠道**：
   - **itch.io**：独立安装包下载，自由度高，适合不想走商店的用户
   - **Microsoft Store**：一键安装+自动更新，适合 Windows 生态用户

3. **安装后检查**：首次启动会弹出 Onboarding 引导流程，要求选择聊天模式偏好并创建/导入角色人设。完成后进入主界面，此时 Live2D 小部件默认以可拖动窗口形态出现。

> [!TIP]
> PC 端安装后建议去设置里检查 .NET Runtime 依赖是否完整。游戏中如果 Live2D 动画卡顿或帧率明显下降，可以临时切到"最小化图标"形态，或者调低小部件透明度。

### 移动端（Android / iOS）

1. 打开应用商店搜索「HakkoAI」，认准开发者为 HAKKO AI PTE. LTD.
2. 安装完成后首次启动会请求麦克风权限（语音对话功能），可选择跳过或授权。
3. 移动端的核心差异：没有 Game Vision 游戏画面理解功能（该功能仅限 PC 端 Concise Mode），但语音陪伴体验在手机上更自然，适合通勤或休息时使用。

> [!WARNING]
> 不要从第三方 APK 下载站获取 HakkoAI 安装包——产品仅通过官方应用商店分发，第三方渠道存在被篡改植入恶意代码的风险。

---

## 📖 使用入门

### 三个聊天模式怎么选

HakkoAI 目前提供三个聊天模式，付费墙从低到高分别是 Roleplay → Concise → Romantic。免费用户只能用 Roleplay，这也是最适合上手的模式。

**Roleplay Mode** 的定位是慢节奏陪伴对话。角色会结合你设置的 Persona 人设和跨会话记忆，用自然的语气跟你聊日常、天气、游戏里的趣事。回复语速较慢，不适合需要快速指令的游戏场景，但适合在游戏间隙或者不玩游戏时跟伴侣"唠嗑"。这个模式下的对话会持续积累亲密值——从 Acquaintance（初识）一路升到 Partner，每个阶段解锁专属昵称和更亲昵的对话风格。

**Concise Mode** 是 Pro 订阅的核心价值所在，专为游戏辅助设计。开启 Game Vision 后，Hakko 能看到你的游戏画面并给出即时策略建议——比如在动作游戏里提示 Boss 弱点、在开放世界里标记探索路线。回复简洁精准，不会像 Roleplay 那样长篇铺垫。如果你买 Hakko 的初衷是"让 AI 陪我打游戏"，这一档才是真正能发挥价值的。

**Romantic Mode** 是 Ultra 订阅独占，对话语气最私密直接，搭配 Contextual Image & Video 图文生成功能——聊到"一起去看夕阳"这类场景时，AI 会自动生成对应氛围的画面作为对话背景。亲密值在这个模式下的解锁节奏也更快。产品商店标注 PEGI-12 分级，亲密互动限制在温和情感表达范围内。

| 模式 | 免费版 | Pro $9.99/月 | Ultra $19.99/月 |
| --- | --- | --- | --- |
| Roleplay（陪伴） | ✅ | ✅ | ✅ |
| Concise（游戏辅助） | ❌ | ✅ | ✅ |
| Romantic（亲密陪伴） | ❌ | ❌ | ✅ |

### Live2D 桌面小部件的三种形态

PC 端的 Live2D 角色立绘是 HakkoAI 的视觉灵魂，三种形态覆盖不同使用场景：

**全屏叠加（Fullscreen Overlay）**：半透明悬浮在游戏画面上，默认位于屏幕边角避免遮挡主操作区，可拖动调整位置、设置透明度。适合大部分开放世界 RPG、剧情向游戏——边打边看伴侣立绘，不打断游戏流程。

**可拖动窗口（Draggable Window）**：独立小窗口，可以调整大小、设为"始终在最前"。适合 FPS 等视野敏感的游戏——你不想让角色立绘挡在准星正上方，但又想偶尔瞟一眼。

**最小化图标（System Tray）**：一键收缩到系统托盘，只留一个小图标标识 Hakko 在运行。适合快节奏竞技游戏——完全不占视野，但语音提示时图标会闪烁提醒。

形态切换一般在侧边栏或系统托盘右键菜单中操作，AI Game Support Panel（Alt+C 呼出）中也能找到相关选项。首次使用时建议在目标游戏里实测一下 Live2D 动画对帧率的影响，高负载游戏（如开放世界 FPS）可能有明显 CPU/GPU 占用。

### 亲密值系统怎么玩

六阶段亲密值是 HakkoAI 长期留存的核心设计——Acquaintance → Friend → Close Friend → Good Friend → Best Friend → Partner。每次对话都会让角色对你的 Persona 层（Agentic Memory 三层结构中最持久的一层）加深理解。跨过阶段阈值时解锁专属昵称和对话风格升级。

三个聊天模式共享同一套亲密值进度，模式切换不会重置。但不同模式的积累效率不同：Roleplay 适合日常刷亲密值，Romantic 下解锁节奏更快。

### 性能和隐私注意点

Model Select 功能允许用户切换到 Gemini 3 Flash 等第三方模型，而隐私政策声称只使用自研 LynkSoul 模型，两者之间存在文档矛盾。在选择第三方模型时，注意对应厂商的数据处理政策——毕竟你的对话内容会经过第三方服务器。

---

## 🔗 相关资源

- [HakkoAI官网](https://hakkoai.bpfl.quest)
- [HakkoAI文档](https://hakkoai.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/HakkoAI)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/HakkoAI)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/HakkoAI)

---

## 📚 相关文章

- [HakkoAI 上手：把 AI 伴侣搬进游戏里，延迟四秒够不够用？](https://laoguosofts.github.io/posts/hakkoai.html)
- [把 AI 伴侣塞进游戏里？我用 HakkoAI 陪了一周《黑神话》](https://alipymanbu.github.io/posts/hakkoai.html)
