## ✨ Lottie 是什么

Lottie 是 Airbnb 开源的一套跨平台动效解决方案——它让设计师在 After Effects 里做好的合成动画，**一键导出成 JSON 文件**，然后在 iOS、Android、Web、React Native 等几乎所有主流平台上原生渲染，效果和 AE 里看到的一模一样。

在 Lottie 出现之前，跨平台动效一直是前端和客户端的痛点：设计师给的是视频或 GIF，体积大、不支持缩放、无法交互；各平台原生动画 API 又各自为政，同一个效果 iOS 和 Android 要写两遍甚至三遍。Lottie 把问题拆成了两半——**设计师负责产出（AE 里做动画），开发者负责渲染（各平台 SDK 吃 JSON）**，中间的 JSON 就是一份动效描述语言，谁都可以读、谁都可以写。

完整的 Lottie 工作链路长这样：**After Effects + Bodymovin 插件 → 导出 JSON → 各平台 Lottie SDK 渲染**。如果你不用 AE，还可以用 Lottie Creator（浏览器端）直接在线制作关键帧动画，或者从 LottieFiles 社区下载现成的动效库直接用。

适合谁用：产品团队有品牌动效需求、想让界面更活但不想上视频的开发者；想用动效表达概念但写代码又不够得心应手的设计师；需要一套跨平台保持视觉一致性的动效方案的产品经理。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://lottie.fedq.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：iOS 端在 App Store 搜 Lottie，Android 端在 Google Play 搜同名应用；AE 插件 Bodymovin 从 GitHub 仓库的 build/extension 目录下载 .zxp 扩展包

---

## 🔧 安装步骤

Lottie 本身不是一个需要"安装"的单体软件——它是一套生态，不同环节需要装不同的东西：

### Bodymovin 插件（AE 设计师必装）

Bodymovin 是 Airbnb 开发的 After Effects 扩展插件，装到 AE 里之后就能把合成一键导出成 Lottie 兼容的 JSON 文件。它不是独立软件，就是 AE 里的一个面板。

**方式一：直接下载 ZXP（推荐）**

1. 从 `airbnb/lottie-web` 仓库的 `/build/extension/` 目录下载 `bodymovin.zxp`
2. 关闭 After Effects
3. 用 Adobe ZXP 安装工具（Anastasiy Extension Manager 最方便）安装，或者 7-Zip 解压后手动放到 AE 扩展目录：
   - Windows：`C:\Users\<用户名>\AppData\Roaming\Adobe\CEP\extensions\`
   - macOS：`~/Library/Application Support/Adobe/CEP/extensions/`

**方式二：Extension Manager 拖拽安装**

1. 下载 Anastasiy Extension Manager
2. 选择你的 AE 版本
3. 把 `.zxp` 文件直接拖进扩展管理器窗口
4. 重启 AE

### 开启脚本权限

> [!TIP]
> Bodymovin 需要 AE 写入文件和访问网络的权限，装完插件后**别忘了**在 `编辑 → 首选项 → 常规`（macOS 是 `After Effects → 首选项 → 常规`）里勾选「允许脚本写入文件和访问网络」，否则导出时会报权限错误。

### 版本对应关系

> [!WARNING]
> 不同版本的 AE 对应不同版本的 Bodymovin，如果导出的 JSON 在 SDK 上出现兼容性问题，先查版本匹配：AE CC 2019 及以前用 Bodymovin v5.x，AE CC 2020+ 用 v5.7+，最新 AE 用 GitHub master 分支构建。

### 客户端 SDK 安装

- **iOS**：CocoaPods 一行 `pod 'lottie-ios'`，Swift Package Manager 直接加仓库依赖
- **Android**：Gradle `implementation 'com.airbnb.android:lottie:x.y.z'`
- **Web**：`<script src="https://cdnjs.cloudflare.com/ajax/libs/lottie-web/x.y.z/lottie.min.js">` 或 npm install
- **React Native**：`npm install lottie-react-native` + iOS 端 `pod install`（Expo 用户直接 `npx expo install lottie-react-native`）

---

## 📖 使用入门

### AE 里导出动效的标准流程

打开 After Effects，装好 Bodymovin 插件后，整个导出流程就三步：在 AE 里做好合成 → 打开 `窗口 → 扩展 → Bodymovin` 面板 → 选好合成、设置导出参数（是否压缩、包含哪些图层） → 点 Render 生成 JSON。

社区还有一个 zLottie 插件，本质是 Bodymovin 的封装加了一些额外能力（比如自动检查不支持的 AE 特性）。如果官方 Bodymovin 版本更新不及时，可以试试这个替代方案。

### Lottie Creator：浏览器里做关键帧

不想装 AE？Lottie Creator 是一个纯浏览器端的在线动效编辑器，内置完整的关键帧时间轴和 Graph Editor，导出的 JSON 和 AE+Bodymovin 完全兼容，两种工具的产出可以互换。

时间轴布局和 AE 思路一致但更直观：顶部时间标尺拖动手柄跳帧，中间每个图层占一行、关键帧用小菱形标记，底部展开 Graph Editor 可以可视化调整缓动曲线——线性、贝塞尔、预设的 ease-in/out/in-out 都有。

Creator 还提供了几个 AE 里没有的辅助功能：**关键帧镜像**（选中一组关键帧水平翻转时间位置，快速做对称动画）、**时间拉伸**（整体拉伸或压缩关键帧组调整节奏）、**多图层关键帧一键对齐**。这些小工具在调动画细节的时候特别省时间。

### React Native 跨端集成

React Native 本身就是一套 JS 代码驱动两端原生渲染，Lottie 完美契合：JS 层只负责传入 JSON 路径和播放参数，原生层各自调用 lottie-ios 和 lottie-android 渲染引擎，**同一个 JSON 文件在 iOS 和 Android 上表现完全一致**，不需要两端分别适配。

```jsx
import React, { useRef, useEffect } from 'react';
import LottieView from 'lottie-react-native';

