# dependency-analyze - 依赖分析（DM 级）

**触发词**: `/dependency-analyze`, "分析依赖", "并行分析"

**职责**: 分析 Domain Model 内 Operation 间依赖，确定并行可行性

---

## 层级说明

```
项目级：task-breakdown（服务/模块间任务分解）
    ↓
Domain Model 级：dependency-analyze（Operation 间依赖分析）
    ↓
parallel-implement（执行）
```

**注意**：`parallel-implement` 已内置依赖分析，如已做 task-breakdown 可跳过。

---

## When to Use

- contract-first 完成后，parallel-implement 前
- 需要确定 Domain Model 内哪些 Operation 可以并行

**When NOT to Use:**
- 已通过 task-breakdown 完成项目级任务分解
- 只有一个 Operation 需要实现

---

## 依赖类型

### 数据依赖
Op2 需要 Op1 的输出作为输入
```
Op1: GetUser → 返回 user
Op2: UpdateUser → 需要 user.id
→ Op2 依赖 Op1
```

### 状态依赖
Op2 需要读取 Op1 修改后的状态
```
Op1: DeductCredit → 修改 user.balance
Op2: CreateOrder → 检查 user.balance >= total
→ Op2 依赖 Op1
```

### 无依赖
```
Op1: GetUserProfile → 读 user
Op2: GetProductList → 读 product
→ 无依赖，可以并行
```

---

## 分析步骤

### Step 1: 列出所有 Operation

从契约文档提取所有 Operation。

### Step 2: 识别依赖关系

构建依赖矩阵。

### Step 3: 识别并行组

```
Group 1（无依赖，可并行）
- GetUser, GetProduct

Group 2（依赖 Group 1）
- UpdateUser, CreateOrder

Group 3（串行，关键路径）
- DeductCredit, SendNotification
```

---

## 下一步

依赖分析完成后 → `/parallel-implement`

**自动执行**：自动更新 PROJECT_STATUS

**需要用户确认**：无（此步骤为分析工作，无需人工确认）

## Red Flags

| 信号 | 含义 |
|------|------|
| 所有 Operation 串行 | 并行化空间为 0，可能设计有问题 |
| 依赖关系形成环 | 业务流程有死循环 |
| 共享状态过多 | 并行冲突风险高，需重新设计 |
