## ✨ AssetStudio：Unity 资源提取与浏览利器

AssetStudio 是一款专门面向 Unity 引擎构建产物的资源浏览与提取工具——它能直接打开 APK、AssetBundle（.bundle / .ab）、OBB、Unity 构建目录等多种 Unity 打包形式，把里面的贴图、音频、3D 模型、动画、字体、Live2D 立绘等资源分门别类地展示出来，支持实时预览和一键导出。

它最早由 Perfare 开发维护，后来社区 Fork 出了 **AssetStudioMod** 分支，在原版基础上扩展了 Unity 版本支持（覆盖 1.7 到 Unity 6 / 6000.x）、CLI 命令行模式、Live2D Cubism 模型完整提取等功能，目前 AssetStudioMod 是活跃开发的主力版本。

对于二次元游戏爱好者、Mod 制作者、游戏资源研究者来说，AssetStudio 是扒取 Unity 游戏素材的第一站——比手动解包省时间，比 UABE / AssetRipper 更轻量（只读不写，不需要还原完整工程）。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://assetstudio.laze.skin/download.html) —— 下载页整理了当前可用的版本和安装说明
> - GitHub 发行页：搜索 AssetStudioMod（活跃分支）或 AssetStudio（原版，维护模式），下载最新 Release 的压缩包

---

## 🔧 安装步骤

AssetStudio 有 GUI 图形界面版和 CLI 命令行版两种形态，两者都依赖微软 .NET 运行时。安装过程分三步走：

**第一步：确认运行时**

> [!WARNING]
> AssetStudioMod 需要 **.NET 8 Desktop Runtime**（Windows 下要桌面版，不是纯控制台版）。如果双击 exe 弹"缺少运行时"的报错，直接去微软官网下载安装 .NET 8 x64 Desktop Runtime 即可。

原版 AssetStudio 依赖旧版 .NET Framework 4.7+，AssetStudioMod 统一迁移到了 .NET 8，建议直接用 Mod 分支。

**第二步：解压到固定目录**

