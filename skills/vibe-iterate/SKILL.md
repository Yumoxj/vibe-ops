---
name: vibe-iterate
description: "Use when executing implementation plan steps one by one. Dispatches subagent per step with two-stage review (spec + quality). Handles progress tracking and git commits."
---

# Vibe Iterate

## Overview

Iterative execution skill for AI pair programming. Execute the implementation plan step by step, with the user always in control.

**Mode: Controller + Subagent**
- The main conversation is the coordinator: dispatch, review, documentation, commits
- Each step dispatches an independent subagent, no context pollution
- Post-execution two-stage review: spec compliance → code quality

Core rules:
1. **Read** — Read memory-bank documents as needed; skip archive/ unless exploring for context
2. **Never modify** — Never modify design or plan without permission; conflicts must be escalated first
3. **Adhere to** — Execute according to plan steps, update progress.md
4. **Simple** — Keep progress.md entries concise

---

## When to Use

**Use cases:**
- Execute implementation plans created by /vibe-plan
- Complete steps in implementation-plan.md or feature-plan-*.md one at a time

**Not for:**
- Design documents (use /vibe-design)
- Reviewing plans or code (use /vibe-review)

---

## Prerequisites

### 1. Identify Input Source

| Input | File |
|-------|------|
| Main implementation plan | `memory-bank/implementation-plan.md` |
| Feature implementation plan | `memory-bank/feature-plan-*.md` |

If unclear, use AskUserQuestion to confirm.

### 2. Confirm Commit Strategy

Before execution, ask the user to choose their preferred Git commit frequency:
1. **All-at-once**: Complete all planned tasks before making a single final commit.
2. **Phase-by-phase**: Commit after completing major phases (each phase contains multiple small steps).
3. **Step-by-step**: Explicitly confirm and commit after every single small step.

*Hint: To ensure a smooth experience without frequent interruptions, recommend **Phase-by-phase** or **All-at-once** by default.*

### 3. Iteration Strategy (Inline Rules)

| Strategy | Rule |
|----------|------|
| progress.md | Update after each step, record date, step, key changes |
| Plan document update | After each phase, update `implementation-plan.md` or `feature-plan-*.md`, mark completed items |
| Design document update | Only update `memory-bank/feature-phases-*.md` for architecture changes |
| Architecture/tech-stack check | After each phase, compare changes against `architecture.md` and `tech-stack.md`, suggest updates if affected (advisory, not blocking) |
| Status tracking | After completing a phase, update phase status in `feature-phases-*.md` (designing → done) and `feature-design-*.md` (pending → done) |
| Git commits | Execute according to the user's chosen **Commit Strategy** (All-at-once, Phase-by-phase, or Step-by-step) |
| Archive | Do not read records under `memory-bank/archive/` |
| Verification | After each step, wait for user confirmation ✅/❌ |
| Session | At the start of each step, ask if /new /clear /compact is needed |

---

## Model Selection

Choose subagent model based on task complexity:

| Complexity | Signals | Model |
|------------|---------|-------|
| Low | 1-2 files, complete spec, pure mechanical implementation | haiku (fast and cheap) |
| Medium | Multiple files, requires integration coordination or pattern matching | sonnet |
| High | Requires design judgment, architecture decisions, broad code understanding | opus |

---

## Iteration Loop (7 Steps)

Repeat the following loop for each step in the plan:

### 1. Session Management

Ask user: Ready to execute step N, /continue /clear /compact?

### 2. Clarification Questions

Read memory-bank documents relevant to step N (skip archive/ unless more context is needed). Confirm whether step N is fully clear. Clarify any questions first.

### 3. Dispatch Implementer Subagent

Provide the implementer subagent with:
- Complete text and context of the step
- Relevant design document summary
- File paths to modify
- Verification criteria

**Handling Subagent Status:**

| Status | Meaning | Action |
|--------|---------|--------|
| DONE | Execution complete | Proceed to Step 4 review |
| DONE_WITH_CONCERNS | Complete with doubts | Read concerns, decide if action needed |
| NEEDS_CONTEXT | Missing information | Supply context and redispatch |
| BLOCKED | Cannot complete | Assess blocker: add context / switch model / split task / escalate to user |

Subagents do not read memory-bank files — the controller provides complete information.

### 4. Two-Stage Review

**4a. Spec Compliance Review**

Dispatch spec reviewer subagent to check if implementation exactly matches the step specification:
- No omissions: every requirement in the spec has been implemented
- No extras: no features implemented that the spec did not request
- Non-compliant → implementer fixes → re-review

**4b. Code Quality Review**

After spec compliance passes, dispatch code quality reviewer subagent:
- DRY, KISS, SOLID principles
- Security vulnerabilities, error handling
- Not passed → implementer fixes → re-review

After both review stages pass, wait for user confirmation: ✅ pass / ❌ fail

### 5. Update Documentation

**After each step:**
- Update `memory-bank/progress.md`, recording:
  - Date (YYYY-MM-DD)
  - Completed step number and name
  - Key decisions or changes (if any)

Format example:
```
## 2026-01-15
- [x] Step 3: Add user authentication endpoint
  - Decided: use JWT over session tokens (stateless, scales better)
  - Files: src/auth/jwt.ts, src/routes/login.ts
```

**After each phase:**
- Update `memory-bank/implementation-plan.md` or `feature-plan-*.md`
- Mark all steps in that phase as completed (e.g., change `[ ]` to `[x]`)
- **Architecture/tech-stack consistency check:**
  1. Read `memory-bank/architecture.md` and `memory-bank/tech-stack.md` (skip if they don't exist)
  2. Compare with the actual changes made in this phase
  3. If changes affect architecture, components, tech choices, or directory structure:
     - Use AskUserQuestion to suggest updates (e.g., "This phase added a new component. Should architecture.md be updated?")
     - Do not block execution — this is advisory only
  4. If user agrees, update the relevant document and record in its Changes Log section
  5. Update the `Last reviewed` date in both documents regardless of changes

### 6. Git Commit Confirmation

Decide whether to commit here based on the chosen **Commit Strategy**:
- **Step-by-step**: Generate a commit message and wait for user confirmation (confirm / skip / edit).
- **Phase-by-phase**: If the current step completes a major phase, generate a commit message for the phase and wait for confirmation; otherwise, skip.
- **All-at-once**: Skip this step entirely until all tasks in the plan are complete, then do a final commit.

### 7. Prepare Next Step

Return to Step 1 and continue until all steps are complete.

---

## Common Mistakes

| Mistake | Consequence | Correct approach |
|---------|-------------|------------------|
| Skip clarification | Wrong direction | Must clarify questions first |
| Auto push | Security risk | Never execute git push |
| Continue without review | Introduces defects | Must complete two-stage review |
| Modify plan unilaterally | Deviates from user intent | Conflicts must be escalated first |
| Let subagent read plan files | Context waste | Controller provides complete text |
| Skip spec, go straight to quality | Wrong review order | Spec compliance first, then quality |
| Retry same model when subagent is blocked | Repeated failures | Switch model or split task |

---

## Completion Criteria

- **Main project**: All steps in `implementation-plan.md` are complete
- **Feature development**: All steps in `feature-plan-*.md` are complete

After completion, suggest user invoke `/vibe-review` for final review.
