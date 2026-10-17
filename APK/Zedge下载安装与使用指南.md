## ✨ Zedge 是什么

Zedge 是一款面向移动设备的个性化定制应用，核心提供壁纸、铃声、通知音和图标包四大类内容。它最特别的地方在于**内容量巨大且持续更新**——官方数字称社区贡献超过 5000 万条，覆盖静态壁纸、动态壁纸（Live Wallpaper）、短视频铃声、系统级图标包等多个品类。

对 Android 用户来说，Zedge 的价值远不止换个壁纸这么简单。配合第三方 Launcher（Nova、Hyperion、Smart Launcher 等），可以把桌面彻底改成自己的风格：图标一键替换、网格自由调整、文件夹形状自定义、通知角标换颜色，再搭上 Zedge 社区贡献的各类 Widget，桌面完成度能提升好几个档次。iOS 用户虽然受系统限制做不了图标替换，但壁纸和铃声同样能直接用上。

近年来 Zedge 还加入了 AI 生成功能——用户可以用文字描述想要的风格，让 AI 生成专属壁纸或铃声。免费版每天有配额，Premium 订阅解锁无限次数和更高分辨率。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://zedge.ggcd.store/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 去 Google Play 搜 "Zedge"；iOS 去 App Store 搜 "Zedge Wallpapers & Ringtones"

---

## 🔧 Zedge 安装步骤

### Android 用户

**Google Play 安装（首选）**：直接搜索 "Zedge" 找到官方应用，点安装即可。后续更新由 Google Play 自动处理，省心省力。

> [!TIP]
> 如果 Google Play 因为地区限制或账号问题搜不到 Zedge，可以从官网下载 APK 手动安装。安装前记得在系统设置里开启「允许从未知来源安装」——不同品牌的位置略有差异，通常在 **设置 → 安全 → 未知来源** 或 **设置 → 隐私 → 特殊权限** 下。

**APK 安装注意点**：第三方渠道下载的 APK，建议先核对包名（应为 `net.zedge.android`），避免下载到被修改过的版本。安装包体积约 30MB，安装完成后首次启动会请求存储权限和通知权限。

### iOS 用户

iOS 生态封闭，**App Store 是唯一官方渠道**。搜索 "Zedge Wallpapers & Ringtones" 找到应用后点获取即可。iOS 版本安装包接近 100MB，包含更多内置音频资源。

> [!NOTE]
> iOS 版和 Android 版功能存在明显差异：壁纸功能两者都有；铃声设置 Android 可以一键完成，iOS 需要走 GarageBand 工作流；AI 铃声生成 Audio AI 目前仅 Android 版支持。

### 安装核对

安装完成后，可以通过以下方式确认安装包没有问题：

1. **Android**：在应用详情页看包名是否为 `net.zedge.android`
2. **iOS**：在设置 → 通用 → iPhone 存储空间里确认应用名和版本号
3. 首次启动后尝试浏览几页壁纸，能正常加载就说明安装成功

---

## 📖 Zedge 使用入门

### 注册与登录（可选但强烈推荐）

Zedge 不强制注册，游客也能浏览内容、下载壁纸和铃声，甚至免费使用 AI 生成器（每次看广告）。但游客收藏夹**只存在本地**——卸载应用、清理缓存或换手机后收藏夹就会消失。花一分钟注册是值得的。

三种注册方式任选其一，都免费：

| 方式 | 特点 | 注意事项 |
|---|---|---|
| Google 账号 | 最方便，10 秒完成，不用记新密码 | 需要设备已登录 Google |
| Facebook 账号 | 流程同 Google，一键授权 | 部分用户可能不想绑定社交账号 |
| 邮箱注册 | 适合不想绑定第三方的用户 | 验证邮件可能进垃圾邮件文件夹 |

注册后，收藏的壁纸、铃声、上传内容和 Credits 积分都会自动同步到云端。**但从收藏夹重新下载到手机的文件是每个设备独立的**——收藏清单同步了，换手机后登回来点一下重新下载就行。

