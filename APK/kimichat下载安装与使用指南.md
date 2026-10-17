## ✨ Kimi 是什么——从聊天助手到能自主干活的 AI 智能体

Kimi 是北京月之暗面科技有限公司（Moonshot AI）开发的通用 AI 助手。和市面上大多数只能聊天回答问题的产品不同，Kimi 的核心设计理念从一开始就偏向 Agent 模式——让 AI 能自主规划并执行任务，而不只是被动地输出文本。

从 2023 年 10 月 Kimi Chat 发布、主打 20 万汉字超长上下文，到 2025 年推出 OK Computer Agent 模式、2026 年 K2.5 引入 Agent Swarm 集群能力（最多 300 个子智能体并行）、2026 年 4 月 K2.6 开源并升级至 4000+ 工具调用，再到 2026 年 7 月 K3 模型发布（2.8T 参数、全球首个开放式 3T 级模型、1M token 上下文、原生视觉、完整权重开放下载），Kimi 的演进路线清晰地指向一个方向：让 AI 从"回答问题的机器人"变成"能独立干活的智能体"。

Kimi 拥有完整的产品线矩阵：Kimi Chat 是基础对话与轻量任务入口，通用 Agent 负责多模态任务执行（建站、PPT、研究报告），OK Computer 是拥有虚拟电脑的全栈助理（仅限 Kimi Work 桌面端），Agent Swarm 支持大规模并行任务处理，Kimi Work 提供本地文件级桌面 Agent（Windows / macOS），Kimi Code 是编程专用 Agent（CLI / VS Code / Cursor / JetBrains），Kimi Claw 则是零部署云端自动化平台。

> [!TIP]
> 和普通聊天机器人的根本区别：当你切换到 K3 或 K3 Swarm 模型、提交一个复杂需求时，Kimi 会自动拆解任务、调用工具、执行步骤、处理异常、交付成果——整个过程你不需要一步步引导。

Kimi K3 采用原生多模态架构（不是外挂 OCR），意味着它从模型设计层面就理解图片、视频、PDF、Excel 等文件格式，而不是先转成文字再理解。这使得它能直接写 Excel 公式、生成 PPT VBA 代码、从设计稿生成前端代码。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://kimichat.uerg.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：移动端 App Store / Google Play 搜索「Kimi Chat」；桌面端 Windows / macOS 前往官网下载 Kimi Work 安装包；编程用户可安装 Kimi Code CLI 或 VS Code 插件

---

## 🔧 多平台安装指南

### 网页端（最简单，零安装）

直接在浏览器打开 Kimi 网页版即可使用全功能。无需下载任何客户端，适合临时使用或轻量任务。

### 移动端

在 App Store 或 Google Play 搜索「Kimi Chat」，点击安装后用手机号注册登录。移动端适合随时提问和轻量对话，但复杂的 Agent 任务（如 OK Computer）仍建议在桌面端完成。

### 桌面端 Kimi Work（Windows / macOS）

1. 前往 Kimi 官网下载 Kimi Work 安装包
2. Windows 用户双击 `.exe` 安装程序，按向导选择安装路径（建议保持默认）
3. macOS 用户打开 `.dmg` 文件，将 Kimi Work 拖入「应用程序」文件夹
4. 首次启动时，Kimi Work 会提示你挂载本地文件夹——**不要授权整个 C 盘或用户目录根节点**，只授权需要 Kimi 访问的具体文件夹（如项目文件夹、文档文件夹）

> [!WARNING]
> 安全提醒：Kimi Work 挂载的本地文件夹内容会在执行 Agent 任务时发送到云端做模型推理，但文件不会上传存储，推理完成后云端不保留原始文件。授权前确认目录里没有敏感文件（如未加密的密钥文件、个人隐私数据）。

### Kimi Code（编程专用）

Kimi Code 支持 CLI 直接调用，也提供 VS Code、Cursor、JetBrains 系列 IDE 的插件安装。CLI 安装方式：

- macOS / Linux：通过 Homebrew 或 curl 安装
- Windows：通过 winget 或直接下载二进制文件

IDE 插件安装：在对应的扩展市场搜索「Kimi Code」，点击安装后在 IDE 设置里登录 Kimi 账号即可使用。

---

## 📖 Kimi 实战入门——会话管理、Memory 记忆与安全边界

### Session 与 Turn：为什么应该给每个任务开新对话

Kimi 把会话（Session）和轮次（Turn）做了明确区分：Session 是一次完整的对话过程（点击"新对话"开始），Turn 是一次你问我答的交互。

| 概念 | 生活类比 | 技术定义 |
|---|---|---|
| Session | 一次完整的会议（从开始到结束） | 有明确起点和终点的完整对话 |
| Turn | 会议中的一轮发言（你说 → 我说） | 你的输入 + Kimi 的回复 |

