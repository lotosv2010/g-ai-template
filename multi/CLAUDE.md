# CLAUDE.md - Claude Code 专用配置

> 本文件是 Claude Code 的专用入口。
> 通用 AI 开发规范请参见 [`AGENTS.md`](AGENTS.md)。

## 引用关系

```
AGENTS.md          ← 通用规范（所有 AI 工具共享）
CLAUDE.md          ← Claude 专用配置（本文件）
.claude/rules/     ← Claude 硬性规则
.claude/protocols/ ← Claude 任务协议
.claude/skills/    ← Claude 可复用能力
.claude/memory/    ← Claude 项目记忆
.claude/guardrails/← Claude 自动守卫
.claude/hooks/     ← Claude 事件钩子
```

## 文档阅读顺序

Claude 在执行任何操作前，必须按以下顺序阅读：

| 优先级 | 文档 | 用途 |
|--------|------|------|
| P0 | `AGENTS.md` | 通用 AI 规范 - 编码、安全、架构规则 |
| P0 | `.claude/rules/` | Claude 硬性规则 - 必须遵守，无例外 |
| P0 | `.claude/guardrails/` | 自动检查约束 - 违反则停止 |
| P1 | 目标模块的 `AGENTS.md` | 模块级上下文和约束 |
| P1 | `docs/PRODUCT.md` | 产品边界 |
| P1 | `docs/ARCHITECTURE.md` | 架构白皮书、技术栈、环境变量 |
| P2 | `docs/team/ROLES.md` | 角色分工和模块归属 |
| P2 | `docs/tasks/BOARD.md` | 任务看板 |
| P2 | `docs/tasks/CURRENT.md` | 活跃任务索引 |
| P3 | `.claude/protocols/` | 任务执行协议 |
| P3 | `.claude/skills/` | 可复用能力模板 |
| P4 | `.claude/memory/` | 项目记忆 |
| P4 | `.claude/hooks/` | 事件钩子 |

## 工作流协议

### 开始任务前

```
1. 阅读 AGENTS.md —— 通用规范
2. 阅读 .claude/rules/ —— Claude 硬性约束
3. 阅读 .claude/guardrails/ —— 自动检查规则
4. 阅读 docs/team/ROLES.md —— 确认角色和模块范围
5. 阅读 docs/tasks/CURRENT.md —— 找到自己的当前任务
6. 阅读目标模块的 packages/[name]/AGENTS.md —— 模块上下文
7. 阅读对应的 .claude/protocols/ —— 执行方式
8. 查阅 .claude/memory/ —— 避免已知陷阱
```

### 执行过程中

```
1. 遵循当前任务类型的协议（功能/缺陷/重构）
2. 保持在 .claude/guardrails/file-scope.md 定义的文件范围内
3. 遵循 .claude/guardrails/naming.md 的命名规范
4. 绝不触碰 .claude/guardrails/forbidden.md 中的禁止项
5. 修改模块代码前，确认符合该模块 AGENTS.md 中的约束
```

### 完成后

```
1. 按 .claude/rules/review.rules.md 进行自审
2. 触发 .claude/hooks/post-task.md 的收尾动作
3. 如发现新模式或问题，更新 .claude/memory/
4. 更新 docs/tasks/BOARD.md 任务状态和 docs/tasks/CURRENT.md 进度
```

## 快速参考

- **新增功能**: 遵循 `.claude/protocols/feature.protocol.md`
- **修复缺陷**: 遵循 `.claude/protocols/bugfix.protocol.md`
- **一般任务**: 遵循 `.claude/protocols/task.protocol.md`
- **架构变更**: 先在 `docs/decisions/` 创建 ADR，获批后执行
- **新增接口**: 先更新 `docs/API_SPEC.md` 契约，再实现
- **模型变更**: 先更新 `docs/DATA_MODEL.md`，再迁移
- **新增子包**: 先创建 `packages/[name]/AGENTS.md`，定义职责和边界
