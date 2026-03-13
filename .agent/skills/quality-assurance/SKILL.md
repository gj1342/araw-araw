---
name: quality-assurance
description: Adds or updates tests to validate behavior against acceptance criteria. Use when the user asks to add tests, run QA, validate acceptance criteria, or ensure test coverage after implementation. Applies to frontend and backend code.
---

# Quality Assurance

## When to use this skill

- Implementation is complete and tests need to be added or updated
- User asks to "add tests", "QA", "validate acceptance criteria", or "test coverage"
- Spec has success criteria that require test evidence
- Before merge when quality gates are in use

## Workflow

- [ ] Load the spec for the current task from `specs/shaped-specs/<name>/spec.md`
- [ ] Identify acceptance criteria and success criteria from the spec
- [ ] Load `standards/testing-approach.md` if it exists (shared at root)
- [ ] Add or update tests to cover the acceptance criteria
- [ ] Save test evidence to `specs/shaped-specs/<name>/quality-assurance.md`
- [ ] Flag auth, API, or sensitive data changes for security-audit if applicable

## Instructions

### Test scope by layer

| Layer | Test types | Examples |
|-------|------------|----------|
| Frontend | Unit, integration, E2E | Components, hooks, user flows |
| Backend | Unit, integration, API | Services, handlers, database |
| Shared | Unit | Utilities, validation, data transforms |

Follow project conventions in `standards/testing-approach.md` when present (shared at root). If no testing standard exists, use common patterns for the stack (e.g., Vitest/Jest for unit, Playwright/Cypress for E2E).

### Acceptance criteria mapping

For each unchecked success criterion in the spec:

1. Determine the test type needed (unit, integration, E2E)
2. Write the test
3. Run the test and confirm it passes
4. Update the spec to mark the criterion as verified or add a brief evidence note

### Evidence recording

Save to `specs/shaped-specs/<name>/quality-assurance.md`:

```markdown
## Quality assurance — [feature]

**Date:** [YYYY-MM-DD]

### Test evidence
- [Criterion X]: Covered by `path/to/test.ts` — passes
- [Criterion Y]: Covered by integration test in `path/to/integration.test.ts`
```

### Security-sensitive changes

If the implementation touches authentication, API security, or sensitive data handling, note that the `security-audit` skill should be run before merge.

## Handoff

- If auth/API/sensitive data: recommend running `security-audit` next
- Otherwise: recommend running `code-review` before merge

## Resources

- Spec: `specs/shaped-specs/<name>/spec.md`
- Output: `specs/shaped-specs/<name>/quality-assurance.md`
- Testing standards: `standards/testing-approach.md` (shared)
- Next step: `.agent/skills/security-audit/SKILL.md` (for auth/API/sensitive) or `.agent/skills/code-review/SKILL.md`
