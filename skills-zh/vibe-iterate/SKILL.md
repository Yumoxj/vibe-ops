---
name: vibe-iterate
description: "Use when executing implementation plan steps one by one. Dispatches subagent per step with two-stage review (spec + quality). Handles progress tracking and git commits."
---

# Vibe Iterate

## Overview

AI 结对编程的迭代执行技能。逐步执行实施计划，用户始终掌握控制权。

**模式：Controller + Subagent**
- 主对话是协调者：调度、审查、文档、提交
- 每个步骤派发独立 subagent 执行，无上下文污染
- 执行后两阶段审查：规格合规 → 代码质量

核心规则：
1. **Read** — 按需阅读 memory-bank 文档；跳过 archive/，除非需要探索更多上下文
2. **Never modify** — 严禁私自修改 design 或 plan，冲突必须先请示
3. **Adhere to** — 按计划步骤执行，更新 progress.md
4. **Simple** — progress.md 记录保持简洁

---

## When to Use

**使用场景：**
- 执行 /vibe-plan 创建的实施计划
- 逐步完成 implementation-plan.md 或 feature-plan-*.md 中的步骤

**不适用场景：**
- 设计文档（使用 /vibe-design）
- 审查计划或代码（使用 /vibe-review）

---

## 前置准备

### 1. 确定输入源

| 输入 | 文件 |
|------|------|
| 主实施计划 | `memory-bank/implementation-plan.md` |
| 功能实施计划 | `memory-bank/feature-plan-*.md` |

不明确时用 AskUserQuestion 确认。

### 2. 确定提交策略 (Commit Strategy)

在开始执行前，向用户提供以下 Git 提交频率选项并确认其偏好：
1. **统一提交 (All-at-once)**：一次性完成所有计划任务后再统一提交。
2. **阶段提交 (Phase-by-phase)**：完成大的阶段性任务后提交（每个阶段包含多个小步骤）。
3. **步步提交 (Step-by-step)**：用户明确指定每个小步骤完成后均需要提交和确认。

*提示：为保证流畅体验，默认推荐使用 **阶段提交 (Phase-by-phase)** 或 **统一提交 (All-at-once)**。*

### 3. 迭代策略（内联规则）

| 策略 | 规则 |
|------|------|
| progress.md | 每步完成后更新，记录日期、步骤、关键变更 |
| 计划文档更新 | 每阶段完成后更新 `implementation-plan.md` 或 `feature-plan-*.md`，标注已完成事项 |
| 设计文档更新 | 仅架构变更时更新 `memory-bank/feature-phases-*.md` |
| 架构/技术栈检查 | 每阶段完成后，对比变更与 `architecture.md`/`tech-stack.md`，受影响则建议更新（仅建议，不阻塞） |
| 状态追踪 | 阶段完成后，更新 `feature-phases-*.md` 中的阶段状态（designing → done）和 `feature-design-*.md` 状态（pending → done） |
| Git 提交 | 根据用户选择的**提交策略**执行（统一、阶段或步步提交） |
| 归档 | 不读取 `memory-bank/archive/` 下的归档记录 |
| 验证 | 每步执行后等待用户确认 ✅/❌ |
| 会话 | 每步开始时询问是否需要 /new /clear /compact |

---

## 模型选择

按任务复杂度选择 subagent 模型：

| 复杂度 | 信号 | 模型 |
|--------|------|------|
| 低 | 1-2 文件，规格完整，纯机械实现 | haiku（快且省） |
| 中 | 多文件，需要集成协调或模式匹配 | sonnet |
| 高 | 需要设计判断、架构决策、广泛代码理解 | opus |

---

## 迭代循环（7 步）

对计划中的每个步骤重复以下循环：

### 1. 会话管理

询问用户：准备执行第 N 步， /continue /clear /compact？

### 2. 澄清问题

阅读与第 N 步相关的 memory-bank 文档（跳过 archive/，除非需要更多上下文）。确认第 N 步是否完全清晰。有问题先澄清。

### 3. 派发 Implementer Subagent

