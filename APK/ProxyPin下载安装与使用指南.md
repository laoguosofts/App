## ✨ ProxyPin 是什么

ProxyPin 是一款面向移动端的 HTTP/HTTPS 抓包代理工具，核心定位是让开发者和测试人员在手机上直接观察和修改 App 的网络请求。不同于传统 PC 端 Charles、Fiddler 需要电脑配合，ProxyPin 在 Android 设备上以本地 VPN 的方式工作，无需 root，装上证书即可拦截和解密 HTTPS 流量。

它能做的事覆盖了抓包调试的全流程：请求列表实时展示、域名级流量过滤、HTTPS 解密查看明文、请求/响应的脚本化修改、多种压缩编码自动解压。对移动开发者来说，最常用的场景是调试 API 调用链路、核对请求参数、排查 App 与后端的协议对接问题；对测试人员来说，域名过滤 + 脚本引擎的组合可以快速构造边界条件做接口测试。

值得一提的是它的脚本引擎——内置 onRequest 和 onResponse 两个 JavaScript 钩子，配合 context 对象的持久化存储、日志输出、延迟模拟等 API，能实现自动注入鉴权 Token、临时切换 API 版本、条件屏蔽特定请求等自动化操作。这种"抓包 + 可编程钩子"的组合，让它在移动端调试工具里脱颖而出。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://proxypin.fewf.click/download.html) —— 下载页整理了当前可用的版本和安装说明
> - 应用商店：Android 平台可在应用宝、小米应用商店等渠道搜索 ProxyPin；iOS 平台因系统限制建议走官方下载页

---

## 🔧 安装与初始配置

ProxyPin 的安装分两步：先装应用，再配置代理证书。HTTPS 解密必须有证书信任才能工作，这一步不能跳过。

### Android 安装流程

1. **下载 APK**：从下载页获取最新版本，确认文件大小和版本号与下载页标注一致。

   > [!NOTE]
   > 部分第三方下载渠道可能提供旧版本。建议优先从官方下载页获取，避免使用过期版本。v1.1.9 及以上版本新增了 zstd 压缩算法支持，旧版本解压这类响应时会显示乱码。

2. **开启未知来源安装**：进入手机「设置 → 安全 → 安装未知应用」，找到浏览器或文件管理器，授予"允许安装应用"权限。不同机型路径略有差异，小米/OPPO/华为等品牌的安全设置入口可能在更深层的菜单里。

3. **安装 APK**：点击下载好的 APK 文件，按提示完成安装。如果手机弹出风险提示（来自系统安全中心的常规警告），选择"仍然安装"即可——这是未上架 Google Play 应用的正常现象。

4. **安装 HTTPS 证书**：首次打开 ProxyPin 时，应用会引导你安装自签名证书。进入「设置 → 安全 → 加密与凭据 → 从存储设备安装证书」，选择 ProxyPin 生成的 CA 证书文件，证书用途选"VPN 和应用"。

   > [!WARNING]
   > Android 7.0 默认不信任用户安装的 CA 证书，部分 App 会绕过系统证书存储导致抓不到明文。如果遇到这个问题，需要手机 root 后把证书移到系统证书目录，或让 App 的网络库显式信任用户证书。

5. **启动代理服务**：回到 ProxyPin 主界面，点击中间的"启动"按钮，系统会弹出 VPN 授权弹窗，点击"允许"。状态栏出现钥匙图标即代表代理已运行。

### 安装后核对清单

| 检查项 | 验证方式 | 通过标准 |
|--------|----------|----------|
| 应用版本 | 「我的 → 关于」 | 与下载页最新版本一致 |
| 代理服务 | 状态栏钥匙图标 | 持久显示，不自动消失 |
| HTTP 抓包 | 打开任意 App | 请求列表开始刷新 |
| HTTPS 解密 | 访问 https 站点 | 响应体显示明文而非乱码 |

---

## 📖 使用入门：从抓包到脚本自动化

ProxyPin 的日常使用可以分成四层：先过滤掉噪音流量，再查看和分析请求，然后按需修改，最后用脚本自动化重复性工作。多数人只用前两层，但脚本引擎才是它真正的生产力所在。

### 第一步：域名过滤——只看关心的流量

移动端 App 的网络通信远不止目标 API。Google Play Services、Apple Push、友盟、Firebase、Adjust、AppsFlyer、Google Analytics 这些统计和追踪 SDK 会产生大量与业务无关的请求，不做过滤的话列表会被淹没。

域名过滤在连接层生效——不符合规则的域名直接跳过 TLS 握手，不仅减少干扰，还能提升抓包性能。

**配置入口**：「我的 → 代理过滤」，推荐白名单模式。三种模式的区别：

| 模式 | 行为 | 适用场景 |
|------|------|----------|
| 不过滤 | 拦截所有域名 | 首次启动时查看全景 |
| 白名单模式 | 只拦截列表中的域名 | 已知目标 App 域名，精准抓包 |
| 黑名单模式 | 拦截除列表外的所有域名 | 只想排除少数噪音源 |

规则用正则表达式，几个常用模式：

```
# 精确匹配子域
api.myapp.com

# 匹配所有子域（api.myapp.com、cdn.myapp.com 都能匹配）
.*\.myapp\.com

# 多个独立域名用 | 分隔
api\.myapp\.com|cdn\.myapp\.com
```

> [!TIP]
> 正则中的 `.` 匹配任意字符，域名里的点必须用 `\.` 转义。常用的规则放在列表前面，正则引擎从前往后匹配，先匹配到就跳过后续规则。避免过于宽泛的规则（如 `.*`），等于没过滤。

