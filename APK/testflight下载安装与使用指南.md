## ✨ TestFlight 是什么

TestFlight 是 Apple 提供的官方 Beta 测试分发平台。它让开发者可以把尚未上架 App Store 的 iOS、iPadOS、macOS、tvOS、visionOS、watchOS 应用构建版本（也就是 Archive）分发给内部团队成员或外部测试用户，在正式发布之前收集真实设备上的反馈和崩溃数据。

对开发者来说，TestFlight 解决的是「怎么把 Beta 版 App 送到用户手里」这个老问题——在它出现之前，分发测试构建依赖 Ad Hoc 签名、UDID 注册、企业证书等繁琐手段，覆盖范围有限且管理混乱。TestFlight 把分发、版本管理、崩溃收集、反馈收集全部整合进 App Store Connect 后台，开发者只需要上传一个 Archive，剩下的签名、分发、过期、统计全由 Apple 托管。

对测试者来说，TestFlight 就是一个独立的 App（iOS 上叫 TestFlight、macOS 上也有单独客户端），打开后可以看到收到的所有 Beta 邀请，一键安装、一键升级到最新构建、一键发送反馈——不需要 Xcode、不需要电脑、不需要懂开发者工具。

适用人群很广：独立开发者自己的 App 内测、公司 QA 团队批量回归、种子用户小范围公测、给媒体和合作伙伴提前体验——只要你的 App 准备好进 App Store，TestFlight 都是绕不开的一环。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://testflight.kook.yoga/download.html) —— 下载页整理了当前可用的版本和安装说明
> - App Store：iOS / iPadOS / macOS / tvOS 搜索「TestFlight」，Apple 官方客户端免费下载

---

## 🔧 怎么开始用

TestFlight 的使用分两种角色：**开发者**（上传构建、管理测试组）和**测试者**（接受邀请、安装测试）。两种角色的准入门槛完全不同，入门方式也不一样。

### 开发者视角：把 App 送进 TestFlight

1. 注册 Apple Developer 账号，年费 99 美元，必须。没有这个账号你连 App Store Connect 的 TestFlight 入口都进不去。
2. 在 Xcode 里选好签名团队、配置好 Bundle ID，正常 Archive 出一个构建——这个步骤跟你准备上架 App Store 没区别。
3. Xcode Organizer 里点 **Distribute App → App Store Connect → Upload**，等 Apple 后台解析完成。解析阶段会解压、校验签名、重新签名（这是 TestFlight 分发的核心——签名由 Apple 控制，防止非官方签名篡改构建）、提取版本号和构建号等元数据。
4. 进入 App Store Connect → **TestFlight → Builds**，新构建出现在列表里，状态列先是 Processing，处理完变成 Ready to Submit。
5. 加进测试组：内部组不需要审核，几分钟后组内成员就能收到邀请；外部组的**第一个构建要走 TestFlight 审核**，通常 1~3 天，过了之后同版本号的后续构建可以自动推送，不用再等审核。

> [!WARNING] 构建号不能重复
> CFBundleVersion（构建号，比如 123）必须每次上传都递增，而且在同一个 App Store Connect 账户下**所有 App 的构建号必须全局唯一**。重复了 Apple 会直接拒绝上传。版本号（CFBundleShortVersionString，比如 1.0.0）可以重复，同版本号下可以有多个构建版本。

### 测试者视角：接受邀请、安装 Beta App

- 内部测试者（App Store Connect 团队成员）：开发者在后台把你的 Apple ID 加进内部测试组，你会收到邮件或 TestFlight App 内的推送，点开就能装。
- 外部测试者（公开链接或邮件邀请）：通过公开链接接受邀请后，打开 TestFlight App 就能看到对应的 Beta App，点「安装」即可。
- 装完之后 TestFlight 会持续推送更新——开发者上传了新构建、你这边打开 TestFlight 就能看到「可用更新」。

> [!TIP] Provisioning Profile 的隐藏要求
> 用于 TestFlight 的 Provisioning Profile 必须包含 Application Identifier。手动签名模式下如果你发现构建上传后 Processing 完了但 Ready to Submit 之前卡住，或者加测试组时报错「Profile missing Application Identifier」，回 Apple Developer 后台找到这个 Profile → Edit → 勾选 App ID → Save → Download → 重新 Archive + Upload。自动签名模式（推荐）下 Xcode 会自动帮你搞定，不会缺。

---

## 📖 使用入门

下面从开发者和测试者两个实际使用场景出发，把 TestFlight 日常操作中高频遇到的功能、限制、坑点梳理清楚。

### 开发者侧：构建上传与状态监控

上传完 Archive 之后，构建会依次经历以下状态流转：

```
Processing（Apple 正在解析）
   ↓
Ready to Submit（解析完，可以加进测试组）
   ↓ 加进内部组
Testing（内部测试组里分发中）
   ↓ 加进外部组（首次）
Waiting for Review（等 TestFlight 审核，1~3 天）
   ↓ 审核通过
Testing（外部测试组里分发中）
   ↓ 90 天到了 或 手动 Expire
Expired / Inactive
```

