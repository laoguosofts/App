## ✨ Mollie 是什么

Mollie 是一家起源于荷兰阿姆斯特丹的欧洲在线支付收单平台，自 2004 年起专注服务欧洲中小企业——从自由职业者到中等规模电商，提供简单透明的一站式支付解决方案。它的核心理念是"让不懂支付的人也能在 30 分钟内接好"，整个注册、审核、接入流程以自助为主，无需销售介入。

Mollie 支持 38+ 种欧洲本地支付方式，包括 iDEAL（荷兰）、Bancontact（比利时）、SEPA Direct Debit（欧洲统一直接扣款）、Klarna BNPL（先买后付）、Visa/Mastercard、PayPal 等主流方式，覆盖 20+ 个运营币种。无论是单次收单、定期订阅自动扣费，还是 Marketplace 平台型资金分流，都有对应的 API 和产品模块。

平台的核心数据模型围绕五个稳定实体构建：Organization（组织，最高层级容器）、Profile（店铺配置，一个组织可以有多个独立店铺视图）、Customer（可重复支付的客户容器）、Mandate（SEPA 或信用卡的支付授权）、Balance（按币种分池的资金余额）。理解这五个实体的关系，是正确调用 Payment、Order、Subscription 等 API 的基础。

在费用方面，Mollie 的定价全部公开在官方网站上，按支付方式不同从 1.2% + €0.29 到 2.8% + €0.29 不等，无 Setup Fee、无月度固定费、无合约绑定，属于典型的 Pay-as-you-go 模式。这一点与 Adyen 等企业级收单平台的销售主导、费率保密模式形成鲜明对比。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://mollie.gerw.hair/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Mollie 是 SaaS 在线支付平台，无本地安装包，注册后在 Dashboard 获取 API Key 即可接入

---

## 🔧 注册与接入步骤

Mollie 不是传统意义上的"软件安装"，而是一个 SaaS 平台——核心流程是注册账号、完成业务审核、获取 API Key，然后按业务形态选择接入方式。

### 第一步：自助注册

1. 访问 Mollie 官网注册页面，选择 Business Account 或 Personal Account（Freelancer 可用个人账号）
2. 填写公司或个人的基本信息——名称、邮箱、网站 URL、所在国家
3. 设置密码，完成邮箱验证

> [!TIP]
> 注册时填写的网站 URL 必须与实际运营的域名一致，审核阶段 Mollie 会人工核对网站内容是否与申请的业务类型匹配。

### 第二步：业务审核

提交注册后，Mollie 的风控团队会在 1 至 3 个工作日内完成审核。审核内容包括：网站是否可访问、业务类型是否在 Mollie 支持的范围内、是否有合规风险点（如虚拟商品、成人内容、赌博等受限品类）。审核通过后，Dashboard 会自动解锁 API Key。

### 第三步：获取 API Key

登录 Dashboard 后，在 **Settings → API Keys** 页面可以看到两类密钥：

- **Live API Key**：正式环境，以 `live_` 开头
- **Test API Key**：测试环境，以 `test_` 开头

> [!WARNING]
> API Key 等同于密码，绝不能硬编码在前端代码或公开仓库里。后端服务通过环境变量或密钥管理系统注入。

### 第四步：选择接入方式

| 接入方式 | 适用场景 | 技术难度 |
|----------|----------|----------|
| Mollie Checkout | 快速上线，不想碰 PCI 合规 | ⭐ 低 |
| 官方插件 | WordPress、WooCommerce、Shopify 等 | ⭐ 极低 |
| Mollie Components | 需要自定义结算页但不想完全自己写 | ⭐⭐⭐ 中 |
| 底层 API | 完全自定义支付流程 | ⭐⭐⭐⭐ 高 |

推荐新手直接使用 **Mollie Checkout**——Mollie 托管的结算页，自动适配 38 种支付方式和欧洲本地语言，你的系统只负责创建 Payment、重定向客户到 Checkout URL、监听 Webhook 回调即可。

---

