---
name: vibe-hunt
description: "Use when encountering bugs, errors, unexpected behavior, or failing tests. Diagnoses root cause before applying any fix. Not for code review or new features."
---

# Vibe Hunt

## Overview

Diagnosis-driven debugging skill. Patching symptoms only creates new bugs elsewhere.

**Do not touch code until you can state the root cause in one sentence:**
> "The root cause is [X], evidence is [Y]."

You must point to a specific file, function, line number, or condition. "A state management issue" is unverifiable. "The `useUser` cache at `src/hooks/user.ts:42` is stale because the dependency array is missing `userId`" is verifiable. If you can't be this specific, you don't have a hypothesis yet.

---

## When to Use

**Use cases:**
- Bugs, errors, crashes, unexpected behavior
- Test failures
- Development stuck, multiple attempts with no progress

**Not for:**
- Code review (use /vibe-review)
- New feature development
- Design discussions

---

## Rationalization Monitoring

When these thoughts appear, stop and reassess:

| What you're thinking | What it actually means | Rule |
|---------------------|----------------------|------|
| "Let me just try this" | No hypothesis, random walk | Stop. Write a hypothesis first. |
| "I'm sure it's X" | Confidence is not evidence | Run a tool that proves it. |
| "Probably the same as last time" | Treating new symptoms as a known pattern | Re-read the execution path from scratch. |
| "Works on my machine" | Environment differences ARE the bug | List every environment difference before judging. |
| "One more restart should fix it" | Avoiding the error message | Read the last error verbatim. No more than two restarts without new evidence. |
| "Finally, a different error!" | After 1h of random changes, new symptoms are coincidence, not progress | If Level 3 threshold is reached, save state and start fresh regardless of recent changes. |

---

## Progress Signals

When these appear, the diagnosis is on track:

| What you're thinking | It means | Next step |
|---------------------|----------|-----------|
| "This log line matches my hypothesis" | Found positive evidence | Find one more independent piece of evidence to cross-validate |
| "I can predict the next error" | Mental model is forming | Run the prediction; if it matches, the model is correct |
| "Root cause is at A but symptoms appear at B" | Propagation path is understood | Trace the A→B call chain, confirm each link |
| "I can write a test that fails on the old code" | Hypothesis is specific and testable | Write the test before fixing |

Without observable evidence matching any of these signals, you cannot claim progress.

---

## Hard Rules

- **Report findings before fixing.** Output a diagnostic report with root cause, affected files, and proposed fix. Wait for user confirmation before touching any code.
- **Same symptom after fix = hard stop.** The previous hypothesis was wrong. Re-read the execution path from scratch.
- **After three failed hypotheses, stop.** Report to user: what was checked, what was ruled out, what is unknown, ask how to proceed.
- **Do not state environment details from memory.** Run detection commands first (`node --version`, `rustc --version`, etc.), state actual output.
- **External tool failure: diagnose before switching.** When an MCP tool or API fails, determine the cause first (service running? key valid? config correct?), then try alternatives.
- **Watch for avoidance signals.** When someone says "that part doesn't matter," treat it as a clue. Avoided areas are often where the problem lies.
- **Fix the cause, not the symptom.** If the fix involves more than 5 files, pause and confirm scope with the user.
- **Grep before touching code.** Before modifying any file or calling any identifier, grep to confirm it exists at the expected location. No results = re-check execution path, do not edit from memory.

---

## Stuck Escalation Strategy

When the standard diagnostic process stalls, escalate progressively:

### Level 1: Hypothesis Failure (15min+ on one angle)
- Same symptom persists after fix → re-read execution path from scratch
- Switch analysis angle: logs, call chains, state tracing

### Level 2: 3 Hypothesis Failures (30min+ total)
- Execute `/rewind` to roll back session
- Deep analysis: code structure, how it works, key components
- Report known/ruled out/unknown to user

### Level 3: Severely Stuck (1h+ total debugging time)
The 1h threshold is absolute — it measures total time spent, not time since last "signal." After 1 hour of debugging, your mental model is unreliable. A "new error" after a series of random changes is coincidental variation, not progress.

- First execute `git stash` to save current changes
- Use AskUserQuestion to confirm whether to `git reset --hard` (note: stash has saved changes)
- Start a new `/new` session
- Global analysis: architecture, module breakdown, cascade effect assessment
- Build mental model from scratch

---

## Verify or Refute

Add a targeted detection tool: a log line, a failing assertion, or a minimal test that fails if the hypothesis is correct. Run it. Evidence contradicts hypothesis → completely overthrow the hypothesis, reposition with what was just learned. Do not retain hypotheses overturned by evidence.

---

## Diagnostic Report (required before any code change)

Once root cause is identified, output this report and stop:

```
Root cause:    [what went wrong, file:line]
Evidence:      [what proves this is the root cause]
Affected:      [files that will be modified]
Proposed fix:  [what to change and why]
Risk:          [what else could break from this fix]
```

**Stop.** Wait for user to confirm before making any changes.

User may: approve the fix, adjust scope, request more diagnosis, or propose an alternative.

---

## Fix Result (output after fix is applied)

```
Root cause:  [what went wrong, file:line]
Fix:         [what was changed, file:line]
Confirmed:   [evidence or test proving the fix]
Tests:       [pass/fail counts, regression test locations]
```

Status: **resolved** / **resolved with caveats** (explain) / **blocked** (explain unknowns).

---

## Common Mistakes

| Mistake | Consequence | Correct approach |
|---------|-------------|------------------|
| Change code without a hypothesis | Introduces new bugs | State hypothesis first, then act |
| Symptom persists after fix | Getting worse with each change | Stop and re-read execution path |
| State version numbers from memory | Wrong diagnosis direction | Run detection commands, read actual output |
| Restart multiple times without reading errors | Wasted time | Read the last error message verbatim; no more than two restarts without new evidence |