### 收藏跨设备同步

换手机时同步收藏的标准流程：

1. **旧手机先做**：打开 Zedge → 进"我的收藏" → **下拉刷新**（确保最新收藏全部上传到云端）→ 强制退出 Zedge
2. **新手机后做**：安装 Zedge → 用和旧手机**完全相同的账号**登录（Google / Facebook / 邮箱三者要一致）→ 登录后让 Zedge 在前台停留 30 秒到 2 分钟 → 打开"我的收藏"下拉刷新

> [!WARNING]
> Zedge 的收藏同步不是实时上传。它在应用空闲或退出时**批量上传**收藏列表，所以刚收藏完立刻在另一台设备上登录可能还看不到——给它留几秒钟。

如果同步没生效，依次排查：确认两台设备登录的是同一个账号 → 旧手机收藏夹下拉刷新再等 30 秒 → 两台都连稳定 Wi-Fi → 退出 Zedge 重开。

跨平台同步（Android ↔ iOS）完全支持，收藏清单和 Credits 都能同步。只是 Android 上下载的音频文件不会自动搬到 iOS，iOS 需要重新从收藏夹下载并走 GarageBand 工作流。

### Android 第三方 Launcher 配合

这是 Zedge 真正发挥威力的地方。Android 原生 Launcher（Google Now Launcher / Pixel Launcher）自定义能力非常有限——不能替换应用图标、桌面网格选项少、不能隐藏应用。厂商定制系统（小米 MIUI / 华为 EMUI / OPPO ColorOS）虽然好一些，但也是"够用但不多"的水平。想做出有自己风格的桌面，**第三方 Launcher 是必经之路**。

**推荐的 Launcher 选择**：

- **Nova Launcher**：用户量最大、最成熟，图标包支持最完整，Zedge 所有图标包都能一键启用。有免费版和 Prime 付费版，付费解锁手势、隐藏应用等高级功能。
- **Hyperion Launcher**：基于 Nova 开发，界面更现代，额外支持整个 Launcher 皮肤主题化。
- **Smart Launcher 6**：自动把应用分类到文件夹，喜欢简约风格可以试试。
- **Microsoft Launcher**：和微软生态深度集成（Office / OneNote / To Do），有时间线功能和 Windows 桌面打通。

**完整配合方案**：

1. 安装 Launcher 后**设为默认桌面**——按 Home 键系统会弹出选择框，选你装的 Launcher 并确认"始终使用"
2. Zedge 里下载想要的图标包 → Nova Launcher 会自动检测并弹出"启用"提示 → 确认即可。没自动提示的话，手动去 **Nova 设置 → 外观和感觉 → 图标主题** 选择
3. Zedge 里找到合适的壁纸 → 点 Set → Nova Launcher 会接管设置。Live Wallpaper 同样支持
4. Nova Launcher 设置里调整桌面网格（4×6 / 5×5 / 6×6 等）、图标大小、文件夹圆角、应用标签字体大小。**建议让图标大小和网格间距和壁纸构图匹配**——壁纸下半部分如果是空白的，网格可以拉大一点让图标区覆盖更大空间
5. 长按桌面空白处 → Widgets → 把系统时钟、天气、音乐播放器等拖到桌面，Nova 可以自定义 Widget 的大小和位置
6. **Nova Prime 付费版**还支持隐藏应用（**Nova 设置 → 应用抽屉 → 隐藏应用**），把不常用的应用藏起来让桌面更干净

> [!NOTE]
> Live Wallpaper 会额外消耗 GPU 资源，电池续航比静态壁纸短约 10~15%。中低端机型建议每天切换回静态壁纸省电。

---

## 🔗 相关资源

- [Zedge官网](https://zedge.ggcd.store)
- [Zedge文档](https://zedge-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Zedge)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Zedge)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Zedge)
