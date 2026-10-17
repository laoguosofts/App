## ✨ KeepassDX 是什么

KeepassDX 是一款运行在 Android 平台的开源密码管理器，基于经典的 KeePass 数据库格式（KDBX）构建。它的核心定位很明确：**在手机上安全地管理你所有的账号密码、令牌和敏感凭证**，不依赖任何云服务，数据完全由你自己掌控。

和市面上常见的在线密码管家不同，KeepassDX 采用本地加密存储——你的密码数据库是一个经过强加密的 `.kdbx` 文件，只有输入正确的主密码才能解锁。这种离线架构带来了两个好处：一是没有云端被拖库的风险，二是即使断网也能随时查看密码。

KeepassDX 的功能覆盖了日常密码管理的几乎所有场景：自动填充（支持 Magikeyboard 输入法模式）、TOTP/Steam 验证码生成、Passkey（WebAuthn）管理、附件存储、分组标签整理、多数据库切换。4.3.0 版本之后还引入了用户验证（User Verification）机制，在敏感操作时追加一层行为确认。

适合谁用？如果你有几十到上百个账号需要管理、不信任在线密码管家、在意数据隐私，KeepassDX 是目前 Android 上最轻量也最可靠的选择之一。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://keepassdx.boop.uno/download.html) —— 下载页整理了当前可用的版本和安装说明
> - F-Droid 仓库：搜索 "KeePassDX" 安装 Libre 版，完全开源、无 Google 依赖
> - Google Play：搜索 "KeePassDX" 安装 Free 版，可应用内付费解锁 Pro

---

## 🔧 安装步骤

KeepassDX 的安装方式取决于你选择哪个分发渠道。三个渠道各有侧重，但核心都是拿到 APK 文件装进手机。

### 方式一：F-Droid（最省心）

如果手机上装了 F-Droid 客户端，直接搜索 "KeePassDX" 就能找到 Libre 版。F-Droid 上的包经过社区二次审核，完全开源、不包含任何追踪组件，更新也会自动推送。安装流程和 Google Play 装 App 一样简单。

### 方式二：GitHub Release（最灵活）

GitHub Release 是 KeepassDX 发新版的第一站，F-Droid 和 Google Play 通常会晚几天到几周同步。如果你想第一时间拿到新版本或者测试版，GitHub Release 是最快的通道。

1. 在手机或电脑浏览器上打开 KeepassDX 的 GitHub Release 页面
2. 找到最新版本，在 Assets 区域下载 APK 文件
3. 直接用手机浏览器下载，或者先下到电脑再传到手机
4. 在文件管理器里找到 APK 点击安装

> [!WARNING]
> Android 默认禁止安装未知来源应用。安装时如果弹出拦截提示，需要去「设置 → 安全」里临时允许该来源的安装权限，装完建议立刻关掉。

GitHub Release 上通常会同时提供多个 APK：

| 文件名 | 适用场景 |
| --- | --- |
| `KeePassDX-<版本号>-free.apk` | 功能完整的免费版，可内购解锁 Pro |
| `KeePassDX-<版本号>-libre.apk` | 完全开源版，和 F-Droid 版相同 |
| `arm64-v8a` 后缀 | 2018 年后 Android 手机主流架构，绝大多数用户选这个 |
| `armeabi-v7a` / `x86` / `x86_64` | 老设备或模拟器专用 |

如果你不确定选哪个，直接下 `arm64-v8a` 的 free 包就对了。

### 方式三：Google Play（最方便）

已经在用 Google Play 生态的话直接搜 "KeePassDX" 安装即可。Play 上是 Free 版，内购约 4 美元可解锁 Pro 功能（主要是 Steam TOTP 支持）。

> [!TIP]
> GitHub Release 页面同时提供 PGP 签名文件（`.asc` 后缀）。安全意识强的用户可以下载 APK + .asc 文件，用 GPG 工具校验签名——通过就意味着 APK 没有被篡改。

---

## 📖 使用入门

### 版本怎么选

KeepassDX 同时在三个渠道分发，对应四个版本，功能差异其实很清晰：

| 版本 | 渠道 | 源码 | 价格 | 核心差异 |
| --- | --- | --- | --- | --- |
| Libre | F-Droid | 完全开源 | 免费 | 最干净，无任何闭源依赖 |
| Free | GitHub / Play | 部分开源 | 免费 | Free/Libre 核心功能相同，Play 版多了 Google Play 服务集成 |
| Pro | Play 内购 | 闭源 | 约 4 美元 | 解锁 Steam TOTP + 更多数据库设置选项 |

推荐优先级：F-Droid Libre 版 > GitHub Release Free APK > Google Play Free 版 > Pro 版。99% 的用户用 Free/Libre 版就够了，只有需要管理 Steam 账户验证码的人才需要 Pro。

