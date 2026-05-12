# CLAUDE.md

Guidance for Claude Code (claude.ai/code) working in this repo.

## Project Overview

Vibe Skills = 6 structured AI pair-programming skills enforcing dev discipline. Skills = Markdown files (`SKILL.md` + `agents/references/scripts`), loaded by Claude Code at runtime. No build step, no runtime deps, no compiled output.

## Commands

No build/lint/test commands. Documentation-only skill repo.

- **Test skills**: TDD via pressure scenarios in `tests/scenarios/` (RED → GREEN → REFACTOR), run manually with subagents

## Architecture

### Skill Lifecycle Flow

```
vibe-design → vibe-plan → vibe-review → vibe-iterate → vibe-hunt / vibe-archive
```

Each skill invoked as `/vibe-<name>`, operates on shared `memory-bank/` in target project.

### Memory Bank (target project)

`memory-bank/` = single source of truth across all skills:

```
memory-bank/
├── architecture.md          # Project architecture (never archived)
├── tech-stack.md            # Technology choices (never archived)
├── progress.md              # Execution log
├── designs/
│   └── feature-design-*.md  # Feature design with phases, plan groups, and per-phase design
├── plans/
│   ├── feature-design-*-g*-plan.md  # Per-group implementation plans
│   └── feature-plan-*.md            # Ad-hoc feature plans
└── archive/                 # Completed items (never read by vibe-iterate)
```

| File | Created by | Purpose |
|------|-----------|---------|
| `architecture.md` | vibe-design | Single source of truth for project architecture (never archived) |
| `tech-stack.md` | vibe-design | Single source of truth for technology choices (never archived) |
| `designs/feature-design-*.md` | vibe-design | Feature design document with phases table, plan groups, and per-phase design sections |
| `plans/feature-design-*-g*-plan.md` | vibe-plan | Per-group implementation plan with verification criteria |
| `plans/feature-plan-*.md` | vibe-plan | Ad-hoc feature-specific implementation plan |
| `progress.md` | vibe-iterate | Execution log (date, step, key changes) |
| `archive/` | vibe-archive | Completed items (never read by vibe-iterate) |

**Plan file naming:** `feature-design-[name]-g[N]-plan.md` in `plans/` — one plan file per Plan Group defined in `designs/feature-design-*.md`. Groups are evaluated by vibe-design based on phase coupling and complexity.

### vibe-iterate: Controller + Subagent Pattern

Main conversation = controller. Each plan step dispatches independent subagent (defined in `skills/vibe-iterate/agents/`):

- **implementer.md** — Executes single step, reports DONE/DONE_WITH_CONCERNS/NEEDS_CONTEXT/BLOCKED
- **spec-reviewer.md** — Checks implementation matches step spec exactly
- **quality-reviewer.md** — Checks code quality (DRY, KISS, security)

Model selection by complexity: haiku (low), sonnet (medium), opus (high).

### vibe-review: Depth-Based Expert System

Three depth levels by diff size:

| Depth | Lines | Actions |
|-------|-------|---------|
| Quick | < 100 | Basic review |
| Standard | 100–500 | + conditional expert agents (`agents/reviewer-security.md`, `agents/reviewer-architecture.md`) |
| Deep | 500+ | + all experts + adversarial analysis |

Expert activation rules in `references/persona-catalog.md`.

### vibe-hunt: Hypothesis-Driven Debugging

Requires specific, verifiable root cause statement before touching code. Hard stops: fix doesn't resolve symptom → start over; 3 failed hypotheses → escalate to user. Escalation levels in SKILL.md (Level 1–3).

## Dual-Language Structure

Two language versions:

| Directory | Language | Role |
|-----------|----------|------|
| `skills/` | English (primary) | Authoritative source |
| `skills-zh/` | Chinese | Mirror |

**Sync rule: every skill modification must update both `skills/` and `skills-zh/`.** Includes `SKILL.md` files, agent definitions in `agents/`, reference templates in `references/`. Both dirs must maintain file-count parity.

### README Sync

Two README files:

| File | Language | Role |
|------|----------|------|
| `README.md` | English (primary) | Authoritative |
| `README.zh-CN.md` | Chinese | Mirror |

**Sync rule: any change to `README.md` or `README.zh-CN.md` must update the other for content parity.**

### GitHub Description Sync

Repo has GitHub description set manually. After skill updates, check if description still accurate, update if needed.

## Key Conventions

- **No code in plans**: vibe-plan steps = instructions only, never implementation code
- **No auto-execution**: After design/plan/review, skills stop, wait for user instruction
- **Skill frontmatter**: Each `SKILL.md` has YAML frontmatter with `name` and `description` for skill discovery
- **Agent definitions**: Markdown files in `agents/` subdirectories define subagent behavior
- **Reference templates**: `references/` directories contain output format templates

## Testing the Skills

Skills tested via pressure scenarios (not unit tests):

- `tests/scenarios/discipline-enforcing-skills.md` — Tests vibe-design and vibe-review under pressure (time, sunk cost, authority, fatigue)
- `tests/scenarios/pattern-skills.md` — Tests vibe-design, vibe-plan, vibe-iterate pattern detection and mode switching
- `tests/scenarios/technique-skills.md` — Tests vibe-hunt and vibe-archive techniques
- `tests/execution_logs/` — Records of actual test runs

Methodology: run scenario without skill (RED), with skill (GREEN), close rationalization loopholes (REFACTOR).