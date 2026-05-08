# Vibe Ops

AI pair-programming protocols. 6 structured skills enforcing development discipline — design, plan, review, execution, debugging, and archiving — each with hard rules and escalation strategies.

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
| **vibe-hunt** | Diagnosis-driven debugging | Encountering bugs, errors, unexpected behavior, or test failures |
| **vibe-archive** | Memory Bank archiving | When memory-bank content grows large and needs context optimization |

## Typical Workflows

### New Project (4 phases)

```
Preparation & Design → Planning & Validation → Execution → Maintenance
         ↓                    ↓                  ↓            ↓
    vibe-design          vibe-plan          vibe-iterate  vibe-hunt
                         vibe-review                      vibe-archive
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
│       ├── feature-design-template.md
│       └── feature-phases-template.md
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
├── vibe-hunt/
└── vibe-archive/
```

## Memory Bank Structure

```
project-root/
├── docs/
│   └── plans/                       # Plans / design documents
└── memory-bank/
    ├── feature-phases-[name].md     # Phased feature list (architecture, tech stack, status)
    ├── feature-design-[name].md     # Per-phase design documents
    ├── implementation-plan.md       # Implementation plan
    ├── progress.md                  # Progress log
    └── archive/                     # Archived entries (not read during iteration)
```

### Document Ownership Matrix

| Document | Initial Creation | Content Population | Primary Maintenance |
| :--- | :--- | :--- | :--- |
| `feature-phases-*.md` | vibe-design | vibe-design | vibe-design / vibe-iterate |
| `feature-design-*.md` | vibe-design | vibe-design | vibe-design |
| `implementation-plan.md` | vibe-plan | vibe-plan | vibe-review / vibe-archive |
| `progress.md` | vibe-iterate | vibe-iterate | vibe-iterate / vibe-archive |

---

## Acknowledgments

This project draws inspiration from the following open-source projects:

- [**Waza**](https://github.com/tw93/Waza) — Translating engineering habits into executable Claude Code skills. vibe-hunt's diagnosis-driven debugging and vibe-review's review process design are inspired by its `/hunt` and `/check` skills.
- [**Vibe Coding CN**](https://github.com/2025Emma/vibe-coding-cn) — AI pair-programming guide in Chinese. The Memory Bank structure, plan-driven development workflow, and the "plan before code" methodology are core references for this project.
