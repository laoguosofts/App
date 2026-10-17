## ✨ Cryptomator 是什么

Cryptomator 是一款开源的云盘加密工具，自 2015 年起由德国 Skymatic 团队维护，采用 GPLv3 许可证发布。它的核心思路很简单——在你的云盘（Dropbox、OneDrive、iCloud Drive、坚果云等）同步文件夹里创建一个加密容器（叫作 Vault，保险库），Cryptomator 在本地用 FUSE 虚拟文件系统把它解密成一个可直接读写的虚拟盘，你往里面存什么，落盘时都会被 AES-256 加密后才同步到云端。

这种"客户端加密 + 云盘即存储"的架构让 Cryptomator 区别于其他加密工具：它不要求你迁移到特定云服务商、不绑定账号体系、甚至连注册都不需要——下载、安装、设个密码，就能开始用。

**核心功能一览：**

- **实时加解密**：解锁保险库后，文件在虚拟盘里以明文呈现，实际写入云盘的永远是密文
- **Recovery Key 恢复机制**：Vault Format 8 支持生成一个独立的恢复密钥，忘了密码也能靠它找回数据
- **多保险库并行**：一个应用里可以同时管理和挂载多个保险库，各自独立密码、独立 Recovery Key、独立挂载盘符
- **跨平台**：桌面端覆盖 Windows、macOS、Linux，移动端有 Android 和 iOS 原生 App
- **Cryptomator Hub（付费增值）**：多设备同步保险库配置、团队共享保险库、自部署或托管两种模式

> [!TIP]
> Cryptomator 不收集任何个人数据——这一点在 Skymatic 的隐私声明里写得很清楚。你的密码、Recovery Key、主密钥、云盘账号信息全部只存在你自己的设备上，公司既看不到也存不了。

**适合谁用？**

- 已经在用云盘但又担心服务商数据泄露的个人用户
- 有大量敏感文件（证件扫描件、密码备份、私人笔记）需要云端备份的人
- 小团队想用共享云盘协作但不想买昂贵的企业加密方案
- 对开源透明度有要求、不愿信任闭源加密软件的技术用户

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://cryptomator.fedq.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：移动端 Android 搜 Cryptomator（Google Play）、iOS 搜 Cryptomator（App Store）；桌面端 Windows / macOS / Linux 均从官网获取安装包

---

## 🔧 安装步骤

Cryptomator 的安装在各平台上都比较轻量——桌面端是单文件安装包，移动端直接从应用商店装。下面分平台说明注意事项。

### Windows

1. 从官网下载 `.exe` 安装程序，双击运行
2. 选择安装路径（默认 `C:\Program Files\Cryptomator`，也可以自定义到非系统盘）
3. 安装过程中会提示是否创建桌面快捷方式，按需勾选
4. 安装完成后首次启动，系统会弹出 FUSE 驱动安装提示（Cryptomator 依赖 FUSE 来挂载虚拟盘）——**一定要允许安装**，否则保险库无法解锁挂载

> [!WARNING]
> 部分杀毒软件可能会把 Cryptomator 的 FUSE 驱动标记为可疑——这是误报，可以在杀毒软件里把 Cryptomator 加入白名单。驱动来自官方签名，不会有安全问题。

### macOS

1. 下载 `.dmg` 文件，打开后把 Cryptomator 拖入 Applications 文件夹
2. 首次启动时系统可能弹出"无法验证开发者"提示，去 **系统设置 → 隐私与安全性** 里点"仍要打开"即可
3. macOS 自带 FUSE 驱动（macFUSE），Cryptomator 安装时会自动处理依赖

### Linux

- Debian / Ubuntu：下载 `.deb` 包，`sudo dpkg -i cryptomator_*.deb`，缺依赖的话 `sudo apt-get install -f` 补装
- Fedora / openSUSE：下载 `.rpm` 包，`sudo rpm -ivh cryptomator_*.rpm`
- Linux 下需要系统预装 FUSE 或 fuse3

