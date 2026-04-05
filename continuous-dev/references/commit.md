# commit - 规范化提交

**触发词**: `/commit`, "提交代码", "规范提交"

**职责**: 规范化提交信息，确保提交质量

---

## When to Use

- test-review-simplify 通过后，需要提交代码
- 任何需要提交代码的场景

**When NOT to Use:**
- 代码还在开发中
- 测试未通过
- 提交信息需要临时存储

---

## 提交格式

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type 规范

| Type | 含义 |
|------|------|
| feat | 新功能 |
| fix | 修复 bug |
| refactor | 重构（非功能修改） |
| docs | 文档更新 |
| test | 测试相关 |
| chore | 构建/工具变更 |
| perf | 性能优化 |
| ci | CI/CD 相关 |

### Scope 规范

```
feat(auth):     # auth 模块
feat(order):    # order 模块
fix(payment):   # payment 模块
```

### Subject 规范

- 不超过 50 字符
- 使用动词开头
- 不使用句号结尾
- 使用中文描述

### Body 规范（可选）

- 解释 **what** 和 **why**，不解释 **how**
- 每行不超过 72 字符
- 使用列表时用 `-` 而不是 `*`

### Footer 规范（可选）

```
Closes #123
Fixes #456
```

---

## 提交前检查清单

```
□ 所有测试通过
□ 代码已审查
□ 无临时调试代码
□ 提交信息准确反映变更
□ 变更范围合理（单一职责）
```

---

## 下一步

提交后 → 开始新迭代 `/iteration-plan`

**自动执行**：自动更新 PROJECT_STATUS

**需要用户确认**：检查点 CP6（提交确认）