理解这个区分至关重要。在同一个 Session 里，Kimi 会记住之前所有的内容——包括你的提问、它的回答、上传的文件、联网搜索的结果等。但如果在一个 Session 里堆太多不相关的任务（先写 3 万字小说，然后问税务问题），Kimi 会把小说里的人物名和税务表格搞混。

长 Session 还有一个隐性问题：虽然 Session 内所有内容理论上都在上下文里，但 Turn 数量太多（50+）后早期内容可能被 Kimi 自动压缩，上下文接近 token 上限（K3 是 1M token，约 75 万汉字）时最早的内容会被截断。

> [!NOTE]
> 实战习惯：**一个独立任务 = 一个新 Session**。切换主题时直接点"新建对话"，旧 Session 需要时可以回来查看，但不要在里面继续堆新任务。

如果确实有复杂的多步骤任务需要长 Session，可以在 20+ Turn 后主动让 Kimi 做一次阶段性总结，把早期内容浓缩成一个"记忆锚点"，减轻后续 Turn 的上下文压力。

### Memory 记忆功能——让 Kimi 跨 Session 记住你的偏好

Memory 是 Kimi 内置的智能记忆功能，和 Web Search、Fetch Tool 一样开箱即用。它的作用是让 Kimi 跨 Session 记住关于你的信息——你的偏好、习惯、常用技术栈、回答风格要求等。

Memory 的触发有两种方式：**自动触发**和**手动触发**。自动触发时，Kimi 会在对话中观察并记录稳定偏好（比如你每次都让回答简洁、总是用 Python + FastAPI、总是要中文回答）；手动触发则是你直接告诉 Kimi 要记住什么，比如"帮我记住：我做后端开发主要用 Go + Gin 框架，解释代码时优先用这两个技术栈举例"。

在 Kimi 设置里找到 Memory 管理入口，你可以查看所有已记录的条目、编辑、删除、手动添加或一键清空。

> [!TIP]
> 换设备后 Memory 仍然有效——它跟你的账号绑定，只要用同一个 Kimi 账号登录，无论什么设备都会同步。Memory 消耗 credits 吗？不消耗，是免费内置功能。

但 Memory 也有边界：敏感个人信息（手机号、地址、银行卡号等）Kimi 不应该记录，如果你担心可以手动检查 Memory 管理页面；一次性的临时任务（"帮我查一下今天北京天气"）不会让 Memory 记录；你明确说过"不要记住这个"的内容也不会被记录。如果 Kimi 记错了，直接说"等一下，我刚才说的不是这个意思，帮我修正 Memory"即可。

### 数据安全与隐私边界

理解 Kimi 处理数据的边界才能安全使用。

| 数据类型 | 存储位置 | 谁能访问 | 生命周期 |
|---|---|---|---|
| 对话历史 | Kimi 云端 | 只有你（登录后） | 长期保存，直到你主动删除 |
| 上传的文件 | Kimi 云端临时存储 | 只有你 + 模型推理进程 | Session 删除后清理 |
| Kimi Work 挂载的本地文件 | 你的电脑本地 | 只有 Kimi Work Agent 在执行时按你指令读取 | 本地文件始终在你控制中 |
| Memory 记录 | Kimi 云端 | 只有你 | 长期保存，直到你主动删除 |

对话历史存储在国内合规服务器、加密存储，Kimi 官方明确承诺**不会用于模型训练**。你可以随时删除单个 Session 或清空所有历史。上传的文件在解析完成后，云端只保留解析结果的文本表示（不是原始文件），Session 删除后所有临时数据清理。

**绝对不要在对话里明文贴 API Key、密钥、密码、银行卡号等敏感信息**。如果不小心贴了，立刻删除这个 Session，然后立刻到对应平台轮换密钥/修改密码——Kimi 的 Session 删除是云端删除，但密钥一旦泄露，光删 Kimi 里的记录不够。正确做法是用环境变量存密钥（本地 `.env` 文件），在 Kimi 里只说"读取我的环境变量 XX"，不要直接贴值。

> [!WARNING]
> 账号安全建议：用强密码 + 启用双重验证（手机号+验证码天然有双重验证）；不要共享 Kimi 账号——所有人能看到所有人的对话历史、Memory 里混入不同人的偏好、Session 删除互相影响、Credits 额度也共享。

处理完敏感任务后立即清理：把结果保存到安全位置 → 删除这个 Session → 检查 Memory 有没有不该有的记录 → 下次用新 Session 继续。

---

## 🔗 相关资源

- [kimichat官网](https://kimichat.uerg.quest)
- [kimichat文档](https://kimichat.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/kimichat)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/kimichat)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/kimichat)

## 📚 相关文章

- [用 Kimi 大半年，踩过的那些"看着简单其实有坑"的瞬间](https://laoguosofts.github.io/posts/kimichat.html)
- [用 Kimi 踩过的五个坑——别等 credits 烧光才反应过来](https://alipymanbu.github.io/posts/kimichat.html)