下载 zip 压缩包后解压到一个你不会随手删掉的位置（比如 `D:\Tools\AssetStudioMod\`），目录结构如下：

```
AssetStudioMod/
├── AssetStudioMod.exe          ← GUI 版入口
├── AssetStudioModCLI.exe       ← CLI 版入口（批量导出用）
└── 其他依赖 dll
```

> [!TIP]
> 不要解压到桌面、下载目录或带中文路径的文件夹——.NET 程序对中文路径偶尔会出诡异的加载失败。放纯英文路径最稳。

**第三步：首次启动验证**

双击 `AssetStudioMod.exe`，如果界面弹出来了就说明装好了。主界面左侧是 Scene Hierarchy（场景层级）面板，中间是 Asset List（资源列表），右侧是预览区。

CLI 版的验证命令：

```
AssetStudioModCLI.exe --help
```

能打印帮助信息就 OK。

---

## 📖 使用入门

AssetStudio 的核心工作流就是"加载文件 → 浏览 → 导出"，但里面有不少值得熟练掌握的细节。

### 加载：喂对文件是第一步

AssetStudio **只吃 Unity 引擎产物**——这是新手最容易踩的坑：

| 文件类型 | 能喂吗 | 说明 |
| --- | --- | --- |
| `.assets` / `.resS` | ✅ | Unity 构建的序列化文件，直接拖 |
| `.bundle` / `.ab` | ✅ | AssetBundle，直接拖 |
| APK 里的 `assets/bin/Data/` 目录 | ✅ | 用 7-Zip 当 zip 解开 APK，取这个目录拖进去 |
| OBB 文件 | ✅ | 直接拖 |
| Zip 压缩包 | ❌ | 先解压再喂 |
| 普通图片 / 音频文件夹 | ❌ | AssetStudio 不识别 |
| `global-metadata.dat` 单独喂 | ❌ | 它只是 IL2CPP 的元数据，不是资源容器 |

XAPK 文件先当 zip 解开，取里面的 APK 和 OBB 分别加载。APK 本身不用安装，7-Zip 右键打开就能拿到内部目录。

### Asset List 空了？五步排查

加载完文件发现 Asset List 什么都没有——按这个顺序查：

1. **喂对文件了吗？** 回顾上面的表格，确认拖进去的是 Unity 产物而不是 zip 或普通文件夹；
2. **Unity 版本匹配吗？** 原版 AssetStudio 最高只到 Unity 2022.1，Unity 2023+ 或 Unity 6 的包必须用 AssetStudioMod，还不行就 CLI 加 `--unity-version 6000.0.23f1` 手动指定；
3. **类型被筛了？** Asset List 顶部有 Filter Type 下拉，只勾了 Texture2D 而包里没贴图就是空的——右键 Asset List → Filter Type → Select All；或者走 Options → Display all assets，把 Shader、MonoBehaviour 这些不可导出的也显示出来；
4. **加密包？** UnityCN 加密包原版连提示都没有直接空列表，AssetStudioMod 会弹提示说 detected encrypted bundle——这种情况换 Razmoth Studio 或 AnimeStudio 试试；
5. **文件坏了？** 从游戏目录重新复制一遍 AssetBundle，核对文件大小，从手机拷的话用 USB 存储设备模式比 MTP 协议稳。

```
列表空 → 喂对了吗？ → 版本对吗？ → 筛选勾对了吗？ → 加密了吗？ → 文件坏了吗？
```

五步下来 99% 能定位。

### Scene Hierarchy：按场景对象找资源

Asset Studio 默认显示 Asset List（按资源类型分组），但左侧还有个 Scene Hierarchy 标签页——它按 Unity GameObject 的父子关系把整个场景组织成树形结构，每个节点展开可以看到挂在上面的组件（MeshRenderer、MonoBehaviour、Transform 等）。

**什么时候用它而不是 Asset List？**

- Asset List 适合"把所有 BGM 找出来"这类按类型的查找；
- Scene Hierarchy 适合"这个 Character 角色身上挂了哪些组件"这类按场景对象的查找。

两个面板通过 **Show Related Assets** 联动：在 Scene Hierarchy 里右键某个 GameObject → Show Related Assets，Asset List 会自动过滤出这个对象身上挂的所有贴图、Sprite、音频等组件。这个功能在做 Mod 的时候特别好用——先从场景树定位到要改的角色，一键跳过去拿具体组件。

树的操作也很顺手：面板顶部搜索框支持模糊/精确匹配，勾选父节点会自动递归勾选所有子节点（再点一次取消也会递归），AssetStudioMod 右键菜单还有 Expand All / Collapse All 一键全展开全折叠。

### 三大工具怎么选：AssetStudio、UABE、AssetRipper

很多人会纠结这三者的区别——它们不是竞争关系，是互补：

| 工具 | 一句话定位 |
| --- | --- |
| **AssetStudio** | 浏览 + 预览 + 提取 + 导出（只读） |
| **UABE** | 提取 + **修改** + 打包回 AssetBundle |
| **AssetRipper** | 把构建产物**还原成可打开的 Unity 工程** |

详细维度对比：

| 维度 | AssetStudioMod | UABE | AssetRipper |
| --- | --- | --- | --- |
| 核心功能 | 浏览 + 提取 + 导出 | 浏览 + 编辑 + 重新打包 | 反序列化 + 还原工程 |
| 能改资源吗 | ❌ 只读 | ✅ 改贴图、改文本、替换资产 | ⚠️ 还原后在 Unity 编辑器里改 |
| 能打包回去吗 | ❌ | ✅ 改完写回 AssetBundle | ✅ 还原成工程后打包 |
| Unity 版本范围 | 1.7 – 6000+ | 5.0 – 2022 | 3.5 – 6000+ |
| 活跃开发 | ✅ 很活跃 | ❌ 维护模式 | ✅ 非常活跃 |
| Live2D 提取 | ✅ 独有 | ❌ | ❌ |
| CLI 批量导出 | ✅ 独有 | ❌ | ❌ |

典型配合流程：

```
AssetStudio     →  快速浏览资源，找到想改的贴图
       ↓
