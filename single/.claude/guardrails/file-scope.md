# 文件范围守卫

> 定义 AI 可以操作的文件范围。超出范围的操作必须先获得人工确认。

## 自由操作区（AI 可直接修改）

```
src/modules/**              # 功能模块代码
src/services/**             # 应用服务代码
tests/**                    # 测试代码
.claude/memory/**           # AI 记忆（AI 自主维护）
docs/tasks/CURRENT.md       # 任务进度更新
```

## 受限操作区（修改前需说明理由）

```
src/domain/**               # 领域模型（影响面大）
src/infra/**                # 基础设施（影响面大）
```

## 禁止操作区（必须人工审批）

```
CLAUDE.md                   # AI 控制中枢
docs/PRODUCT.md             # 产品边界
docs/ARCHITECTURE.md        # 架构白皮书
docs/decisions/**           # 架构决策记录
.claude/rules/**            # 硬性规则
.claude/guardrails/**       # 守卫规则
.claude/protocols/**        # 执行协议
.claude/hooks/**            # 事件钩子
```

## 需先更新文档再改代码的场景

| 代码变更 | 必须先更新的文档 |
|----------|-----------------|
| 新增/修改 API 接口 | `docs/API_SPEC.md` |
| 新增/修改数据表 | `docs/DATA_MODEL.md` |
| 架构级变更 | `docs/decisions/` 新建 ADR |

## 新建文件规则

- 新文件必须放在 `src/` 对应目录下
- 文件命名遵循 `naming.md` 规范
- 禁止在项目根目录创建新文件（除非有明确指示）
