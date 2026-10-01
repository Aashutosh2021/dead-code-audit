
---

### 6. `examples/sample-audit.md`

Ek **fake/sample** report rakh sakta hai, taaki user ko output format samajh aaye:

```markdown
# Sample Dead Code Audit

> This is a fictional example. It does not represent a real project.

## Executive Summary

The audit identified:

- 3 Confirmed Dead items
- 4 Likely Dead items
- 2 Potentially Used items
- 1 Legacy implementation
- 1 Debug/Test exposure

No project files were modified.

---

## Confirmed Dead Code

### Finding

Location:

`src/utils/oldLogger.ts`

Symbol:

`legacyLogger()`

Evidence:

- No imports
- No exports consumed
- No runtime registration
- No configuration references

Risk:

Low

Confidence:

High

Recommended Action:

Remove only after final external-consumer verification.