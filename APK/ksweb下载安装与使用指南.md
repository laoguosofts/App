## ✨ KSWEB 是什么：把安卓手机变成一台完整的 Web 开发服务器

KSWEB 是一款运行在 Android 平台上的集成式 Web 服务器套件。它把 Lighttpd / Nginx / Apache 三款 Web 服务器、MySQL 5.7 与 MariaDB 10.4 双数据库、PHP 解释器、phpMyAdmin 数据库管理界面、Msmtp 命令行发信工具等组件打包在一起——在手机上装好这一个应用，就能完整跑起 WordPress、Joomla、Drupal 这类典型的 PHP+MySQL CMS，也能用来做接口调试、脚本执行、内网穿透测试。

和传统 Linux VPS 相比，KSWEB 的优势在于随身携带、零月租、拿起手机就能搭站；局限也很明显：手机厂商的后台策略会杀服务、4G/5G 运营商 NAT 让公网直连比较折腾、长时间运行发热耗电。定位上它更像是一台"口袋开发测试机"，而不是替代传统服务器的生产环境。

适合人群：想在移动设备上调试 PHP 项目的开发者、折腾 CMS 的建站爱好者、需要临时搭建内网服务的运维人员。如果你只想长期跑一个正式站点，每月几十块的 VPS 稳定性还是比手机靠谱得多。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://ksweb.wxhp.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Google Play 搜 KSWEB；部分国产应用商店也有上架（注意核对包名 ru.kslabs.ksweb 避免下到仿冒品）

---

## 🔧 安装步骤：Android 平台的常规安装流程

KSWEB 是 Android APK 应用，安装流程和其他 APK 一样，但有几个容易踩的坑值得提前注意。

### 安装前核对

在下载完成后、点击安装之前，先核对两件事：

1. **包名**：`ru.kslabs.ksweb`——应用的官方包名，任何其他包名都不是原版
2. **版本号**：下载页标注的最新版本号和你手上的 APK 版本号是否一致

> [!WARNING]
> 第三方应用商店有时会上架旧版本或修改版，尽量从官方渠道获取安装包，避免遇到已经修复的 bug。

### 安装过程

1. 在 Android 设置里开启**未知来源应用安装权限**（路径因系统而异，通常在「设置 → 安全 → 允许安装未知来源应用」或「设置 → 隐私 → 特殊应用权限」）
2. 用文件管理器定位到下载的 APK 文件，点击安装
3. 系统可能弹出「Google Play Protect 检测到未知应用」之类的安全提示，选择继续安装即可
4. 安装完成后，**先不急着打开**——先去下一节按手机品牌设置后台白名单

### 首次启动

第一次打开 KSWEB 时，应用会请求存储权限和网络权限，全部允许即可。进入主界面后，你会看到几个组件标签页：WEB、MYSQL、MARIADB、PHP、PYTHON、PERL 等——这就是整个服务器的控制面板。

---

## 📖 使用入门：从建站到外网访问的完整路径

KSWEB 的能力面很广，下面从数据库配置、CMS 部署、邮件发送、后台稳定性、80 端口问题几个方面展开，覆盖日常使用中最常遇到的场景。

### 数据库：MySQL 5.7 与 MariaDB 10.4 双引擎

KSWEB 内置了 MySQL 5.7.34 和 MariaDB 10.4.34，两个数据库**独立启停**，不能同时监听 3306 端口——开一个就行，官方文档推荐先用 MySQL。

| 项目 | MySQL | MariaDB |
|---|---|---|
| 版本 | 5.7.34 | 10.4.34 |
| 默认监听 | 3306 | 3306（需停 MySQL） |
| 默认账号 | root / 空密码 | root / 空密码 |
| 主机 | localhost 或 127.0.0.1 | 同左 |
| 启动入口 | MYSQL 标签页开关 | MARIADB 标签页开关 |

> [!TIP]
> root 空密码只适合本地开发测试，部署到可外网访问的环境后**必须立刻改掉**——在 phpMyAdmin 或 Adminer 里执行 `ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码';` 即可。

跑 WordPress、Joomla、Drupal 这类 CMS 时，数据库连接参数直接填：主机 `localhost` 或 `127.0.0.1`，端口 `3306`，用户名 `root`，密码空，数据库名在 phpMyAdmin 里先建好（比如 `wordpress`）。

