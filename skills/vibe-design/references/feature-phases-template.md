# Feature Phases Document Template

This document contains the phased feature list template used by the `vibe-design` skill.

---

## feature-phases-xxxx.md Template

```markdown
# [Project Name] Feature Phases

> Created: YYYY-MM-DD

---

## Architecture Overview

> See [architecture.md](architecture.md) and [tech-stack.md](tech-stack.md) for full details. These documents are created by vibe-design Mode A Step 2; if missing, run /vibe-design to create them first.

### Purpose
[One sentence describing the project purpose]

### Architecture Diagram
[ASCII diagram or description — full version in architecture.md]

---

## Phases

| Phase | Name | Goal | Dependencies | Verification | Status |
|-------|------|------|-------------|-------------|--------|
| Phase 0 | [Name] | [One-line goal] | - | [How to verify] | pending |
| Phase 1 | [Name] | [One-line goal] | Phase 0 | [How to verify] | pending |
| ... | ... | ... | ... | ... | ... |

### Phase Details

#### Phase 0: [Name]
**Goal:** [Specific goal]
**Scope:** [What's included, what's not]
**Verification:** [How to verify this phase is complete and independently testable]

#### Phase 1: [Name]
**Goal:** [Specific goal]
**Scope:** [What's included, what's not]
**Verification:** [How to verify this phase is complete and independently testable]

[Continue for each phase]

---

## Risks and Constraints

[Known risks, memory budget, device requirements, etc.]

---

## Next Steps

After confirming the phased feature list, use /vibe-design to design each phase, then /vibe-plan to create implementation plans.
```
