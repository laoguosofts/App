## ✨ OpenClaw 是什么

OpenClaw 是一个运行在你自己电脑或服务器上的个人 AI 网关。它把大模型能力接进你每天用的聊天工具里——Telegram、Discord、Slack、WhatsApp、Signal 等主流渠道都有官方或社区插件——让你随时随地发消息就能调用 AI，而不用在浏览器里反复打开托管服务的网页。

它的核心定位是「自托管」：数据存在你本地的 Markdown 文件里，模型提供商覆盖三十多家（从云端主流厂商到本地 Ollama 都能混搭），计费结构跟着你实际消耗走——没有月费订阅，用多少付多少。更重要的是，工具和权限边界由你自己划定：可以让它跑本地命令、开浏览器抓内容、读写文件、调用 ClawHub 上的第三方技能，这些都是托管式 AI 助手在沙盒里碰不到的领域。

适合谁用？官方文档把目标人群描述得很清晰：希望拥有一个可以随时随地发消息的个人 AI 助手，同时不放弃对自己数据的控制，也不依赖托管服务的开发者和高级用户。如果你愿意用一次部署换长期控制权，OpenClaw 就是为你准备的；如果你只想在手机上偶尔问几个问题、完全不关心数据在哪，那托管式助手可能更省心。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://openclaw.fewf.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 命令行安装：macOS 用 Homebrew（`brew install openclaw/tap/openclaw`），Linux 支持 apt、yum 和二进制包，Windows 提供原生安装器

---

## 🔧 安装与初始化

### 跨平台安装路径

OpenClaw 提供了比较完整的跨平台支持，官方文档明确覆盖了 macOS、Linux 和 Windows 三大桌面环境，以及 Android/iOS 作为移动端节点。安装方式分两种：**包管理器一键装**适合已经习惯终端的用户，**原生安装器**适合不想碰命令行的场景。

```bash
# macOS / Homebrew
brew install openclaw/tap/openclaw

# Linux / curl 一键脚本（官方推荐路径，脚本会自动识别发行版）
curl -fsSL https://get.openclaw.ai | bash

# Windows：直接下载 .exe 安装器，双击运行即可
```

> [!NOTE]
> 安装脚本默认把可执行文件放到 `/usr/local/bin` 或系统 PATH 目录，装完后在终端里跑 `openclaw --version` 能输出版本号就说明路径配置正确。

### 首次启动：workspace 与配置文件

OpenClaw 不强制全局配置——它以 **workspace（工作区）** 为单位管理智能体，每个 workspace 是一个独立目录，里面放配置、技能、会话历史和状态数据。第一次启动时需要先初始化一个 workspace：

1. `openclaw init` 在当前目录生成骨架，自动创建 `config.yaml` 和基础目录结构。
2. 打开 `config.yaml`，先填模型提供商的 API Key——官方文档建议「为获得最佳质量与安全性，请使用最新一代中能力最强的模型」，工具型智能体对模型能力敏感，用太弱的模型会明显感觉它「变笨」。
3. 运行 `openclaw doctor` 做首次健康检查：它会验证 API Key 有效性、端口占用、磁盘空间和权限配置，给出一份诊断报告。

> [!WARNING]
> 不要跳过 `doctor` 直接启动服务——API Key 错、端口被占、权限不足这些问题在运行时才暴露的话，排查成本会高很多。

### 进程保活与安全基线

OpenClaw 是一个长驻进程，进程挂了就是挂了——官方文档建议用系统级守护机制保活：macOS 用 launchd、Linux 用 systemd、Windows 用任务计划程序或 nssm。上线前过一遍安全基线三件事：**收紧工具**（关掉不需要的内置工具）、**限制私信**（启用配对/白名单机制）、**群组必须提及**（防止群聊里被随便触发），三件事都做再开放给同事或公开群。

---

## 📖 使用入门

### 接入聊天渠道：让它成为你手机里的助手

安装只是第一步，真正让 OpenClaw 用起来的关键是**把它接进你每天用的聊天工具**。官方插件覆盖了多个主流渠道，以 Slack 为例，接入路径是四个步骤：建工作区应用、装插件、写配置、批准私信。

```bash
# 从 ClawHub 官方安装 Slack 插件
openclaw plugins install clawhub:@openclaw/slack
```

配置层面需要在 `channels.slack` 块里填入 Bot Token，然后用 `channels` 键限定允许的频道列表——每个频道还可以单独设提及规则。Slack 的多人协作属性比 Discord 更强，群聊路由细节也更细：频道消息默认需提及，回复机器人的消息属于隐式提及但不会绕过发送者白名单，频道白名单和发送者白名单是两层独立检查。Telegram、Discord、WhatsApp、Signal 等渠道的接入思路类似，都从 ClawHub 装插件，各自的权限模型和白名单规则略有差异。

> [!TIP]
> 如果你只想自己用，建一个只有自己的私信会话就行，把 Slack 当作工作助手入口；想让团队共享的话，结合技能把内部文档变成它的知识源，在指定频道里回答「本团队的部署流程是什么」这类问题。

### ClawHub：社区技能的应用商店

OpenClaw 的生态核心是 **ClawHub**——公共技能与插件注册表，类比的话就是这个生态的应用商店。搜索、安装、更新、发布都围绕它，命令行四连就能搞定：

