# CLAUDE.md - AI Agent 控制中枢

> 本文件是 AI 的主入口，包含核心规则和工作流。
> 完整细则见 `.claude/` 目录（rules / guardrails / protocols / skills / hooks / memory）。

## 项目概述

- **项目名称**: [PROJECT_NAME]
- **技术栈**: [例: TypeScript / Node.js / PostgreSQL / React]
- **架构风格**: [例: 模块化单体 / 整洁架构]

## 核心原则

1. **约束优先**: 写代码前先读 `docs/` 下的约束文档
2. **禁止猜测**: 需求不明确时必须提问，绝不假设
3. **增量迭代**: 小步提交、可审查的变更
4. **先设计后编码**: API 变更先更新 `docs/API_SPEC.md`，模型变更先更新 `docs/DATA_MODEL.md`

## 文档阅读顺序

| 优先级 | 文档 | 用途 |
|--------|------|------|
| P0 | `CLAUDE.md` | 核心规则与工作流（本文件） |
| P0 | `.claude/rules/` | 编码、架构、审查的完整细则 |
| P0 | `.claude/guardrails/` | 文件范围、命名、禁止项 |
| P1 | `docs/PRODUCT.md` | 产品边界 - 做什么、不做什么 |
| P1 | `docs/ARCHITECTURE.md` | 架构、技术栈、环境变量 |
| P2 | `docs/DATA_MODEL.md` | 数据模型 |
| P2 | `docs/API_SPEC.md` | API 契约 |
| P3 | `docs/runbooks/` | 运维手册 - 部署与排错 |
| P3 | `docs/tasks/CURRENT.md` | 当前任务 |

## 编码规则

### 基本规范

- 单一职责：每个函数/类只做一件事
- 禁止硬编码：魔法数字和字符串必须提取为常量或配置
- 类型安全：禁止 `any`，确有需要时用 `unknown` + 类型守卫
- 错误处理：禁止空 catch，所有错误必须妥善处理或向上传播
- 纯函数优先，副作用隔离到边界层
- 函数 ≤ 50 行，文件 ≤ 300 行，嵌套 ≤ 3 层

### 安全规范

- 禁止 SQL 拼接，必须参数化查询或 ORM
- 禁止信任用户输入，所有外部输入必须校验
- 禁止明文存储密码（bcrypt/argon2）
- 禁止在代码中硬编码密钥（通过环境变量注入）
- 禁止在日志和错误响应中暴露敏感信息

### 绝对禁止

- `eval()` / `Function()` 等动态代码执行
- `// @ts-ignore` / `// @ts-nocheck`
- `console.log` 留在生产代码中
- 未经确认执行 `DROP TABLE` / `TRUNCATE`

## 架构规则

### 分层依赖

```
src/
├── modules/          # 功能模块（垂直切片）
│   └── [feature]/
│       ├── controller/   # HTTP 处理器
│       ├── service/      # 业务逻辑
│       ├── repository/   # 数据访问
│       └── dto/          # 数据传输对象
├── services/         # 横切应用服务
├── domain/           # 纯领域模型（零依赖）
└── infra/            # 基础设施适配器
```

**依赖方向（严格）**:
```
controller → service → domain
                 └→ repository → infra
```

- `domain/` 不依赖任何层
- `service/` 仅依赖 `domain/`
- `controller/` 仅依赖 `service/`
- 禁止反向依赖、禁止跨层调用

### 变更流程

- 架构变更 → 先在 `docs/decisions/` 创建 ADR
- API 变更 → 先更新 `docs/API_SPEC.md`
- 模型变更 → 先更新 `docs/DATA_MODEL.md`

## 工作流

### 开始任务前

```
1. 阅读本文件（CLAUDE.md）
2. 阅读 docs/tasks/CURRENT.md —— 了解当前任务
3. 阅读相关的 docs/ 文档（如涉及 API 则读 API_SPEC.md）
```

### 执行过程中

```
1. 小步前进，保持可回退
2. 遵循上述编码和架构规则
3. 新增功能按由内向外顺序：domain → service → controller → 测试
4. 修复缺陷先写失败测试，再修复，再验证
```

### 完成后

```
1. 按 .claude/rules/review.rules.md 进行自审
2. 确保所有测试通过
3. 如发现新模式或问题，更新 .claude/memory/
4. 更新 docs/tasks/CURRENT.md 的进度
```

## 版本管理

- **分支策略**: [例: GitHub Flow / Trunk-based]
- **提交规范**: [例: Conventional Commits]
- **分支命名**: `feature/xxx`、`fix/xxx`、`refactor/xxx`

## 环境配置

- **包管理器**: [例: pnpm / npm]
- **安装依赖**: `[例: pnpm install]`
- **构建**: `[例: pnpm build]`
- **测试**: `[例: pnpm test]`
- **检查**: `[例: pnpm lint]`
- **开发**: `[例: pnpm dev]`
