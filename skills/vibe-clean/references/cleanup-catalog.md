# Cleanup Agent Activation Catalog

The controller reads target scope and determines depth, then activates agents based on this catalog.

## Always Active

The controller always runs basic structure analysis (unused imports, console.logs, commented-out code) before dispatching subagents.

## Conditional Agents

### Simplification Analyzer

**Agent file:** `agents/simplification-analyzer.md`
**Activation depth:** All levels (Surface, Standard, Deep)

Always activated. This is the baseline cleanup agent.

### Dead Code Analyzer

**Agent file:** `agents/dead-code-analyzer.md`
**Activation depth:** Standard or Deep

Activate when:
- Target scope includes > 5 files
- Project uses a language with good dead-code detection tools (TypeScript, JavaScript, Python, Go)
- User explicitly requests dead code removal

Do not activate: < 5 files or surface-only cleanup.

### Duplicate Analyzer

**Agent file:** `agents/duplicate-analyzer.md`
**Activation depth:** Deep only

Activate when:
- Target scope includes > 15 files
- Codebase has been maintained by multiple contributors
- User explicitly requests duplicate consolidation

Do not activate: Surface or Standard depth, or single-module scope.

## Dispatch Rules

- Agents are dispatched in parallel when multiple are activated
- Each agent receives the same target scope
- Controller consolidates and deduplicates findings across agents
