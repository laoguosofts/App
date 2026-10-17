## ✨ Prisma 是什么

Prisma 是一个面向 TypeScript 和 Node.js 的**开源 ORM（对象关系映射）工具集**。它让你用声明式的 Schema 语言定义数据模型，然后自动生成类型安全的数据库客户端，帮你省去手写 SQL、维护数据类型、同步数据库结构这些重复劳动。

Prisma 三件套分别是：**Prisma Schema Language（PSL）** 负责数据建模、**Prisma Client** 是自动生成的查询代码、**Prisma Migrate** 管数据库迁移。它目前支持 PostgreSQL、MySQL、SQLite、MongoDB、SQL Server、CockroachDB 六种主流数据库，Node.js 和 TypeScript 项目里几乎是 ORM 的首选方案。

适合谁用？典型场景包括：全栈开发（Next.js、Nuxt 搭配 Prisma 已经是标配）、快速原型验证（配合 SQLite 零配置起步）、生产级后端服务（配合 migrate deploy + CI/CD 流程）。如果你还在手写 SQL 或者用 Sequelize、TypeORM 这类传统 ORM，值得花时间试试 Prisma 的开发体验。

*本文由个人利用网络公开资料整理编写，非官方文档，仅供学习交流参考；软件信息以官方发布为准，使用中遇到的问题请查阅官方渠道，本站不承担任何责任。*

---

## 📥 下载入口

