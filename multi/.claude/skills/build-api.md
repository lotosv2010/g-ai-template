# 技能: 构建 API 接口

> 当需要新建或修改 API 接口时，使用此技能模板。

## 前置条件

- [ ] `docs/API_SPEC.md` 中已定义该接口的契约
- [ ] `docs/DATA_MODEL.md` 中已包含相关数据模型（如有）
- [ ] 确认接口所属模块已存在（否则先创建模块骨架）

## 执行步骤

### 1. 定义 DTO

- 创建请求 DTO（包含字段校验规则）
- 创建响应 DTO（与 API_SPEC 契约一致）
- 位置: `packages/server/src/modules/[module]/dto/`

### 2. 实现 Service 方法

- 在 Service 层实现业务逻辑
- 注入所需的 Repository 和其他 Service
- 位置: `packages/server/src/modules/[module]/service/`

### 3. 实现 Repository 方法（如需新增数据访问）

- 实现数据查询/写入方法
- 确保使用参数化查询
- 位置: `packages/server/src/modules/[module]/repository/`

### 4. 实现 Controller

- 定义路由和 HTTP 方法
- 添加请求参数校验
- 添加权限检查
- 调用 Service 方法
- 按照统一信封格式返回响应
- 位置: `packages/server/src/modules/[module]/controller/`

### 5. 编写测试

- Service 层单元测试
- Controller 层集成测试（含成功和失败路径）
- 位置: `packages/server/tests/[module]/`

## 检查清单

- [ ] 请求/响应格式与 `API_SPEC.md` 一致
- [ ] 错误码与全局错误码定义一致
- [ ] 有输入校验
- [ ] 有权限控制
- [ ] 有错误处理
- [ ] 有测试覆盖
