# Vibe Ops

AI pair-programming protocols. 7 structured skills enforcing development discipline — design, plan, review, execution, debugging, cleanup, and archiving — each with hard rules and escalation strategies.

[中文文档](README.zh-CN.md)

## Skills Overview

### Preparation & Design

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| **vibe-design** | Project initialization, design document creation | Starting a new project or adding a feature |

### Planning & Validation

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| **vibe-plan** | Implementation plan creation, iteration strategy design | Creating implementation plans with optional iteration strategies |
| **vibe-review** | Document, plan, or code review | Reviewing design docs, implementation plans, or code changes |

### Execution

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| **vibe-iterate** | Controller + subagent iterative execution | Executing implementation plans with dispatched subagents and two-phase review |

### Maintenance

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| **vibe-clean** | Code cleanup analysis via subagents | Dead code accumulation, duplicate logic, or periodic codebase health check |
| **vibe-hunt** | Diagnosis-driven debugging | Encountering bugs, errors, unexpected behavior, or test failures |
| **vibe-archive** | Memory Bank archiving | When memory-bank content grows large and needs context optimization |

## Typical Workflows

### New Project (4 phases)

```
Preparation & Design → Planning & Validation → Execution → Maintenance
         ↓                    ↓                  ↓            ↓
    vibe-design          vibe-plan          vibe-iterate  vibe-clean
                         vibe-review                      vibe-hunt
                                                          vibe-archive
```

### Feature Development (3 phases)

```
    Design      →   Validation   →   Execution
       ↓               ↓                ↓
  vibe-design     vibe-review     vibe-iterate
  vibe-plan
```

## Core Concepts

- **Brainstorming**: Complex designs explored through interactive Q&A.
- **Step-by-Step Verification**: One step, one verification during execution — user stays in control.
- **Memory Bank**: The central storage structure maintaining project context across all skills.

## Directory Structure

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

## Memory Bank Structure

```
project-root/
└── memory-bank/
    ├── architecture.md              # Project architecture (never archived)
    ├── tech-stack.md                # Technology choices (never archived)
    ├── designs/
    │   └── feature-design-[name].md # Feature design with phases, plan groups, per-phase design
    ├── plans/
    │   ├── feature-design-[name]-g[N]-plan.md  # Per-group implementation plans
    │   └── feature-plan-[name].md              # Ad-hoc feature plans
    ├── progress.md                  # Progress log
    └── archive/                     # Archived entries (not read during iteration)
```

### Document Ownership Matrix

| Document | Initial Creation | Content Population | Primary Maintenance |
| :--- | :--- | :--- | :--- |
| `feature-design-*.md` | vibe-design | vibe-design | vibe-design / vibe-iterate |
| `feature-design-*-g*-plan.md` | vibe-plan | vibe-plan | vibe-iterate / vibe-archive |
| `feature-plan-*.md` | vibe-plan | vibe-plan | vibe-iterate / vibe-archive |
| `progress.md` | vibe-iterate | vibe-iterate | vibe-iterate / vibe-archive |

---

## Acknowledgments

This project draws inspiration from the following open-source projects:

- [**Waza**](https://github.com/tw93/Waza) — Translating engineering habits into executable Claude Code skills. vibe-hunt's diagnosis-driven debugging and vibe-review's review process design are inspired by its `/hunt` and `/check` skills.
- [**Vibe Coding CN**](https://github.com/2025Emma/vibe-coding-cn) — AI pair-programming guide in Chinese. The Memory Bank structure, plan-driven development workflow, and the "plan before code" methodology are core references for this project.
- [**Everything Claude Code**](https://github.com/affaan-m/everything-claude-code) — Production-ready Claude Code agents and skills collection. vibe-clean's dead code detection, code simplification, and duplicate consolidation workflow are inspired by its `/refactor-clean` command and agent definitions.
