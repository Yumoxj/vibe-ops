# Vibe Ops

AI 结对编程的操作协议。7 个结构化技能，强制开发纪律——设计、计划、审查、执行、调试、清理、归档，每个阶段有硬规则和升级策略。

## 技能总览

### 准备与设计阶段技能

| 技能 | 用途 | 何时使用 |
|------|------|----------|
| **vibe-design** | 项目初始化、设计文档创建 | 新项目或添加功能时，创建设计文档 |

### 计划与确认阶段技能

| 技能 | 用途 | 何时使用 |
|------|------|----------|
| **vibe-plan** | 实施计划创建、迭代策略设计 | 创建实施计划和可选的迭代策略 |
| **vibe-review** | 文档、计划或代码审查 | 设计文档、实施计划或代码变更需要审查时 |

### 执行阶段技能

| 技能 | 用途 | 何时使用 |
|------|------|----------|
| **vibe-iterate** | Controller + Subagent 迭代执行 | 执行实施计划，派发子代理实现并两阶段审查 |

### 维护阶段技能

| 技能 | 用途 | 何时使用 |
|------|------|----------|
| **vibe-clean** | 通过子代理进行代码清理分析 | 废代码积累、重复逻辑，或定期代码库健康检查 |
| **vibe-hunt** | 诊断驱动的调试 | 遇到 bug、错误、异常行为或测试失败时 |
| **vibe-archive** | Memory Bank 归档 | memory-bank 内容膨胀时，优化上下文 |

## 典型工作流

### 新项目完整流程（4 个阶段）

```
准备与设计 → 计划与确认 → 执行 → 维护
     ↓            ↓          ↓        ↓
 vibe-design  vibe-plan  vibe-iterate  vibe-clean
                vibe-review              vibe-hunt
                                         vibe-archive
```

### 功能开发流程（3 个阶段）

```
设计 → 确认 → 执行
  ↓       ↓      ↓
vibe-design  vibe-review  vibe-iterate
vibe-plan
```

## 核心理念

- **Brainstorming**: 复杂设计通过交互式问答探索。
- **Step-by-Step Verification**: 执行阶段一步一验证，用户掌握控制权。
- **Memory Bank**: 维护项目上下文的核心存储结构。

## 目录结构

```
skills/
├── vibe-design/
│   └── references/
│       └── feature-design-template.md
├── vibe-plan/
│   └── references/
│       └── feature-plan-template.md
├── vibe-review/
│   ├── agents/
│   │   ├── reviewer-architecture.md
│   │   └── reviewer-security.md
│   ├── references/
│   │   └── persona-catalog.md
│   └── scripts/
│       └── run-tests.sh
├── vibe-iterate/
│   └── agents/
│       ├── implementer.md
│       ├── spec-reviewer.md
│       └── quality-reviewer.md
├── vibe-clean/
│   ├── agents/
│   │   ├── dead-code-analyzer.md
│   │   ├── simplification-analyzer.md
│   │   └── duplicate-analyzer.md
│   └── references/
│       └── cleanup-catalog.md
├── vibe-hunt/
└── vibe-archive/
```

## Memory Bank 结构

```
project-root/
└── memory-bank/
    ├── architecture.md              # 项目架构（永不归档）
    ├── tech-stack.md                # 技术选型（永不归档）
    ├── designs/
    │   └── feature-design-[name].md # 功能设计（含阶段划分、计划分组、逐阶段设计）
    ├── plans/
    │   ├── feature-design-[name]-g[N]-plan.md  # 分组实施计划
    │   └── feature-plan-[name].md              # 临时功能计划
    ├── progress.md                  # 进度记录
    └── archive/                     # 归档记录（迭代时不读取）
```

### 文档权责矩阵

| 文档名称 | 初始创建 | 内容填充 | 主要维护 |
| :--- | :--- | :--- | :--- |
| `feature-design-*.md` | vibe-design | vibe-design | vibe-design / vibe-iterate |
| `feature-design-*-g*-plan.md` | vibe-plan | vibe-plan | vibe-iterate / vibe-archive |
| `feature-plan-*.md` | vibe-plan | vibe-plan | vibe-iterate / vibe-archive |
| `progress.md` | vibe-iterate | vibe-iterate | vibe-iterate / vibe-archive |

---


## 致谢

本项目灵感来源于以下开源项目：

- [**Waza**](https://github.com/tw93/Waza) — 将工程师习惯转化为可执行的 Claude Code 技能。vibe-hunt 的诊断驱动调试、vibe-review 的审查流程设计受其 `/hunt` 和 `/check` 技能启发。
- [**Vibe Coding CN**](https://github.com/2025Emma/vibe-coding-cn) — AI 结对编程中文指南。Memory Bank 结构、规划驱动的开发流程、以及"先规划后编码"的方法论是本项目的核心参考。
- [**Everything Claude Code**](https://github.com/affaan-m/everything-claude-code) — 生产级 Claude Code agent 和技能集合。vibe-clean 的废代码检测、代码简化、重复代码合并流程受其 `/refactor-clean` 命令和 agent 定义启发。
