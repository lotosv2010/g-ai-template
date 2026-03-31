# 命名规范守卫

> AI 在创建或重命名任何标识符时必须遵循此规范。

## 文件命名

| 类型 | 规则 | 示例 |
|------|------|------|
| 模块目录 | kebab-case | `user-order/` |
| 源码文件 | kebab-case | `user-order.service.ts` |
| 测试文件 | 与源码同名 + `.test` | `user-order.service.test.ts` |
| 类型定义 | kebab-case + `.types` | `user-order.types.ts` |
| 常量文件 | kebab-case + `.constants` | `user-order.constants.ts` |

## 代码命名

| 类型 | 规则 | 示例 |
|------|------|------|
| 变量 | camelCase | `userName`, `orderList` |
| 函数 | camelCase，动词开头 | `getUserById`, `createOrder` |
| 类 | PascalCase | `UserService`, `OrderRepository` |
| 接口 | PascalCase，无 I 前缀 | `UserRepository`（非 `IUserRepository`） |
| 常量 | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT`, `DEFAULT_PAGE_SIZE` |
| 枚举 | PascalCase（枚举名+值） | `UserStatus.Active` |
| 类型 | PascalCase | `CreateUserDto`, `UserEntity` |

## 数据库命名

| 类型 | 规则 | 示例 |
|------|------|------|
| 表名 | snake_case 复数 | `user_orders` |
| 字段名 | snake_case | `created_at`, `user_id` |
| 索引名 | `idx_表名_字段名` | `idx_users_email` |
| 外键名 | `fk_表名_关联表` | `fk_orders_users` |

## API 路径命名

| 规则 | 示例 |
|------|------|
| kebab-case 复数 | `/api/v1/user-orders` |
| 资源嵌套不超过 2 层 | `/api/v1/users/:id/orders` |
| 操作用 HTTP 动词表达 | `POST /orders`（非 `/create-order`） |

## 禁止的命名

- 禁止单字母变量（循环索引 `i/j/k` 除外）
- 禁止无意义缩写（`usr`, `mgr`, `impl`）
- 禁止拼音命名
- 禁止以类型为名（`data`, `info`, `list`, `item`）
