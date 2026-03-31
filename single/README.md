# [PROJECT_NAME]

> 单人单 Agent 项目模板

## 项目结构

```
.
├── CLAUDE.md              # AI 主入口（核心规则与工作流）
├── README.md              # 项目说明
│
├── .claude/               # AI 行为控制层
│   ├── rules/             # 编码、架构、审查细则
│   ├── guardrails/        # 文件范围、命名、禁止项
│   ├── protocols/         # 任务执行协议
│   ├── skills/            # 可复用能力模板
│   ├── hooks/             # 事件钩子
│   └── memory/            # 项目长期记忆
│
├── docs/                  # 约束与规格
│   ├── PRODUCT.md         # 产品范围与边界
│   ├── ARCHITECTURE.md    # 架构白皮书（含技术栈、环境变量）
│   ├── API_SPEC.md        # API 契约
│   ├── DATA_MODEL.md      # 数据模型
│   ├── decisions/         # 架构决策记录（ADR）
│   ├── runbooks/          # 运维手册（部署与排错）
│   └── tasks/
│       └── CURRENT.md     # 当前任务
│
├── src/                   # 业务代码
│   ├── modules/           # 功能模块（垂直切片）
│   ├── services/          # 横切应用服务
│   ├── domain/            # 领域模型（零依赖）
│   └── infra/             # 基础设施适配器
│
└── tests/                 # 测试
```

## 快速开始

1. 替换 `[PROJECT_NAME]` 等占位符
2. 填写 `docs/PRODUCT.md` 和 `docs/ARCHITECTURE.md`
3. 在 `docs/tasks/CURRENT.md` 创建第一个任务
4. 让 AI 从 `CLAUDE.md` 开始

## 给 AI Agent

**从这里开始: [`CLAUDE.md`](CLAUDE.md)**
