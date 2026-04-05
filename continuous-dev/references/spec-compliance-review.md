# spec-compliance-review - 契约一致性审查

**触发词**: `/spec-compliance-review`, "契约审查", "spec审查"

**职责**: 验证实现与契约完全一致

---

## When to Use

- parallel-implement 完成后，验证实现是否遵循契约
- 代码审查阶段

**When NOT to Use:**
- 契约还未生成
- 探索阶段

---

## 审查原则

```
契约是唯一的真值来源（Single Source of Truth）

契约声明的 → 必须实现
契约未声明的 → 不能添加
契约禁止的 → 必须删除
```

---

## 审查维度

### 1. Postconditions（后置条件）
每个 Postcond R 是否实现？是否有测试验证？

### 2. Preconditions（前置条件）
每个 Precond P 是否验证？Precond Violation 时是否返回正确 Error？

### 3. Side Effects（副作用）
是否只执行契约声明的副作用？有没有额外添加的？

### 4. Boundaries（边界条件）
每个边界条件是否处理？

### 5. Error Contracts（错误契约）
是否返回正确的错误码和错误信息？

---

## 契约 vs 实现 一致性检查

```
Contract.md:
  Operation: CreateOrder
    Postcond R1: order.id 是新生成的UUID
    Postcond R2: order.status = "pending"

Implementation: createOrder.ts
  ✅ R1: uuid() 生成 UUID
  ✅ R2: status = "pending"

Extra（契约未声明）:
  ❌ await analytics.track("order_created") — 契约未声明！
```

---

## Severity Labels

| 标签 | 含义 | 作者行动 |
|------|------|----------|
| **Critical** | 必须修复，否则上线后出问题 | 立即修复 |
| (无前缀) | 需要修复 | 合并前处理 |
| **FYI** | 信息仅供参考 | 不需要行动 |

---

## 下一步

契约审查通过后 → `/test-review-simplify`

审查通过后 → `/commit`

**自动执行**：自动更新 PROJECT_STATUS

**需要用户确认**：检查点 CP5（所有审查通过）
