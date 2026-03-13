---
name: code-review
description: Validates changes against the spec and standards before merge. Use when the user asks to review, REV, perform final review, or confirm ready for merge. Ensures scope alignment, completeness, and that security-audit was run when required.
---

# Code Review

## When to use this skill

- User asks to "review", "REV", "final review", or "ready for merge"
- After quality-assurance and security-audit (when applicable)
- Before merging a feature or significant change

## Workflow

- [ ] Load the spec from `specs/shaped-specs/<name>/spec.md`
- [ ] Load relevant standards from `standards/`
- [ ] Review changed code against the spec
- [ ] Verify scope alignment — no out-of-scope changes, no missing in-scope items
- [ ] Confirm security-audit was run if auth/API/sensitive data changed
- [ ] Check documentation and spec status are updated
- [ ] Save review to `specs/shaped-specs/<name>/code-review.md`

## Instructions

### Scope validation

- Every success criterion in the spec is addressed (implemented or explicitly deferred)
- No changes fall outside the spec's in-scope items
- Out-of-scope items were not introduced without spec update

### Standards compliance

- Code follows patterns in `standards/` (naming, structure, tech stack)
- Exceptions are documented or justified

### Security gate

If the change touches authentication, API endpoints, or sensitive data:

- Security-audit must have been run
- No Critical or High findings remain open
- If audit was skipped, block merge and recommend running `security-audit`

### Documentation

- Spec status reflects current state (e.g., In Progress → Done)
- New modules or APIs are documented per project conventions
- Test evidence or coverage noted where applicable

### Review output

**Pass:** Confirm readiness for merge. Summarize what was reviewed and any minor notes.

**Block:** List blocking issues with file/line references and required fixes. Do not recommend merge until resolved.

### Output format

Save to `specs/shaped-specs/<name>/code-review.md`:

```markdown
## Code review — [feature]

**Result:** Pass / Block

### Scope
- [x] All success criteria addressed
- [x] No out-of-scope changes

### Security
- [x] Security-audit run (required for auth/API/sensitive)
- [x] No Critical/High findings

### Notes
[Any observations or recommendations]
```

## Resources

- Spec: `specs/shaped-specs/<name>/spec.md`
- Output: `specs/shaped-specs/<name>/code-review.md`
- Standards: `standards/`
- Security audit: `.agent/skills/security-audit/SKILL.md` (run before review when applicable)
