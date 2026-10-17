## ✨ Taskade 是什么

Taskade 是一款把任务管理、项目协作、AI Agent 和自动化工作流整合到同一个 Workspace 的智能协作平台。和 Notion 侧重文档、Asana 侧重任务分配不同，Taskade 的 DNA 有三层：Projects（结构化数据的载体）、Agents（AI 决策能力）、Automations（工作流执行引擎）——三层之间可以互相调用，形成闭环。

对于个人用户，它是比 Todoist 更强的全能待办；对于团队，它可以替代 Jira + Slack + 一部分 Zapier 的组合；对于开发者，它开放了 REST API 和 MCP 协议，能让 AI IDE 直接读取你的业务数据。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://taskade.usea.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 版在 Google Play、iOS 版在 App Store 搜索 Taskade；桌面端推荐官方安装包（比商店版稳定、支持企业分发）

---

## 🔧 安装步骤

### Windows 平台

从下载页获取 `.exe` 安装包后双击运行，默认安装到 `C:\Program Files\Taskade`，可以在安装向导里自定义路径。首次启动时 Windows Defender 实时保护可能会慢几秒（安全扫描），属于正常现象。

> [!WARNING]
> 某些第三方杀毒软件可能对 Electron 框架产生误报——如果遇到拦截，把 Taskade 安装目录加入白名单即可，**不要**从非官方渠道下载破解版或二次打包的安装包。

安装完成后系统托盘区会常驻 Taskade 图标，右键可以快速添加任务、查看今日待办概览，这才是真的"没退出"；关掉主界面窗口不等于退出，要退出必须右键托盘图标选退出。

### macOS 平台

下载 DMG 安装包，打开后把 Taskade 拖进 Applications 文件夹。macOS Gatekeeper 首次启动时可能弹出安全提示——打开"系统设置 → 隐私与安全"，在底部找到"仍要打开"点击确认即可。

> [!TIP]
> DMG 安装包同时支持 Apple Silicon（M 系列芯片）和 Intel 双架构，系统会自动选择对应版本，不需要分别下载。

### Linux 平台

Linux 是官方支持但"社区优先"的平台，推荐用 AppImage 方式——不需要 root 权限，不需要安装，双击就能跑：

```bash
chmod +x Taskade_*.AppImage
./Taskade_*.AppImage
```

也可以通过 Snap 安装：`snap install taskade`，Snap 会自动处理依赖和更新。

> [!NOTE]
> Wayland 桌面环境（GNOME Wayland、KDE Plasma Wayland）下全局快捷键可能不生效，这是 Wayland 的安全限制而非 Taskade 的问题。解决方案是切换到 X11 会话登录，或改用浏览器扩展的 Quick Add 功能。

### 跨设备同步

无论用哪个平台的客户端，数据都是实时同步的——在手机上添加的任务，桌面端毫秒内就能看到。离线时桌面端会把已加载的 Projects 缓存到本地 SQLite 数据库，断网期间可以继续编辑，恢复联网后自动合并到云端。

---

## 📖 Taskade 核心功能与进阶玩法

### 项目管理与视图

Projects 是 Taskade 的基本工作单元，每个 Project 可以切换多种视图：List（列表）、Board（看板）、Calendar（日历）、Table（表格）、Timeline（甘特）等——不需要为不同场景建不同项目，切换视图就行。字段类型比 Notion 更丰富：除了文本、数字、日期，还有成员分配、标签、下拉选项、关联其他 Project 的跨表引用。

### AI Agent：让 AI 帮你干活

Agent 是 Taskade 的大脑。每个 Workspace 可以创建多个 Agent，每个 Agent 有自己的 System Prompt、知识库和工具权限。Agent 可以：

- 读取和写入 Projects 的数据（比如自动整理新工单、分类客户反馈）
- 被 Automations 调用作为决策节点（比如判断 Lead 评分是否达标再决定跟进策略）
- 和用户对话（作为 Genesis 应用里的聊天机器人）
- 通过 MCP 协议暴露给 Cursor、VS Code 等 AI IDE 直接调用

### Durable Automation：可靠的自动化引擎

Taskade 把自己的自动化引擎叫 Durable Automation——和 Zapier、Make 同类但定位不同：每个步骤的输入输出都完整记录，失败后可以精确到某一步重试，不需要整条流程从头跑。

一个 Automation 的基本结构是 **触发器 + 动作序列 + 可选的条件分支和循环**：

**触发器**决定"什么时候启动"，常用的有：
- 项目数据变化（新记录创建、字段值改变、满足筛选条件）
- 定时调度（每小时/每天/自定义 CRON 表达式）
- 表单提交（Genesis 应用里用户填表后触发）
- Webhook（外部系统 POST 请求触发，适合和自研系统对接）
- Mailhook（给 Taskade 的专属邮件地址发邮件，附件自动存到 Media Files）