数据库图形化管理不通过命令行——KSWEB 在 TOOLS 标签页里内置了 phpMyAdmin 和 Adminer，点一下就在浏览器里打开管理界面，建库建表、导入导出 SQL 备份都点几下完成。

MySQL 启动失败时常见三个原因：3306 端口被 MariaDB 或其他应用占用、上次断电导致数据文件损坏（可在 MYSQL 标签页点 Repair 尝试修复）、Android app 私有目录存储空间不足。

### CMS 邮件发不出去？配置 Msmtp 解决

WordPress 安装、密码重置、订单通知这些功能都依赖 PHP 的 `mail()` 函数。但 Android 没有系统级的 MTA（邮件传输代理），直接调 `mail()` 会报错或被拦截。

KSWEB 内置的 **Msmtp v1.8.32** 就是专门解决这个问题的——它是一个轻量级命令行 SMTP 客户端，通过外部邮箱的 SMTP 服务代发邮件。

配置分三步：

1. **到邮箱获取 SMTP 凭证**：以 Gmail 为例，需要先到 Google 账号安全设置里开**应用专用密码**（不是常规登录密码）
2. **写 Msmtp 配置文件**：

   ```
   account gmail
   host smtp.gmail.com
   port 465
   from 你的Gmail@gmail.com
   user 你的Gmail@gmail.com
   password 应用专用密码
   tls on
   auth on

   account default : gmail
   ```

3. **改 php.ini 让 PHP 用 Msmtp**：在 KSWEB PHP 标签页点 Edit，找到 `sendmail_path` 这一行改成：

   ```ini
   sendmail_path = /data/data/ru.kslabs.ksweb/files/msmtp/bin/msmtp -t
   ```

   保存后重启 Web 服务器即可。

其他常见邮箱的 SMTP 参数速查：

| 邮箱 | 主机 | 端口 |
|---|---|---|
| Gmail | smtp.gmail.com | 465 |
| QQ 邮箱 | smtp.qq.com | 465 |
| 163 邮箱 | smtp.163.com | 465 |
| Outlook | smtp.office365.com | 587 |

> [!NOTE]
> 虽然改 php.ini 后 `mail()` 能工作，但推荐在 CMS 里用**专用 SMTP 插件**——比如 WordPress 的 WP-Mail-SMTP、Joomla 全局配置里的 SMTP 方式、Drupal 的 SMTP Authentication Support 模块。插件方案让邮件发送不依赖 `mail()` 函数本身，也方便追踪发信日志。

Msmtp 和普通邮件客户端的关键区别在于它是**命令行工具**——PHP 通过 exec() 或 shell 调用它，发信完全自动化，CMS 注册、密码重置、评论通知全不用人工干预。

### 后台稳不稳：各厂商系统省电策略差异大

同样开着 KSWEB 的"保护服务器被非授权终止"开关，在 Google Pixel 上能连续跑一周不被杀，在小米上跑两小时就停了。根源是**国产 ROM 在 Android 原生后台管理之上叠加了自己的一套额外规则**，优先级高于系统设置。

#### 原生 Android（Pixel / LineageOS / OxygenOS）——最省心

后台策略最宽松，前台服务几乎永远不会被杀。只需要开 KSWEB SETTINGS 里的三个开关就行，不用去系统设置里找。

#### 小米 / Redmi（MIUI / HyperOS）——最凶

有"后台限制""自启动管理""电池优化""省电策略"四套机制叠加，默认情况下任何第三方后台服务几分钟就被杀。**必须做**：

1. 设置 → 应用设置 → 应用管理 → KSWEB → 省电策略 → **无限制**
2. 设置 → 应用设置 → 应用管理 → KSWEB → 自启动 → **允许**
3. 设置 → 电池与性能 → 右上角齿轮 → 省电模式 → 把 KSWEB 加到**不受限制的应用列表**
4. 如果以上都做了还断：设置 → 应用设置 → 应用管理 → KSWEB → 权限 → 打开"后台弹出界面""显示悬浮窗"

HyperOS 设置路径和 MIUI 稍有不同，搜索"省电策略""后台活动""自启动"能找到对应项。

