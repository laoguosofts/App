## ✨ JsHook 是什么——Android 上的 Hook 框架一站式工具

JsHook 是一款面向 Android 平台的 Hook 框架管理应用，核心定位是把 frida 生态里常用的组件（fridamod、fridainject、fridagadget、内核驱动模式）打包成一个可直接操作的图形化工具。开发者或逆向分析人员可以用它在不需要电脑端 frida-server 的情况下，直接在手机上完成脚本注入、内核驱动加载、脚本加密保护等工作。

它的主要能力包括：支持多种注入模式（普通 fridamod 注入 + 内核驱动模式）、自带脚本加密工具保护源码不被直接阅读、可以订阅远程脚本仓库批量管理脚本、以及提供 VS Code Copilot 的 MCP 服务让 AI 辅助生成 Hook 脚本。对于不想折腾电脑端 frida-server 部署、也不想手动编译内核驱动的用户来说，JsHook 把门槛降低了不少。

适用人群大致分两类：一类是逆向分析或安全研究人员，需要在真机上快速验证 Hook 思路；另一类是有 Android 开发基础、想给自己或团队写的工具脚本加一层源码保护的开发者。普通用户如果只是想跑别人写好的脚本，也可以用——前提是设备已 root。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://jshook.xnyx.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 平台，直接从下载页获取 APK 安装包，Google Play 目前未上架，需开启未知来源安装权限

---

## 🔧 安装步骤与前置准备

JsHook 只支持 Android，且设备必须已经获取 root 权限（Magisk 或其他 root 方案均可）。安装过程本身不复杂，但有几个前置条件需要先确认。

### 环境检查

1. **确认 root 状态**：用 Magisk Manager 看版本号，或者在终端里执行 `su` 看能不能拿到 root shell。
2. **确认 SELinux 状态**：在终端里跑 `getenforce`，如果输出 `Enforcing`，后面激活内核驱动模式时可能需要临时切成 `Permissive`（`setenforce 0`）。
3. **网络连通性**：JsHook 的内核驱动包和脚本仓库列表走 GitHub 托管，大陆直连经常超时。装之前先确保手机能访问 GitHub——要么开全局 VPN，要么准备好手动下载驱动的兜底方案。

### 安装 APK

1. 从上面的下载入口拿到最新版 APK 文件，传到手机（USB 连接、网盘下载都行）。
2. 在手机文件管理器里点击 APK，首次安装未知来源应用会弹窗，允许即可。
3. 装完打开 App，按提示授予 root 权限。

> [!WARNING]
> 某些定制 ROM（小米 MIUI、OPPO ColorOS 等）对未知来源安装有额外拦截，如果系统设置里找不到开关，可以直接在浏览器里打开 APK 链接下载，浏览器会自动处理安装流程。

### 首次激活

打开 JsHook 主界面后，点击「激活」按钮，它会自动检测当前内核版本、释放匹配的 `.ko` 内核驱动文件到 `/data/adb/jshook/.kernel/`，然后尝试加载。正常情况下这一步全自动，不需要手动干预。

---

## 📖 使用入门——注入、加密、AI 联动三条主线

JsHook 的功能集中在三个方向：脚本注入与管理、脚本加密保护、以及和 VS Code Copilot 的 MCP 联动。下面分别展开。

### 脚本注入的两种模式

JsHook 支持两种注入框架，用户可以按需求切换：

- **fridamod**：最常用的注入方式，Hook 以模块形式加载到目标应用进程里，大多数脚本默认都是 frida 引擎语法。
- **内核驱动模式**：需要先加载 `.ko` 内核模块，加载成功后会在内存层面提供 `kernel.getModuleBase`、`kernel.readFloat` 等 API，适合做 native 层逆向。激活时自动匹配当前内核版本的 `.ko` 文件，极少数定制 ROM 或虚拟机的内核版本没有匹配项时，内核驱动模式开关能打开但一开 Hook 服务就会报错，这种情况只能退回普通 fridamod 注入，或者去 JsHook 的社区报告内核版本号等作者更新支持。

