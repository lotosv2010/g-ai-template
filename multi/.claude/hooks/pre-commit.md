# Hook: 提交前检查

> 在 AI 提交代码前自动执行的检查清单。
> 任何一项失败则阻止提交。

## 触发时机

- `git commit` 执行前

## 检查项

### 1. 代码质量检查

```bash
# Lint 检查
[例: pnpm lint]

# 类型检查
[例: pnpm typecheck]
```

### 2. 测试检查

```bash
# 运行受影响的测试
[例: pnpm test --changed]
```

### 3. 安全检查

```bash
# 检查是否有硬编码密钥
[例: grep -r "password\|secret\|token" --include="*.ts" src/ | grep -v ".test."]

# 检查是否有 console.log 残留
[例: grep -r "console.log" --include="*.ts" src/ | grep -v ".test."]
```

### 4. 范围检查

- 检查修改的文件是否在 `.claude/guardrails/file-scope.md` 允许的范围内
- 检查是否有未经授权的禁止区文件被修改

### 5. 提交信息检查

- 符合 Conventional Commits 格式
- 格式: `type(scope): description`
- type 限定: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`

## 失败处理

检查失败时：
1. 显示失败原因
2. 阻止提交
3. 提供修复建议
