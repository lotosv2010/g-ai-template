# 技能: 数据库设计

> 当需要新建或修改数据表时，使用此技能模板。

## 前置条件

- [ ] 需求已明确，知道需要存储什么数据
- [ ] 已检查现有表是否能满足需求（避免重复建表）

## 执行步骤

### 1. 更新数据模型文档

在 `docs/DATA_MODEL.md` 中添加/修改表定义：

- 定义所有字段（类型、可空、默认值、说明）
- 定义索引
- 定义约束和外键
- 更新 ER 图

### 2. 获取确认

- 向用户展示数据模型变更
- 等待人工审批后再继续

### 3. 编写迁移脚本

- 创建迁移文件
- 确保迁移可正向执行和反向回滚
- 位置: `packages/server/src/infra/database/migrations/`

### 4. 更新领域模型

- 更新或创建对应的实体类
- 位置: `packages/server/src/domain/entities/`

### 5. 更新 Repository

- 实现新表的数据访问方法
- 位置: `packages/server/src/modules/[module]/repository/`

## 设计原则

- 表名用复数形式 snake_case（如 `user_orders`）
- 必须有 `id`、`created_at`、`updated_at`
- 考虑是否需要软删除（`deleted_at`）
- 枚举值在文档中记录，代码中用常量管理
- 索引要覆盖常用查询路径
- 外键显式定义级联行为

## 检查清单

- [ ] `DATA_MODEL.md` 已更新
- [ ] 迁移脚本可正向和反向执行
- [ ] 实体类与表结构一致
- [ ] 索引覆盖了主要查询场景
- [ ] 已获得人工审批
