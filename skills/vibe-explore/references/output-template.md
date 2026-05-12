# Output Template

Output format for vibe-explore documents.

---

```markdown
# [Title]

**Date:** YYYY-MM-DD
**Depth:** surface / standard / deep

## Summary

[2-3 sentence overview of what this document covers and why it exists.]

## [Section 1]

[Main content organized by subtopic (research findings) or chapter (documentation).]

- Key point with source reference [^1]
- Key point with code example:

\```js
// concrete, runnable example
\```

## [Section 2]

[Continue organizing content by subtopic or chapter.]

## Sources

### External sources (if applicable)

[^1]: [Source title](URL) — Key takeaway in one sentence
[^2]: [Source title](URL) — Key takeaway in one sentence

### Local sources (if applicable)

| File / Module | What was analyzed |
|---------------|-------------------|
| `src/module-a/index.ts` | Public API surface |
| `src/module-b/config.ts` | Configuration options |

## Open Questions

[What remains unclear. Omit section if none.]

- [Question 1]
- [Question 2]
```

---

## Notes

- Sections organize content by topic — the same document can mix external research findings with local codebase analysis
- Sources section adapts to what was actually used: external URLs, local files, or both
- No placeholders (TBD/TODO) — every section must have real content
- Concrete code examples over abstract descriptions
