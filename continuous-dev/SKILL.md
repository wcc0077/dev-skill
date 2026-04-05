---
name: continuous-dev
description: |
  连续开发 Skill 体系，用于契约驱动的迭代开发。按 Domain Model 组织，支持并行实现。
  触发词：/idea-research, /domain-modeling, /contract-first, /contract-verify,
  /dependency-analyze, /parallel-implement, /spec-compliance-review,
  /test-review-simplify, /user-acceptance-test, /commit, /iterate-rollback, /iteration-plan
---

# 连续开发 Skill 体系

## 概述

按 Domain Model 组织开发流程，支持并行实现。核心是**契约优先**（Contract-First）和**数据与元数据分离**。

```
想法探索澄清 ─────────────────→ 制定执行计划 ─────────────────→ 按计划执行
    ↓                               ↓                              ↓
/idea-research              /iteration-plan              domain-modeling
    ↓                               ↓                              ↓
想法逐渐清晰                  迭代目标                    contract-first
                                                         ↓
                                                     system-architecture
                                                         ↓
                                                     contract-verify
                                                         ↓
                                                 dependency-analyze
                                                         ↓
                                                     task-breakdown
                                                         ↓
                                                 parallel-implement
                                                         ↓
                                                 spec-compliance-review
                                                         ↓
                                                 test-review-simplify
                                                         ↓
                                                 user-acceptance-test
                                                         ↓
                                                    /commit
```

## Skill 索引

| 触发词 | 职责 | 层级 |
|---------|------|------|
| `/iteration-plan` | 迭代规划+追踪 | 项目级 |
| `/idea-research` | 意图澄清+调研想法 | 项目级 |
| `/domain-modeling` | 4步法领域建模 | DM级 |
| `/system-architecture` | 系统架构设计 | 项目级 |
| `/task-breakdown` | 任务分解 | 项目级 |
| `/contract-first` | 契约+测试同步生成 | DM级 |
| `/contract-verify` | 契约矛盾检测，4级验证 | DM级 |
| `/dependency-analyze` | Operation间依赖分析 | DM级 |
| `/parallel-implement` | 并行TDD实现 | DM级 |
| `/spec-compliance-review` | 契约一致性审查 | 项目级 |
| `/test-review-simplify` | 测试审查+简化 | 项目级 |
| `/user-acceptance-test` | 用户验收测试 | 项目级 |
| `/commit` | 规范化提交 | 项目级 |
| `/iterate-rollback` | 回退决策 | 项目级 |

## 执行顺序

```
第一步：想法探索澄清
    ↓
第二步：制定执行计划
    ↓
第三步：按计划执行（DM 级串行链）
    ↓
第四步：提交
    ↓
第五步：新迭代（循环）
```

| 步骤 | 场景 | Skill | 依赖 | 说明 |
|------|------|-------|------|------|
| 1 | 想法模糊，需要探索澄清 | `/idea-research` | - | 先想清楚做什么 |
| 2 | 想法清晰，制定计划 | `/iteration-plan` | - | 再规划怎么做 |
| 3.1 | 领域建模 | `/domain-modeling` | - | 定义领域概念 |
| 3.2 | 契约生成 | `/contract-first` | 3.1 | 定义每个 Operation 的契约 |
| 3.3 | 系统架构 | `/system-architecture` | 3.2 | 基于契约设计系统结构 |
| 3.4 | 契约验证 | `/contract-verify` | 3.3 | 验证契约矛盾 |
| 3.5 | 依赖分析 | `/dependency-analyze` | 3.4 | 分析 Operation 间依赖 |
| 3.6 | 任务分解 | `/task-breakdown` | 3.5 | 基于契约分解任务 |
| 3.7 | 并行实现 | `/parallel-implement` | 3.6 | 基于依赖分析的并行 TDD |
| 3.8 | 契约审查 | `/spec-compliance-review` | 3.7 | 审查契约一致性 |
| 3.9 | 测试审查 | `/test-review-simplify` | 3.8 | 最终测试审查 |
| 3.10 | 用户验收 | `/user-acceptance-test` | 3.9 | 用户验收功能 |
| 4 | 提交 | `/commit` | 3.10 | 提交代码 |

**层级说明**：
- **迭代级**：每个迭代都要执行的活动
- **DM级**：每个 Domain Model 独立执行的活动

## 核心原则

### 人类介入黄金规则

```
Claude 能做的 → Claude 必须做到
人类只做 → 需要判断的事（视觉/体验决策）
```

### 证据先于断言

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

### 4级验证标准

| Level | 名称 | 验证内容 |
|-------|------|----------|
| 1 | Exists | 文件存在 |
| 2 | Substantive | 真实实现，非stub |
| 3 | Wired | 连接到系统 |
| 4 | Functional | 实际可用 |

**核心**：存在 ≠ 实现

### 3种检查点

| 类型 | 比例 | 说明 |
|------|------|------|
| human-verify | 90% | Claude 完成自动化，人类验证 |
| decision | 9% | 人类做选择 |
| human-action | 1% | 真正需要人类手动操作 |

## 检查点（Checkpoint）索引

