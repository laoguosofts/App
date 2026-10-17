## ✨ Dynadot 是什么

Dynadot 是一家成立于 2002 年的老牌域名注册商，总部位于美国加州，ICANN 认证注册机构。与 GoDaddy、Namecheap 等同行相比，Dynadot 的定位更偏向开发者和域名投资者——界面简洁无广告、价格透明、API 文档完善，长期以来在小众圈子里积累了不错的口碑。

核心能力围绕域名的全生命周期展开：域名搜索与注册、DNS 解析管理、Whois 联系人维护、免费 SSL 签发、域名停放与转发、Backorder 竞价、二级市场交易。对于个人站长来说，它是一个"够用且干净"的域名管家；对于批量操作的开发者来说，API Sandbox 沙箱测试环境让自动化脚本的调试成本降到了零。

Dynadot 支持 500+ 种顶级域名后缀，包括主流的 .com、.net、.org，以及 .io、.dev、.tech 等开发者友好的新后缀。账户体系支持多子账户、API Key 独立授权、IP 白名单限制，适合团队分工管理。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://dynadot.fwen.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Dynadot 为纯 Web 服务，无需安装客户端——浏览器打开官网注册账户即可使用全部功能；移动端可通过手机浏览器访问

---

## 🔧 注册与账户设置

Dynadot 是 SaaS 模式的域名管理平台，没有传统意义上的"安装"——取而代之的是账户注册和初始配置。以下是第一次使用的完整流程。

### 创建账户

1. 访问 Dynadot 官网，点击右上角「Sign Up」
2. 填写邮箱和密码，邮箱必须真实可用（后续所有域名转移、续费确认、Whois 验证都靠它）
3. 完成邮箱验证后登录，进入「My Account」开始配置

> [!TIP]
> 注册时建议同时开启两步验证（2FA）。域名账户一旦被盗，攻击者可以直接把你的域名转出，损失不可逆。Dynadot 支持 Google Authenticator、Authy 等 TOTP 应用。

### 充值与付款方式

Dynadot 支持信用卡（Visa / MasterCard / American Express）和 PayPal。注册新域名前需要保证账户余额充足，也可以在结账时直接扣款。续费默认自动扣费，如果不想续记得提前在账户设置里关闭自动续费。

### 基础偏好设置

登录后建议先进入「My Account」→「Account Settings」调整几项默认行为：

| 设置项 | 推荐值 | 说明 |
| --- | --- | --- |
| **Auto-Renew** | 按需开启 | 不想手动续费就开，怕忘记关了域名过期就惨 |
| **Whois Privacy** | 默认开启 | 隐藏真实联系人信息，防止骚扰邮件 |
| **Domain Lock** | 默认开启 | 防止域名被未授权转出 |
| **Default Nameserver** | Dynadot DNS | 用 Dynadot 的 DNS 才能免费申请 SSL |

---

## 📖 域名管理实战

完成账户初始化后，下面深入几个高频使用场景——联系人记录管理、API 自动化、空置域名停放、免费 SSL 签发。

### Whois 联系人记录管理

Dynadot 把联系人记录作为**账户级别的独立资源**管理，而不是绑在每个域名上。这个设计的好处是：你可以保存多套联系人（一套个人信息、一套公司信息），注册新域名时选一套就行；修改联系人后可以选择是否同步到所有使用该记录的域名。

每个域名的 Whois 数据库包含四类联系人：

| 联系人 | 作用 | 常见填写 |
| --- | --- | --- |
| **Registrant（注册人）** | 域名的法定所有者，收转移确认邮件 | 个人或公司名 |
| **Admin（管理员）** | 管理域名设置的人 | 与注册人相同 |
| **Technical（技术）** | 负责 DNS / 服务器的人 | 与注册人相同或技术负责人 |
| **Billing（账单）** | 处理续费和发票 | 与注册人相同 |

大多数场景下四类联系人填成一样即可。只有团队分工明确时才分开填写——比如公司注册域名，Admin 填 IT 负责人，Billing 填财务。

> [!WARNING]
> ICANN 要求 Whois 联系人信息完整，以下字段不能留空：姓名、邮箱、国家/地区、省/州、城市、街道地址、邮编、电话。邮箱必须有效，因为域名转移、争议解决、续费确认都会发邮件到这里。

**管理联系人的操作路径**：进入「My Account」→「Contact Records」，可以创建新联系人、编辑现有记录、设置哪套为默认。批量应用的话，通过 Smart Folders 或域名列表的批量操作，把一套联系人同步到选中的所有域名。

需要注意隐私保护对联系人的影响：开启 Whois 隐私保护后，公开 Whois 里显示的是 Dynadot 的代理信息，真实联系人只在系统内部保留。但域名转移、实名认证、争议解决这三种场景需要你主动关闭隐私保护，否则目标方验证不了你的身份。

### API Key 与 Sandbox 沙箱

