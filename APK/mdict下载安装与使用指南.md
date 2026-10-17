## ✨ MDict：跨平台的 mdg 格式词典阅读器

MDict 是一款开源的词典阅读工具，专注于解析 mdg / mdd / css 这套词典文件格式。它最早在 Windows 平台流行，后来逐步扩展到 Android、iOS 和 macOS，是目前少数能在四个主流桌面与移动端上稳定运行的词典阅读器之一。

MDict 的核心定位很明确：**它不提供内容，只负责读内容**。你需要自己去下载或制作 mdg 格式的词典文件（常见来源包括各类开源词典库、社区分享的外语词典、以及把维基百科或专业术语库转成 mdg 的自制词典），然后放进 MDict 指定的目录，就能用统一的界面查词。MDict 本身是免费软件，没有内购、没有广告，所有功能都集中在「快速查词」这一件事上。

适合谁用？如果你是外语学习者（英语、日语、俄语都行）、技术文档阅读者（需要查专业术语）、或者手里攒了一堆 mdg 格式的词典想在多设备上共用，MDict 基本是首选。它的搜索速度很快，跨平台同步词库也省心。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://mdict.wrwq.fun/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 端搜索 MDict 下载 APK；iOS 端在 App Store 搜 MDict（App Store ID 389083586）；macOS 在 Mac App Store 同样能找到

---

## 🔧 各平台安装与词库导入

MDict 四个平台的安装方式差异不大，真正需要注意的是**词库文件的存放位置**——每个平台有自己的目录约定，放错了 MDict 就扫不到词典。

### Windows PC

PC 版是 C++ 原生开发，安装时可以自定义安装路径，默认词库目录是安装目录下的 `content` 子文件夹。你也可以在设置里添加多个外部目录，MDict 会同时扫描这些路径。

> [!TIP]
> 如果你的词典包含发音和图片（mdd 文件），必须保证 `.mdx` 和 `.mdd` 放在**同一个目录**下、**文件名完全一致**（扩展名除外）。从其他平台拷贝过来时经常有人把 mdd 留在原设备，导致 PC 版打开词典只有正文没有声音和图片。

### Android

Android 版 v2.3.7 有一个硬性约束：词库必须放在 `/sdcard/mdict/doc/` 目录下。这是旧版 Android 的外部存储规范，新版 Android 设备往往没有预置这个路径，需要你手动创建：打开文件管理器 → 进入根目录 → 新建文件夹 `mdict` → 进入后再新建子文件夹 `doc`。

也可以在「设置 → 词库 → 额外词库目录」里添加自定义路径，但社区反馈自定义目录在部分厂商定制系统上可能被权限拦截，首选默认目录最稳妥。

### iOS

iOS 版通过 App Store 分发，受沙盒机制限制，不能直接访问文件系统。导入词典有三条路：

1. **iTunes / Finder 文件共享**（推荐）：数据线连接电脑后，在 Finder（macOS Catalina 及以上）或 iTunes（Windows）中找到 iPhone → 文件标签 → 展开 MDict 文件夹 → 把 mdx/mdd/css 文件拖进去
2. **其他 App 分享**：在文件管理类 App（如"文件"、Documents、iZip）中打开 mdx 文件 → 点击系统分享按钮 → 选择 MDict
3. **云盘同步**：把词典上传到 iCloud Drive，在 iPhone 上用"文件" App 定位到这个文件 → 分享 → MDict

> [!NOTE]
> iOS 版对最新版 mdd 2.0 发音文件的支持不完整，部分词典在 iPhone 上点击发音按钮可能没反应。iOS 系统本身不允许 App 使用系统 TTS 以外的在线 TTS 服务，扩展发音方案在 iOS 上行不通。

### macOS

Mac 版同样在 App Store 分发，但沙盒限制比 iOS 宽松。最直接的导入方式：

1. 打开 Finder → 按 `Cmd + Shift + G` 弹出路径输入框
2. 输入 `~/Library/Application Support/MDict/` 回车
3. 把 mdx/mdd/css 文件拖进这个文件夹

也可以用 AirDrop 从 iPhone 发送，Mac 上选择"打开方式 → MDict"即可自动导入。

---

## 📖 使用入门

安装完成、词库放好之后，MDict 就能正常工作了。下面介绍几个日常高频用到的核心功能，以及一些容易被忽略的进阶技巧。

### 同一套词库跑遍全平台

mdx / mdd 文件是纯数据格式，与操作系统无关。MDict 在四个平台上的核心读取引擎是同源的——PC 版 C++、Android 版 Java、iOS 版 Objective-C、Mac 版 Swift 都调用了相同的 mdx 解析逻辑，所以同一个词典文件在任何平台上的渲染效果基本一致。