典型实践：先用域名过滤屏蔽 90% 无关域名，再在已拦截的流量中做高级搜索定位特定请求。域名过滤是连接层的"粗筛"，高级搜索是请求级的"细筛"，两层配合才高效。

### 第二步：查看请求与响应

过滤完成后，ProxyPin 的主列表会实时展示拦截到的请求。点击任意一条进入详情页，可以看到完整的请求头、请求体、响应头、响应体。

ProxyPin 会自动处理多种响应体格式：

| 内容编码 | 处理方式 |
|----------|----------|
| gzip | 自动 gunzip 后显示明文 |
| deflate | 自动 inflate |
| Brotli | 自动解压（HTTPS + HTTP/2 环境常见） |
| zstd | v1.1.9+ 自动解压，移动端 App 越来越常见 |
| image/* | 直接预览图片 |
| application/json | 格式化显示 |

如果看到响应体是乱码，先检查 `Content-Encoding` 头——ProxyPin 会读取这个头来决定是否解压。有些 App 用自定义压缩（不走 Content-Encoding 声明），或者压缩叠加加密（AES + zstd），这种情况需要先解密再解压。

> [!NOTE]
> Brotli 编码在 HTTPS + HTTP/2 下非常常见。如果用 HTTP/1.1 连接（没有 ALPN），服务器可能不返回 Brotli——这是 App 网络库的协商逻辑决定的。开启 ProxyPin 的 HTTP/2 支持后，Brotli 会正常工作。

### 第三步：修改请求——脚本引擎 onRequest 钩子

onRequest 钩子在请求发出前被调用，可以修改 URL、方法、请求头、请求体，甚至返回 null 直接屏蔽请求。这是 ProxyPin 最有价值的功能之一，适合做临时调试而不改 App 代码。

几个高频场景：

**自动注入鉴权 Token**——测试环境需要临时给请求加 Token：

```javascript
function onRequest(context, request) {
  if (request.url.includes('/api/')) {
    request.headers['Authorization'] = 'Bearer test-token-xyz';
  }
  return request;
}
```

**API 版本切换**——临时测试新版 API 但不想改 App 里的所有路径：

```javascript
function onRequest(context, request) {
  if (request.url.includes('/api/v1/')) {
    request.url = request.url.replace('/api/v1/', '/api/v2/');
  }
  return request;
}
```

**修改请求参数**——把 GET 请求里的某个查询参数改掉：

```javascript
function onRequest(context, request) {
  if (request.method === 'GET' && request.url.includes('/users')) {
    let url = new URL(request.url);
    url.searchParams.set('page', '2');
    request.url = url.toString();
  }
  return request;
}
```

**条件性屏蔽请求**——某些请求不希望发出去，但又不想配置全局屏蔽规则：

```javascript
function onRequest(context, request) {
  if (request.url.includes('/payment/confirm')) {
    return null;  // 返回 null = 屏蔽这个请求
  }
  return request;
}
```

脚本执行出错时，ProxyPin 会在控制台打印错误堆栈，请求回退到原始状态（未被脚本修改），不会中断整个代理流程。调试脚本时建议多加 `context.log()` 打印关键节点。

### 第四步：context 对象——跨调用持久化与高级 API

context 对象是 onRequest 和 onResponse 之间共享数据的桥梁，也是脚本自动化的关键。

**storage：跨调用持久化**——同一个脚本的每次 onRequest / onResponse 调用，都能读写同一份 context.storage。实现"记住上一次"、"累积状态"全靠它。

```javascript
// 记录某个 API 被调用了多少次
function onRequest(context, request) {
  if (!context.storage['api-count']) {
    context.storage['api-count'] = 0;
  }
  if (request.url.includes('/api/')) {
    context.storage['api-count']++;
  }
  return request;
}
```

storage 的生命周期：脚本启用期间持续存在，禁用脚本后清空，不同脚本之间不共享。

典型用法是"在 onRequest 里存、在 onResponse 里取"的请求-响应对偶处理：

```javascript
// 请求里记录预期
function onRequest(context, request) {
  context.storage['expect-success'] = !request.url.includes('/error');
  return request;
}

// 响应里对比实际
function onResponse(context, response) {
  if (context.storage['expect-success'] && response.statusCode >= 400) {
    context.log('预期成功但实际失败: ' + response.statusCode + ' ' + response.url);
  }
  return response;
}
```

**sleep：延迟模拟**——让 onResponse 延迟返回，模拟网络慢或高延迟环境：

```javascript
function onResponse(context, response) {
  if (response.url.includes('/api/')) {
    context.sleep(1500);  // 延迟 1.5 秒
  }
  return response;
}
```

sleep 是阻塞的，不要在 onRequest 里滥用，会导致请求还没发出去就卡住。

**async：异步任务**——需要做耗时操作时用 async，但 async 里的操作不能改变当前 request / response（因为请求已经发出），适合做日志上报、统计采集等旁路操作。

**context.log：调试输出**——脚本里的 console.log 在 ProxyPin 环境里可能不生效，要用 context.log 输出到 ProxyPin 的脚本控制台。

---

## 🔗 相关资源

- [ProxyPin官网](https://proxypin.fewf.click)
- [ProxyPin文档](https://proxypin.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/ProxyPin)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/ProxyPin)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/ProxyPin)

---

## 📚 相关文章

- [ProxyPin 装了一年半后，哪些功能真的在天天用](https://laoguosofts.github.io/posts/proxypin.html)
- [用 ProxyPin 一年半，从第一次装到现在没卸载过](https://alipymanbu.github.io/posts/proxypin.html)
