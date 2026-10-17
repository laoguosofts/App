## ✨ ARCore 是什么：Google 为 Android 打造的增强现实平台

ARCore 是 Google 推出的增强现实开发平台，让 Android 手机具备感知真实世界并叠加虚拟内容的能力。它把手机摄像头、IMU 传感器和深度硬件的数据融合在一起，构建出一条从现实到虚拟的桥梁——开发者不需要从零实现 SLAM 或环境理解算法，直接调用 ARCore 的 API 就能在应用里实现桌面放家具、墙上贴海报、户外虚拟导航等 AR 体验。

ARCore 的核心技术栈建立在两大支柱之上：SLAM（同时定位与地图构建）负责追踪手机自身的姿态变化，环境理解则负责识别眼前的平面和物体。在此基础上，深度感知、光照估算、命中测试等能力进一步补足了 AR 体验的完整性。对于普通用户来说，ARCore 体现在各种 AR 应用里——家具购物 App 里的虚拟摆放、相机应用里的 AR 贴纸、户外导航里的方向箭头。对于开发者来说，ARCore 是一套开箱即用的底层能力，Android、Unity、iOS 都有对应的 SDK 支持。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://arcore.gltd.quest/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：在 Google Play 搜索 "ARCore" 或 "Google Play Services for AR"，根据你的设备机型选择对应的版本安装

> [!NOTE]
> ARCore 在不同地区和设备上的可用性可能不同，如果在应用商店搜索不到，通常意味着你的设备或所在地区暂未被 Google 支持。

---

## 🔧 安装与配置准备

### 设备兼容性检查

ARCore 对设备有明确的硬件要求，主要包括：

| 硬件 | 要求 | 说明 |
| --- | --- | --- |
| 摄像头 | 至少一个后置摄像头 | 运动跟踪和环境理解都依赖摄像头画面 |
| IMU | 加速度计 + 陀螺仪 | SLAM 融合视觉数据的必要传感器 |
| 处理器 | 支持 OpenGL ES 3.0 | 渲染虚拟物体的图形 API 版本 |
| 操作系统 | Android 8.0+ | 覆盖绝大多数在售设备 |

Google 会持续更新支持列表，较新的旗舰机型通常都在名单里，部分入门机型可能被排除。如果你的设备不在支持列表上，ARCore 将无法正常运行。

### 安装步骤

1. 打开 Google Play 商店，搜索 "Google Play Services for AR"（有些地区直接显示为 "ARCore"）
2. 找到对应的应用，点击安装。这个服务是后台运行的，没有独立图标
3. 安装完成后，打开任意一个支持 ARCore 的应用（比如 Google 搜索里的 3D 动物、宜家 AR 家具），首次运行时应用会引导你完成环境扫描

> [!WARNING]
> 安装完成后如果发现 AR 应用提示 "Unsupported device" 或 "ARCore not available"，先确认 Google Play 服务是最新版本，然后重启手机再试一次。如果仍然不行，基本就是设备不在支持名单里。

### 开发环境配置（开发者视角）

如果你是要开发 ARCore 应用，需要额外准备：

- Android Studio（推荐 Hedgehog 或更新版本）
- Android SDK Platform 7.0+
- 在 AndroidManifest.xml 中声明 `CAMERA` 权限和 `com.google.ar.core` 特性
- 通过 Gradle 引入 ARCore 的 AAR 依赖，或者直接在 Android Studio 的 SDK Manager 里勾选安装

Unity 开发者则需要通过 Package Manager 安装 AR Foundation 和 ARCore XR Plugin，iOS 侧用 ARKit 作为后备。

---

## 📖 五大核心能力拆解与使用入门

ARCore 的能力体系可以归纳为五个基础模块，理解它们的工作原理和 API 接口，就能灵活组合出各种 AR 交互效果。

### 运动跟踪：让手机知道自己在哪

运动跟踪是所有 AR 能力的地基。它解决的核心问题是：手机在空间中移动时，如何准确推算它当前的位置和朝向（术语叫「姿态 pose」）。ARCore 把两种数据融合在一起——视觉侧实时分析摄像头画面，从中提取特征点（那些视觉上独特的小区域，比如地砖缝、墙皮裂纹、物体边缘的纹理变化），每一帧都能提取出数千个特征点，通过跨帧匹配看出位移方向和幅度；惯性侧则读取手机 IMU 的加速度计和陀螺仪数据，在摄像头两帧画面之间补充更细致的姿态变化。视觉位移和惯性数据通过扩展卡尔曼滤波或因子图优化融合，就能得到一条平滑、连续的设备姿态轨迹。

ARCore 用关键帧机制维护地图，不会把每一帧都存进去——当设备移动足够远或旋转足够多时，当前帧被标记为关键帧，记录当时的姿态、特征点和 BRIEF 描述子。新帧和历史关键帧做特征点匹配，计算相对姿态变化；当历史关键帧的特征点与当前帧高度重叠时，ARCore 会触发回环检测，修正整个地图的累积误差。

> [!TIP]
> 影响 SLAM 效果最大的环境因素是纹理丰富度和光照条件。纯白墙面、光滑金属桌面、黑暗或强逆光场景都会让特征点密度急剧下降，导致跟踪丢失。首次启动 AR 应用时，建议引导用户缓慢扫过房间，建立足够密集的特征点地图。