如果你需要批量注册域名、自动续期、监控域名状态，Dynadot 提供了完整的 REST API。最贴心的是它的 **Sandbox 沙箱**——零成本测试环境，命令格式和生产完全一致，但不会真的注册域名、不会扣钱、不会改 DNS。

#### 开启 API 访问

1. 进入「My Account」→「API Settings」
2. 勾选「Enable API Access」
3. 系统生成 API Key——**只显示一次，立即复制保存**
4. 设置 IP 白名单（强烈推荐），只允许指定服务器调用

> [!WARNING]
> API Key = 你的密码。谁拿到了 Key 就能用你的账户操作域名，所有费用从你余额扣。绝对不要提交到 Git 仓库，用环境变量存储代码里。

#### Sandbox 怎么用

在 API 请求里加 `&test=1` 参数就切换到沙箱：

```
https://api.dynadot.com/api3.json?key=YOUR_API_KEY&command=search&domain=testexample.com&test=1
```

Sandbox 返回与生产相同格式的 JSON 响应，但不会产生实际影响。开发流程建议严格按这个顺序：

1. 在 Sandbox 里先跑查询命令（list_domain、search）
2. 再跑管理命令（set_dns、lock_domain）
3. 最后跑生产命令（register、renew）
4. 去掉 test=1 切生产，先对 1 个测试域名验证
5. 确认无误后再批量

#### Sandbox 的局限

Sandbox 不会返回真实的域名可用性状态（所有域名都返回可注册），也不模拟支付成功/失败的真实场景。它只是帮你验证代码逻辑对不对——实际生产运行前，还是要在少量真实域名上跑一次。

### 域名停放页面

当你注册了域名但还没有搭建网站时，Dynadot 默认展示一个停放页。它的作用是告诉访客域名已被注册、防止空域名被浏览器标记为危险、同时也干净专业——Dynadot 的免费停放页不带广告。

停放页的开关逻辑很简单：

- DNS 设为 Dynadot DNS 且**没有任何 A 记录** → 展示停放页
- 添加了 A 记录指向自己的服务器 → 停放页自动关闭
- 切换到自定义 DNS → 停放页自动关闭

免费版停放页无法自定义文案。如果想展示"域名正在出售"之类的内容，有两条低成本路径：一是域名转发到一个临时展示页（比如 GitHub Pages），二是用 Dynadot 免费网站构建器建一个单页站。

对于域名投资者，建议保持默认停放页即可，干净不扰客；在 Watchlist 或 Smart Folders 里标记好到期提醒，想挂牌出售时通过 Dynadot 二级市场功能直接挂单，不需要改停放页。

### 免费 SSL 证书

Dynadot 为使用其 DNS 服务的域名提供免费 SSL 证书，基于 Let's Encrypt，3 个月有效期，到期前约 1 个月系统自动续期。

申请前提只有两个：DNS 由 Dynadot 管理，域名处于正常状态（未过期、未挂起）。申请步骤：

1. 进入域名管理中心 → 选中域名 → Action → DNS Settings
2. 找到 SSL / HTTPS 选项卡，点击「申请免费 SSL 证书」
3. 验证域名所有权（DNS 已在 Dynadot，通常自动通过）
4. 下载 .crt 证书和 .key 私钥，部署到你的 Web 服务器

证书是标准 X.509 格式，Nginx、Apache、Caddy 等主流服务器都能直接用。如果用 Dynadot 免费网站构建器，SSL 自动生效，不需要手动操作。

自动续期后需要注意：**私钥可能会变**，务必同时替换 .crt 和 .key 两个文件，然后重载 Web 服务器。

| 特性 | Dynadot 免费 SSL | Cloudflare / Vercel 免费 SSL |
| --- | --- | --- |
| 有效期 | 3 个月 | 90 天 |
| 自动签发 | ✅ | ✅ |
| 自动部署 | ❌ 需手动 | ✅ |
| 自动续期 | ✅（签发） | ✅（签发 + 部署） |
| 通配符 | ❌ 不支持 | ✅ |
| DNS 绑定 | 必须用 Dynadot DNS | 不绑定 |

Dynadot 免费 SSL 适合直接用 Dynadot DNS + 自建服务器的场景。如果网站在 Cloudflare 或 Vercel 后面，它们的方案更省心——自动签发、自动部署、自动续期一条龙。

---

## 🔗 相关资源

- [Dynadot官网](https://dynadot.fwen.ren)
- [Dynadot文档](https://dynadot-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/Dynadot)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/Dynadot)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/Dynadot)

---

## 📚 相关文章

- [Dynadot 十年迭代史——从域名注册商到开发者域名平台](https://laoguosofts.github.io/posts/dynadot-docs.html)
- [Dynadot 版本进化史——从小众注册商到域名一站式管家](https://alipymanbu.github.io/posts/dynadot-docs.html)