```bash
openclaw skills search "calendar"            # 按关键词搜
openclaw skills install @owner/slug          # 装进当前 workspace
openclaw skills install @owner/slug --global # 装进全局共享目录（所有本地智能体可见）
openclaw skills update --all                 # 全部更新
```

默认装到当前 workspace 的 `skills/` 目录，`--global` 会装到共享托管目录让所有本地智能体都能看到。也支持从 Git 仓库装（`git:用户名/仓库名@引用`）和本地目录装。值得一提的是**信任边界**：安装前后都可以跑 `openclaw skills verify @owner/slug` 向注册表请求该技能的信任边界报告，ClawHub 页面上还提供病毒扫描、静态分析等安全扫描详情。官方文档有一句加粗警告——把第三方技能当不受信任的代码对待，装之前先读内容。渠道插件（Discord、WhatsApp 等）同样从 ClawHub 分发，`openclaw plugins search / install clawhub:包名 / update --all` 是插件对应的命令。

想发布自己的技能也不复杂：本地准备好含 `SKILL.md` 的文件夹，安装独立的 `clawhub` CLI 并登录，然后一条命令发布：

```bash
clawhub skill publish ./my-skill --slug my-skill --name "My Skill" --version 1.0.0
```

发布时可以带语义化版本、更新日志与标签；插件发布用 `package publish` 命令，支持 `dry-run` 预演。任何人都可以上传，但发布者账号需要满足注册时长门槛，被扫描拦下的版本会从公共目录消失。

### 媒体能力：不只发文字的聊天助手

聊天助手不能只会发文字，OpenClaw 的媒体链路覆盖了**收、转、发、生成**四段。各渠道发来的图片、音频、视频、文档会规范化成统一信封交给智能体，模型可以看图、读文档、听语音。WhatsApp 等渠道的长语音可以自动转录成文字再进模型，这在海外场景下非常实用。支持情况因渠道而异——Telegram 和 Discord 这类 Bot API 完善的渠道最完整，部分渠道只保文本。

出站侧，智能体可以生成或引用文件后发回渠道，Markdown 图片语法在 Telegram 上会尽量转成真正的媒体消息。长回复自动分块、富消息格式（表格、清单等）在支持的渠道上可选开启，默认关闭以保证旧客户端兼容。

生成能力做成了**统一接口**，底层接 Fal、Runway、ComfyUI（本地）等提供商，配置里选谁就用谁：

| 能力类型 | 底层提供商举例 | 注意事项 |
| --- | --- | --- |
| 图像生成 | Fal、Runway、ComfyUI（本地） | 按量计费，常驻智能体建议设用量预算 |
| 视频生成 | Runway | 成本最高，慎用 |
| 音乐生成 | 多家提供商可选 | 按需开启 |
| 文本转语音 | Azure Speech、ElevenLabs、Deepgram | 手机节点可把回复直接朗读出来 |

媒体能力和**移动端节点**组合才有完整体验：Android/iOS 节点提供 camera 拍照与短视频命令，智能体可以主动请求镜头；手机端支持 Talk 模式连续语音对话，识别与合成走网关配置的提供商链；聊天里长按消息选择聆听，网关 TTS 不可用时还能回退到设备系统语音。

> [!NOTE]
> 媒体文件会持续增长占用磁盘空间，定期清理或把媒体目录挂到大盘是个好习惯。公开群里的媒体也是上下文的一部分，记得配合上下文可见性过滤，避免陌生人图片意外进提示词。

### 自托管 vs 托管式：五个维度的取舍

开篇就提到 OpenClaw 的核心选择是自托管，这里再把官方文档里反复出现的五个维度摊开对比一下，方便你判断到底适不适合自己：

| 维度 | 托管式 AI 助手 | OpenClaw |
| --- | --- | --- |
| 数据存放 | 服务商的云上 | 你的电脑/服务器，纯 Markdown 文件可随时搬走 |
| 模型选择 | 固定一家（或其联盟） | 35 个以上提供商 + 本地模型，按任务混搭 |
| 账单结构 | 月费/订阅 | 按实际消耗的模型 API 量计费，用多少付多少 |
| 能力边界 | 服务商划定的沙盒 | 自己决定工具、技能和权限，可以让它跑命令、开浏览器 |
| 可用性责任 | 服务商保障 | 自己保活，进程挂了就是挂了 |

换来这些自由要付的代价也很明确：**运维是自己的**（升级、重启、看日志、配 HTTPS），**安全是自己的**（能接进聊天软件也意味着配置失误时陌生人可能发消息），**模型质量自己兜底**（用太弱的模型会明显感觉变笨）。官方文档为此准备了诊断命令和安全加固基线，但决策始终在你自己手里。

---

## 🔗 相关资源

- [OpenClaw官网](https://openclaw.fewf.ren)
- [OpenClaw文档](https://openclaw-zh-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/OpenClaw)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/OpenClaw)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E8%BD%AF%E4%BB%B6%E4%B8%8B%E8%BD%BD/OpenClaw)

---

## 📚 相关文章

- [OpenClaw 版本进化史：一个个人 AI 网关的迭代节奏与取舍](https://laoguosofts.github.io/posts/openclaw-zh-docs.html)
- [OpenClaw 版本进化史：这只龙虾从 CLI 工具长成多渠道 AI 助手用了多久](https://alipymanbu.github.io/posts/openclaw-zh-docs.html)
