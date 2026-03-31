# 架构白皮书

> **重要**: 本文档是受控制品。任何修改都需要在 `docs/decisions/` 中创建 ADR（架构决策记录）。

## 架构风格

**模式**: [例: 模块化单体 / 整洁架构 / 六边形架构]

**理由**: [为什么选择这个模式]

## 系统概览

```
[用 ASCII 或 Mermaid 绘制高层架构图]

示例:
┌─────────┐     ┌──────────┐     ┌──────────┐
│  客户端  │────>│  API 网关 │────>│  服务层  │
└─────────┘     └──────────┘     └─────┬────┘
                                       │
                                 ┌─────▼────┐
                                 │   数据库  │
                                 └──────────┘
```

## 分层架构

```
src/
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

## 技术栈

| 组件 | 技术 | 版本 | 选型理由 |
|------|------|------|----------|
| 语言 | [例: TypeScript] | [版本] | [理由] |
| 运行时 | [例: Node.js] | [版本] | [理由] |
| 框架 | [例: Express/Fastify] | [版本] | [理由] |
| 数据库 | [例: PostgreSQL] | [版本] | [理由] |
| 缓存 | [例: Redis] | [版本] | [理由] |
| ORM | [例: Prisma/Drizzle] | [版本] | [理由] |
| 测试 | [例: Vitest/Jest] | [版本] | [理由] |

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

| 变量名 | 说明 | 示例值 | 必填 |
|--------|------|--------|------|
| `DATABASE_URL` | 数据库连接串 | `postgresql://...` | 是 |
| `REDIS_URL` | Redis 连接串 | `redis://...` | 是 |
| `JWT_SECRET` | JWT 签名密钥 | — | 是 |
| `PORT` | 服务端口 | `3000` | 否 |

## 部署架构

```
[部署架构图]
```

| 环境 | 用途 | 基础设施 |
|------|------|----------|
| 开发环境 | 本地开发 | [例: Docker Compose] |
| 预发环境 | 上线前验证 | [例: AWS ECS] |
| 生产环境 | 线上系统 | [例: AWS ECS + RDS] |
