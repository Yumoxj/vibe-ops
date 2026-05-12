---
name: vibe-archive
description: "Use when memory-bank becomes bloated (progress.md > 50 steps, > 5 completed features, or > 3 completed phases). Analyzes state and archives completed tasks to optimize AI context."
---

# Vibe Archive

## Overview

Memory Bank 归档技能。当 memory-bank 内容膨胀时，分析当前状态，安全地归档已完成的任务和功能。

**核心原则：智能分析，建议优先，安全归档。**

---

## When to Use

```dot
digraph archive_usage {
    rankdir=TB;
    node [fontname="Arial", fontsize=12];

    start [shape=ellipse, label="Memory Bank 膨胀"];
    analyze [shape=box, label="生成归档建议"];
    confirm [shape=diamond, label="用户确认"];
    execute [shape=box, label="执行归档"];

    start -> analyze;
    analyze -> confirm;
    confirm -> execute [label="确认"];
}
```

**触发条件（任一即触发）：**
- `progress.md`: > 50 步
- `memory-bank/plans/feature-plan-*.md`: > 5 个已完成功能
- `memory-bank/plans/feature-design-*-g*-plan.md`: > 3 个已完成分组计划

**不归档的内容：**
- 主设计文档（`memory-bank/designs/feature-design-*.md`）— 始终保留
- 架构文档（`memory-bank/architecture.md`）— 始终保留
- 技术栈文档（`memory-bank/tech-stack.md`）— 始终保留
- 进行中的功能文档（未完成的 `memory-bank/plans/feature-plan-*.md`）
- 进行中的分组计划文件（未完成的 `memory-bank/plans/feature-design-*-g*-plan.md`）

---

## 执行流程

### 第一步：分析 Memory Bank 状态

分析以下内容：
1. 统计 `progress.md` 行数和日期范围
2. 扫描 `memory-bank/plans/feature-plan-*.md` 文件，识别已完成和进行中的功能
3. 扫描 `memory-bank/plans/feature-design-*-g*-plan.md` 文件，识别已完成的分组计划
4. 评估是否需要归档

### 第二步：生成归档建议

输出当前状态统计和归档建议。

- **达到阈值**：✅ 建议归档，列出具体收益
- **未达阈值**：⚠️ 警告"当前项目规模较小，归档可能导致上下文碎片化"，使用 AskUserQuestion 确认（选项：取消操作 / 强制归档）

### 第三步：确认归档计划

使用 AskUserQuestion 确认归档范围。

### 第四步：执行归档

**归档目录结构：**

```
memory-bank/archive/
├── progress/
│   └── progress-archive-YYYY-MM-DD.md
├── features/
│   └── features-archive-YYYY-MM-DD.md
├── plans/
│   └── plans-archive-YYYY-MM-DD.md
└── archived-items.md
```

**操作步骤：**
1. 创建归档目录
2. 移动已完成的文件到归档目录
3. 创建/更新 `memory-bank/archive/archived-items.md` 索引
4. 更新 `progress.md`（保留最近 50 步）

**索引模板：**

```markdown
# Memory Bank 归档索引

> 最后更新：YYYY-MM-DD

| 归档日期 | 归档文件 | 归档内容摘要 |
|----------|----------|--------------|
| YYYY-MM-DD | progress-archive-YYYY-MM-DD.md | progress.md [日期范围] 的已完成步骤 |
| YYYY-MM-DD | features-archive-YYYY-MM-DD.md | [功能名称列表] |
| YYYY-MM-DD | plans-archive-YYYY-MM-DD.md | 分组计划 [1-N] |
```

### 第五步：验证

| 验证项 | 检查内容 |
|--------|----------|
| 归档完整性 | 归档文件正确创建 |
| 索引完整性 | archived-items.md 索引完整 |
| 进行中内容 | 进行中内容未受影响 |
| 主设计文档 | `memory-bank/designs/feature-design-*.md` 未被移动 |
| 架构/技术栈文档 | architecture.md 和 tech-stack.md 未被移动 |

---

## 常见错误

| 错误 | 后果 | 正确做法 |
|------|------|----------|
| 归档主设计文档 | 丢失核心上下文 | 主设计文档、architecture.md、tech-stack.md 始终保留 |
| 归档未完成的功能 | 功能信息丢失 | 只归档已完成的 feature |
| 跳过用户确认 | 误删重要文件 | 必须等待用户确认 |

---

## 注意事项

- 归档文件存放在 `memory-bank/archive/`，迭代技能（vibe-iterate）不读取归档记录
- 归档是单向操作，执行前确认范围