#### OPPO / realme（ColorOS）

后台策略同样严格：

1. 设置 → 应用 → 应用管理 → KSWEB → 电池 → **无限制**
2. 设置 → 应用 → 自启动管理 → 允许 KSWEB
3. 设置 → 电池 → 耗电保护 → 把 KSWEB 设为"允许后台活动"

#### 华为 / 荣耀（HarmonyOS / EMUI）

后台管理叫"应用启动管理"：

1. 设置 → 应用和服务 → 应用启动管理 → 找到 KSWEB → **手动管理**
2. 打开"自动启动""关联启动""后台活动"三个子开关
3. 设置 → 电池 → 启动管理 → KSWEB → 关掉"自动管理"

#### 三星（OneUI）

后台策略相对温和：

1. 设置 → 应用 → KSWEB → 电池 → **不受限制**
2. 设置 → 应用 → 特殊访问 → 优化电池使用量 → 找到 KSWEB → 设为**不优化**
3. 设置 → 电池和设备维护 → 电池 → 右上角 → 设置 → 自动休眠下的应用 → 设为"从不"

#### vivo / iQOO（OriginOS）

和 ColorOS 类似：

1. 设置 → 应用与权限 → 应用管理 → KSWEB → 省电策略 → **无限制**
2. 设置 → 应用与权限 → 自启动 → 允许 KSWEB

> [!WARNING]
> 改完之后一定要做**睡眠测试**：开 Lighttpd + MySQL，屏幕灭放一边，10 分钟、1 小时、一整夜（8 小时）各检查一次——夜间深度休眠才是真正考验。哪个时间点服务停了就说明哪个设置没改对。

如果是 MIUI / ColorOS 这类策略最凶的系统，所有设置都改了还是被停，可以开发选项里开"充电时保持唤醒"，或者考虑换一台原生 Android 设备。实在不行就放弃手机长期挂机——每月二十块的 VPS 稳定性是手机的一百倍，手机当开发测试主机足够了。

### 80 端口问题：Android 系统级限制

Linux 上 1024 以下的端口叫"特权端口"，只有 root 用户能绑定。Android 继承了这个限制，KSWEB 本身是普通应用没有 root 权限，所以：

- 默认绑定 **8080 / 8000 / 9090** 这类高位端口 → 直接跑
- 想绑定 **80 / 443** 端口 → **必须 root 手机**

这是内核级限制，绕不开。

日常使用**推荐高位端口 + 穿透工具**的方案，更稳更安全，也不用 root 手机：

| 方案 | 适用场景 | 说明 |
|---|---|---|
| **frp + VPS** | 对外服务 | 手机监听 8080 → frpc 穿透到 VPS → VPS 上 Nginx 反代 80/443 → 外网访问带域名还能配 HTTPS |
| **Tailscale** | 自己或团队内部访问 | 组网后电脑和手机各有 100.x.x.x 固定 IP，直接 `http://手机IP:8080/` 访问，不走公网且加密 |
| **ngrok** | 临时演示 | 免费版给一个 `https://xxxx.ngrok.io` 地址，自带 HTTPS，即开即用 |

如果坚持要绑 80 端口，需要 root 手机（Magisk 或其他 rooting 工具），然后在 KSWEB SETTINGS → Root Mode 里开启。代价是 root 后大部分厂商保修失效，部分国产系统 root 流程极其复杂，系统更新也会变困难。

> [!NOTE]
> KSWEB 官方文档只在括号里提到"只有当你想把服务器绑定到 80 端口时才需要 root"，并没有把它当成推荐路径。花几十块钱买个 VPS 跑 frp + Nginx 反代，比 root 自己的手机省心得多。

---

## 🔗 相关资源

- [ksweb官网](https://ksweb.wxhp.skin)
- [ksweb文档](https://ksweb-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/ksweb)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/ksweb)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/ksweb)

---

## 📚 相关文章

- [把安卓手机变成服务器——KSWEB 能力面完整盘点](https://laoguosofts.github.io/posts/ksweb-docs.html)
- [KSWEB 横向盘点：把一台手机变成完整的 Web 开发服务器，它做到了什么](https://alipymanbu.github.io/posts/ksweb-docs.html)
