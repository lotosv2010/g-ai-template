# g-ai-template

> AI Agent 驱动的项目通用架构模板

提供两种模板，覆盖不同规模的项目场景。

## 模板选择

| 模板 | 目录 | 适用场景 |
|------|------|----------|
| **单人单Agent** | [`single/`](single/) | 个人项目、原型验证、小型项目 |
| **多人多Agent** | [`multi/`](multi/) | 团队协作、中大型项目、多 AI 工具并行 |

## 对比

| 维度 | single | multi |
|------|--------|-------|
| AI 入口 | `CLAUDE.md` 一个文件 | `AGENTS.md`（通用）+ `CLAUDE.md`（工具特定） |
| 规则管理 | `CLAUDE.md` 概要 + `.claude/` 细则 | `AGENTS.md` 概要 + `.claude/` 细则 |
| 代码结构 | 单包 `src/` | Monorepo `packages/`（web/server/ai/shared） |
| 模块上下文 | 不需要 | 每个包有独立 `AGENTS.md` |
| 协作机制 | 无多人协调 | ROLES 角色分工 + BOARD 任务看板 + 文件范围守卫 |
| 文档量 | ~25 个文件 | ~40 个文件 |
| 核心理念 | 效率优先，一个大脑记住一切 | 一致性优先，隐式知识显式化 |

## 使用方式

```bash
# 选择模板后，将内容复制到你的项目根目录（注意隐藏文件）
cp -r single/* single/.claude single/.gitignore /path/to/your-project/
# 或
cp -r multi/* multi/.claude multi/.gitignore /path/to/your-project/

# 然后替换占位符
# [PROJECT_NAME] → 你的项目名
# [scope] → 你的 npm scope
```

## 目录结构

```
g-ai-template/
├── README.md          # 总说明（本文件）
│
├── single/            # 单人单Agent模板
│   ├── CLAUDE.md      # AI 主入口（核心规则与工作流）
│   ├── README.md
│   ├── .claude/       # AI 行为控制（rules/guardrails/protocols/skills/hooks/memory）
│   ├── docs/          # 约束文档
│   ├── src/           # 业务代码
│   └── tests/
│
└── multi/             # 多人多Agent模板
    ├── AGENTS.md      # 通用 AI 规范
    ├── CLAUDE.md      # Claude 专用配置
    ├── README.md
    ├── .claude/       # AI 行为控制层（rules/protocols/skills/memory/guardrails/hooks）
    ├── docs/          # 约束文档
    ├── packages/      # Monorepo（web/server/ai/shared）
    ├── tools/         # 工具层
    └── tests/         # 全局测试
```
