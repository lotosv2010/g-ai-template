# API 契约规格

> **唯一真相源**: 前后端通信的统一契约。
> 实现任何接口前，必须先更新本文档。

## 基础配置

- **基础路径**: `[例: /api/v1]`
- **认证方式**: `[例: Authorization 头携带 Bearer Token]`
- **内容类型**: `application/json`
- **日期格式**: ISO 8601 (`YYYY-MM-DDTHH:mm:ssZ`)

## 响应信封

所有 API 响应遵循统一信封格式：

```json
{
  "code": 0,
  "message": "success",
  "data": {},
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z",
    "requestId": "uuid"
  }
}
```

### 错误响应

```json
{
  "code": 40001,
  "message": "参数校验失败",
  "errors": [
    { "field": "email", "message": "邮箱格式不正确" }
  ],
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z",
    "requestId": "uuid"
  }
}
```

## 错误码定义

| 错误码 | HTTP 状态码 | 含义 |
|--------|------------|------|
| 0 | 200 | 成功 |
| 40001 | 400 | 参数校验错误 |
| 40101 | 401 | 未认证 |
| 40301 | 403 | 无权限 |
| 40401 | 404 | 资源不存在 |
| 50001 | 500 | 服务器内部错误 |

## 分页约定

```
GET /resource?page=1&pageSize=20

响应 meta 包含:
{
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

---

## 接口列表

### [模块名称]

#### `POST /api/v1/[resource]` - 创建资源

**请求体:**
```json
{
  "field1": "string",
  "field2": 0
}
```

**成功响应 (200):**
```json
{
  "code": 0,
  "data": {
    "id": "uuid",
    "field1": "string",
    "field2": 0,
    "createdAt": "ISO8601"
  }
}
```

#### `GET /api/v1/[resource]/:id` - 获取资源详情

**成功响应 (200):**
```json
{
  "code": 0,
  "data": {
    "id": "uuid",
    "field1": "string",
    "field2": 0
  }
}
```

<!-- 按相同格式继续添加更多接口 -->
