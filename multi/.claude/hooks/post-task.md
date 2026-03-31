# Hook: 任务完成后

> AI 完成一个任务后自动触发的收尾动作。

## 触发时机

- AI 声明任务完成时

## 执行动作

### 1. 更新任务状态

- 更新 `docs/tasks/BOARD.md` 中的任务状态（进行中 → 待审查/已完成）
- 更新 `docs/tasks/CURRENT.md` 中自己的当前任务索引

### 2. 更新记忆

- 检查是否有值得记录的新模式 → 更新 `.claude/memory/patterns.md`
- 检查是否发现新问题 → 更新 `.claude/memory/known-issues.md`
- 检查是否有新约束 → 更新 `.claude/memory/constraints.md`

### 3. 自审

- 按 `.claude/rules/review.rules.md` 执行自审清单
- 输出自审结果摘要

### 4. 变更摘要

- 列出本次任务修改的所有文件
- 统计新增/修改/删除的行数
- 说明核心变更内容
