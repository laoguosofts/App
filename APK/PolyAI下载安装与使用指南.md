## ✨ PolyAI 是什么：企业级对话 AI 平台

PolyAI 是一个面向企业客服和销售场景的对话 AI 平台，核心能力是让 AI 智能体通过电话、Webchat 等渠道与客户进行多轮自然对话，并在对话过程中实时访问业务系统数据、执行操作、转接人工。和市面上常见的通用聊天机器人不同，PolyAI 从设计之初就是为生产级客户服务场景打造的——它内置了语音识别与合成、多步对话编排（Flow）、CRM 深度集成、三级环境隔离、审计日志等一整套企业级基础设施。

平台的核心组件包括 Agent Studio（无代码/低代码的可视化配置界面）、对话管道（实时处理语音转写、实体提取、LLM 推理）、Connect Portal（管理外部系统集成和密钥）、Conversations 面板（逐通对话的完整轨迹与日志）。它支持 130+ 预构建的系统集成，覆盖 Salesforce、Zendesk、ServiceNow、HubSpot 等主流 CRM 和工单平台，也可以通过 HTTP API、Python 转场函数或 MCP 协议做自定义集成。

PolyAI 适合有以下诉求的团队：想要把高频重复的客服电话交给 AI 处理、希望在 AI 对话中实时查询客户数据、需要 AI 自动创建工单或更新 CRM 记录、或者需要搭建一条从 Sandbox 测试到 Live 生产的安全发布流水线。它不是一个单机软件，而是以 SaaS 形态交付——注册账号后创建项目、配置 Flow、接入渠道电话号码即可开始。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://www.fedq.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：PolyAI 为云端 SaaS 平台，无传统客户端安装包；注册账号后通过浏览器访问 Agent Studio 即可使用，也支持 iOS / Android 端的内部测试 App

---

## 🔧 快速上手：从注册到接通第一通电话

PolyAI 不需要传统意义上的"安装"——它是全云端 SaaS 服务，整个上手过程更接近"注册 → 配置 → 发布"而非"下载 → 安装 → 激活"。以下是完整流程。

### 第一步：注册账号与创建项目

1. 访问 PolyAI 官方网站注册企业账号，完成邮箱验证和企业信息填写
2. 登录后进入 Agent Studio，点击"Create Project"创建第一个项目
3. 选择项目默认语言和时区（后续可随时调整）
4. 项目创建完成后默认处于 **Sandbox** 环境——这是 100% 安全的 playground，所有改动自动保存、不连接任何真实电话号码，随便改随便试都不会影响生产

> [!TIP]
> 新项目默认只有 Sandbox，Staging 和 Live 需要在 Connect Portal 里单独开启。**永远不要直接在生产环境上改配置**——所有构建和测试都在 Sandbox 完成，Staging 做验收，Live 只处理真实客户流量。

### 第二步：配置渠道接入

PolyAI 支持多种渠道，最常见的是电话（Twilio、AudioCodes 等 SIP 中继）和 Webchat：

- **电话渠道**：在 Connect Portal → Channels 里添加电话号码提供商，填入 API 凭证，完成号码绑定。Sandbox 环境不需要真实号码，用浏览器内通话就能测试。
- **Webchat**：在 Agent Studio 里生成一段 iframe 嵌入代码，复制到你的网站页面即可。

### 第三步：选择发布环境

| 环境 | 用途 | 数据 | 电话号码 |
|---|---|---|---|
| **Sandbox** | 所有构建和测试的起点 | 独立副本，改动随时可丢弃 | 无（浏览器内通话） |
| **Staging** | 预发布验收测试 | 独立副本 | 独立测试号码 |
| **Live** | 生产环境 | 独立副本 | 生产号码 |

三个环境底层数据完全隔离——Sandbox 里删光所有配置也不会影响 Live。环境切换在 Agent Studio 顶部的下拉框里一键完成，切换后每个环境的配置独立展示，一眼就能看出"哪个环境比哪个环境多了什么"。

---

## 📖 使用入门：Flow、CRM 集成与自定义能力

