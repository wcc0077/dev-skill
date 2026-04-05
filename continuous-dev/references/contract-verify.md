# contract-verify - 契约验证

**触发词**: `/contract-verify`, "验证契约"

**职责**: 契约完整性、矛盾检测、4级验证

---

## When to Use

- contract-first 完成后，需要验证契约质量
- 发现实现与契约不一致

**When NOT to Use:**
- 契约还未生成
- 还在修改契约阶段

---

## 4级验证标准

| Level | 名称 | 验证内容 |
|-------|------|----------|
| 1 | Exists | 文件存在 |
| 2 | Substantive | 真实实现，非stub |
| 3 | Wired | 连接到系统 |
| 4 | Functional | 实际可用 |

**核心**：存在 ≠ 实现

---

## 完整性检查

```
□ 每个 Entity 有 CRUD 契约
□ 每个 Transition 有对应 Operation
□ 每个 Process 有完整契约
□ 每个 Operation 有 ≥ 3 类测试
```

---

## 矛盾检测

```
□ 状态矛盾：Op1 设置状态 vs Op2 要求特定状态
□ 条件重叠：两个 Op 声称可同时执行
□ 副作用冲突：连续操作发送冲突事件
```

---

## 覆盖度检查

| 契约元素 | 要求 |
|----------|------|
| Postcond | ≥ 1 测试 |
| Precond violation | ≥ 1 测试 |
| Error | ≥ 1 测试 |
| Boundary | ≥ 1 测试 |

---

## 验证报告

每个 Domain Model 的契约验证结果保存在：
```
<domain-model>/
├── Contract.verify/
│   ├── metadata.md   # 验证标准
│   └── data.md      # 验证结果
```

---

## 下一步

契约验证通过后 → **自动触发** `/dependency-analyze`

**自动化行为**：
- 自动更新 PROJECT_STATUS
- 自动运行矛盾检测脚本
- 覆盖度≥60% 时自动进入下一阶段

**需要用户确认**：检查点 CP3（覆盖度 + 矛盾检测）
- 回复"继续"或空 → 进入依赖分析
- 回复"修复矛盾" → 留在契约阶段

## Red Flags

| 信号 | 含义 |
|------|------|
| 状态矛盾无法协调 | 领域模型有环或冲突的状态转换 |
| 条件重叠 | 两个 Operation 可同时执行但声称互斥 |
| 副作用冲突 | 连续操作发送冲突的事件 |
| 覆盖度 < 60% | 契约定义不完整，缺少必要的 postcond |
