## ✨ LibreTorrent 是什么

LibreTorrent 是一款开源免费的 Android 端 BT 下载客户端，基于成熟的 libtorrent 内核构建，界面简洁、功能完整。相比许多带广告、需付费解锁高级功能的同类应用，LibreTorrent 完全由社区驱动开发，没有任何商业植入，所有功能开箱即用。

核心能力包括：多种子并行下载、协议加密保护、RSS 订阅自动发现新资源、自定义 Tracker 管理、主题切换，以及后台常驻下载。对于需要在手机上稳定下载 BT 资源、又不想忍受广告和订阅墙的用户，LibreTorrent 是目前 Android 平台上口碑最好的选择之一。

> [!NOTE]
> BT（BitTorrent）是一种点对点文件分发协议，下载时你会同时从多个其他用户那里获取文件分片，下载完成后也可以选择继续做种供他人获取。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://libretorrent.hfdk.space/download.html) —— 下载页整理了当前可用的版本和安装说明
> - Google Play 搜索「LibreTorrent」可直接安装最新稳定版
> - GitHub 项目页也有每版对应的 APK 附件

---

## 🔧 安装步骤详解

### 第一步：允许安装未知来源应用

LibreTorrent 的 APK 包不在 Google Play 以外的渠道分发时，Android 会默认拦截第三方来源的安装。安装前需要：

1. 打开系统设置 → 安全（或隐私）
2. 找到「未知来源安装」或「安装未知应用」选项
3. 允许浏览器或文件管理器安装 APK

> [!WARNING]
> 只对可信的来源开启此权限，安装完成后建议关闭，避免误装恶意应用。

### 第二步：获取并安装 APK

从下载页拿到 APK 文件后，点击文件管理器中的 APK 图标，系统会弹出安装确认界面。确认应用名和图标后点击安装即可。

### 第三步：核对版本和权限

安装完成后首次打开应用，检查：

- **版本号**：在设置 → 关于中核对是否为当前最新稳定版
- **存储权限**：首次启动会请求存储访问权限，授权后才能在设备指定目录保存下载的文件
- **电池优化**：Android 系统可能在后台杀死下载进程，建议将 LibreTorrent 加入电池优化白名单，确保后台下载稳定

---

## 📖 使用入门与进阶技巧

### 添加种子开始下载

主界面右下角有「+」按钮，点击后可以选择：

- 从本地文件管理器导入 `.torrent` 文件
- 直接粘贴磁力链接（以 `magnet:?xt=` 开头的字符串）

添加后在任务列表中可以看到当前的下载进度、连接的同伴数量、上传/下载速率。

### 协议加密：让 ISP 无法识别 BT 流量

部分运营商会通过深度包检测（DPI）识别 BT 流量并限速。LibreTorrent 内置了协议加密功能：

**设置 → 网络 → 加密**中有三个选项：

| 选项 | 含义 | 推荐 |
| --- | --- | --- |
| 强制加密 | 只接受加密连接 | 最安全，但可能连接数减少 |
| 优先加密 | 优先加密，对方不支持则退为明文 | 推荐，平衡安全与连接数 |
| 只允许明文 | 关闭加密 | 不推荐 |

加密后的流量在 ISP 眼中看起来与普通 HTTPS 连接类似，无法被轻易识别。需要注意的是，加密无法绕过端口封锁，如果 6881 端口被运营商直接封禁，加密也起不了作用。

### Tracker 服务器管理：让种子找到更多同伴

Tracker 是 BT 网络的「通讯录」——种子能不能被别人找到，取决于它配置了哪些 Tracker 地址。

LibreTorrent 支持在下载中随时补充 Tracker：进入种子详情页 → Tracker 标签 → 点击添加，输入地址后客户端会立即向新 Tracker 报告自己并请求同伴列表。补充 Tracker 是「复活」低健康度种子（下载人数少、速度慢）的常用方法。

创建自己的种子时建议填入 5~10 个 Tracker，优先选 UDP 协议（速度更快、资源占用更低），混合加入一些 HTTP Tracker 以应对 UDP 受限的网络环境。面向中文用户时可以加一个国内 Tracker。

### RSS 订阅：自动跟踪新资源发布

很多 BT 发布站会提供 RSS 源，每当有新种子发布就会推送通知。LibreTorrent 的 RSS 管理器可以：

- 订阅多个 BT 发布源
- 按设定的间隔（比如每 30 分钟）自动检查新种子
- 根据规则自动下载匹配的内容

**添加订阅步骤**：侧边栏打开 RSS 管理器 → 添加订阅 → 粘贴 RSS 源的 URL（通常以 `/rss.xml` 结尾）→ 设置刷新间隔 → 保存。常见的 RSS 格式包括标准 RSS 2.0 XML 和 ATOM 格式，LibreTorrent 对主流格式兼容性较好。

### 主题切换：浅色、深色还是纯黑

LibreTorrent 提供三种界面主题，设置路径为 **设置 → 界面 → 主题**：

| 主题 | 说明 | 最佳场景 |
| --- | --- | --- |
| 浅色（Light） | 默认白底黑字 | 白天室外强光下使用 |
| 深色（Dark） | 暗灰底白字 | 晚上室内、省电模式下 |
| 纯黑（Amoled） | 纯黑底白字 | OLED 屏幕、最省电 |
| 跟随系统 | Android 10+ 自动切换 | 懒人选这个 |

在 OLED 屏幕上，黑色像素会完全熄灭不发光也不耗电，纯黑主题比深色主题节省约 30%~50% 的屏幕功耗。长时间夜间下载时（比如挂一整晚），切换到纯黑主题能显著延长电池续航，也对眼睛更友好。

设置实时生效，不需要重启应用。

---

## 🔗 相关资源

- [LibreTorrent官网](https://libretorrent.hfdk.space)
- [LibreTorrent文档](https://libretorrent.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/LibreTorrent)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/LibreTorrent)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E7%BD%91%E7%89%88/LibreTorrent)

---

## 📚 相关文章

- [LibreTorrent 内核拆解：一个 Android BT 客户端的设计取舍](https://laoguosofts.github.io/posts/libretorrent.html)
- [从 libtorrent 到 AMOLED：LibreTorrent 的 Android BT 客户端设计原理](https://alipymanbu.github.io/posts/libretorrent.html)