PolyAI 的核心工作方式是 **Flow 驱动的多步对话 + 外部系统实时联动**。下面从四个维度展开：Flow 对话编排、CRM/工单系统集成、自定义集成（HTTP/Python/MCP）、以及常见故障的排查思路。

### Flow：多步对话的核心编排

Flow 是 PolyAI 描述"AI 应该怎么跟客户一步步对话"的核心概念——每一个 Flow 都是一棵有向图，由 Step 和 Edge 组成。常见的 Step 类型包括：

- **Collect Step**：向客户提问并提取特定实体（比如"请问您的订单号是多少"→ 提取 `order_id`）
- **Route Step**：根据条件走不同分支（比如 Route 后判断库存是否足够 → 走"有货发货"Edge 或"缺货推荐替代"Edge）
- **API Step**：调用外部系统（CRM、ERP、自有 API）获取数据或执行操作
- **Handoff Step**：转接人工坐席

在 Agent Studio 里拖拖拽拽就能搭出一个 Flow——新建 Flow → 添加 Step → 用 Edge 连接 → 给每个 Step 写 Prompt 和实体配置。写完后直接在 Sandbox 里用浏览器内通话测试，**不需要 Publish 就能在 Sandbox 里验证**，实时生效。

> [!NOTE]
> Flow 的调试神器是 Conversations 面板。每通走了 Flow 的对话都会显示完整的执行轨迹——哪一步成功、哪一步实体提取失败、哪一步 API 返回了什么，一目了然。排查问题时先打开对应对话的轨迹，定位到卡住的那一步再针对性修复。

### 与 CRM / 工单系统集成

没有 CRM 集成的 PolyAI 智能体就是一个"不知道你是谁"的工具——客户打进来，它不知道你是哪位客户、历史订单是什么、上次联系是什么时候。接上 CRM 之后，智能体可以在对话里：

- 自动识别客户身份（基于来电号码或对话中提供的账号）
- 实时查询账户余额、订单状态、历史工单
- 对话结束后自动创建工单（带完整对话摘要）
- 根据客户分层（VIP vs 普通客户）走不同的服务路径

以 Salesforce 为例，配置流程是：

1. 确保 Salesforce 有 API 访问权限（Enterprise / Unlimited 版默认开启）
2. 在 Salesforce 里创建一个 Connected App，获取 Consumer Key 和 Secret
3. 在 PolyAI Connect Portal 里点 Salesforce → OAuth 授权 → **字段映射**

字段映射是整个集成最关键的环节——告诉 PolyAI "你从 Salesforce Account.Name 字段取值，在对话里用变量 `{{crm.customer_name}}` 引用"：

| PolyAI 变量 | → | CRM 字段 |
|---|---|---|
| `{{crm.customer_name}}` | ← | Salesforce Account.Name |
| `{{crm.account_balance}}` | ← | Salesforce Account.Balance__c |
| `{{crm.open_tickets}}` | ← | Salesforce Case（Status = Open）计数 |

映射完成后，你在 Flow Step 的 Prompt 里就可以自然地引用这些变量——比如欢迎语里写"您好 `{{crm.customer_name}}`，很高兴再次为您服务"，智能体自动填上客户的真实名字。

除了 Salesforce，Zendesk 和 ServiceNow 也是高频集成目标：Zendesk 适合工单管理场景（智能体处理不了的问题自动创建 Ticket 并附带对话摘要），ServiceNow 适合 IT 服务管理（ITSM）场景（查询 Incident / Problem / Change 状态、自动创建 IT 故障单）。所有 CRM 集成都走 OAuth 2.0，Token 加密存储、自动刷新，数据只在该客户的 PolyAI 集群内处理。

### 自定义集成：HTTP API、Python 转场函数与 MCP 协议

当 130+ 预构建系统覆盖不了你的业务时，PolyAI 提供三种灵活的自定义集成方式，复杂度递增、灵活度也递增：

| 方式 | 复杂度 | 适合场景 |
|---|---|---|
| **HTTP API 端点** | 低 | 你有一个 REST API，PolyAI 可以直接调用 |
| **Python 转场函数** | 中 | 需要复杂逻辑、参数处理、多 API 串联 |
| **MCP 协议** | 高 | 需要同时给多个 AI 工具暴露你的系统能力 |