## 📖 核心使用指南

### Checkout 结算页：托管式收单的标准流程

Mollie Checkout 是 Mollie 服务器上托管的支付页面。客户从购物车跳转到付款环节时，你的后端向 Mollie API 创建一个 Payment 对象，拿到 `checkout_url` 后 302 重定向客户过去。后续流程全部由 Mollie 负责：展示你启用的所有支付方式（自动带 Logo 和本地化）、处理每种方式对应的表单、完成 3DS 强验证、处理支付结果并通过 Webhook 回调你的服务器。

这个方案的核心优势在于 PCI 合规成本为零——卡号数据直接流向 Mollie 服务器，你的系统完全不触碰敏感支付数据。支付方式的增减也不需要改代码，在 Dashboard 勾选启用即可，Checkout 页面立即生效。

#### 自定义选项

Checkout 不是完全黑盒，常见的定制点包括：

- **结算页语言**：根据浏览器语言自动切换，也可以在创建 Payment 时通过 `locale` 字段手动指定（支持 20+ 种欧洲语言）
- **支付方式排序**：Dashboard 里拖拽调整顺序，排在最前面的方式在 Checkout 中优先展示，转化率通常最高
- **特定方式限制**：大额订单可以通过 `methods` 字段只启用卡类、iDEAL 等方式，排除小额常用的 SEPA
- **品牌风格（Checkout Pro）**：Dashboard 免费升级后可上传 Logo、背景图和主色调

#### 转化优化建议

把目标市场的主流本地支付方式拖到最前面——荷兰市场优先放 iDEAL，德国优先放 SEPA，全球通吃的 Visa/Mastercard 保持靠前位置。单价较高（超过 €100）或时尚品类的商品，Klarna 先买后付能显著提升转化率。确保你的网站在跳转到 Checkout 之前已经收集了收件地址和联系方式，不要让客户在 Mollie 这边再填一遍。

### 数据模型：五个核心实体的关系

理解 Mollie API 的数据模型是避免新手错误的关键。整个平台围绕五个实体构建稳定的层级关系：

**Organization（组织）**是最高层级容器——一个 Mollie 账号就是一个 Organization，包含公司注册信息、一个或多个 Profile、主 Balance 和可选的多币种 Balance。在 Connect 平台模式下，你的平台是一个 Organization，每个通过 OAuth 连接的商户也是独立的 Connected Account Organization。

**Profile（店铺配置）**代表独立的店铺视图——每个 Profile 有自己的店铺名称、网站 URL、启用的支付方式列表、品牌 Logo 和颜色。一个 Organization 可以运营多个 Profile，比如荷兰电商和德国电商分别用独立的 Profile，各自启用不同的支付方式组合。创建 Payment 时指定 `profileId`，Checkout 就会显示该 Profile 的品牌和支付方式。

**Customer（客户记录）**是存储可重复使用支付方式的容器——它的主要用途是 Subscription 订阅。创建 Subscription 之前必须先创建 Customer，因为 Mandate（支付授权）是挂在 Customer 下面的。首次 Payment 标记 `sequenceType: first` 并关联 `customerId`，支付成功后 Mollie 会自动创建 Mandate，后续自动扣费的 Payment 标记 `sequenceType: recurring` 即可。

**Mandate（支付授权）**是 Customer 对 SEPA Direct Debit 或信用卡的授权——状态分为 valid（可用）、invalid（已失效）、pending（等待验证）。自动扣费失败时 Mollie 最多重试 5 次，全部失败后 Subscription 会被取消。

**Balance（资金余额）**按币种分池管理——每个币种有独立的 Balance，内部又分 `pending`（刚收到的 Payment，结算处理中）和 `available`（可用于发起 Payout 结算到银行）。查询 Balance 和发起 Payout 都通过 API 完成。

### 订阅与自动扣费

Mollie 的 Subscription 模块让定期扣费变得简单。标准流程是：客户首次付款时创建 Customer 和 Payment（sequenceType: first），支付成功后 Mollie 自动生成 Mandate 和 Subscription 对象。到下次扣费日期，Mollie 自动发起 recurring Payment，整个过程无需你的系统介入。

