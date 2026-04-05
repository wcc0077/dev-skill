# contract-first - 契约优先

**触发词**: `/contract-first`, "生成契约", "契约驱动"

**职责**: 从领域模型生成契约文档 + 同步生成测试用例

---

## When to Use

- domain-modeling 完成后，需要具体实现
- 需要多人协作，需要统一理解
- 需要 TDD 驱动开发

**When NOT to Use:**
- 简单脚本或一次性任务
- 探索性原型
- 已有完善契约

---

## 契约 vs 接口

```
契约 = 做什么（语义层）
接口 = 怎么做（技术层）

契约优先：先定义"是什么"，再决定"怎么暴露"
```

---

## 契约文档结构

```markdown
# Contract: <模块名>

## 数据模型
（直接复用领域模型的 Entities 和 Relations）

## 操作契约

### Operation: <操作名>
**前置条件** (Preconditions):
- [P1] ... — 验证方式

**后置条件** (Postconditions):
- [R1] ... — 验证方式

**边界条件** (Boundaries):
- [B1] ... — 处理方式

**副作用** (Side Effects):
- [SE1] 事件: ...

## 错误契约

### Error: <错误码>
**触发条件**: 当...
**返回**: { code, message, data? }
**恢复建议**: ...
```

---

## 契约 ↔ 测试用例 同步生成

```
每个 Operation 必须对应 3 类测试用例：

1. Happy Path — 验证所有 postcond 实现
2. Precondition Violation — 每个前置条件各一个
3. Boundary — 每个边界条件各一个

测试命名：
test_<operation>_postcond_<R1>
test_<operation>_precond_<P1>_violated
test_<operation>_boundary_<B1>
```

---

## 完整性检查清单

```
□ 每个 Postcond 有对应测试覆盖
□ 每个 Precond 有"违反"测试
□ 每个 Boundary 有边界测试
□ 错误契约覆盖所有错误场景
□ 副作用是声明式的，不是实现式的

□ 契约间无矛盾：
  - Op1的后置条件 ≠ Op2的前置条件冲突
  - 状态转换无环（除非业务需要）
```

---

## 产物文件

```
<domain-model>/
├── Contract.md          # 契约文档
├── *.test.ts            # 测试用例
└── errors.ts            # 错误码定义
```

---

## 下一步

契约生成后 → **自动触发** `/system-architecture`

**自动化行为**：
- 自动更新 PROJECT_STATUS
- 自动生成架构模板（基于契约中的 Operation）
- 自动识别服务边界

**需要用户确认**：检查点 CP3（契约完整性）
- 回复"继续"或空 → 进入系统架构
- 回复"补充契约" → 留在契约阶段

## Red Flags

| 信号 | 含义 |
|------|------|
| 契约矛盾无法协调 | 模型阶段有遗漏 |
| 覆盖度 < 60% | 大部分 postcond 无法定义测试 |
| 副作用写成了实现式 | 违反了声明式原则 |