**HTTP API 端点**是最常见的方式——在 Python 转场函数里用 `requests` 库直接调你的 REST API，PolyAI 的 Python 执行环境允许标准库和 `requests`，但有硬性安全边界：只能访问你在 Connect Portal 里配置的白名单端点、执行超时 5-10 秒、无文件系统和子进程权限。

**Python 转场函数**适合需要复杂逻辑的场景——比如身份验证要先调 CRM 查客户存在性、再调银行 API 验证卡号、再调风控 API 查欺诈历史，三个 API 串联后的结果决定 Flow 走哪个分支，这用无代码配置做不到，必须用 Python。ADK 把 Python 函数作为独立 `.py` 文件管理，push 到 PolyAI 后在对话管道里执行。

**MCP（Model Context Protocol）协议**是最开放的方式——Anthropic 推动的开放协议，让 AI 模型以标准化方式访问外部工具和数据。PolyAI 原生支持 MCP，好处是一次实现多处复用：你写一个 MCP Server 暴露业务系统能力（查询订单、修改库存等），PolyAI、Claude Desktop、Cursor、VS Code Copilot 都能通过同一个 Server 访问你的系统，不用为每个 AI 工具单独写集成。

> [!WARNING]
> 无论用哪种自定义集成，API Key、密码等敏感信息都**不要硬编码**在 Python 函数里——用 PolyAI 的 Secrets Management 功能安全存储，在代码里用占位符引用即可。

### Flow 卡住的常见排查思路

Flow 启动了但在某个 Step 上反复重试、或者 Route Step 走错了分支、或者用户问了杂项问题 Flow 直接退出——这类故障的排查路径分三步：

**第一步：看 Conversations 里的完整 Flow 轨迹。** 每通走了 Flow 的对话都有完整的执行日志，类似这样：

```
[启动 Flow: Reservation]
  Step 1 (Collect people): 用户说 "4 个人" → 实体提取: people_count = 4 → 成功
  Step 2 (Collect date): 用户说 "明天" → 实体提取: date = 2026-10-14 → 成功
  Step 3 (Collect time): 用户说 "晚上" → 实体提取: time = ??? → 失败
  系统重试 Step 3...
  用户说 "8 点" → 实体提取: time = 20:00 → 成功
  Step 4 (Route availability): 调用 API → 返回 "available=false" → 走 Edge: !available
```

定位到卡住的那段轨迹，再对症修复。

**第二步：三类典型故障的修复方法。**

| 故障类型 | 现象 | 常见原因 | 修复 |
|---|---|---|---|
| 实体提取失败 | Collect Step 反复重试 | ASR 把"3 点"识别成"3 店" | 给 Collect Step 配 ASR Biasing，把目标读音优先匹配 |
| Route 走错分支 | Edge 条件太宽泛 | Edge 条件写的是 `response contains "yes"`，但用户说"嗯"也触发 | 用更精确的 Edge 条件，或加一个"不确定"的兜底 Edge |
| Flow 跑偏退出 | 用户问了 Flow 没预料到的杂项 | Step Prompt 太死板或没嵌 RAG | 开启 Flow Settings 里的 "Allow knowledge retrieval within flow"，改 Prompt 允许灵活回应 |

**第三步：在 Sandbox 里用完全相同的对话内容重测。** 最好直接复制那通失败对话里用户说的每句话，确保复现条件一致。修改 Flow 后 Sandbox 实时生效，不需要 Publish。

---

## 🔗 相关资源

- [PolyAI官网](https://www.fedq.quest)
- [PolyAI文档](https://polyai-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/PolyAI)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/PolyAI)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/PolyAI)

## 📚 相关文章

- [把电话那头的客服换成 AI，我试了一周 PolyAI](https://laoguosofts.github.io/posts/polyai-docs.html)

- [花了一周把 PolyAI 的 Studio 摸了个遍，说点真实感受](https://alipymanbu.github.io/posts/polyai-docs.html)
