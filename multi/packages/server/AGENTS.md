# Server 服务端 - AI 上下文

> 本文件定义 `server` 包的模块级 AI 开发上下文。
> AI 在修改本模块代码前必须阅读。

## 模块职责

**服务端应用** - 核心业务逻辑、API 服务与数据持久化。

- 提供 RESTful / GraphQL API 接口
- 实现业务逻辑和领域规则
- 管理数据持久化与迁移
- 处理认证、授权与会话
- 对接外部服务和消息队列

## 内部架构

```
src/
├── modules/          # 功能模块（垂直切片）
│   └── [feature]/
│       ├── controller/   # HTTP 处理器 / 路由入口
│       ├── service/      # 业务逻辑
│       ├── repository/   # 数据访问
│       └── dto/          # 数据传输对象
│
├── services/         # 横切应用服务
│   ├── auth/             # 认证授权
│   ├── notification/     # 通知服务
│   └── scheduler/        # 定时任务
│
├── domain/           # 纯领域模型（最内层，零依赖）
│   ├── entities/         # 实体
│   ├── value-objects/    # 值对象
│   └── events/           # 领域事件
│
└── infra/            # 基础设施适配器
    ├── database/         # 数据库连接与迁移
    ├── cache/            # 缓存
    ├── queue/            # 消息队列
    └── external-api/     # 外部 API 客户端
```

## 依赖规则

### 层级依赖（严格由外向内）

```
controller → service → domain
                 └→ repository → infra
```

- `domain/` 不依赖任何层
- `service/` 仅依赖 `domain/`
- `repository/` 依赖 `domain/` 和 `infra/`
- `controller/` 仅依赖 `service/`

### 包间依赖

| 可以依赖 | 禁止依赖 |
|----------|----------|
| `@[scope]/shared` | `@[scope]/web` |
| `@[scope]/ai`（调用 AI 能力时） | 其他包的内部实现 |

## 本模块特有约定

- 所有 Controller 使用统一响应信封（参见 `docs/API_SPEC.md`）
- 数据库操作统一使用 ORM，禁止 raw SQL（安全审计例外）
- 定时任务放在 `src/services/scheduler/`，不放在 modules 中
- 外部 API 调用统一封装在 `src/infra/external-api/`，不在业务层直接调用
- 所有接口必须有请求参数校验

## 已有模块

| 模块 | 职责 | 状态 |
|------|------|------|
| [例: user] | 用户管理 | [开发中/已完成] |
