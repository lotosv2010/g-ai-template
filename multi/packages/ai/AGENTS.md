# AI 模块 - AI 上下文

> 本文件定义 `ai` 包的模块级 AI 开发上下文。
> AI 在修改本模块代码前必须阅读。

## 模块职责

**AI 能力层** - 封装所有 AI/LLM 相关能力，为其他包提供统一的 AI 服务接口。

- Agent 定义与编排
- Prompt 模板管理与版本化
- LLM 调用链（Chain）封装
- 自定义工具（Tool/Function Calling）
- 模型配置与切换
- 向量检索与 RAG 管道

## 内部架构

```
src/
├── agents/           # Agent 定义
│   └── [agent-name]/
│       ├── index.ts      # Agent 入口
│       ├── prompt.ts     # 系统提示词
│       └── tools.ts      # Agent 可用工具
│
├── prompts/          # Prompt 模板库
│   ├── templates/        # 模板文件
│   └── manager.ts        # 模板加载与渲染
│
├── chains/           # LLM 调用链
│   ├── [chain-name].ts
│   └── ...
│
├── tools/            # Function Calling 工具定义
│   ├── [tool-name].ts
│   └── registry.ts       # 工具注册中心
│
└── models/           # 模型配置与适配
    ├── config.ts         # 模型参数配置
    └── adapters/         # 不同模型供应商适配器
```

## 依赖规则

### 包间依赖

| 可以依赖 | 禁止依赖 |
|----------|----------|
| `@[scope]/shared` | `@[scope]/web` |
| | `@[scope]/server` 的内部实现 |

### 被依赖方

- `server` 通过本包的公开接口调用 AI 能力
- 本包不主动调用 server，如需数据通过参数传入

## 本模块特有约定

### Prompt 管理

- 所有 Prompt 必须模板化，禁止在代码中硬编码长文本提示词
- Prompt 模板使用变量占位符，运行时渲染
- 重要 Prompt 变更需记录版本和变更原因

### Agent 设计

- 每个 Agent 单一职责，职责清晰
- Agent 的可用工具（Tools）必须显式声明，禁止开放所有工具
- Agent 的系统提示词与业务逻辑分离

### 模型调用

- 统一通过 `models/` 层调用 LLM，禁止业务代码直接调 SDK
- 模型选择和参数（temperature、max_tokens 等）通过配置管理
- 所有 LLM 调用必须有超时和错误处理
- 敏感信息禁止发送到 LLM（用户密码、密钥等）

### 成本控制

- 记录每次 LLM 调用的 token 用量
- 大批量调用需有速率控制
- 开发调试时优先使用低成本模型

## 已有 Agent

| Agent | 职责 | 使用模型 | 状态 |
|-------|------|----------|------|
| [例: assistant] | 通用问答助手 | [模型] | [开发中/已完成] |

## 已有工具

| 工具 | 职责 | 被哪些 Agent 使用 |
|------|------|-------------------|
| [例: search] | 知识库检索 | [agent 列表] |
