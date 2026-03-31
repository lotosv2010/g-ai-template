# AGENTS.md - 通用 AI Agent 开发规范

> 本文件是所有 AI 开发工具（Claude Code、Cursor、Copilot、Windsurf 等）的通用入口。
> 各工具特有配置请参见对应文件（如 `CLAUDE.md`）。

## 项目概述

- **项目名称**: [PROJECT_NAME]
- **技术栈**: [例: TypeScript / Node.js / PostgreSQL / React]
- **架构风格**: Monorepo + [例: 模块化单体 / 整洁架构]
- **包管理**: pnpm workspace

## 核心原则

1. **约束优先**: 写代码前必须阅读并遵守约束文档
2. **禁止猜测**: 需求不明确时必须提问，绝不假设
3. **增量迭代**: 小步提交、可审查的变更，禁止一次性重写大模块
4. **可追溯**: 每个变更必须关联任务或决策记录
5. **模块自治**: 每个 package 有独立上下文，修改前先阅读其 `AGENTS.md`

## 文档体系

### 项目级文档（全局约束）

| 优先级 | 文档 | 用途 |
|--------|------|------|
| P0 | `AGENTS.md` | 通用 AI 开发规范（本文件） |
| P1 | `docs/PRODUCT.md` | 产品边界 - 做什么、不做什么 |
| P1 | `docs/ARCHITECTURE.md` | 架构白皮书、技术栈、部署 - 未经 ADR 不可修改 |
| P2 | `docs/DATA_MODEL.md` | 数据模型 - 变更需人工审批 |
| P2 | `docs/API_SPEC.md` | API 契约 - 前后端统一真相源 |
| P3 | `docs/team/ROLES.md` | 角色分工 - 谁负责什么模块 |
| P3 | `docs/tasks/BOARD.md` | 任务看板 - 多任务并行追踪 |
| P3 | `docs/tasks/CURRENT.md` | 活跃任务索引 - 各 Agent 当前任务 |
| P4 | `docs/runbooks/` | 运维手册 - 部署与排错 |

### 模块级文档（包内约束）

每个 `packages/[name]/` 下有独立的 `AGENTS.md`，定义该模块的：
- 模块职责与边界
- 内部架构说明
- 依赖关系（允许依赖哪些其他包）
- 模块特有的开发约定

**阅读顺序**: 先读项目级 `AGENTS.md`，再读目标模块的 `packages/[name]/AGENTS.md`。

## Monorepo 结构

```
packages/
├── web/                 # 前端应用
│   ├── AGENTS.md
│   └── src/  (pages/ components/ hooks/ stores/ api/ styles/)
│
├── server/              # 服务端应用
│   ├── AGENTS.md
│   └── src/  (modules/ services/ domain/ infra/)
│
├── ai/                  # AI 能力层
│   ├── AGENTS.md
│   └── src/  (agents/ prompts/ chains/ tools/ models/)
│
├── shared/              # 通用共享库
│   ├── AGENTS.md
│   └── src/  (types/ constants/ utils/ validators/)
│
└── [more-packages]/     # 按需扩展
```

### 包间依赖规则

```
shared  ← 最底层，零业务依赖
ai      ← 依赖 shared
server  ← 依赖 shared、ai
web     ← 依赖 shared
```

- 新增包间依赖需在对应 `AGENTS.md` 中声明
- 详细的依赖约束见各包的 `AGENTS.md`

## 通用开发规则

> 以下为原则级约束。各 AI 工具可在自身配置中维护完整细则。

### 编码

- 单一职责、禁止硬编码、类型安全（禁 `any`）、禁止空 catch
- 函数 ≤ 50 行、文件 ≤ 300 行、嵌套 ≤ 3 层

### 安全

- 禁止 SQL 拼接、禁止信任用户输入、禁止明文密码、禁止硬编码密钥

### 架构

- 依赖方向由外向内（controller → service → domain），禁止反向
- domain 层零框架依赖，模块间通过公开接口通信，禁止循环依赖

### 版本管理

- **分支策略**: [例: GitHub Flow / Git Flow / Trunk-based]
- **主分支**: `main`
- **分支命名**: `feature/xxx`、`fix/xxx`、`refactor/xxx`
- **提交规范**: [例: Conventional Commits]

### 代码质量

- **Lint**: [例: ESLint + Prettier]
- **类型检查**: [例: TypeScript strict mode]
- **测试覆盖率**: [例: 核心逻辑 > 80%]

### 变更流程

- 架构变更 → 先创建 ADR（`docs/decisions/`）
- API 变更 → 先更新 `docs/API_SPEC.md`
- 模型变更 → 先更新 `docs/DATA_MODEL.md`
- 新增包 → 先创建 `packages/[name]/AGENTS.md` 定义职责和边界

## 工作流

```
1. 阅读 AGENTS.md（本文件）
2. 阅读 docs/team/ROLES.md —— 确认自己的角色和模块范围
3. 阅读 docs/tasks/CURRENT.md —— 找到自己的当前任务
4. 阅读 docs/tasks/BOARD.md —— 了解任务详情和依赖关系
5. 阅读目标模块的 packages/[name]/AGENTS.md —— 模块上下文
6. 执行任务（遵循上述规则，只修改自己负责的模块）
7. 自审（安全、架构合规、测试）
8. 更新 BOARD.md 任务状态和 CURRENT.md 进度
```

## 环境配置

- **包管理器**: pnpm
- **安装依赖**: `pnpm install`
- **构建全部**: `pnpm -r build`
- **测试全部**: `pnpm -r test`
- **检查全部**: `pnpm -r lint`
- **开发前端**: `pnpm --filter web dev`
- **开发服务端**: `pnpm --filter server dev`
