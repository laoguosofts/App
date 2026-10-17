## ✨ Chai 是什么

Chai 是 Node.js 和浏览器端最流行的 BDD / TDD 断言库之一，和 Mocha、Vitest、Jest 等测试框架搭配使用。它的核心理念很简单：**把断言写成像自然语言一样通顺**，让你在几百行测试代码里扫一眼就知道在测什么、哪里出了问题。

相比于测试框架自带的极简断言（比如 Node 内置的 `assert`），Chai 提供了三套风格：

| 风格 | 写法 | 适用场景 |
| --- | --- | --- |
| `expect` | `expect(foo).to.equal('bar')` | 最通用，自然语言链 |
| `should` | `foo.should.equal('bar')` | 直接挂在对象上，可读性强 |
| `assert` | `assert.equal(foo, 'bar')` | 传统断言，风格接近 Node 内置 |

三套风格共享同一套断言方法，选哪个主要看团队习惯。新上手建议从 `expect` 开始——灵活性最高，也最不容易出坑（`should` 会修改 Object.prototype，极老的代码里可能有冲突）。

Chai 本身是轻量的，核心只提供基础断言能力。真正的威力来自插件生态：200+ 社区插件覆盖了 Promise、HTTP 接口、对象子集、字符串、DOM、JSON Schema 等几乎所有测试场景，按需 `chai.use(plugin)` 即可扩展。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://chai.fedq.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - npm：`npm install chai --save-dev`（Node.js / 前端项目）
> - CDN：浏览器端可以直接通过 unpkg 或 jsDelivr 引入 `chai.js`

---

## 🔧 安装与环境准备

Chai 运行在 Node.js 环境或现代浏览器里，最常见的安装方式是通过 npm 作为开发依赖安装。

### Node.js 项目

```bash
npm install chai --save-dev
```

安装完成后，在测试文件里引入：

```javascript
// CommonJS
const chai = require('chai');
const expect = chai.expect;

// ESM / TypeScript
import { expect } from 'chai';
```

> [!TIP]
> **和测试框架配合**：Chai 本身不是测试框架，它只负责断言。你还需要安装 Mocha / Vitest / Jest 等框架来组织 `describe` / `it` 测试用例。最经典的组合是 `Mocha + Chai`，现代前端项目常用 `Vitest + Chai`（Vitest 也自带 `expect`，但 Chai 的插件生态更丰富）。

### 浏览器端

如果不使用构建工具，直接在 HTML 里引入：

```html
<script src="https://unpkg.com/chai/chai.js"></script>
<script>
  const expect = chai.expect;
  // ... 在浏览器控制台或脚本里写断言
</script>
```

### 插件安装

装完 Chai 之后，根据场景按需装插件。比如做 API 测试最常用的三件套：

```bash
npm install chai-http chai-as-promised chai-subset --save-dev
```

然后在测试入口文件里统一注册：

```javascript
import chai from 'chai';
import chaiHttp from 'chai-http';
import chaiAsPromised from 'chai-as-promised';
import chaiSubset from 'chai-subset';

// 注册顺序：chai-as-promised 建议先注册
chai.use(chaiAsPromised);
chai.use(chaiHttp);
chai.use(chaiSubset);
```

> [!NOTE]
> **插件维护状态参差不齐**：Chai 插件是社区驱动的，安装前建议查一下 npm weekly downloads 和 GitHub 最近 commit 时间。长期不维护的插件可能在新版 Node 或 ESM 环境下出问题。

---

## 📖 使用入门

### 语言链：让断言读起来像一句话

Chai 最标志性的特性就是语言链（language chain）——`.to`、`.be`、`.have`、`.a`、`.an` 这些单词什么都不做，纯粹是让句子通顺。你可以自由组合，比如：

```javascript
expect([1, 2, 3]).to.be.an('array').that.includes(2);
expect(user).to.have.property('email').that.is.a('string');
```

常用的链词有：`.to`、`.be`、`.been`、`.is`、`.that`、`.which`、`.and`、`.have`、`.with`、`.at`、`.of`、`.same`、`.but`、`.does`、`.did`、`.will`、`.would`、`.itself`。它们可以任意组合，不加也不影响断言结果，只是为了可读性。

### 数值比较：above / below / least / most / within / closeTo

六个数值比较断言覆盖了从简单大小关系到浮点精度的所有场景：

```javascript
// 大于 / 小于
expect(42).to.be.above(40);
expect(42).to.be.below(50);

// 大于等于 / 小于等于（.at 是语言链，可省略）
expect(42).to.be.at.least(42);
expect(42).to.be.at.most(50);

// 闭区间包含
expect(42).to.be.within(40, 50);
```

> [!WARNING]
> **浮点精度是测试重灾区**。`expect(0.1 + 0.2).to.equal(0.3)` 会失败（结果是 `0.30000000000000004`）。涉及小数运算时**一律用 `.closeTo`**，指定允许的绝对误差：
>
> ```javascript
> expect(0.1 + 0.2).to.be.closeTo(0.3, 0.0001);  // ✅
> ```

日期对象也可以直接比较——Chai 内部会调 `.getTime()` 转成时间戳：

```javascript
expect(new Date('2024-01-02')).to.be.above(new Date('2024-01-01'));
```

### 集合逻辑：.any / .all 配合 keys 和 members

这组修饰器解决的是「全部满足」vs「任意一个满足」的语义问题，在验证 API 返回字段和枚举值范围时特别好用。

**验证对象的键**：

```javascript
const user = { name: 'Alice', email: 'a@b.com' };

// 默认 all：要求所有给定的键都存在
expect(user).to.have.all.keys('name', 'email');           // ✅
expect(user).to.have.all.keys('name', 'phone');          // ❌ phone 不存在

// any：只要有一个就行
expect(user).to.have.any.keys('email', 'phone');          // ✅ email 存在
expect(user).to.have.any.keys('phone', 'fax');           // ❌ 都不存在
```