向 implementer subagent 提供：
- 步骤的完整文本和上下文
- 相关的设计文档摘要
- 需要修改的文件路径
- 验证标准

**处理 Subagent 状态：**

| 状态 | 含义 | 处理方式 |
|------|------|----------|
| DONE | 执行完成 | 进入 Step 4 审查 |
| DONE_WITH_CONCERNS | 完成但有疑虑 | 阅读疑虑内容，判断是否需要处理 |
| NEEDS_CONTEXT | 缺少信息 | 补充上下文后重新派发 |
| BLOCKED | 无法完成 | 评估阻塞原因：加上下文 / 换模型 / 拆任务 / 上报用户 |

Subagent 不读取 memory-bank 文件——由 controller 提供完整信息。

### 4. 两阶段审查

**4a. 规格合规审查**

派发 spec reviewer subagent，检查实现是否完全匹配步骤规格：
- 无遗漏：规格中的每项要求都已实现
- 无多余：没有规格未要求的功能
- 不合规 → implementer 修复 → 重新审查

**4b. 代码质量审查**

规格合规通过后，派发 code quality reviewer subagent：
- DRY、KISS、SOLID 原则
- 安全漏洞、错误处理
- 不通过 → implementer 修复 → 重新审查

两阶段审查均通过后，等待用户确认：✅ 通过 / ❌ 失败

### 5. 更新文档

**每步完成后：**
- 更新 `memory-bank/progress.md`，记录：
  - 日期（YYYY-MM-DD）
  - 完成的步骤编号和名称
  - 关键决策或变更（如有）

格式示例：
```
## 2026-01-15
- [x] Step 3: 添加用户认证端点
  - 决策：使用 JWT 而非 session token（无状态，扩展性更好）
  - 文件：src/auth/jwt.ts, src/routes/login.ts
```

**每阶段完成后：**
- 更新 `memory-bank/implementation-plan.md` 或 `feature-plan-*.md`
- 标注该阶段所有步骤为已完成（如将 `[ ]` 改为 `[x]`）
- **架构/技术栈一致性检查：**
  1. 读取 `memory-bank/architecture.md` 和 `memory-bank/tech-stack.md`（不存在则跳过）
  2. 对比本阶段实际变更与文档内容
  3. 如变更涉及架构、组件、技术选型或目录结构：
     - 通过 AskUserQuestion 建议更新（如"本阶段新增了组件，是否需要更新 architecture.md？"）
     - 不阻塞执行——仅建议
  4. 用户同意后更新对应文档，在变更记录（Changes Log）中记录
  5. 无论是否有变更，更新两个文档的 `Last reviewed` 日期

### 6. Git 提交确认

根据开始时确认的**提交策略**决定是否在此步提交：
- **步步提交**：生成 commit message 并等待用户确认（确认提交 / 跳过 / 编辑）。
- **阶段提交**：如果当前步骤是所在阶段的最后一步，生成该阶段的 commit message 并等待确认；否则跳过提交。
- **统一提交**：跳过此步，直到所有任务全部完成才进行最终提交。

### 7. 准备下一步

回到第 1 步继续执行，直到所有步骤完成。

---

## 常见错误

| 错误 | 后果 | 正确做法 |
|------|------|----------|
| 跳过澄清 | 方向错误 | 必须先澄清问题 |
| 自动 Push | 安全风险 | 永远不执行 git push |
| 跳过审查就继续 | 引入缺陷 | 必须完成两阶段审查 |
| 自行修改计划 | 偏离用户意图 | 冲突必须先请示 |
| 让 subagent 读计划文件 | 上下文浪费 | Controller 提供完整文本 |
| 跳过 spec 直接看质量 | 审查顺序错误 | 先合规，后质量 |
| Subagent 阻塞时重试同一模型 | 重复失败 | 换模型或拆任务 |

---

## 完成标准

- **主项目**：`implementation-plan.md` 所有步骤完成
- **功能开发**：`feature-plan-*.md` 所有步骤完成

完成后建议用户调用 `/vibe-review` 进行最终审查。