**动作**按顺序执行，涵盖：Projects 操作（创建/更新记录）、100+ 外部集成（Slack、Gmail、GitHub、Stripe、HubSpot、Mailchimp 等）、Agent 调用、AI 决策节点、数据处理（变量赋值、文本提取、过滤映射）、通知推送。

**条件分支**让 Automation 具备 if/else 逻辑，比如"新工单进来 → Agent 分析优先级 → 紧急的 Slack @负责人，一般的自动回复常见问题"。**循环**用来批量处理数据，比如"每天抓 CRM 的到期合同 → 循环每条 → 调 Billing Agent 生成续费通知"。

> [!TIP]
> Automation 的运行日志非常详细——每个步骤的输入、输出、状态、失败原因都能看到。遇到故障时打开运行日志，定位到失败步骤，修复配置后可以从**那个失败步骤**精确重试，而不是整条流程重跑。

### 桌面端独有的生产力特性

网页版功能最全，但桌面端有几个网页版做不到的事情：

| 特性 | 说明 |
| --- | --- |
| 常驻后台 | 关掉浏览器窗口，Automations 还在跑、Agent 还在监听 Projects |
| 系统级通知 | 原生通知中心，截止日期临近、@提及、Automation 执行结果都能推 |
| 全局快捷键 | 任何应用里按 `Ctrl+Shift+A`（默认）就能 Quick Add，设置里可自定义 |
| 离线编辑 | 断网时继续改数据，恢复后自动同步 |
| 菜单栏/托盘 | macOS 菜单栏、Windows 系统托盘常驻图标，快速操作不用开主界面 |

macOS 还有 Touch Bar 支持、Dock 未完成计数、Apple Shortcuts 集成（可以用 Siri 创建任务），Windows 支持 Windows Hello 指纹登录。

### 开发者友好：MCP 协议与 REST API

如果你是开发者，Taskade 开放了完整的 MCP（Model Context Protocol）协议支持。MCP 是 Anthropic 发起的开放协议，让 Cursor、VS Code Copilot、Claude Desktop 等 AI IDE 能以标准化方式连接外部服务。

三种连接方式：

1. **Hosted MCP**（最简单）：Taskade 提供托管服务，Settings → Developer → MCP Server 里复制 Endpoint URL 和 Token，粘到 AI IDE 的 MCP 配置里就行
2. **Workspace Server**（本地）：官方开源仓库，部署到自己的服务器或本地开发机，数据不离开你的网络
3. **REST API**（最灵活）：完整的 Workspace 管理、Projects CRUD、Agents、Automations、Genesis Apps 端点，适合深度集成

连好之后，AI IDE 在帮你写代码时可以直接读取 Taskade 里的 Projects 字段结构、Agent 的 System Prompt、Automations 的运行历史——整个过程不需要你手动打开 Taskade 复制粘贴。

Genesis 应用还可以**双向同步到 GitHub**：前端代码、数据结构定义、Automations 配置推到你自己的 GitHub 仓库，两边改一边自动同步。Projects 数据也能导出成 Markdown、CSV 或 JSON，方便迁移到 Obsidian、Notion 或 Excel。

### Genesis：低代码构建与发布

Genesis 是 Taskade 的低代码应用构建器——用 Projects 做数据源、用 Agent 做逻辑、用 Automations 做流程，拼出一个完整的 Web 应用。发布时可以选择可见性：

| 可见性 | 适用场景 |
| --- | --- |
| 公开（Public） | 面向外部的门户、社区模板 |
| 密码保护 | 给临时访客看 Demo、外部合作伙伴填表 |
| 仅团队 | Workspace 内部工具 |
| 自定义域名 + SSO（Business+） | 企业级门户 |

分享方式灵活：直接发链接、iframe 嵌入任何网页、JS Widget 弹出面板（适合在博客放客服聊天框）。还内置了 App User 认证系统——每个 Genesis 应用自带注册/登录/忘记密码，不需要接 Auth0 或 Clerk。Business 及以上还能绑定自定义域名、内置应用分析（PV/UV、地域、设备、Agent 调用次数、用户留存）。

---

## 🔗 相关资源

- [taskade官网](https://taskade.usea.quest)
- [taskade文档](https://taskade.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/Taskade)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/Taskade)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E7%BD%91%E9%A1%B5%E7%89%88/Taskade)

---

## 📚 相关文章

- [用了 Taskade 两年才发现，这 5 个冷门设置我竟然一直没碰](https://laoguosofts.github.io/posts/taskade.html)
- [用了半年 Taskade 才发现的 4 个隐藏功能——原来它藏着一整套企业级暗门](https://alipymanbu.github.io/posts/taskade.html)
