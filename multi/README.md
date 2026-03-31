# [PROJECT_NAME]

> AI Agent 驱动的项目通用开发架构模板

## 简介

本项目采用 **Monorepo + 约束驱动架构**，专为 AI Agent 协作开发设计。通过分层文档体系在产品需求、架构决策、AI 行为规则和业务代码之间建立清晰边界，支持多种 AI 开发工具协同工作。

## 项目结构

```
.
├── AGENTS.md                  # 通用 AI 开发规范（所有 AI 工具共享）
├── CLAUDE.md                  # Claude Code 专用配置
├── README.md                  # 项目说明（给人看）
├── package.json               # 根 package（pnpm workspace）
├── pnpm-workspace.yaml        # Monorepo 工作区配置
│
├── docs/                      # 约束与规格层
│   ├── PRODUCT.md             # 产品范围与边界
│   ├── ARCHITECTURE.md        # 架构白皮书（含技术栈、环境变量、部署）
│   ├── API_SPEC.md            # API 契约定义
│   ├── DATA_MODEL.md          # 数据模型规格
│   ├── decisions/             # 架构决策记录（ADR）
│   ├── runbooks/              # 运维操作手册
│   ├── team/
│   │   └── ROLES.md           # 角色分工与模块归属
│   └── tasks/
│       ├── BOARD.md           # 任务看板（多任务并行追踪）
│       └── CURRENT.md         # 活跃任务索引
│
├── .claude/                   # Claude 行为控制层
│   ├── rules/                 # 硬性规则
│   ├── protocols/             # 任务执行协议
│   ├── skills/                # 可复用能力模板
│   ├── memory/                # 项目长期记忆
│   ├── guardrails/            # 自动约束检查
│   └── hooks/                 # 事件钩子
│
├── packages/                  # Monorepo 子包
│   ├── web/                   # 前端应用
│   ├── server/                # 服务端应用
│   ├── ai/                    # AI 能力层
│   └── shared/                # 通用共享库
│
├── tools/                     # 工具层
│   ├── prompts/               # AI 开发流程 Prompt 模板
│   └── scripts/               # 自动化脚本
│
└── tests/                     # 全局测试（E2E 等）
```

> 每个 `packages/[name]/` 下都有独立的 `AGENTS.md` 描述模块上下文。
> 包间依赖关系、开发规则等详见 [`AGENTS.md`](AGENTS.md)。

## 分层设计理念

| 层级 | 目录 | 稳定性 | 控制权 |
|------|------|--------|--------|
| 约束层 | `docs/` | 极高 | 人类（通过 ADR 流程） |
| 规范层 | `AGENTS.md` + 各包 `AGENTS.md` | 高 | 人类（AI 工具共享） |
| 控制层 | `.claude/` | 高 | 人类 + AI（规则由人类定，记忆由 AI 维护） |
| 工具层 | `tools/` | 中等 | 人类 + AI |
| 业务层 | `packages/` | 低 | AI（在约束范围内） |
| 验证层 | `tests/` | 中等 | AI（遵循协议） |

## 快速开始

1. 替换所有文档中的 `[PROJECT_NAME]`、`[scope]` 等占位符
2. 在 `docs/PRODUCT.md` 中定义产品范围
3. 在 `docs/ARCHITECTURE.md` 中设定架构和技术栈
4. 填充各 `packages/[name]/AGENTS.md` 的模块上下文
5. 在 `docs/tasks/CURRENT.md` 中创建第一个任务

## 给 AI Agent

**通用入口: [`AGENTS.md`](AGENTS.md)** — 所有 AI 工具的共享规范。
**Claude 入口: [`CLAUDE.md`](CLAUDE.md)** — Claude Code 的专用配置和工作流协议。
