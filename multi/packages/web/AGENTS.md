# Web 前端 - AI 上下文

> 本文件定义 `web` 包的模块级 AI 开发上下文。
> AI 在修改本模块代码前必须阅读。

## 模块职责

**前端应用** - 用户界面与交互层。

- 页面路由与视图渲染
- 用户交互与表单处理
- 状态管理
- API 调用与数据展示
- 响应式布局与主题

## 内部架构

```
src/
├── pages/            # 页面/路由视图
│   └── [page-name]/
│       ├── index.tsx
│       └── components/   # 页面私有组件
│
├── components/       # 全局共享组件
│   ├── ui/               # 基础 UI 组件（按钮、输入框等）
│   ├── layout/           # 布局组件（Header、Sidebar 等）
│   └── business/         # 业务组件（跨页面复用）
│
├── hooks/            # 自定义 Hooks
├── stores/           # 状态管理
├── api/              # API 请求封装
│   └── [module].ts       # 按后端模块分组
│
└── styles/           # 全局样式与主题
```

## 依赖规则

### 包间依赖

| 可以依赖 | 禁止依赖 |
|----------|----------|
| `@[scope]/shared` | `@[scope]/server`、`@[scope]/ai` |

### 组件依赖方向

```
pages → business components → ui components
  └→ hooks
  └→ stores
  └→ api
```

- `ui/` 组件不依赖业务逻辑，纯展示
- `business/` 组件可依赖 hooks 和 stores
- `pages/` 组装以上所有

## 本模块特有约定

- 组件使用函数式组件 + Hooks
- 页面级组件与路由一一对应
- API 请求统一封装在 `api/` 目录，页面和组件禁止直接调 fetch/axios
- 样式方案：[例: Tailwind CSS / CSS Modules / styled-components]
- 表单校验规则与后端共用 `@[scope]/shared` 中的定义
- 所有用户可见文案集中管理，为国际化做准备

## 状态管理规则

- 服务端状态（API 数据）使用 [例: TanStack Query / SWR]
- 客户端状态（UI 状态）使用 [例: Zustand / Jotai]
- 禁止将服务端数据复制到客户端 store 中

## 已有页面

| 页面 | 路由 | 职责 | 状态 |
|------|------|------|------|
| [例: 首页] | `/` | [描述] | [开发中/已完成] |
