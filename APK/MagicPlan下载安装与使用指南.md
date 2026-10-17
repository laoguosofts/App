## ✨ MagicPlan 是什么

MagicPlan 是一款面向现场作业人员的**专业测绘与文档化工具**——用手机或平板在现场扫描房间的同时自动生成带尺寸的 2D 平面图，再基于平面图叠加 3D 模型、家具物体、照片注释、清单表单，最后直接导出给下游软件或客户。

它的设计原点很简单：**现场干活的人不想回家再画一遍**。装修师傅、翻新承包商、理赔员、验房师、房地产经纪人——这些人每天走现场，手里拿卷尺、笔、本子、相机，回家之后还要把这些堆成一个文件。MagicPlan 把「量房 → 画平面图 → 加照片 → 出报告」整个链条收进了一台设备。

核心能力包括：扫描即绘图（iOS 带 LiDAR 的设备支持一键 Auto-Scan，走一圈自动抓墙、门窗、家具位置；所有 iOS 设备和 Android ARCore 设备支持手动 AR 扫描）、蓝牙激光测距仪对接（PrecisionLink 协议支持 Bosch GLM、Leica DISTO 等二十多款设备，读数直接覆盖扫描值）、平面图物体库（内置 150+ 门、窗、插座、家具、洁具、电器，拖进去自动对齐墙体）、多楼层管理（每层单独扫描，3D 自动叠起）、照片视频挂接（每张照片挂到房间/墙/物体，导出时自动归位）、Forms 自定义清单（Site Assessment、SOP、QA Check、Punch List 云端建模板，App 里点选填写），以及 PDF 报告、SVG/PNG/JPG、DXF/CAD、OBJ/IFC/USDZ、ESX/Xactimate 理赔直通等多格式导出。

> [!NOTE]
> MagicPlan 不是完整的 CAD 或 BIM——它的优势入口是**现场扫描的实时性**和**从扫描到文档的一体化**，但没有 AutoCAD / Revit 那种高精度的建筑制图能力（复杂曲线、精确弧墙、多层嵌套标注），Android 目前也没有 LiDAR 自动识别墙门窗的能力（全靠 AR 手动标记）。

订阅档位从免费版（2 项目无时限）到 Sketch（Forms 基础、Cloud 存储有限）、Report（360° 全景挂接、FLIR 热像、无照片数量限制）、PRO（团队协作、ESX/FML 对接、价格表管理、PRO Estimator 自动算用量成本）逐级解锁。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://magicplan.bmww.store/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：iOS 用户在 App Store 搜索「MagicPlan」；Android 用户在 Google Play 搜索「MagicPlan」

---

## 🔧 安装步骤

### iOS 安装

MagicPlan 在 App Store 免费提供，无需付费即可下载安装。打开 App Store，搜索「MagicPlan」，点击获取按钮即可自动下载并安装。安装完成后首次打开需要授权相机和 AR 空间感知权限，这些是扫描功能正常工作的必要条件。

> [!TIP]
> iPhone 12 Pro / iPad Pro（2020 年款及以后）带有 LiDAR 激光雷达传感器，可使用 Auto-Scan 功能实现一键自动扫描。iPhone 11 及更新机型、以及所有支持 ARCore 的 Android 设备可使用手动 AR 扫描模式，精度同样可达 1cm 以内。

### Android 安装

在 Google Play 搜索「MagicPlan」安装即可。Android 设备需要支持 ARCore（Google AR 服务），大多数 2020 年以后发售的中高端机型都在支持列表内。安装后首次打开同样需要授权相机权限。注意 Android 版目前没有 LiDAR 自动识别墙门窗的能力，扫描全部依赖 AR 手动标记墙角。

### 蓝牙测距仪配对

MagicPlan 支持 PrecisionLink 协议的蓝牙激光测距仪，可在设置 → Devices 中配对。配对后扫描或绘制过程中，测距仪读出的数值可以直接覆盖 App 里的手动测量值，达到厂商声称的 100% 精度。支持的品牌包括 Bosch GLM、Leica DISTO、Hilti PD、Stanley SLM 等二十多款。

### Cloud 账户注册

安装 App 后可以选择注册 Cloud 账户。免费版用户注册后可同步最多 2 个项目到云端；订阅 Sketch 及以上档位可解锁 Cloud 存储、Forms 模板、360° 全景挂接、PRO Estimator 等云端功能。桌面端编辑 Beta 也需要登录 Cloud 账户后在网页端开启 toggle 开关才能使用。

---

## 📖 使用入门

### 扫描房间：两种模式任选

iOS LiDAR 设备用户可以使用 Auto-Scan 模式：进入项目 → 楼层 → 点击「Scan Room」→ 选择 Auto-Scan → 把手机对准地面缓慢走一圈经过房间所有墙角，系统自动识别墙体、门窗、甚至家具轮廓，一个标准客厅大约 30 秒完成。

没有 LiDAR 或 Android 用户使用手动 AR 扫描模式：进入房间 → 点击「Scan Room」→ 选择 Manual → 将手机对准第一个墙角 → 点击标记 → 依次标记所有墙角 → 系统自动连接墙体并生成带尺寸的 2D 平面图。扫描过程中可以随时停下来调整已标记的位置，或直接点击尺寸数字手动修改。