### 脚本加密——保护你的源码不被直接阅读

写好的 `.js` 脚本如果不想让别人拿到明文源码，JsHook 自带加密工具可以处理：进「工具集」页面，找到「脚本加密」入口，选要加密的文件，执行后会生成一个新文件，原文件自动保留作备份。加密后的脚本文件头有特殊标记，JsHook 自动识别并在运行时解密。

有两个硬限制需要注意：

| 对比项 | fridamod 框架 | fridainject / fridagadget |
|---|---|---|
| 加密脚本支持 | ✅ 自动解密执行 | ❌ 不支持，加密脚本无法加载 |
| 内核驱动模式兼容 | ✅ 解密在内存完成 | 不适用 |

加密前一定要保留原始 `.js` 文件——JsHook 不提供解密功能，加密是单向的。另外加密只是第一道防线，脚本最终还是会在内存里解密执行，如果对安全要求更高，可以考虑把关键逻辑搬到 native 层编译成 `.so`，JS 层只做壳。

### VS Code Copilot MCP——让 AI 帮你写 Hook

JsHook 提供了 MCP（Model Context Protocol）服务，VS Code Copilot 可以直接通过这个端口操作手机上的 JsHook，形成一个闭环工作流：

> [!TIP]
> 给 Copilot 的需求描述越具体，生成质量越高。务必说清楚：目标应用的包名、想 Hook 的具体行为（比如「金币变化」「购买按钮」「登录请求」）、想用的引擎（frida / rhino）、以及有没有内核模式需求。

典型的交互过程是这样的：

```
你: "帮我找到 com.example.app 里处理购买的方法，生成 Hook 代码"
Copilot: 调用 MCP 工具搜索加载的类 → 找到 Shop.purchase(String, int) → 生成 Java.use 代码 → 写入 test.js
你: 推到手机 → 重启目标应用 → 看效果
```

Copilot 能帮你自动查找类和方法、生成完整可运行的脚本（包括正确的 `arguments` 转发、Frida 兼容的字符串拼接格式等细节），甚至在内核模式下自动生成 `runtime.isKernel` 判断分支和 `kernel.getModuleBase` 的正确用法。但 AI 生成的代码仍然需要人工审核——类名对不对、重载指定对不对、有没有漏掉关键参数，这些都要过一遍，先跑空脚本确认注入本身没问题，再加 AI 生成的脚本。

### 排查：Server request failed 和内核驱动加载失败

使用过程中最常见的两个问题，一个是网络相关的，一个是内核驱动相关的。

**Server request failed** 表现为 JsHook 里内核驱动下载失败、脚本仓库列表加载不出来，根因是官方在线服务部分路径走 GitHub 托管，大陆直连经常超时。最快的解决办法是开全局 VPN，或者在电脑上网络好的环境手动下载内核驱动包，通过 `adb push` 传到手机对应目录。如果只用本地脚本注入，这条报错可以直接忽略。

**内核驱动加载失败** 则是激活时的问题。检查步骤：

1. 确认内核版本匹配：`adb shell uname -r` 拿版本号，看 JsHook 自带的驱动包里有没有对应的 `.ko`。
2. 临时关掉 SELinux：`adb shell su -c setenforce 0`，很多 Enforcing 模式下驱动加载会被 SELinux 阻止。
3. 查驱动是否加载成功：`adb shell su -c dmesg | grep jshook`，成功时应该能看到类似 `jshook: driver loaded successfully` 的日志行。

---

## 🔗 相关资源

- [jshook官网](https://jshook.xnyx.quest)
- [jshook文档](https://jshook.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%AD%A3%E7%89%88/jshook)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%AD%A3%E7%89%88/jshook)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%AD%A3%E7%89%88/jshook)

## 📚 相关文章

- [从普通用户到离不开内核驱动——jshook 用了大半年的真实感受](https://laoguosofts.github.io/posts/jshook.html)