`.any.keys` 最经典的场景是检查用户对象**至少有一种联系方式**：

```javascript
expect(user).to.have.any.keys('email', 'phone', 'googleId', 'facebookId');
```

**验证数组的成员**：

```javascript
const arr = [1, 2, 3];

// all.members = 成员完全匹配（集合相等）
expect(arr).to.have.all.members([1, 2, 3]);               // ✅
expect(arr).to.have.all.members([1, 2]);                  // ❌ 缺了 3

// any.members = 包含至少一个
expect(arr).to.have.any.members([1, 4]);                  // ✅ 1 存在
```

别和 `.includeMembers` 搞混——`.includeMembers` 只要求「haystack 包含 needle」，haystack 可以多，而 `.all.members` 要求**完全相等**。

### HTTP 接口测试：chai-http 插件

这是 Node.js 后端开发者最常用的插件，基于 superagent 封装。两种用法：**直接传 Express/Koa 应用实例**或**传远程 URL**。

**对本地 Express 应用测试**——chai-http 会自动管理服务器的 listen 和 close，你不需要手动启动和关闭：

```javascript
import express from 'express';
import chai from 'chai';
import chaiHttp from 'chai-http';

chai.use(chaiHttp);
const expect = chai.expect;

const app = express();
app.get('/health', (req, res) => res.json({ status: 'ok' }));

it('健康检查返回 200', async function () {
  const res = await chai.request(app).get('/health');
  expect(res).to.have.status(200);
  expect(res.body).to.have.property('status', 'ok');
});
```

> [!TIP]
> **多请求连续测试用 `.keepOpen()`**：默认每个 `.end()` 之后服务器就关了。如果你要先登录再访问受保护资源，用 `.keepOpen()` 保持服务器存活，测完手动 `.close()`：
>
> ```javascript
> const requester = chai.request(app).keepOpen();
> const loginRes = await requester.post('/login').send({ username, password });
> const profileRes = await requester.get('/profile').set('Authorization', `Bearer ${token}`);
> requester.close();  // 记得手动关闭
> ```

**对远程 URL 测试**（只在 Node.js 上工作）：

```javascript
const res = await chai.request('https://api.example.com').get('/users/123');
expect(res).to.have.status(200);
```

**请求构造方法链式调用**：

```javascript
await chai.request(app)
  .post('/users')
  .set('Content-Type', 'application/json')
  .query({ debug: true })
  .send({ name: 'Alice' });
```

**响应断言**：chai-http 在 `res` 对象上扩展了状态码、响应头、Cookie 等断言：

```javascript
expect(res).to.have.status(200);
expect(res).to.have.header('content-type', /json/);
expect(res.body).to.have.property('data').that.is.an('array');
expect(res).to.redirectTo('/new-path');
expect(res).to.have.cookie('sessionId');
```

### 插件生态精选

Chai 的插件很多，但常用的也就那几个组合，按场景归类如下：

| 场景 | 推荐插件 | 用途 |
| --- | --- | --- |
| 异步 Promise | **chai-as-promised** | `.eventually` / `.rejectedWith` 断言 Promise 结果 |
| HTTP/API 测试 | **chai-http** | Express/Koa 集成测试，自动服务器管理 |
| 对象子集 | **chai-subset** | `.to.containSubset(...)` 验证 API 返回的部分字段 |
| 字符串增强 | **chai-string** | `.startWith()` / `.endWith()` 前缀后缀断言 |
| DOM 测试 | **chai-dom** | jsdom / 浏览器环境的 DOM 断言 |
| 精度数值 | **chai-bignumber** | bignumber.js / decimal.js 精度断言 |

> [!NOTE]
> **不要为了小功能装插件**。比如只用到一个 `.startWith`，用内置的 `.match(/^hello/)` 或 `.include` 就够了。每多一个插件就多一个 `chai.use()` 调用和一个依赖，轻量优先。

**推荐的插件组合**：

- **Node.js API 测试（Express/Koa）**：chai + chai-http + chai-as-promised + chai-subset
- **前端组件测试（Vitest）**：直接用 Vitest 自带的 `expect` + @testing-library（不一定需要 Chai）
- **通用异步测试**：chai + chai-as-promised

### 写好断言的几个原则

1. **一个断言只测一件事**。`expect(res).to.have.status(200)` 和 `expect(res.body).to.have.property('name')` 分开写，失败时能直接定位是哪个条件不满足。

2. **断言要具体**。`expect(result).to.not.be.null` 不如 `expect(result).to.be.an('object').that.has.property('id')`——后者不仅断言非空，还说了类型和结构。

3. **错误信息要可读**。Chai 默认的错误信息已经很清楚了（比如 `AssertionError: expected 42 to be above 50`），复杂断言可以加自定义消息：`expect(users.length, '应该返回 10 条数据').to.equal(10)`。

4. **善用 `.deep` 做深度比较**。对象和数组默认是引用比较，加 `.deep` 才会逐字段比较：`expect({ a: 1 }).to.deep.equal({ a: 1 })`。

---

## 🔗 相关资源

- [Chai官网](https://chai.fedq.ren)
- [Chai文档](https://chai-zh-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/Chai)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/Chai)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E5%AE%89%E5%8D%93%E7%89%88/Chai)

---

## 📚 相关文章

- [那个被测试坑到想砸键盘的下午，Chai 救了我](https://laoguosofts.github.io/posts/chai-zh-docs.html)
- [从堆满 console.log 的测试文件到三行断言——我是怎么被 Chai 拯救的](https://alipymanbu.github.io/posts/chai-zh-docs.html)