export default function LoadingScreen() {
  const animationRef = useRef(null);

  useEffect(() => {
    animationRef.current?.play();
  }, []);

  return (
    <LottieView
      ref={animationRef}
      source={require('./loading.json')}
      autoPlay
      loop
      style={{ width: 200, height: 200 }}
    />
  );
}
```

`source` 支持四种传法：本地 `require` 引用、远程 CDN URL、内联 JSON 对象、或者 `.lottie` 打包文件。播放控制 API 也很直接——`play()`、`pause()`、`reset()`、`play(30, 60)` 跳到指定帧区间、`speed={1.5}` 倍速、`direction={-1}` 反向播放。事件回调 `onAnimationFinish` 和 `onLoopComplete` 可以在动画结束时触发下一步逻辑。

| 维度 | Lottie React Native | Reanimated / 原生动画 API |
| --- | --- | --- |
| 动画来源 | AE 导出 JSON | JS 里写 |
| 复杂度 | 设计师决定 | 开发者决定 |
| 文件体积 | 每个动画 10~50KB | 无额外文件 |
| 跨平台一致性 | 完全一致 | 可能有差异 |
| 定制化 | 有限（改 JSON 或 ValueCallback） | 完全自由 |

品牌动效和设计师产出的动画用 Lottie，程序化的简单过渡用 Reanimated——这是社区普遍的分工共识。

### LottieFiles 版本追踪

动效文件在团队协作里经常被改来改去——设计师改了一版、前端调了颜色、产品又让改回去。LottieFiles 的工作区每次有人打开文件编辑保存，**自动创建一个版本快照**，不需要手动提交或打 tag。

在文件详情页点 Version History 标签，可以看到版本列表（按时间倒序）、每个版本的保存者和时间、文件大小变化对比，还能逐帧预览版本差异。选中任意历史版本点 Restore 就能回退——原来的最新版本也会保留在历史里，不会丢失。误删文件也不用慌，Team 订阅保留最近 30 天内删除的文件，可以从回收站恢复。

> [!NOTE]
> 版本追踪是 Team 及以上订阅的功能，个人免费账号不支持。养成每次保存前想一下"这一版是不是要保留"的习惯——版本不会自动清理，保留太多历史会让查找变慢。

| 维度 | Git 版本控制 | LottieFiles 版本追踪 |
| --- | --- | --- |
| 面向用户 | 开发者 | 设计师 |
| 操作方式 | 命令行 | 可视化界面 |
| diff 方式 | 文本 diff（JSON 数字变化可读性差） | 帧对比（直观看到动效变化） |
| 快照粒度 | 手动 commit | 每次保存自动创建 |

LottieFiles 版本追踪和 Git 解决的是同一个问题，但面向不同用户群体。动效 JSON 理论上可以用 Git 管理，但关键帧位置变化在 Git diff 里就是一大串数字，可读性很差——LottieFiles 的帧对比让变化一目了然。

---

## 🔗 相关资源

- [Lottie官网](https://lottie.fedq.ren)
- [Lottie文档](https://lottie-zh-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/Lottie)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/Lottie)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%98%E6%96%B9%E7%89%88/Lottie)

## 📚 相关文章

- [终于把设计稿里的动效原样搬上线了——Lottie 上手体验](https://laoguosofts.github.io/posts/lottie-zh-docs.html)
- [把 After Effects 动画直接搬到 App 和网页？我试了 Lottie 一周](https://alipymanbu.github.io/posts/lottie-zh-docs.html)