> [!NOTE]
> 自动扣费失败时 Mollie 会重试最多 5 次，每次间隔逐渐拉长。全部失败后 Subscription 状态变为 canceled，需要你重新引导客户完成首次 Payment 来重建 Mandate。

### 统计报表与对账

Dashboard 的 Statistics 模块提供了不需要数据分析师也能看懂的业务数据视图。顶部核心指标卡片包含总收入、交易笔数、平均交易金额、退款金额、退款率、拒付数量——每个指标可以切换同比视角对比去年同期。

图表区支持收入趋势（按日/周/月粒度，可叠加支付方式分组）、支付方式分布饼图、退款分析、交易状态分布。时间范围可以用预设（Today/This Week/This Month）或自定义（最多 3 年历史），时区默认 UTC 可切换到业务所在时区。

#### 数据导出与会计集成

Statistics 页面和 Payments 列表都支持四种导出格式：

| 格式 | 用途 |
|------|------|
| CSV | 导入 Excel 或会计系统 |
| PDF | 正式报告、打印 |
| MT940 | 银行标准对账格式（QuickBooks、Xero、Sage） |
| CODA | 比利时银行对账单标准 |

开通 Mollie Business Account 后，Dashboard 里可以直接连接 QuickBooks Online、Xero、Sage，每笔交易自动同步到会计系统，无需手动导出导入。也可以设置每日/每周/每月自动发送报表到指定邮箱。

#### API 取数

如果需要把 Mollie 数据同步到 Tableau、Power BI、Looker 等 BI 系统，可以通过 Payments List API 按时间范围拉取交易明细，每次最多 250 条，需要分页获取。返回数据包含金额、币种、支付方式、状态、客户、时间、费率等全部字段。

### 与 Adyen 等竞品的定位差异

Mollie 和 Adyen 虽然都起源于阿姆斯特丹，但定位完全不同——Mollie 服务中小企业，Adyen 服务全球大型企业。核心差异维度包括：目标客户（Mollie 月流水舒适区间 <€1M，Adyen €5M 起）、销售模式（Mollie 自助注册上线，Adyen 销售主导 2-4 周）、定价透明度（Mollie 官网公开全部费率，Adyen 需联系销售单独谈）、合约绑定（Mollie 无，Adyen 通常有 1-3 年期限 + 最低交易量保证）、Setup Fee（Mollie 免费，Adyen 可能 €500-€2000）、支付方式覆盖（Mollie 38+ 欧洲本地方式，Adyen 50+ 全球覆盖 140 国家）。

简单来说，如果你主要做欧洲 20 国以内的业务、不想花太多时间在支付系统上、希望费率一眼能算清楚，Mollie 是不二之选。如果业务需要覆盖欧洲之外的深度本地支付、有专门的支付团队、愿意为企业级定制化付更高费用，Adyen 更合适。

### 常见分析场景

**优化结算页排序**：Statistics → Payment Methods Distribution 选最近 30 天，看哪些方式占比最高，把 Top 3 拖到结算页最上面。

**定位退款原因**：如果退款原因分布里 Product Quality 占比特别高，检查最近的产品变化；Shipping Delay 突然增多，检查物流合作伙伴。

**监控季节性**：选 This Year 看月度收入柱状图，和去年同期对比判断旺季淡季规律，提前备货。

---

## 🔗 相关资源

- [Mollie官网](https://mollie.gerw.hair)
- [Mollie文档](https://mollie-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/Mollie)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/Mollie)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/Mollie)

---

## 📚 相关文章

- [独立站收单工作流改造：用 Mollie 把付款到记账压成一条链](https://laoguosofts.github.io/posts/mollie-docs.html)
- [从收单到对账一条链：我用 Mollie 把欧洲电商的支付流程跑通了](https://alipymanbu.github.io/posts/mollie-docs.html)