ARCore 会持续暴露 `TrackingState` 反馈当前跟踪质量：`TRACKING` 是正常状态，`PAUSED` 表示暂时丢失但正在恢复，`STOPPED` 则需要重新初始化——这时候通常要重置 Session 或提示用户重新扫一遍周围。

### 环境理解：识别面前的桌子和墙壁

光知道自己在哪还不够，AR 应用还需要知道"我面前有什么可以放虚拟物体"。环境理解在三维空间中不断寻找那些看起来像平面的特征点簇——当足够多的特征点呈现共面趋势时，ARCore 就判定这里有一张桌子或一面墙，并生成一个几何平面（Plane）。

几何平面有两个关键属性：类型决定虚拟物体应该怎么放（水平、垂直、倾斜各有各的朝向处理），边界则是 ARCore 尽量检测出的平面实际物理范围，而不是假设它无限大。开发者拿到 Plane 对象后，可以查询其边界多边形来判断虚拟物体是否在有效范围内。

> [!NOTE]
> 没有纹理的平面检测效果差，比如纯白色墙面、光滑桌面、纯灰色地面。开发者在设计 AR 体验时要考虑到这一点，可以用 Depth API 来做补充检测。

### 深度感知：让虚拟物体知道真实物体在哪里

如果说环境理解告诉你"哪里有平面"，深度感知告诉你"每个像素距离相机多远"。Depth API 为每一帧生成一张与摄像头同分辨率的深度图——灰度图，每个像素值代表该点到相机的距离。有了这张图，应用就能做到遮挡渲染（判断虚拟物体在真实物体前面还是后面）、精确尺寸计算（知道距离就能算出物体实际大小）、物理交互（让虚拟球滚到桌边停下来而不是穿模）。

Depth API 的数据来源因设备而异：有 ToF 深度传感器的机型直接输出硬件深度图，没有专用传感器的机型则通过运动过程中不同视角的 RGB 画面做立体匹配（被动深度）。Streetscape Geometry 是 Geospatial API 的附加能力，能获取周围建筑物和地形的 3D 网格数据，配合 Depth API 可以实现精度更高的室外遮挡和碰撞检测。

### 光照估算：让虚拟物体"融入"真实光线

虚拟物体看起来假，很大程度上是因为光不对。光照估算分析摄像头画面中的整体亮度和色温，输出两个值：平均光强（当前环境有多亮）和色温校正值（光线偏暖还是偏冷）。开发者把这两个值喂给 3D 引擎的光照系统，虚拟物体就会呈现与周围环境一致的明暗和色调——白天阳光下是暖色调亮面，晚上室内是白炽灯暖黄，阴天则是冷色调灰调。

对于需要更精细光照控制的场景，ARCore 还提供 HDR 光照估算，输出一个环境贴图让 3D 物体反射周围真实环境的光。

### Hit Test：用户点屏幕，虚拟物体落位置

Hit Test 是连接用户屏幕输入和 3D 世界坐标的桥梁，也是 AR 应用最常用的交互机制。完整流程分四步：首先拿到用户屏幕触点坐标，然后从最新的 Frame 获取 hitTest 接口执行射线投射，接着解析命中结果列表（按距离从近到远排序），最后在第一个命中的平面或特征点位置创建锚点、放置虚拟物体。

命中类型有好几种：`PLANE` 是最理想的情况，射线击中了已检测平面；`POINT` 命中特征点（还没有 Plane）；`INSTANT_PLACEMENT` 是即时放置模式，任何时候都能返回一个近似位置但精度较低；`DEPTH_POINT` 在 Depth API 启用时可用，基于深度图逐像素计算，比只依赖特征点更精确；`SESSION` 则是没有命中任何 Trackable 但返回了深度估计的兜底。

> [!TIP]
> Hit Test 返回空列表时的处理策略：降级到 Instant Placement 返回近似位置、提示用户对准某个平面、或用 Depth API 补充精度。Depth API Hit Test 可以命中没有特征点的光滑表面，这是纯 Plane 检测做不到的。

### Unity 中的开发体验

Unity AR Foundation 把 ARCore 的能力封装得更优雅。`ARRaycastManager` 组件可以直接从屏幕位置向世界投射射线，配置好 `TrackableType.PlaneWithinPolygon` 过滤条件后，Raycast 返回的第一个 `ARRaycastHit` 就是命中点的世界坐标和朝向。订阅 `ARStreetscapeGeometryManager` 的 frameReceived 事件可以实时获取周围的 Streetscape 网格，用来创建 MeshCollider 做物理碰撞检测。

---

## 🔗 相关资源

- [ARCore官网](https://arcore.gltd.quest)
- [ARCore文档](https://arcore-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/APP/ARCore)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/APP/ARCore)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/APP/ARCore)

## 📚 相关文章

- [ARCore 进化史：从"能跑"到"好用"，Google 这八年在死磕什么](https://laoguosofts.github.io/posts/arcore-docs.html)

- [ARCore 版本进化史：Google 这八年死磕的那件事](https://alipymanbu.github.io/posts/arcore-docs.html)
