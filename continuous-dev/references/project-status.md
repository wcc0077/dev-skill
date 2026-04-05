# 项目状态追踪

## 核心原则

```
PROJECT_STATUS/
├── metadata.md   # 元数据（固定不变）
└── data.md      # 数据（每次更新）
```

- **元数据** = 定义结构（字段、阶段、状态符号）— 固定不变
- **数据** = 填入实际值 — 每次更新变化

---

## 产物目录结构

```
project/
├── PROJECT_STATUS/           # 全局永久 - 项目状态追踪
│   ├── metadata.md
│   └── data.md
│
├── domain-models/           # 全局永久 - 领域模型（跨迭代演进）
│   └── <dm>/
│       ├── DomainModel.md
│       ├── Contract.md
│       └── tests/
│
├── docs/                    # 全局永久 - 项目文档
│
├── src/                     # 全局永久 - 源代码
│
└── iterations/              # 迭代级 - 每个迭代独立
    └── iteration-N/
        ├── plan.md
        ├── review.md
        └── outputs/         # 临时，最终移到永久位置
```

---

## 产物定位原则

| 产物 | 位置 | 类型 |
|------|------|------|
| DomainModel, Contract, tests | `domain-models/<name>/` | 全局永久 |
| 项目文档, system-architecture | `docs/` | 全局永久 |
| 源代码 | `src/` | 全局永久 |
| 迭代计划/评审 | `iterations/iteration-N/` | 迭代级 |
| 迭代产出副本 | `iterations/iteration-N/outputs/` | 临时 |

---

## metadata.md 模板

```markdown
# 项目状态元数据

## 阶段定义

| 阶段ID | 阶段名 | Checkpoint | 产物文件 | 层级 |
|--------|--------|------------|----------|------|
| 1 | idea-research | CP1 | docs/idea-research.md | 项目级 |
| 2 | domain-modeling | CP2 | <dm>/DomainModel.md | DM级 |
| 3 | contract-first | CP3 | <dm>/Contract.md | DM级 |
| ... | ... | ... | ... | ... |

## 字段定义

| 字段 | 类型 | 说明 |
|------|------|------|
| 项目名 | string | 项目名称 |
| 创建时间 | date | ISO 格式 |
| 当前迭代 | string | iteration-N |

## Domain Model 定义

| 字段 | 类型 | 说明 |
|------|------|------|
| name | string | Domain Model 名称 |
| stage | stage_id | 当前阶段 |
| iteration | number | 当前迭代号 |
| status | status_symbol | ✅🔄⏳❌ |

## 迭代定义

| 字段 | 类型 | 说明 |
|------|------|------|
| iteration_id | string | iteration-N |
| start_date | date | ISO 格式 |
| timebox | duration | 时间盒 |
| goal | string | 迭代目标 |
| status | status_symbol | ✅🔄⏳❌ |

## 状态符号

| 符号 | 含义 |
|------|------|
| ✅ | 完成 |
| 🔄 | 进行中 |
| ⏳ | 待开始 |
| ❌ | 失败 |
```

---

## data.md 模板

```markdown
# 项目状态数据

**更新时间**：2026-04-15

## 基本信息

| 字段 | 值 |
|------|-----|
| 项目名 | 积分商城 |
| 创建时间 | 2026-04-01 |
| 当前迭代 | iteration-2 |

## 迭代追踪

| 迭代 | 计划 | 状态 | 评审 |
|------|------|------|------|
| iteration-1 | iteration-1/plan.md | ✅ 完成 | iteration-1/review.md |
| iteration-2 | iteration-2/plan.md | 🔄 进行中 | - |

## Domain Model 全局状态

| Domain Model | 阶段 | 迭代 | 最新产出 |
|-------------|------|------|----------|
| Order | contract-first | 2 | domain-models/Order/Contract.md |
| User | domain-modeling | 2 | domain-models/User/DomainModel.md |
| Points | idea-research | 1 | iterations/iteration-1/outputs/docs/调研.md |

## 当前迭代目标

iteration-2: 完成 Order 和 User 的契约
```

---

## 使用规则

1. **元数据 (metadata.md) 定义后不修改**
2. **每次更新只改 data.md**
3. **每次回退创建新的 iteration 文件**
4. **产物文件路径必须与 metadata.md 一致**
