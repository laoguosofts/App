## ✨ LSPatch 是什么

LSPatch 是一个免 Root 的 Android Xposed 框架实现。它不走传统的刷 Magisk 模块路线，而是直接给目标 APK 打补丁，把 LSPosed 的运行时框架嵌入应用本身。这样做的好处很直接——不需要解锁 Bootloader、不需要 Root、不需要刷任何东西，装完补丁版 APK 就能用 Xposed 模块。

整个方案由两个部分组成：一个是电脑端或手机端的 Manager（负责选应用、调参数、执行修补），另一个是 LSPosed 的核心框架（在修补过程中被注入到 APK 里）。修补完成后生成的 APK 自带 Xposed 运行时，启动时会自动去加载你配置好的模块。

它适合几类场景：不想 Root 但想玩 Xposed 模块的普通用户、有特殊模块需求但厂商系统不给解锁的手机、以及需要批量给一组应用打补丁的折腾党。主流的微信增强、微博猪手、抖音去水印、浏览器扩展类模块，只要标记了支持 Non-Root 或 LSPatch，基本都能跑。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://lspatch.yoan.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜索 LSPatch；酷安有第三方维护的 opatch / onpatch 分支

---

## 🔧 安装步骤

LSPatch 的安装分两步：先装 Manager，再装 JRE（如果你要在电脑上用 CLI 修补）。手机端修补不需要 JRE，但功能上和电脑端一样。

### 手机端 Manager 安装

1. 从下载页拿到 Manager 的 APK 文件，传到手机任意目录
2. 如果是 Android 8+，安装时会提示「未知来源」，去系统设置 → 应用 → 特殊访问权限 → 安装未知应用，给你用来传文件的那个 APP（比如文件管理器或浏览器）开权限
3. 安装完成后打开 Manager，它会请求存储权限和 Shizuku 权限

> [!TIP]
> Shizuku 权限推荐配对上。配对后 Manager 可以直接调用 `pm install`，修补完成自动装包，省一次手动操作。配对方式：开发者选项里打开「无线调试」，在无线调试页面点「配对设备」，记下配对码和端口号，回到 Manager 输入即可。

### 电脑端 CLI 环境（可选）

如果想批量修补或想用命令行精细控制参数，需要额外装 Java：

1. 下载 JRE 11 或以上版本（推荐 17 LTS）
2. 安装时注意勾选「设置 JAVA_HOME 环境变量」
3. 打开 PowerShell 或 CMD，跑 `java -version` 确认能输出版本号
4. 下载 LSPatch 的 `lspatch.jar`，放到一个固定目录

> [!WARNING]
> JRE 版本必须 ≥ 11。用 8 或 9 会直接报 `UnsupportedClassVersionError`，别折腾，升级就是了。

### 核对安装是否正确

- 手机端：打开 Manager → 应用管理 Tab 能看到已装应用列表，说明 Manager 正常
- 电脑端：`java -jar lspatch.jar --help` 能打印出参数帮助，说明 jar 文件没问题

---

## 📖 使用入门

LSPatch 的核心工作流很简单：**选应用 → 选模式 → 调参数 → 修补 → 装包 → 配模块作用域 → 重启应用**。但两个模式（本地 vs 便携）的后续操作完全不同，先搞清楚选哪个。

### 本地模式 vs 便携模式

| 维度 | 本地模式 | 便携模式 |
| --- | --- | --- |
| 修补时嵌入模块？ | 不嵌入，只打框架 | 直接把模块 dex 嵌进 APK |
| 运行时谁加载模块？ | Manager 动态加载 | APK 内嵌的加载器自己加载 |
| 换模块方便吗？ | 方便，Manager 里改作用域就行 | 不方便，要重新修补 |
| 需要 Manager 在后台跑吗？ | 需要（负责动态加载） | 不需要，独立运行 |
| 适合场景 | 频繁换模块、多模块组合试错 | 固定模块组、想打包分享给别人 |

**新手建议从本地模式开始**，灵活度高，试错成本低。等你确定了模块组合想固化下来再切便携模式。

### 本地模式完整流程

**第一步：选目标应用**

打开 Manager → 应用管理 → 右下角 **+**。两个选项：
- 从已装应用列表选（最常用）
- 从存储目录选 APK 文件（适合手头只有安装包还没装的情况）

选好应用后进入修补配置页。

**第二步：确认本地模式**

参数面板顶部或中部有模式切换，确保选的是「本地加载器模式」。本地模式下**不需要选模块**——模块留到运行时在 Manager 里配。

**第三步：参数保持默认**

99% 的情况参数不用动：
- 注入文件提供者：默认开着，别动
- 输出日志到目录：默认关，出问题时再临时开
- 覆写版本号：别动，会导致应用商店一直提示有更新
- 签名模式：默认自动覆盖

> [!NOTE]
> 如果你修补的应用有加固，修补完成后装不上、报签名错误，回来把签名模式改成「先清除再签名」试试——这会彻底移除原签名块再加新签名，能绕开大部分加固的签名校验。