> 📥 **获取安装包**
>
> - [官方下载（推荐）](https://prisma.fewf.ren/download.html) —— 下载页整理了当前可用的版本和安装说明
> - npm 安装：`npm install prisma --save-dev` + `npm install @prisma/client`（Node.js / TypeScript 项目标准安装方式）
> - Prisma 是开发者工具，以 npm 包形式分发，没有独立的安装包或应用商店版本

---

## 🔧 Prisma 安装与环境准备

Prisma 是 Node.js 生态工具，安装前提是本地已经装好 Node.js（建议 v18 或以上）和 npm。安装分两个包：CLI 工具 `prisma`（开发依赖）和运行时客户端 `@prisma/client`（生产依赖）。

### 安装步骤

1. **初始化项目**：如果你还没有现成的 Node.js 项目，先创建一个
   ```bash
   mkdir my-prisma-project
   cd my-prisma-project
   npm init -y
   ```

2. **安装 Prisma CLI 和客户端**
   ```bash
   npm install prisma --save-dev
   npm install @prisma/client
   ```

3. **如果用 TypeScript，建议一并装类型支持**
   ```bash
   npm install typescript ts-node @types/node --save-dev
   npx tsc --init
   ```

4. **初始化 Prisma 配置**——指定你要用的数据库类型
   ```bash
   # SQLite（最省心，不需要单独装数据库）
   npx prisma init --datasource-provider sqlite

   # 或者 PostgreSQL / MySQL
   npx prisma init --datasource-provider postgresql
   ```

### 安装后核对

- 执行 `npx prisma --version` 能看到版本号输出
- `prisma/schema.prisma` 文件已经自动创建，`.env` 文件里有默认的 `DATABASE_URL`
- `@prisma/client` 安装完成时会自动触发一次 generate，但第一次写好 Schema 后需要手动再跑一次

> [!WARNING]
> **版本一致性很重要**：`prisma`（CLI）和 `@prisma/client`（运行时）两个包的版本号必须完全一致。用 `npm list prisma @prisma/client` 检查，如果不一样就 `npm install prisma@latest @prisma/client@latest` 统一。版本不一致会导致类型错误、生成失败等各种诡异问题。

### 数据库选择建议

| 数据库 | 安装成本 | 适合场景 |
| --- | --- | --- |
| SQLite | 零安装，文件数据库 | 原型验证、课程作业、Electron/Tauri 客户端应用 |
| PostgreSQL | 需安装服务 | 绝大多数生产项目，Prisma 对 PG 支持最成熟 |
| MySQL | 需安装服务 | 已有 MySQL 基础设施的团队 |
| MongoDB | 需安装服务 | 文档型数据模型的项目 |

---

## 📖 Prisma 使用入门

### Schema 文件：Prisma 的核心

Prisma 的一切从 `schema.prisma` 文件开始。这是一份用 Prisma Schema Language（PSL）写的声明式配置，整个文件由三种顶层声明组成：

- **datasource**：指定连接哪个数据库，一个 Schema 里最多一个
- **generator**：指定要生成什么客户端，常见的是 `prisma-client-js`
- **model / enum**：定义数据模型，一个 Schema 里可以有多个

Prisma CLI 查找 Schema 的优先级是：命令行 `--schema` 参数 → `package.json` 的 `prisma.schema` 字段 → 项目根目录的 `prisma/schema.prisma` → 根目录的 `schema.prisma`。建议统一放在默认的 `prisma/schema.prisma`，省得额外配置。

一个典型的 Schema 长这样：

```prisma
datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model Task {
  id          Int      @id @default(autoincrement())
  title       String
  description String?
  done        Boolean  @default(false)
  createdAt   DateTime @default(now())
}
```

`env("DATABASE_URL")` 表示从 `.env` 文件读取连接字符串，这样敏感信息不会硬编码在 Schema 里。`.env` 文件放在 Schema 同级或上级目录，Prisma CLI 会自动加载。

### 快速跑通：SQLite 5 分钟上手

SQLite 是 Prisma 初学者最友好的数据库选择——零安装、零配置、数据库文件就在项目目录里。完整流程：

1. 执行 `npx prisma init --datasource-provider sqlite`，自动生成 `prisma/schema.prisma` 和 `.env`（`DATABASE_URL=file:./dev.db`）
2. 编辑 Schema，写好 model 定义
3. 执行 `npx prisma migrate dev --name init`——这一条命令会同时做三件事：对比 Schema 和现有数据库差异 → 生成迁移 SQL 并应用 → 自动重新生成 Prisma Client
4. 创建查询脚本，实例化 `new PrismaClient()`，然后用 `prisma.task.create()`、`prisma.task.findMany()` 这些类型安全的方法操作数据库

```typescript
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()

async function main() {
  await prisma.task.create({ data: { title: '学习 Prisma' } })
  const tasks = await prisma.task.findMany()
  console.log(tasks)
}

main().finally(() => prisma.$disconnect())
```

> [!TIP]
> Prisma 内置了 Studio 可视化界面。执行 `npx prisma studio` 后浏览器自动打开本地 5555 端口，可以直接查看、筛选、编辑数据库里的记录，开发调试非常方便。

### 改了 Schema 之后：generate 和 migrate 的区别

很多新手分不清这两个命令的用途：

| 命令 | 作用 | 什么时候用 |
| --- | --- | --- |
| `prisma generate` | 只重新生成 Prisma Client 代码 | 改了 model 定义后，想让代码里的类型提示更新 |
| `prisma migrate dev --name xxx` | 生成迁移文件 + 应用到数据库 + 自动 generate | 改了 model 且需要同步数据库结构 |
| `prisma migrate deploy` | **只**应用已有的迁移文件 | 生产环境部署时用 |

日常开发改 Schema 后跑 `migrate dev` 就行，它会自动帮你 generate。

### 常见问题排查：Client 过期或未生成

改了 Schema 也跑了 generate，但代码里还是旧类型？按这个顺序排查：

1. **确认版本一致**：`npm list prisma @prisma/client`，两个版本必须相同
2. **清理再生成**：删掉 `node_modules/.prisma` 目录后重新 `npx prisma generate`
3. **确认 Schema 位置**：看 generate 输出里有没有 "Schema loaded from prisma/schema.prisma"，不对的话用 `--schema` 参数显式指定
4. **重启 TypeScript 服务器**：VS Code 里 Ctrl+Shift+P → "TypeScript: Restart TS Server"，或者直接重启编辑器
5. **检查 output 配置**：如果你在 generator 里自定义了 output 路径，那 Prisma 不会生成到默认的 `node_modules/.prisma/client`，导入路径也要跟着改

> [!NOTE]
> 最暴力的终极方案：删掉整个 `node_modules` 目录、删掉 `prisma/migrations`（原型阶段可以清），然后 `npm install` 重装、`npx prisma generate` 重新生成。原型阶段这么做没什么代价。

### 生产环境部署：migrate deploy 与 CI/CD

生产环境**绝对不要**用 `migrate dev`——它有能力根据 Schema 变化生成新迁移并直接执行，在生产数据库上这么做风险太大。生产环境的正确姿势是：

```
开发环境：改 Schema → migrate dev --name xxx → git commit migrations/
CI 环境：拉代码 → npm ci → migrate deploy → 跑测试
生产环境：拉代码 → npm ci → migrate deploy → 重启应用
```

`migrate deploy` 只做一件事：读取 `migrations/` 目录下已有的迁移文件，对比数据库里的 `_prisma_migrations` 表，按顺序执行未应用的。它完全不碰 Schema 文件，也不会生成新迁移。

Vercel 部署时，在项目设置的 Pre-build Command 里加 `npx prisma migrate deploy` 即可。Docker 部署的话，在容器启动命令里串上 migrate deploy：

```dockerfile
CMD ["sh", "-c", "npx prisma migrate deploy && npm start"]
```

部署前建议先跑一次 `npx prisma migrate status`，确认生产数据库的迁移历史和本地一致，没有 pending 的未应用迁移。

---

## 🔗 相关资源

- [Prisma官网](https://prisma.fewf.ren)
- [Prisma文档](https://prisma-zh-docs.pages.dev/)
- [GitHub 仓库](https://github.com/alipymanbu/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Prisma)
- [Gitee 镜像](https://gitee.com/manbu1103/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Prisma)
- [GitCode 镜像](https://gitcode.com/qq352325405/APK/tree/main/%E6%9C%80%E6%96%B0%E7%89%88/Prisma)

## 📚 相关文章

- [用 Prisma 两周，我终于把写 SQL 的坏毛病戒掉了](https://laoguosofts.github.io/posts/prisma-zh-docs.html)
