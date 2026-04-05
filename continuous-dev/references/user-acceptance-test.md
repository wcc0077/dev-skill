# user-acceptance-test - 用户验收测试

**触发词**: `/user-acceptance-test`, "UAT", "用户验收"

**职责**: 基于 SUMMARY.md 提取可测试项，逐个呈现预期行为，用户确认是否匹配

---

## When to Use

- parallel-implement 完成后，需要用户验收功能
- 迭代结束前，需要确认功能符合预期
- 需要记录验收结果用于后续追踪

**When NOT to Use:**
- 代码还在开发中
- 测试审查未通过（test-review-simplify 未完成）
- 只需要内部测试，不需要用户参与

---

## 验收流程

```
1. 读取 SUMMARY.md → 提取可测试项
2. 创建 UAT.md 文件
3. 逐个呈现测试（一次一个）
4. 用户确认：yes/pass 或描述实际问题
5. 记录结果：pass/issue/blocked
6. 验收完成后生成报告
```

---

## 验收原则

### 1. 用户可观察优先

只测试**用户可观察**的行为，不测试内部实现：

| ✅ 应该测试 | ❌ 不应该测试 |
|------------|------------|
| 点击按钮后弹出对话框 | 函数返回值正确 |
| 页面加载时间 < 2s | 数据库索引创建成功 |
| 列表按名称排序 | 缓存命中率 90% |

### 2. 预期→确认模式

```
Claude 呈现："预期行为是 XXX"
用户确认：
  - "yes" / "y" / "pass" / "next" / 空回复 → pass
  - 描述实际问题 → issue（记录详细问题）
  - "blocked" / "无法测试" → blocked（记录原因）
```

### 3. 不询问严重程度

用户描述问题后，**自动推断**严重程度：

| 用户描述 | 推断严重程度 |
|----------|-------------|
| "崩溃"、"报错"、"完全不能用" | blocker |
| "不工作"、"没反应"、"错误的" | major |
| "可以用但..."、"有点慢" | minor |
| "颜色"、"字体"、"间距" | cosmetic |

---

## UAT.md 模板

```markdown
---
status: testing
phase: iteration-N
source: [iterations/iteration-N/outputs/SUMMARY.md]
started: 2026-04-05T10:00:00Z
updated: 2026-04-05T10:30:00Z
---

## Current Test

number: 1
name: 创建订单
expected: |
  点击"下单"按钮后，弹出订单确认对话框，显示商品信息和总价。
  点击"确认"后，跳转订单详情页，显示"待支付"状态。
awaiting: user response

## Tests

### 1. 创建订单
expected: [预期行为]
result: [pending]

### 2. 支付订单
expected: [预期行为]
result: [pending]

### 3. 取消订单
expected: [预期行为]
result: [pending]

## Summary

total: 3
passed: 0
issues: 0
blocked: 0
pending: 3

## Issues

[none yet]
```

---

## 验收维度

### 1. 核心流程（必须测试）

- 关键用户旅程（User Journey）
- 主要功能入口
- 数据输入→处理→输出完整链路

### 2. 边界条件（应该测试）

- 空状态（无数据时的显示）
- 错误输入（无效表单、网络错误）
- 极端情况（大量数据、超长文本）

### 3. 体验细节（可选测试）

- 加载状态（Loading 动画）
- 错误提示（Toast/Dialog）
- 响应式布局（不同屏幕尺寸）

---

## 下一步

验收完成后 → **自动触发** `/commit`

**自动化行为**：
- 自动更新 PROJECT_STATUS
- 自动生成验收报告
- 所有测试 pass 时自动进入提交阶段
- 有 issues 时自动生成问题清单

**需要用户确认**：检查点 CP7（验收结果）
- issues = 0 → 回复"继续"或空进入提交
- issues > 0 → 回复"修复"进入问题修复流程

---

## Red Flags

| 信号 | 含义 |
|------|------|
| blocked 测试 > 50% | 测试环境有问题，或功能未完成 |
| blocker issue > 0 | 必须修复后才能发布 |
| 所有测试 pass | 可能测试粒度太粗，遗漏边界情况 |
| 用户无法理解预期 | 预期描述太技术化，需要用业务语言 |