UABE            →  替换贴图、改 MonoBehaviour 字段值、写回 AssetBundle
       ↓
AssetRipper（可选）→ 如果 UABE 改不了，用它还原完整工程
       ↓
Unity 编辑器     → 如果还原了工程，在这里做最终修改 + 打包
```

所以如果你只是想扒资源当素材、听 BGM、看立绘——AssetStudio 就够了。要改东西才需要 UABE，要还原整个工程才开 AssetRipper。

### Live2D 立绘提取：AssetStudioMod 独有

很多二次元游戏的角色立绘用 Live2D Cubism 技术做——一张平面图被切成几十个部件，每个部件可以独立变形、旋转、透明，组合起来就是"会动"的立绘效果。

在 Unity 里，Live2D 模型以 MonoBehaviour（类型名 `CubismMoc`）的形式挂在 GameObject 上，AssetBundle 里包含 `.moc3`（模型核心）、`.model3.json`（配置）、贴图、动画等碎片。原版 AssetStudio 只能把 CubismMoc 当普通 MonoBehaviour Dump 出二进制，而 AssetStudioMod 专门实现了 Live2D 提取逻辑，能把这些碎片拼成完整的标准 Live2D 模型目录。

**GUI 方式：** Asset List 里 Filter Type 找到 CubismMoc → 右键 → Export as Live2D Model，弹窗里可以选要不要带 Motion 动画、Fade 过渡动画。

**CLI 方式（批量导出）：**

```
AssetStudioModCLI <资源目录> -m live2d -o <输出目录>
```

`-m live2d` 是 Live2D 专用模式，配合 `--filter-by-name live2d` 可以只导名字里带 live2d 的模型。

导出的模型目录结构：

```
模型名/
├── xxx.moc3              ← Live2D 模型核心
├── xxx.model3.json       ← 模型配置（引用贴图、参数表）
├── texture_xxx.png       ← 贴图（可能多张）
└── motions/
    ├── xxx_motion1.model3.json
    └── xxx_motion2.model3.json
```

拿这个目录直接喂给 Live2D Cubism Editor、Live2D Viewer、VTube Studio 或 OBS 的 Live2D 插件都能用。

> [!NOTE]
> 如果 Live2D 模型导出来打不开，先排查三件事：Unity 版本是否在 AssetStudioMod 支持范围内、Live2D Cubism SDK 版本是否兼容、贴图是否散在别的 AssetBundle 里没加载全——第三种情况用 Load folder 加载完整游戏数据目录就能解决。

### CLI 批量导出：让 AssetStudio 跑在服务器上

AssetStudioMod 是三者里唯一一个有完整 CLI 模式的——这意味着你可以写脚本批量导出，甚至在没有 GUI 的服务器上跑无人值守任务。

基本语法：

```
AssetStudioModCLI <资源目录> -t <类型筛选> -o <输出目录>
```

常用参数：

| 参数 | 作用 |
| --- | --- |
| `-t texture2d` | 只导出贴图 |
| `-t audioclip` | 只导出音频 |
| `-t mesh` | 只导出 3D 模型 |
| `-m live2d` | Live2D 专用模式 |
| `--filter-by-name <关键词>` | 按文件名筛选 |
| `--unity-version <版本号>` | 手动指定 Unity 版本 |
| `--crop-textures` | 导出贴图时自动裁剪空白边缘 |

---

## 🔗 相关资源

- [assetstudio官网](https://assetstudio.laze.skin)
- [assetstudio文档](https://assetstudio.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/assetstudio)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/assetstudio)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%AD%E6%96%87%E7%89%88/assetstudio)

---

## 📚 相关文章

- [AssetStudio 用了快一年，它帮我扒了几十款 Unity 游戏](https://laoguosofts.github.io/posts/assetstudio.html)
- [AssetStudio 用了两年：从"提个 BGM 当铃声"到每天都开的工具](https://alipymanbu.github.io/posts/assetstudio.html)
