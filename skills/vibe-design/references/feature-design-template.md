# Feature Design Document Template

This document contains the merged feature design template used by the `vibe-design` skill. It combines the phased feature list and per-phase design into a single document.

---

## feature-design-xxxx.md Template

```markdown
# Feature Design: [Project Name]

> Created: YYYY-MM-DD
> Status: active

---

## Architecture Overview

> Full architecture and tech stack details in [architecture.md](../architecture.md) and [tech-stack.md](../tech-stack.md). If either is missing, run /vibe-design to create them first.

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

## Plan Groups

| Group | Phases | Rationale |
|-------|--------|-----------|
| G1 | Phase 0, Phase 1 | [Why grouped together] |
| G2 | Phase 2 | [Why standalone] |
| ... | ... | ... |

> Each group produces one implementation plan file: `plans/feature-design-[name]-g[N]-plan.md`

---

## Phase Designs

### Phase 0: [Name]

#### Design Approach
*Filled in by vibe-design Mode B — approach summary, comparison, technical considerations*

#### User Interaction
*Filled in by vibe-design Mode B — user story and interaction flow*

#### Acceptance Criteria
*Filled in by vibe-design Mode B — testable criteria*
- [ ] [Testable criterion 1]
- [ ] [Testable criterion 2]

---

### Phase 1: [Name]

#### Design Approach
*Filled in by vibe-design Mode B*

#### User Interaction
*Filled in by vibe-design Mode B*

#### Acceptance Criteria
*Filled in by vibe-design Mode B*
- [ ] [Testable criterion 1]
- [ ] [Testable criterion 2]

---

[Continue for each phase]

---

## Risks and Constraints

[Known risks, memory budget, device requirements, etc.]

---

## References

| Source | What it informed |
|--------|-----------------|
| [External URL or `refs/filename.md`] | [What was referenced from this source] |
| [External URL or `refs/filename.md`] | [What was referenced from this source] |

---

## Next Steps

After confirming the design, use /vibe-plan to create implementation plans for each group.
```