实际操作时，先在 PC 上整理好你需要的所有词典（mdx + 对应的 mdd 和 css），然后通过 U 盘或云盘拷贝到各设备。Android 放进 `/sdcard/mdict/doc/`，iOS 通过分享菜单导入，macOS 丢进 Application Support 目录。四个平台就能同时拥有一模一样的词典集合，分组配置在 MDict 内部独立保存，互不影响。

> [!WARNING]
> 不同平台的 MDict 版本差异会导致兼容性问题：PC 1.3 RC4 对 mdx 2.x 格式支持不如 2.0 完善，个别词典可能出现词条丢失；Android v2.2 及更早版本不支持 mdd 2.0 格式，需要升级或改用 DictTango；iOS 版长期不更新，3D Touch 预览词条在部分机型上失效。遇到异常时先检查版本号。

### 词条内点击跳查——不懂的词当场就跳过去

当你打开一条词条（比如查 "metaphor" 得到一条长篇解释），发现里面又出现了 "analogy" 这个你也不懂的词。传统词典要手动退出 → 回到搜索框 → 重新输入 → 再点进去。MDict 的点击取词机制让你**直接点这个词**，它就自动跳去查新词，全程不打断阅读流。

各平台的操作方式略有差异：

| 平台 | 操作方式 |
|------|----------|
| Windows PC | 默认开启，词条页双击单词，或 Ctrl + 单击 |
| Android | 设置 → 控制 → 手动开启"使用点击手势（取词）"，然后在词条页轻按单词 |
| iOS | 设置里打开"词条内点击查词"开关，轻按单词跳转 |
| macOS | 原生支持 Cmd + 点击跳转，也支持三指轻触定义 |

跳查可以形成链——你在第一个词条里点新词、在新词条里又点新词……MDict 会记录你走过的每一步，可以随时点"返回"回到上一层，或者跳到链的起点。举个例子：查 "ephemeral" → 看到 "persistent" 这个对比词，点它 → 再看到 "ubiquitous" 这个扩展词，再点……三个词的区别和关联就在跳查中自然清晰了。这个机制对背单词、精读英语论文特别有帮助。

### 模糊搜索语法——通配符、前缀匹配、拼写纠正

MDict 默认开启**部分匹配模糊搜索**，也就是说你每输入一个词的一部分，它会在前后各加一个 `*` 来搜索。输入 `comp` 等价于搜 `*comp*`，打字时实时弹出候选列表，帮你快速定位目标词条。

支持两个通配符：

- `*` — 匹配任意长度的任意字符（包括零字符）
- `?` — 匹配恰好一个任意字符

```
comp*       → 所有以 comp 开头的词条（complete、compare、computer……）
*tion       → 所有以 tion 结尾的词条（action、nation、situation……）
c*tion      → 以 c 开头、tion 结尾的（combination、connection……）
c?mp        → c 和 mp 之间恰好一个字符的（camp、comp、cimp……）
```

如果想精确查某一个词，在搜索词前后加空格或用引号：`"complete"` 只匹配词条标题正好是 complete 的，不匹配 completeness 或 incompletely。

当你输入一个完全不存在的词时，MDict 会利用内置的拼写纠正数据自动推荐最接近的正确拼写。输入 `compear` 会提示"拼写错误，是否要查 compare？"。英语拼写纠正内置在程序里，其他语言（俄语、德语、法语等）需要额外安装拼写矫正数据包。

> [!NOTE]
> 在联合查询模式下做模糊搜索时，MDict 会同时查询分组里所有词典的 mdx 索引。如果分组里词典数量过多（超过 5 本），响应速度会明显变慢。建议做模糊搜索时切换到"所有词典"或只选 1-2 本词典。

### 多词典统一搜索

MDict 允许你同时选中多本词典进行联合查询，输入一个词时会并行搜索所有选中的词典，结果按词典分组展示。这个功能在查专业术语时特别有用——同时打开一本英汉词典、一本计算机专业词典、一本术语表，搜一个词就能同时看到多份解释，省去了来回切换词典的麻烦。

---

## 🔗 相关资源

- [mdict官网](https://mdict.wrwq.fun)
- [mdict文档](https://mdict.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/mdict)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/mdict)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E4%B8%8B%E8%BD%BD%E5%AE%98%E7%BD%91/mdict)

---

## 📚 相关文章

- [MDict 到底值不值得装？快问快答 9 组](https://laoguosofts.github.io/posts/mdict.html)
- [MDict 问答速查：从入门到精通的 8 个必问](https://alipymanbu.github.io/posts/mdict.html)
