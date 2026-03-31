# 文件范围守卫

> 定义 AI 可以操作的文件范围。超出范围的操作必须先获得人工确认。

## 自由操作区（AI 可直接修改）

```
packages/*/src/modules/**       # 功能模块代码
packages/*/src/services/**      # 应用服务代码
packages/*/src/pages/**         # 前端页面
packages/*/src/components/**    # 前端组件
packages/*/src/hooks/**         # 自定义 Hooks
packages/*/src/stores/**        # 状态管理
packages/*/src/api/**           # API 请求封装
packages/*/src/agents/**        # Agent 定义
packages/*/src/chains/**        # LLM 调用链
packages/*/src/tools/**         # AI 工具定义
packages/*/tests/**             # 测试代码
tests/**                        # 全局测试
.claude/memory/**               # AI 记忆（AI 自主维护）
docs/tasks/BOARD.md             # 任务看板状态更新
docs/tasks/CURRENT.md           # 活跃任务索引更新
```

## 受限操作区（修改前需说明理由）

```
packages/*/src/domain/**        # 领域模型（影响面大）
packages/*/src/infra/**         # 基础设施（影响面大）
packages/shared/src/**          # 共享库（被所有包依赖）
packages/*/src/models/**        # AI 模型配置
packages/*/src/prompts/**       # Prompt 模板
packages/*/src/styles/**        # 全局样式
tools/**                        # 工具脚本
```

## 禁止操作区（必须人工审批）

```
AGENTS.md                       # 通用 AI 规范
CLAUDE.md                       # Claude 控制配置
docs/PRODUCT.md                 # 产品边界
docs/ARCHITECTURE.md            # 架构白皮书
docs/decisions/**               # 架构决策记录
.claude/rules/**                # 硬性规则
.claude/guardrails/**           # 守卫规则
.claude/protocols/**            # 执行协议
.claude/hooks/**                # 事件钩子
packages/*/AGENTS.md            # 模块级 AI 上下文
package.json                    # 根配置
pnpm-workspace.yaml             # 工作区配置
packages/*/package.json         # 子包配置
```

## 需先更新文档再改代码的场景

| 代码变更 | 必须先更新的文档 |
|----------|-----------------|
| 新增/修改 API 接口 | `docs/API_SPEC.md` |
| 新增/修改数据表 | `docs/DATA_MODEL.md` |
| 架构级变更 | `docs/decisions/` 新建 ADR |
| 新增子包 | 创建 `packages/[name]/AGENTS.md` |
| 变更包间依赖 | 更新相关包的 `AGENTS.md` |

## 新建文件规则

- 新文件必须放在对应包的对应目录下
- 文件命名遵循 `naming.md` 规范
- 禁止在项目根目录创建新文件（除非有明确指示）
- 新建子包必须包含 `AGENTS.md` 和 `package.json`
