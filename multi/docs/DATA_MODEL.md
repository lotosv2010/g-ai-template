# 数据模型规格

> **重要**: 模型变更需要人工审批。
> AI 必须先更新本文档并获得确认，才能修改任何数据库 Schema。

## 设计原则

1. 所有表必须包含 `id`（UUID）、`created_at`、`updated_at` 字段
2. 需要软删除的表使用 `deleted_at` 字段
3. 所有字段名使用 snake_case
4. 外键必须显式定义级联规则
5. 索引必须与表定义一起记录

## 实体关系图

```
[用 ASCII 或 Mermaid 绘制 ER 图]

示例:
┌──────────┐     ┌──────────────┐     ┌──────────┐
│  users   │──1:N──│   orders     │──N:1──│ products │
└──────────┘     └──────────────┘     └──────────┘
```

## 数据表

### `users` - 用户表

| 字段 | 类型 | 可空 | 默认值 | 说明 |
|------|------|------|--------|------|
| id | UUID | 否 | gen_random_uuid() | 主键 |
| email | VARCHAR(255) | 否 | — | 唯一，用户邮箱 |
| name | VARCHAR(100) | 否 | — | 显示名称 |
| password_hash | VARCHAR(255) | 否 | — | Bcrypt 哈希 |
| status | ENUM('active','inactive','banned') | 否 | 'active' | 账户状态 |
| created_at | TIMESTAMP | 否 | NOW() | 创建时间 |
| updated_at | TIMESTAMP | 否 | NOW() | 更新时间 |
| deleted_at | TIMESTAMP | 是 | NULL | 软删除标记 |

**索引:**
- `UNIQUE (email) WHERE deleted_at IS NULL`
- `INDEX (status)`

**约束:**
- `email` 必须符合邮箱格式（应用层校验）

---

### `[下一张表]`

<!-- 按相同格式继续添加更多表 -->

## 枚举定义

| 枚举名称 | 可选值 | 使用位置 |
|----------|--------|----------|
| user_status | active, inactive, banned | users.status |

## 迁移规则

1. 生产环境禁止直接删除字段 —— 先废弃再清理
2. 新增字段必须有默认值或允许为空
3. 生产环境的索引变更必须使用 CONCURRENTLY
4. 数据迁移必须可回滚
