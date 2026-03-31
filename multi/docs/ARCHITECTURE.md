# 架构白皮书

> **重要**: 本文档是受控制品。任何修改都需要在 `docs/decisions/` 中创建 ADR（架构决策记录）。

## 架构风格

**模式**: Monorepo + [例: 模块化单体 / 整洁架构 / 六边形架构]

**理由**: [为什么选择这个模式]

## 系统概览

```
[用 ASCII 或 Mermaid 绘制高层架构图]

示例:
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│   web    │────>│  server  │────>│    ai    │     │  shared  │
│  前端应用 │     │  服务端   │     │  AI能力层 │     │  共享库   │
└──────────┘     └────┬─────┘     └──────────┘     └──────────┘
                      │                                  ▲
                 ┌────▼─────┐          所有包共同依赖 ────┘
                 │  数据库   │
                 └──────────┘
```

## Monorepo 包架构

```
packages/
├── web/             → 前端应用（依赖 shared）
├── server/          → 服务端应用（依赖 shared、ai）
├── ai/              → AI 能力层（依赖 shared）
└── shared/          → 通用共享库（零业务依赖）
```

> 各包内部架构详见 `packages/[name]/AGENTS.md`。

## Server 分层架构

> 以下为 `packages/server/src/` 的内部分层。

```
packages/server/src/
├── modules/          # 功能模块（垂直切片）
│   └── [feature]/
│       ├── controller/   # HTTP 处理器 / 入口点
│       ├── service/      # 业务逻辑
│       ├── repository/   # 数据访问
│       └── dto/          # 数据传输对象
│
├── services/         # 横切应用服务
│   ├── auth/
│   ├── notification/
│   └── ...
│
├── domain/           # 纯领域模型与业务规则
│   ├── entities/
│   ├── value-objects/
│   └── events/
│
└── infra/            # 基础设施与外部适配器
    ├── database/
    ├── cache/
    ├── queue/
    └── external-api/
```

## 依赖规则

> 这些规则是绝对的。AI 绝不可违反。

### 层级依赖（Server 内部）

1. `domain/` 不依赖任何层 —— 它是最内层
2. `modules/service/` 仅依赖 `domain/`
3. `modules/repository/` 依赖 `domain/` 和 `infra/`
4. `modules/controller/` 仅依赖 `modules/service/`
5. `services/` 可依赖 `domain/` 和 `infra/`
6. `infra/` 仅依赖外部库，对外暴露接口

```
controller -> service -> domain
                  \-> repository -> infra
```

### 包间依赖

```
shared  ← 被所有包依赖，自身零业务依赖
ai      ← 依赖 shared
server  ← 依赖 shared、ai
web     ← 依赖 shared，禁止依赖 server 和 ai
```

## 技术栈

| 组件 | 技术 | 版本 | 选型理由 |
|------|------|------|----------|
| 语言 | [例: TypeScript] | [版本] | [理由] |
| 运行时 | [例: Node.js] | [版本] | [理由] |
| 前端框架 | [例: React / Vue / Next.js] | [版本] | [理由] |
| 后端框架 | [例: Express / Fastify / NestJS] | [版本] | [理由] |
| 数据库 | [例: PostgreSQL] | [版本] | [理由] |
| 缓存 | [例: Redis] | [版本] | [理由] |
| 队列 | [例: BullMQ] | [版本] | [理由] |
| ORM | [例: Prisma / Drizzle] | [版本] | [理由] |
| AI/LLM | [例: Claude API / OpenAI] | [版本] | [理由] |
| 测试 | [例: Vitest / Jest] | [版本] | [理由] |

## 横切关注点

### 错误处理策略

[描述错误的分类方式、传播路径和返回格式]

### 日志策略

[描述日志级别、结构化日志格式、记录内容]

### 认证与授权

[描述认证流程、会话管理、权限模型]

### 配置管理

[描述配置加载方式、环境变量、密钥管理]

### 环境变量

| 变量名 | 说明 | 所属包 | 示例值 | 必填 |
|--------|------|--------|--------|------|
| `DATABASE_URL` | 数据库连接串 | server | `postgresql://...` | 是 |
| `REDIS_URL` | Redis 连接串 | server | `redis://...` | 是 |
| `JWT_SECRET` | JWT 签名密钥 | server | — | 是 |
| `PORT` | 服务端口 | server | `3000` | 否 |
| `LLM_API_KEY` | LLM 服务密钥 | ai | — | 是 |
| `LLM_MODEL` | 默认模型 | ai | `claude-sonnet-4-5-20250514` | 否 |

## 部署架构

```
[部署架构图]
```

| 环境 | 用途 | 基础设施 |
|------|------|----------|
| 开发环境 | 本地开发 | [例: Docker Compose] |
| 预发环境 | 上线前验证 | [例: AWS ECS] |
| 生产环境 | 线上系统 | [例: AWS ECS + RDS] |
