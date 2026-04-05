# iteration-plan - 迭代计划

**触发词**: `/iteration-plan`, "开始迭代", "迭代计划"

**职责**: 规划迭代目标、参与 Domain Model、任务分解、时间盒管理

---

## When to Use

- 项目开始时，规划第一个迭代
- 每个迭代结束时，规划下一个迭代
- 需要追踪迭代执行状态

**When NOT to Use:**
- 日常开发中（只执行任务）
- 迭代已规划好，只需要执行

---

## 迭代结构

```
iterations/
├── iteration-1/           # 迭代1（已归档）
│   ├── plan.md
│   ├── review.md
│   └── outputs/           # 临时，最终移到永久位置
└── iteration-2/           # 迭代2（进行中）
    ├── plan.md
    ├── review.md
    └── outputs/
```

---

## 迭代计划模板

```markdown
# Iteration N Plan

**迭代号**：2
**日期**：2026-04-15
**时间盒**：2周（到 2026-04-29）
**状态**：🔄 进行中

## 迭代目标

[这个迭代要交付什么？]

## Domain Model 参与

| Domain Model | 起点阶段 | 终点阶段 | 负责人 | 状态 |
|-------------|----------|----------|--------|------|
| Order | contract-first | parallel-implement | Agent-A | 🔄 |
| User | domain-modeling | contract-first | Agent-B | 🔄 |

## 任务列表

| Task ID | 任务 | Domain Model | 依赖 | 负责人 | 状态 |
|---------|------|-------------|------|--------|------|
| TASK-001 | Order 契约编写 | Order | - | Agent-A | 🔄 |
| TASK-002 | Order 实现 | Order | TASK-001 | Agent-A | ⏳ |

## 验收标准

- [ ] Order Contract.md 完成
- [ ] Order 实现通过测试
- [ ] User DomainModel.md 完成

## 时间盒

| 里程碑 | 目标日期 | 状态 |
|--------|----------|------|
| Domain Model 阶段完成 | 2026-04-20 | 🔄 |
| 迭代结束 | 2026-04-29 | ⏳ |
```

---

## 迭代评审模板

```markdown
# Iteration N Review

**迭代号**：2
**结束日期**：2026-04-29

## 目标回顾

| 目标 | 状态 | 说明 |
|------|------|------|
| Order 契约完成 | ✅ | 按时完成 |
| Order 实现通过测试 | ⚠️ | 延期 2 天 |

## 产出统计

| 类型 | 数量 |
|------|------|
| 新增 Contract | 1 |
| 新增测试 | 15 |
| 代码行数 | +500 |

## 产物归档

| 产出 | 从 | 到 |
|------|-----|-----|
| Contract.md | iteration-2/outputs/ | domain-models/Order/ |

## 遗留问题

- [ ] 未完成的 Task-004 移到 iteration-3
```

---

## 更新 PROJECT_STATUS

```markdown
## 迭代追踪

| 迭代 | 计划 | 状态 | 评审 |
|------|------|------|------|
| iteration-1 | iteration-1/plan.md | ✅ 完成 | iteration-1/review.md |
| iteration-2 | iteration-2/plan.md | 🔄 进行中 | - |

## Domain Model 全局状态

| Domain Model | 阶段 | 迭代 | 最新产出 |
|-------------|------|------|----------|
| Order | contract-first | 2 | domain-models/Order/Contract.md |
```

**注意**：产出文件存放在永久位置（domain-models/, docs/）

---

## 下一步

迭代计划完成后 → `/domain-modeling` 开始执行

**自动执行**：自动更新 PROJECT_STATUS

**需要用户确认**：检查点 CP1（迭代目标确认）

---

## Red Flags

| 信号 | 含义 |
|------|------|
| 迭代目标不明确 | 可能无法验收 |
| 任务依赖混乱 | 并行策略有问题 |
| 时间盒估算偏差大 | 任务分解不够细致 |
