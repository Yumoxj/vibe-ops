# Feature Plan Templates

This document contains the feature plan templates used by the `vibe-plan` skill.

---

## Group Plan Template: feature-phases-[name]-g[N]-plan.md

```markdown
# Implementation Plan: Group [N]

> Plan Group: G[N]
> Phases: Phase X, Phase Y
> Source: feature-phases-[name].md
> Status: Pending / In Progress / Completed

---

## Goal

[What this group of phases should accomplish together]

---

## Implementation Steps

### Phase X: [Phase Name]

#### Step 1: [Step Name]
**Goal:** [What this step should accomplish]
**Instructions:** [What to do specifically, no code]
**Verification:** [How to verify this step succeeded - must be compilable/testable]

#### Step 2: [Step Name]
...

### Phase Y: [Phase Name]

#### Step N: [Step Name]
**Goal:** [What this step should accomplish]
**Instructions:** [What to do specifically, no code]
**Verification:** [How to verify this step succeeded - must be compilable/testable]

---

## Acceptance Criteria

- [ ] [Criterion 1 - testable]
- [ ] [Criterion 2 - testable]

---

## Dependencies

**Dependent modules:** [Which existing modules are needed]
**Affected files:** [Which existing files will be modified]
**New files:** [Which new files will be created]
```

---

## Ad-hoc Feature Plan Template: feature-plan-xxxx.md

```markdown
# Feature Implementation: [Feature Name]

> Created: YYYY-MM-DD
> Status: Pending / In Progress / Completed
> Design document: feature-design-[name].md

---

## Goal

[Clearly describe what this feature should accomplish]

---

## Implementation Steps

### Step 1: [Step Name]
**Goal:** [What this step should accomplish]
**Instructions:** [What to do specifically, no code]
**Verification:** [How to verify this step succeeded - must be compilable/testable]

### Step 2: [Step Name]
...

---

## Acceptance Criteria

- [ ] [Criterion 1 - testable]
- [ ] [Criterion 2 - testable]

---

## Dependencies

**Dependent modules:** [Which existing modules are needed]
**Affected files:** [Which existing files will be modified]
**New files:** [Which new files will be created]
```