### Android / iOS

- 直接在 Google Play 或 App Store 搜索 "Cryptomator" 下载安装即可
- 移动端 App 不需要 FUSE 驱动，而是通过云盘原生 API 直接访问云端密文目录，在 App 内部解密展示

### 装完怎么核对

启动应用后，主界面应该呈现一个干净的保险库列表（首次安装是空的），底部有版本号可以核对。点击右上角「+」→ **Create New Vault** 能正常打开创建向导，说明安装没问题。

---

## 📖 使用入门

### 创建第一个保险库

打开 Cryptomator 主界面，点击右下角的「+」按钮，选择 **Create New Vault**。创建向导会引导你完成四步：

1. **选择存放位置**：保险库要建在一个本地文件夹里——这个文件夹就是你的云盘同步目录（比如 Dropbox 的 `C:\Users\你\Dropbox\MyVault`），Cryptomator 会在里面创建加密子目录
2. **设置密码**：至少 8 位。建议用 Diceware 密码短语（3-5 个随机单词拼起来，好记且熵值高），或者直接存进密码管理器
3. **生成 Recovery Key**：**一定要做这一步**——Recovery Key 是 Vault Format 8 引入的核心安全特性，忘了密码时靠它能重建主密钥。生成后把它存到安全的地方（密码管理器、纸质笔记、另一台设备），**不要跟保险库放在同一个云盘里**
4. **选择挂载点**：Windows 选一个没被占用的盘符（比如 `Y:`），macOS 选一个挂载目录，Cryptomator 会自动帮你找空闲盘符

> [!TIP]
> Recovery Key 的安全存储建议：密码管理器存一份、手写一份放保险箱、另一台设备的加密存储里放一份。三份足够保险了。

### 解锁与日常使用

创建完保险库后，它会自动出现在主界面的列表里。日常操作就是：

1. **解锁**：选中保险库 → 点击 **Unlock** → 输入密码（或用 Recovery Key）
2. **读写**：解锁后系统会出现一个虚拟盘（比如 `Y:`），跟普通文件夹一样用——拖文件进去、新建文档、编辑保存，全都是透明的
3. **锁定**：用完后点击 **Lock** 或者直接关闭 Cryptomator，虚拟盘会自动卸载
4. **自动锁定**：在保险库设置里可以配超时自动锁定（比如锁屏 1 分钟后、空闲 10 分钟后），防止人离开时保险库一直挂着

### Vault Format 版本检查与迁移

Cryptomator 从 2015 年到现在经历了多个 Vault Format 版本迭代。当前默认是 **Format 8**（2021 年 10 月引入）。如果你是老用户，**Format 7 及更早版本有明显的功能缺失**：

| 功能 | Format 7 | Format 8 |
| --- | --- | --- |
| Recovery Key | ❌ 没有 | ✅ 完整支持 |
| Recovery Key 重置密码 | ❌ | ✅ |
| Recovery Key 重建 masterkey | ❌ | ✅ |
| Cryptomator Hub | ❌ 不能导入 | ✅ 原生支持 |
| EventView / Decrypt File Name | ❌ | ✅ |

**怎么检查自己的保险库版本？** 打开保险库目录看一眼：

- 有 `vault.cryptomator` 文件 → Format 8 或以上（正常）
- 有 `masterkey.cryptomator` 但没有 `vault.cryptomator` → Format 7 或更早（需要迁移）

迁移的最稳妥做法不是用 Cryptomator 自带的迁移功能（容易中途失败留下混合格式），而是**新建一个 Format 8 保险库，把数据拖过去**：

1. 打开 Cryptomator → 新建保险库（这一步自然就是 Format 8）
2. 解锁旧保险库 → 把里面所有文件拖到新保险库的解锁状态里
3. 等加密写入 + 云盘同步完成
4. **换另一台设备验证新保险库能正常打开**
5. 确认无误后，删除旧保险库的密文目录（本地 + 云端），清空云盘版本历史

