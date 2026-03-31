# AI 开发流程 Prompt 模板

> **用途**: 给 AI 开发工具（Claude Code、Cursor 等）使用的流程性 Prompt。
> **区别**: `packages/ai/src/prompts/` 存放业务运行时的 LLM Prompt 模板（给最终用户的 AI 功能用），本目录存放开发过程中引导 AI Agent 干活的模板。

## 项目初始化 Prompt

```
你正在参与 [PROJECT_NAME] 项目的开发。

在开始任何工作前，请先阅读以下文件:
1. AGENTS.md - 了解通用 AI 开发规范
2. 目标模块的 packages/[name]/AGENTS.md - 了解模块上下文
3. docs/tasks/CURRENT.md - 了解当前任务

严格遵循 AGENTS.md 中的所有规则。
```

## 功能开发 Prompt

```
当前任务: 开发 [功能名称]
目标模块: packages/[name]

请按以下步骤执行:
1. 阅读 packages/[name]/AGENTS.md 了解模块上下文
2. 确认 docs/API_SPEC.md 中的接口定义
3. 确认 docs/DATA_MODEL.md 中的数据模型
4. 按照由内向外的顺序实现（domain → service → controller）
5. 编写测试
6. 自审（安全、架构合规、测试覆盖）
```

## 缺陷修复 Prompt

```
当前任务: 修复 [缺陷描述]
目标模块: packages/[name]

请按以下步骤执行:
1. 先编写失败测试复现问题
2. 定位根因（不是表面现象）
3. 最小化修复
4. 确保所有测试通过
```

## 代码审查 Prompt

```
请审查以下变更:

重点关注:
- 安全性问题（注入、信息泄露）
- 架构合规性（依赖方向、模块边界）
- 代码质量（命名、复杂度、重复）
- 测试覆盖
```
