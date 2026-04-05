# test-review-simplify - 测试审查简化

**触发词**: `/test-review-simplify`, "审查代码", "简化"

**职责**: 测试验证、代码审查、简化实现

---

## When to Use

- parallel-implement 完成后，需要审查代码
- 需要确保测试覆盖率达标

**When NOT to Use:**
- 代码还在开发中
- 只需要简单检查

---

## 铁律：证据先于断言

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE

声称前必须：
1. 识别 — 什么命令证明？
2. 运行 — 执行完整命令
3. 读取 — 完整输出，检查退出码
4. 验证 — 输出是否确认说法？
5. 然后 — 才能声称结果
```

---

## Phase 1: Test

```bash
npm run test:coverage
```

**覆盖率**：单元测试 ≥ 80%，契约每个 postcond 有测试

---

## Phase 2: Review

### 2A: Spec Compliance（契约一致性）
```
□ 实现与契约完全一致
□ 副作用与契约声明一致
□ 错误处理符合错误契约
```

### 2B: Code Quality（代码质量）
```
□ 无死代码
□ 无重复逻辑（>3处 → 提取utility）
□ 函数 < 50行，文件 < 800行
```

### 2C: AI盲点检查
```
□ Sandbox/Production 路径一致性
□ SELECT 遗漏
□ 错误状态泄露
□ 乐观更新无回滚
```

---

## Phase 3: Stub & Wiring 检测

### Stub 检测
```bash
grep -E "(TODO|FIXME|XXX|placeholder|not implemented)" "$file"
```

**常见 stub 模式**：
```typescript
return <div>Component</div>
export async function POST() { return Response.json({ message: "Not implemented" }) }
onClick={() => {}}
```

### Wiring 验证（4级验证 Level 3）
```
Component → API:
  □ fetch 调用存在
  □ response 被使用

API → Database:
  □ 查询存在
  □ result 被返回
```

---

## Phase 4: Simplify

```
删除优先级：
1. 🔴 死代码
2. 🔴 重复逻辑（>2处 → utility）
3. 🟡 过度抽象
4. 🟡 冗余注释
```

---

## 输出标准

```
✅ All tests pass (fresh evidence)
✅ Coverage ≥ 80%
✅ Spec compliance: ALL passed
✅ No stubs found
✅ Wiring verified
✅ Ready to commit
```

---

## 下一步

测试审查通过后 → **自动触发** `/user-acceptance-test`

**自动化行为**：
- 自动更新 PROJECT_STATUS
- 自动生成测试覆盖率报告
- 自动进入 UAT 阶段

**需要用户确认**：检查点 CP6（覆盖率≥80%）
- 回复"继续"或空 → 进入 UAT
- 回复"补充测试" → 留在当前阶段

## Red Flags

| 信号 | 含义 |
|------|------|
| grep 发现 TODO/FIXME | 实现不完整 |
| wiring 验证失败 | 组件未连接 |
| AI 盲点反复出现 | 需要重新设计 |