### 多保险库并行管理

大多数人其实需要不止一个保险库——至少个人的和工作的应该分开。Cryptomator 完全支持在一个应用里管理多个保险库，每个都有独立的密码、Recovery Key、挂载盘符和自动锁定时间。

并行解锁时，每个保险库会在系统上挂一个独立虚拟盘。比如同时解锁三个，Windows 上会出现 `Y:\`、`Z:\`、`AA:\` 三个盘符，Cryptomator 自动从 Z 往 A 倒序找空闲盘符，也可以在保险库设置里手动指定。

多保险库管理里有几个常见的坑要注意：

| 坑 | 说明 | 建议 |
| --- | --- | --- |
| 所有保险库用同一个密码 | 一个泄露等于全部失守 | 用 Diceware 密码短语 + 密码管理器 |
| 保险库互相嵌套 | 在一个保险库的虚拟盘里建另一个 | 每个保险库放在独立的本地文件夹 |
| 删掉的保险库想找回来 | 右键 Remove 只是从列表移除 | Open Existing Vault 找到 masterkey 重新添加 |
| 云盘版本历史没清 | 删了旧保险库但版本历史还在 | 迁移完成后清空云盘的版本历史 |

### 安全性为什么值得信任

Cryptomator 的安全设计建立在三个公开可验证的支柱上：

**开源（GPLv3）+ 10 年持续维护**。桌面端核心、Android、iOS、Hub 全部代码公开在 GitHub 上，加密逻辑、FUSE 实现、密钥派生谁都能看。加密原语用的是 AES-256、scrypt、AES Key Wrap——都是 NIST 推荐的工业标准，没有自造轮子。

**独立安全审计**。Skymatic 定期委托专业安全公司做完整审计，最近几次包括 Cure53（2021、2025）和 NCC Group（2023），审计报告公开可查，发现的问题会在 changelog 里公开披露并修复。

**持续自动化安全测试**。GitHub Actions 里每个 PR 都跑 CodeQL、SpotBugs、单元测试（覆盖率 > 90%）和 Fuzzing 模糊测试，任何新提交引入的安全问题会在合并前就被 CI 拦下。

10 年的安全记录里，**没有发现过主密钥被绕过的漏洞**，也没有爆出过后门或窃取密码的丑闻——这在开源加密工具里属于上层水准。

### 隐私设计的取舍

Cryptomator 的隐私定位是"默认不采集"——连注册账号都不需要，自动更新发送的版本号还是可选可关的。但这种隐私设计也意味着某些便利功能**故意不做**：没有云备份保险库、没有跨设备同步密码、没有找密码功能、没有云端 Recovery Key 备份。这些都不是"还没实现"，而是隐私和便利此消彼长的结果——隐私越强，需要云端存储数据的便利功能就越少。

如果你对云盘服务商本身的数据可见性也介意（毕竟云盘知道你有个文件夹在同步），可以考虑 rclone 加密云盘层 + Cryptomator 本地加密的双重加密方案，或者自建 Nextcloud / ownCloud / MinIO 私有云彻底绕开第三方。

---

## 🔗 相关资源

- [cryptomator官网](https://cryptomator.fedq.ren)
- [cryptomator文档](https://cryptomator-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%AD%A3%E7%89%88/cryptomator)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%AD%A3%E7%89%88/cryptomator)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%AD%A3%E7%89%88/cryptomator)

## 📚 相关文章

- [Cryptomator 用了半年我踩过的那些坑——从备份翻车到 RAM 丢数据](https://laoguosofts.github.io/posts/cryptomator-docs.html)

- [Cryptomator 用了半年踩过的七个坑，第三个差点让我丢了所有文件](https://alipymanbu.github.io/posts/cryptomator-docs.html)