点右下角对勾开始注入。进度走完后修补好的 APK 会输出到你之前选的目录。

**第四步：卸载原版 → 安装修补版**

修补版和原版签名不一样，不能覆盖安装。先去系统应用管理手动卸载原版（记得备份应用内数据），然后回到 Manager 点「安装」，或者手动把输出目录的 APK 传到手机装。

> [!WARNING]
> 修补完成后如果 Shizuku 是连着的，Manager 会自动尝试安装——但这时候原版还在，肯定会报签名不匹配错误。**别让它装**，先手动卸载原版再说。

**第五步：配置模块作用域**

这是最容易被忽略、但决定模块能不能生效的关键一步。Manager → 应用管理 → 点刚才装的修补版应用 → **模块作用域**。弹出模块列表后，勾选你想让这个应用加载的模块。

这里有个反直觉的地方：LSPatch 本地模式是「应用 → 模块」的勾选逻辑（点应用条目，勾哪些模块给它用），而 LSPosed 是「模块 → 应用」（点模块，勾它作用于哪些应用）。两种方式效果一样，只是入口不同，别搞错方向。

**第六步：强制停止 → 清缓存 → 重开**

LSPatch 在应用进程启动时初始化。如果应用已经在跑了，模块来不及注入。所以：

1. 系统设置 → 应用管理 → 目标应用 → **强制停止**
2. 存储 → **清除缓存**（清除数据会删应用内部文件，谨慎）
3. 重新打开应用

### 模块不生效的排查清单

按这个顺序查，90% 的问题能自己解决：

1. **模块真的装到手机上了吗？** Manager → 模块管理 Tab 里能看到吗？
2. **强制停止 → 重开做了吗？** 应用已经在跑的话模块没法注入
3. **模块支持 LSPatch / Non-Root 吗？** 看模块仓库或说明文档有没有标记
4. **作用域勾对了吗？** 确认目标应用的作用域里确实勾了那个模块
5. **版本匹配吗？** 模块版本和 LSPatch 核心版本差距太大可能 API 不兼容

> [!TIP]
> 误区提醒：作用域勾多了模块不会更快生效，反而每个模块都要扫一遍宿主应用的方法，启动更慢、冲突更多。**宁缺毋滥**——一个应用配两三个模块就够了。

### 电脑端 CLI 批量修补

如果你有一批 APK 要打同样的补丁，用 `lspatch.jar` 命令行可以批量跑。基本语法：

```
java -jar lspatch.jar [全局选项] <APK路径> [修补选项]
```

常用参数：

| 参数 | 作用 |
| --- | --- |
| `-o, --output <path>` | 指定输出目录 |
| `-m <module.apk>` | 便携模式嵌入模块，可多次指定 |
| `--clear-sign` | 修补前先清除原签名（加固应用用） |
| `-v, --verbose` | 输出详细日志（调试用） |

给 TikTok 打便携补丁的完整示例：

```powershell
cd D:\Tools\LSPatch

java -jar lspatch.jar ^
  input\tiktok.apk ^
  -o output ^
  -m modules\tiktok_download.apk ^
  -m modules\tiktok_adblock.apk ^
  --clear-sign ^
  -v
```

批量修补一个目录里所有 APK 的 PowerShell 脚本：

```powershell
$jar = "D:\Tools\LSPatch\lspatch.jar"
$input = "D:\Tools\LSPatch\input"
$module = "D:\Tools\LSPatch\modules\default_module.apk"
$output = "D:\Tools\LSPatch\output"

Get-ChildItem $input -Filter *.apk | ForEach-Object {
    java -jar $jar $_.FullName -o $output -m $module
}
```

注意批量前确认模块对所有目标 APK 都适用——不同应用可能需要不同模块组，别一把梭搞完发现一半不生效。

### 日常维护要点

- **换模块**（本地模式）：回到 Manager 改作用域 → 强制停止应用 → 重开，**不需要重新修补**
- **更新模块**（本地模式）：直接更新模块 APK → 强制停止目标应用 → 重开，**不需要重新修补**
- **更新宿主应用**：下载新版 APK → 卸载旧修补版 → 修补新版 → 安装 → 配作用域，**需要重新修补**
- **Manager 自身更新**：更新后修补过的应用可能显示「可优化」，点一下 Manager 会自动重跑修补同步新版核心

---

## 🔗 相关资源

- [LSPatch官网](https://lspatch.yoan.quest)
- [LSPatch文档](https://lspatch.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/LSPatch)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/LSPatch)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%89%8B%E6%9C%BA%E7%89%88/LSPatch)

## 📚 相关文章

- [LSPatch 用了大半年：从 Shizuku 连不上到脚本化批量修补](https://laoguosofts.github.io/posts/lspatch.html)
- [LSPatch 用了一年半：从"终于不用解锁 BL"到"它替我挡了多少麻烦"](https://alipymanbu.github.io/posts/lspatch.html)