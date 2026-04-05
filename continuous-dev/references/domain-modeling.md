# domain-modeling - 领域建模

**触发词**: `/domain-modeling`, "建个模型", "领域建模"

**职责**: 4步法定义数据模型和业务流程

---

## When to Use

- idea-research 完成后，需要具体落地
- 开始写代码前，需要明确数据模型
- 发现实体关系混乱，需要梳理

**When NOT to Use:**
- 需求还没确定
- 只是简单增删改查
- 已有完善的领域模型

---

## 4个核心概念

```
Entity     → 什么被持久化存储
Relation   → 实体如何关联
Transition → 状态如何变迁
Process    → 有序步骤 + 回滚
```

> 任何额外概念都是这4个的**标签**，不是新概念

---

## Step 1 — Entities（实体）

问自己：**"服务器重启后什么还活着？"**

```
Entity: <Name> — <代表什么>
  + <field>: <验证规则/约束>
  + <derived>: <type> (derived: COUNT|SUM|..., stored|computed)
```

---

## Step 2 — Relations（关系）

```
<Entity> → <Entity>  [1:N / N:1 / N:N / Self]
  (required) — 子实体不能独立存在
  (cascade) — 父删除级联删除子
```

---

## Step 3 — Transitions（变迁）

```
<Entity>.<from> → <Entity>.<to>
  Trigger:     [动作/事件/定时器]
  Precondition: [前置条件]
  Side effect: [副作用]
```

---

## Step 4 — Processes（流程）

```
Process: <Name>
Trigger:  [什么触发]
Steps:    1. ... 2. ... 3. ...
Rollback: [第N步失败时回滚什么]
```

---

## 输出模板

```markdown
## Entities
- <Entity>: <description>
  + <field>: <validation>
  + <derived>: <type>

## Relations
<Parent> → <Child> [1:N] (required) (cascade)

## Transitions
<Entity>.<from> → <Entity>.<to>
  Trigger: ...
  Side effect: ...

## Processes
Process: <Name>
Trigger: ...
Steps: 1. ... 2. ...
Rollback: ...
```

---

## 验证清单

1. ✅ FK required? 子实体必须有非空外键
2. ✅ Cascade explicit? 每个required FK都有级联策略
3. ✅ State coverage? 每个状态都可从某触发到达
4. ✅ Rollback defined? 多步流程都有回滚计划
5. ✅ No implicit state? 跨实体依赖显式声明

---

## 下一步

领域建模完成后 → **自动触发** `/contract-first`

**自动化行为**：
- 自动更新 PROJECT_STATUS
- 自动生成 Contract.md 模板
- 自动提取 Entity/Relation/Transition 作为契约基础

**需要用户确认**：检查点 CP2（4 步法验证 5 项）
- 回复"继续"或空 → 进入契约生成
- 回复"修改" → 留在领域建模阶段

## Red Flags

| 信号 | 含义 |
|------|------|
| Entity 数量 > 15 | 模型太复杂 |
| 关系无法确定 required/cascade | 实体边界不清晰 |
| 状态转换有环 | 业务流程未想清楚 |
| Process 无法定义 rollback | 多步流程风险未识别 |
