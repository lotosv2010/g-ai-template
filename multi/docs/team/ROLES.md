# 团队角色与分工

> 定义项目中人类和 AI Agent 的角色、职责和权限。
> 每个 Agent 开始工作前必须确认自己的角色和管辖范围。

## 角色定义

### 人类角色

| 角色 | 负责人 | 职责 |
|------|--------|------|
| 产品负责人 | [姓名] | 产品需求决策、优先级排序、验收确认 |
| 技术负责人 | [姓名] | 架构决策审批、ADR 审核、技术选型 |
| 项目管理 | [姓名] | 任务分配、进度跟踪、风险管理 |

### AI Agent 角色

| Agent ID | 工具 | 负责模块 | 职责范围 |
|----------|------|----------|----------|
| agent-web | [Claude/Cursor/...] | `packages/web` | 前端页面、组件、样式 |
| agent-server | [Claude/Cursor/...] | `packages/server` | 后端接口、业务逻辑、数据访问 |
| agent-ai | [Claude/Cursor/...] | `packages/ai` | Agent 编排、Prompt、LLM 调用 |
| agent-shared | [Claude/Cursor/...] | `packages/shared` | 共享类型、工具函数、校验规则 |

## 模块归属

> AI Agent 只能修改自己负责的模块。跨模块修改需在 BOARD.md 中标注并获得对方确认。

| 模块 | Owner | Reviewer |
|------|-------|----------|
| `packages/web` | agent-web | [人类/其他 Agent] |
| `packages/server` | agent-server | [人类/其他 Agent] |
| `packages/ai` | agent-ai | [人类/其他 Agent] |
| `packages/shared` | agent-shared | 所有相关 Agent 联审 |
| `docs/` | 人类 | 技术负责人 |

## 审批权限

| 操作 | 审批人 |
|------|--------|
| 架构变更（ADR） | 技术负责人 |
| 数据模型变更 | 技术负责人 |
| API 契约变更 | 技术负责人 + 相关 Agent Owner |
| `shared` 包变更 | 所有依赖方 Agent 确认 |
| 新增子包 | 技术负责人 |
| 新增外部依赖 | 技术负责人 |
| 产品需求变更 | 产品负责人 |

## 协作规则

1. **各守边界**: 每个 Agent 只修改自己 Owner 的模块
2. **跨模块协调**: 需修改他人模块时，在 BOARD.md 创建协作任务并标注双方
3. **shared 变更需广播**: 修改 shared 前通知所有依赖方，变更后各方验证
4. **冲突升级**: Agent 间产生分歧时，升级给对应的人类角色决策
5. **交叉审查**: 关键变更由非作者的 Agent 或人类审查