| CP | 阶段 | Skill | 验证内容 | 验证命令 | 失败回滚 |
|----|------|-------|----------|----------|----------|
| CP1 | 迭代计划 | iteration-plan | 迭代目标确认 | `cat iterations/iteration-N/plan.md` | 重新 `/idea-research` |
| CP2 | 领域建模 | domain-modeling | 4 步法验证（5 项） | `cat domain-models/*/DomainModel.md` | 修改 DomainModel.md |
| CP3 | 契约生成 | contract-first | 每个 Entity 有 CRUD 契约 | `grep -c "### Operation" domain-models/*/Contract.md` | 修改 Contract.md |
| CP4 | 系统架构 | system-architecture | 服务边界清晰 | `cat docs/system-architecture.md` | 修改架构文档 |
| CP5 | 契约验证 | contract-verify | 覆盖度≥60%，无矛盾 | `grep -c "postcond" domain-models/*/Contract.md` | 修改 Contract.md |
| CP6 | 任务分解 | task-breakdown | 任务卡片完整 | `cat iterations/iteration-N/plan.md \| grep TASK` | 修改任务列表 |
| CP7 | 测试审查 | test-review-simplify | 覆盖率≥80% | `npm run test:coverage` | 补充测试 |
| CP8 | 用户验收 | user-acceptance-test | 核心流程 pass | `cat iterations/iteration-N/UAT.md` | 修复 issue |

### CP 详细验证步骤

**CP1 - 迭代计划验证**
```bash
# 检查迭代计划文件存在
test -f iterations/iteration-N/plan.md && echo "✅ Plan exists"
# 检查迭代目标明确
grep -q "## 迭代目标" iterations/iteration-N/plan.md && echo "✅ Goals defined"
```

**CP2 - 领域建模验证**
```bash
# 检查 DomainModel 文件存在
ls domain-models/*/DomainModel.md
# 检查 4 步法完整性
grep -l "## Entities" domain-models/*/DomainModel.md
grep -l "## Relations" domain-models/*/DomainModel.md
grep -l "## Transitions" domain-models/*/DomainModel.md
grep -l "## Processes" domain-models/*/DomainModel.md
```

**CP3 - 契约生成验证**
```bash
# 检查契约文件存在
ls domain-models/*/Contract.md
# 检查 Operation 数量
grep -c "### Operation" domain-models/*/Contract.md
# 检查每个 Operation 有 postcond
grep -c "postcond" domain-models/*/Contract.md
```

**CP4 - 系统架构验证**
```bash
# 检查架构文档存在
test -f docs/system-architecture.md && echo "✅ Architecture exists"
# 检查服务定义
grep -q "## 服务" docs/system-architecture.md && echo "✅ Services defined"
```

**CP5 - 契约验证**
```bash
# 检查契约矛盾检测
grep -c "矛盾" domain-models/*/Contract.verify/data.md || echo "无矛盾"
# 检查覆盖度
grep "覆盖度" domain-models/*/Contract.verify/data.md
```

**CP7 - 测试验证**
```bash
# 运行测试并检查覆盖率
npm run test:coverage
# 检查覆盖率文件
test -f coverage/coverage-summary.json && echo "✅ Coverage report exists"
```

**CP7 - 用户验收验证**
```bash
# 检查 UAT 文件存在
test -f iterations/iteration-N/UAT.md && echo "✅ UAT exists"
# 检查验收结果
grep "status: complete" iterations/iteration-N/UAT.md
```

## 产物目录

```
project/
├── PROJECT_STATUS/        # 全局永久 - 项目状态
├── domain-models/         # 全局永久 - 领域模型
│   └── <dm>/
│       ├── DomainModel.md
│       ├── Contract.md
│       └── tests/
├── docs/                 # 全局永久 - 项目文档
├── src/                  # 全局永久 - 源代码
└── iterations/           # 迭代级 - 每迭代独立
    └── iteration-N/
        ├── plan.md
        ├── review.md
        └── outputs/
```

## 产物定位

| 产物 | 位置 | 类型 |
|------|------|------|
| DomainModel, Contract | `domain-models/<name>/` | 全局永久 |
| 项目文档 | `docs/` | 全局永久 |
| 源代码 | `src/` | 全局永久 |
| 迭代计划/评审 | `iterations/iteration-N/` | 迭代级 |
| 迭代产出副本 | `iterations/iteration-N/outputs/` | 临时 |

## 数据与元数据分离

```
PROJECT_STATUS/
├── metadata.md   # 元数据（固定不变）
└── data.md      # 数据（每次更新）
```

## 项目状态追踪

详见 `references/project-status.md`

## 详细内容索引

| 主题 | 文件 |
|------|------|
| idea-research 详细 | `references/idea-research.md` |
| domain-modeling 详细 | `references/domain-modeling.md` |
| contract-first 详细 | `references/contract-first.md` |
| contract-verify 详细 | `references/contract-verify.md` |
| dependency-analyze 详细 | `references/dependency-analyze.md` |
| parallel-implement 详细 | `references/parallel-implement.md` |
| spec-compliance-review 详细 | `references/spec-compliance-review.md` |
| test-review-simplify 详细 | `references/test-review-simplify.md` |
| user-acceptance-test 详细 | `references/user-acceptance-test.md` |
| iterate-rollback 详细 | `references/iterate-rollback.md` |
| iteration-plan 详细 | `references/iteration-plan.md` |
| system-architecture 详细 | `references/system-architecture.md` |
| task-breakdown 详细 | `references/task-breakdown.md` |
| checkpoint-verify 详细 | `references/checkpoint-verify.md` |
| 项目状态追踪 | `references/project-status.md` |
| commit 规范 | `references/commit.md` |