### 添加物体：从库拖或自己画

平面图生成后，内置 150+ 物体库（门、窗、插座、家具、洁具、电器等）可以直接拖到图上，新放置的物体自动吸附到墙体上并对齐。如果内置库没有需要的物体，可以在 App 里手动绘制 2D 形状，或从外部导入 SVG 文件。多楼层项目可以点击「Add Floor」添加新楼层，每层单独扫描后 3D 视图会自动叠起来。

### 照片视频挂接：拍的时候就归档

传统工作流是现场拍一堆照片、回家手动整理文件夹、再插到报告里。MagicPlan 的做法是**拍的时候就归档**——每拍一张照片或视频，直接选择要挂在哪个层级上：整个项目、某个楼层、某个房间、某面墙、或某个具体物体。

挂接操作有三种方式：

1. **扫描过程中直接拍**：AR 扫描界面有一个相机按钮，扫描过程中随时可以拍当前画面，照片自动挂到正在扫描的房间
2. **扫完后手动添加**：进入项目 → 楼层 → 房间 → 选中目标位置（整间房 / 某面墙 / 某个物体）→ 从详情菜单找到 Photos 标签 → Add Photo → 选择 Camera（调用相机直接拍）、Library（从相册选）或 Files（从文件管理器选视频或照片）
3. **批量添加**：如果现场已经用手机相机 App 拍了一堆照片，回家可以选中房间或墙 → Photos → Add → Library → 多选批量导入，统一挂到这个位置

> [!WARNING]
> 不要"先拍一堆回家整理"——现场花 5 秒归位，回家能省 30 分钟整理。挂好的照片会自动出现在 2D 视图（右下角缩略图标记）、3D 视图（点击墙或物体弹出）、导出的 PDF 报告（自动按归属归位）、以及 Cloud 网页端（可以按照片反查位置）。

### Forms 清单：云端建模板，现场点选

Forms 功能（Sketch 订阅及以上可用）允许在 Cloud 网页端创建自定义清单模板——比如 Site Assessment（现场评估）、SOP（标准作业程序）、QA Check（质量检查）、Punch List（尾项清单）等。创建好的模板在 App 里下拉刷新后就能看到，现场作业时直接在清单里打勾、填文字、拍照片即可。适合理赔员记录房屋损坏情况、验房师逐项检查房屋状况、装修师傅记录施工进度。

### PRO Estimator 自动报价（PRO 订阅专属）

PRO Estimator 能**自动根据平面图计算材料用量和成本**——前提是先在 Cloud 网页端的 Price List Manager 里建好价格表。价格表包含材料名称、分类、计价单位、单价、人工费单价、用量系数（含损耗）、关联对象等字段。建好发布后，App 里 Estimate 标签下选择相关材料，系统自动从平面图读取墙体面积、地板面积、天花板面积、门窗数量等数据，乘以用量系数和单价得到总成本。可以手动调整任何条目，还能设置加价（20%）、税费（6%）、折扣（-10%）等成本规则，最终生成报价 PDF 或 XLS。

### 桌面端编辑 Beta：电脑上精确调整

MagicPlan 正在 Cloud 网页端测试浏览器内直接编辑平面图的功能（Beta 阶段，所有订阅包括免费版都能开启）。开启方式：登录 Cloud → 右上角 toggle 开关 → 打开 Beta Desktop Editing → 刷新页面。

桌面端的最佳定位是**App 扫描后的精细化编辑**：现场用手机扫完大致形状，回家用电脑精确调整尺寸、对齐家具、布置细节。电脑屏幕大、键盘输入快，可以框选多元素同时编辑、使用 Ctrl+Z 撤销等快捷键。但桌面端做不了扫描（没有摄像头），核心编辑工作还是建议在 App 里完成，桌面端用于后期细化。两端自动同步，不需要手动搬运文件。

### 导出：从 PDF 报告到专业格式

导出格式覆盖全链条需求：PDF 报告（带位置标注的照片自动归位）、SVG/PNG/JPG 图像、DXF/CAD、OBJ/IFC/USDZ 三维模型、ESX/Xactimate 理赔直通格式、FML/Cotality 等。不同订阅档位可用的导出格式不同，ESX/Xactimate 对接仅 PRO 档支持。视频在 PDF 报告中会以缩略图 + 链接形式出现，在 Cloud 端可以直接播放。

---

## 🔗 相关资源

- [MagicPlan官网](https://magicplan.bmww.store)
- [MagicPlan文档](https://magicplan.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%AD%A3%E7%89%88/MagicPlan)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%AD%A3%E7%89%88/MagicPlan)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%AD%A3%E7%89%88/MagicPlan)

---

## 📚 相关文章

- [MagicPlan 用了三年才发现的五个隐藏功能，大多数人扫完 AR 就关掉了](https://laoguosofts.github.io/posts/magicplan.html)
- [用了 MagicPlan 才发现：这几个隐藏功能能把理赔估价时间砍掉 70%](https://alipymanbu.github.io/posts/magicplan.html)