在 App Store Connect → TestFlight → **Builds** 面板里，每行代表一个构建版本。表格从左到右的列分别是 Version（版本号）、Build（构建号）、Platform（平台）、Status（当前状态）、Activity（总会话数）、Crashes（崩溃总次数）、Feedback（反馈总数）、Installs（安装设备数）。点击任意一行可以进入详细视图，看到会话时间分布、崩溃按平台/设备型号分布、反馈列表、关联的测试组等。

这三个指标是开发者日常盯数据的核心：

| 指标 | 怎么解读 |
| --- | --- |
| Activity | 总会话次数——每个测试者每次打开 Beta App 算一次。**Installs 高但 Activity 低** = 很多人装了但没在用，回头检查 Beta 描述是不是没把核心场景说清楚 |
| Crashes | 崩溃总次数。**崩溃数飙升** = 这次改动引入新 bug，立刻去崩溃面板看高频堆栈；**崩溃数下降** = 说明修的 bug 生效了 |
| Feedback | 反馈总数。关键看**质量和类型分布**——大部分是功能建议 → App 可能太无聊需要加新功能；大部分是崩溃报告 → 稳定性差；完全没反馈 → 可能 Beta Description 没引导测试者反馈 |

### TestFlight 的硬限制

这些是 Apple 白纸黑字写死的数字，规划测试策略之前先记下来：

| 限制项 | 上限 | 说明 |
| --- | --- | --- |
| 同时 Beta 的 App 数 | 100 | App Store Connect 账户里同时处于 TestFlight 状态的 App 最多 100 个 |
| 内部测试者 | 100 人 | 必须持有 Account Holder / Admin / App Manager / Developer / Marketing 五种角色之一，Managed Apple ID（企业集中管理的那种）不支持 |
| 外部测试者 | 10,000 人 | 所有外部测试组合计，按人头不按设备 |
| 每个 App 可上传的构建 | 最多 100 个在 TestFlight | 超出时需要手动过期旧构建 |
| 每个构建有效期 | 90 天 | 过期后测试者设备上的 Beta App 无法打开，上传新构建就能续期 |

> [!NOTE] 订阅和内购的特殊行为
> TestFlight 环境下，付费订阅每天续期一次、一周最多续 6 次，不管你设置的是月度还是年度。所有 App 内购买项目在测试期内免费，但测试期结束后这些内购记录**不会自动转移**到 App Store 正式版。

### 测试者侧：设备管理与多设备使用

每个 Apple ID 在 TestFlight 中**最多可以在 30 台设备上同时测试 Beta App**。这里的设备包括 iPhone、iPad、iPod touch、Mac、Apple TV、Apple Vision Pro。Apple Watch 跟随配对的 iPhone 算一台，不是独立名额。30 台不是某个特定 App 的限制，是整个 TestFlight 账户级别的限制——不管你接受了多少个开发者的 Beta 邀请，所有设备加在一起不能超过 30 台。

查看和清理设备的方法很简单：iOS 上打开 TestFlight → 右上角齿轮设置 → 找到「设备」选项；macOS 上打开 TestFlight → 顶部菜单栏 TestFlight → 偏好设置 → 通用标签页。清理方式有两种：直接在 TestFlight 设置里移除旧设备，或者在旧设备上打开 TestFlight → 选中某个 Beta App → 向下滚 → 点「停止测试」。两种方式都会把那台设备从你的测试列表里移除，让出名额。

> [!WARNING] 切换 Apple ID 的代价
> 切换 Apple ID 会清除设备上**所有当前 Apple ID 接受的 Beta App**。如果你需要同时测试多个开发者的 Beta App，用不同 Apple ID 分别接受没问题，但每次切换等于重新来过。

### 其他容易踩的坑

**加密合规自声明（Export Compliance）**：App 只要用了 HTTPS（实际上几乎所有 App 都用），上传时 Apple 会要求你填写加密合规声明。大多数 App 勾「Yes, use encryption」然后选「App uses standard encryption」就能过，不需要额外提交文档。

**公开链接的隐私边界**：开发者通过公开链接邀请测试者时，**看不到测试者的姓名和邮箱**，只能看到设备数、安装日期、使用次数、崩溃次数等统计数据。只有通过邮件邀请进来的测试者，开发者才能看到邮箱。

**内部分发 vs 外部分发的审核差异**：内部测试完全免审，上传加组几分钟就能用；外部测试的第一个构建必须过 TestFlight 审核（比 App Store 正式上架宽松很多，主要查崩溃、描述准确性、明显违规），审核通过后同版本号的后续构建可以直接推送，不用再走审核。

---

## 🔗 相关资源

- [testflight官网](https://testflight.kook.yoga)
- [testflight文档](https://testflight-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%AD%A3%E7%89%88/testflight)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%AD%A3%E7%89%88/testflight)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%AD%A3%E7%89%88/testflight)

---

## 📚 相关文章

- [TestFlight 我踩过的五个坑——从开发者注册到测试者邀请，这些弯路别走了](https://laoguosofts.github.io/posts/testflight-docs.html)
- [TestFlight 避坑指南：99美元花之前，这几个坑我都踩过](https://alipymanbu.github.io/posts/testflight-docs.html)