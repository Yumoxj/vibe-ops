---
name: vibe-archive
description: "Use when memory-bank becomes bloated (progress.md > 50 steps, > 5 completed features, > 3 completed group plans, or > 3 completed designs). Analyzes state and archives completed tasks and designs to optimize AI context."
---

# Vibe Archive

## Overview

Memory Bank archiving skill. When memory-bank content becomes bloated, analyze current state and safely archive completed tasks and features.

**Core principle: Smart analysis, suggest first, safe archiving.**

---

## When to Use

```dot
digraph archive_usage {
    rankdir=TB;
    node [fontname="Arial", fontsize=12];

    start [shape=ellipse, label="Memory Bank bloated"];
    analyze [shape=box, label="Generate archive suggestions"];
    confirm [shape=diamond, label="User confirms"];
    execute [shape=box, label="Execute archive"];

    start -> analyze;
    analyze -> confirm;
    confirm -> execute [label="Confirmed"];
}
```

**Trigger conditions (any one triggers):**
- `progress.md`: > 50 steps
- `memory-bank/plans/feature-plan-*.md`: > 5 completed features
- `memory-bank/plans/feature-design-*-g*-plan.md`: > 3 completed group plans
- `memory-bank/designs/feature-design-*.md`: > 3 completed designs

**Never archive (always keep):**
- Architecture document (`memory-bank/architecture.md`)
- Tech stack document (`memory-bank/tech-stack.md`)

**Conditionally archivable (user choice at confirmation):**
- Completed design documents (`memory-bank/designs/feature-design-*.md`) — user chooses: archive all or keep latest (recommended)

**Do not archive:**
- In-progress design documents (incomplete `memory-bank/designs/feature-design-*.md`)
- In-progress feature documents (incomplete `memory-bank/plans/feature-plan-*.md`)
- In-progress group plan files (incomplete `memory-bank/plans/feature-design-*-g*-plan.md`)

---

## Execution Flow

### Step 1: Analyze Memory Bank State

Analyze the following:
1. Count `progress.md` lines and date range
2. Scan `memory-bank/plans/feature-plan-*.md` files, identify completed and in-progress features
3. Scan `memory-bank/plans/feature-design-*-g*-plan.md` files, identify completed group plans
4. Scan `memory-bank/designs/feature-design-*.md` files, identify completed and in-progress designs
5. Assess whether archiving is needed

### Step 2: Generate Archive Suggestions

Output current state statistics and archiving suggestions.

- **Threshold reached**: ✅ Suggest archiving, list specific benefits
- **Threshold not reached**: ⚠️ Warn "current project scale is small, archiving may cause context fragmentation", use AskUserQuestion to confirm (options: cancel / force archive)

### Step 3: Confirm Archive Plan

Use AskUserQuestion to confirm archiving scope.

**Design archiving mode** (when completed designs exist):
- **Keep latest (recommended)**: Archive all completed designs except the most recently modified one. The kept design remains as active context for ongoing work.
- **Archive all**: Move all completed designs to archive. Use when starting a new major phase with no dependency on prior designs.

Include the chosen mode in the confirmation summary before executing.

### Step 4: Execute Archive

**Archive directory structure:**

```
memory-bank/archive/
├── progress/
│   └── progress-archive-YYYY-MM-DD.md
├── features/
│   └── features-archive-YYYY-MM-DD.md
├── plans/
│   └── plans-archive-YYYY-MM-DD.md
├── designs/
│   └── designs-archive-YYYY-MM-DD.md
└── archived-items.md
```

**Steps:**
1. Create archive directories
2. Move completed files to archive directory
3. Create/update `memory-bank/archive/archived-items.md` index
4. Update `progress.md` (keep most recent 50 steps)

**Index template:**

```markdown
# Memory Bank Archive Index

> Last updated: YYYY-MM-DD

| Archive date | Archive file | Content summary |
|-------------|-------------|-----------------|
| YYYY-MM-DD | progress-archive-YYYY-MM-DD.md | Completed steps from progress.md [date range] |
| YYYY-MM-DD | features-archive-YYYY-MM-DD.md | [Feature name list] |
| YYYY-MM-DD | plans-archive-YYYY-MM-DD.md | Group plans [1-N] |
| YYYY-MM-DD | designs-archive-YYYY-MM-DD.md | [Design name list] (mode: keep latest / archive all) |
```

### Step 5: Verify

| Check | Content |
|-------|---------|
| Archive completeness | Archive files created correctly |
| Index completeness | archived-items.md index is complete |
| In-progress content | In-progress content is unaffected |
| Foundation docs | architecture.md and tech-stack.md were not moved |
| Design archiving mode | If "keep latest": exactly 1 completed design remains in designs/ |
| Design archiving mode | If "archive all": no completed designs remain in designs/ |

---

## Common Mistakes

| Mistake | Consequence | Correct approach |
|---------|-------------|------------------|
| Archive architecture.md or tech-stack.md | Lost foundation context | These are never archived, regardless of user request |
| Archive incomplete designs or features | Active work lost | Only archive completed items |
| Skip user confirmation | Accidentally delete important files | Must wait for user confirmation |
| Skip design archiving mode question | Unclear what remains in designs/ | Always ask user to choose archive all or keep latest |

---

## Notes

- Archived files are stored in `memory-bank/archive/`, the iteration skill (vibe-iterate) does not read archive records
- Archiving is a one-way operation, confirm scope before executing
