# task-breakdown - 任务分解

**触发词**: `/task-breakdown`, "分解任务", "任务拆解"

**职责**: 将架构分解为可执行任务卡片

---

## When to Use

- contract-first 完成后，需要基于契约分解为可执行任务
- parallel-implement 前，需要明确任务依赖
- 需要多人/多 agent 并行开发时

**When NOT to Use:**
- 架构还未确定
- 任务太简单（<1天工作量）

---

## 分解原则

### 粒度控制

| 级别 | 范围 | 示例 | 工期 |
|------|------|------|------|
| XS | 1 个函数 | 验证器、正则 | <2h |
| S | 1-2 个文件 | 单个 API | 0.5-1天 |
| M | 3-5 个文件 | 单个功能 | 1-3天 |

⚠️ 避免任务粒度过大，难以并行和跟踪

---

## 分解步骤

### Step 1: 按服务分解

每个服务独立分解。

### Step 2: 按依赖关系排序

```
1. user-service（无依赖）
2. order-service（依赖 user-service）
3. notification-service（依赖 order-service）
4. api-gateway（依赖所有服务）
```

### Step 3: 每个服务内按功能分解

### Step 4: 识别可并行任务

```
并行组：
- user-service, order-service, notification-service 可以并行开发
- api-gateway 必须最后开发
```

---

## 任务卡片模板

```markdown
## Task: [任务名称]

**Task ID**: TASK-[序号]
**服务**: [服务名]
**类型**: [XS/S/M]
**依赖**: [TASK-ID 或 "无"]
**优先级**: [P0/P1/P2]

### 描述
[一句话描述]

### 验收标准
- [ ] [标准1]

### 工期
[估算天数]

### 风险
[高/中/低]
```

---

## 输出模板

```markdown
# 任务分解

## 服务间并行分组

### Group A（可并行）
- user-service
- order-service

### Group B（必须最后）
- api-gateway

## 任务列表

| Task ID | 任务 | 类型 | 依赖 | 工期 |
|---------|------|------|------|------|
| TASK-001 | ... | S | 无 | 1天 |

## 关键路径
TASK-001 → TASK-002 → ...
```

---

## 下一步

任务分解完成后 → `/parallel-implement`

**自动执行**：自动更新 PROJECT_STATUS

**需要用户确认**：检查点 CP4（任务分解确认）

---

## Red Flags

| 信号 | 含义 |
|------|------|
| 任务粒度过大（>3天） | 需要继续拆分 |
| 依赖关系混乱 | 架构设计可能有疏漏 |
| 任务数 >50 | 项目太大，考虑拆分 |