### 创建第一个数据库

打开 KeepassDX，在欢迎界面选择「创建新数据库」，向导会一步步带你完成关键配置。

**数据库格式**：选 KDBX 4.x。它支持 Argon2d 密钥派生函数（比旧版 AES-KDF 更抗 GPU 暴力破解）和 ChaCha20-Poly1305 加密，所有主流 KeePass 客户端都兼容。

**加密算法**：默认 AES-256-GCM 就行，ChaCha20-Poly1305 在极低端设备上性能略好，但安全性相当。

**密钥派生函数**：Argon2d 是当前推荐，参数保持默认（迭代 10 次、内存 64MB）即可。

**数据库文件存哪**：这一步别乱选——文件一旦丢了，主密码再强也没用。三个选择：

- 内部存储 / KeePassDX 目录：最简单，文件名起个有意义的比如 `personal.kdbx`
- SD 卡 / USB OTG：物理隔离，手机丢了数据不会被拿走，但访问速度略慢
- Syncthing / 自托管云存储：计划多设备同步就选这个

> [!NOTE]
> 数据库文件 ≠ 密钥文件。KeepassDX 4.1.0+ 支持生成 `.bin` 格式密钥文件作为第二道锁——建议生成一个放在 U 盘或 SD 卡上，和数据库文件物理隔离。密钥文件哪怕改一个字节数据库就打不开，别随便动它。

**主密码**：这是整个流程最关键的一步——一旦忘记永远找不回。长度至少 16 字符，混合大小写、数字、符号，别用生日、手机号这类可猜测的组合。KeepassDX 在输入时会实时显示密码强度评估，绿色「非常强」就够了。

全部参数确认后，KeepassDX 会生成加密数据库并自动打开，你可以开始添加条目了。

### 用户验证：敏感操作的额外保护层

从 4.3.0 版本起，KeepassDX 默认启用了用户验证（User Verification，简称 UV）机制。这是一个容易被忽略但很实用的安全层。

它解决的问题是：**即使数据库已经解锁，你拿到手机的人也不能直接看密码、复制凭证**。没有 UV 的话，数据库解锁后里面的数据在内存里是明文状态，任何能操作手机的人都能直接浏览。

| 对比项 | 设备解锁 | 用户验证（UV） |
| --- | --- | --- |
| 触发时机 | 打开数据库时 | 每次敏感操作时 |
| 做什么 | 用主密码解密数据库 | 再确认一次「你是你」 |
| 技术 | Android Keystore 加密主密码 | 纯行为验证 |

以下操作会触发 UV：

- 查看或复制密码、OTP、自定义保护字段
- 编辑已有的 Passkey 条目
- 修改数据库加密算法或主密码
- Passkey WebAuthn 登录仪式（强制，不可关闭）

UV 的验证方式优先级：主密码前 4 字符 → 设备锁屏凭证 → 生物识别。默认推荐主密码前 4 字符，4 键就能完成，不用触发锁屏也不用等指纹传感器。

> [!WARNING]
> UV 只是行为验证，不是加密机制。它防不了屏幕录制、也防不了有充足时间的物理入侵——核心安全仍然依赖数据库加密本身和设备锁屏。UV 是锦上添花的第二层，不是替代品。

### 日常使用节奏

建好数据库、加好条目之后，日常使用 KeepassDX 的节奏大概是这样的：

- 打开 App → 输入主密码（或用生物识别快速解锁）→ 浏览/搜索条目
- 需要密码时：点击条目 → UV 验证 → 复制密码到剪贴板
- 自动填充：在支持的 App 或浏览器里触发自动填充，KeepassDX 会弹出对应条目确认后直接填进去
- Magikeyboard：把 KeepassDX 设为输入法，在任何需要输入密码的地方直接调起
- OTP 验证码：条目详情页就能看到 TOTP 倒计时，Steam 等自定义算法的验证码 Pro 版也能支持

### 数据备份

KeepassDX 的数据库文件就是你的全部密码。定期把 `.kdbx` 文件备份到另一个地方——云存储、另一台设备、甚至打印出来存保险柜都可以。不要把备份和原件存在同一个物理位置，那样手机丢了备份也跟着没了。

---

## 🔗 相关资源

- [KeepassDX官网](https://keepassdx.boop.uno)
- [KeepassDX文档](https://keepassdx.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/KeepassDX)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/KeepassDX)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/KeepassDX)

---

## 📚 相关文章

- [周五咖啡馆上线夜，我终于把一百多个密码从脑子里清空了](https://laoguosofts.github.io/posts/keepassdx.html)
- [Steam 账户被盗那晚，我用 KeepassDX 重建了整套凭证](https://alipymanbu.github.io/posts/keepassdx.html)
