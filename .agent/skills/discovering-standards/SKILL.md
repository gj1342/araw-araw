---
name: discovering-standards
description: Extracts coding patterns, conventions, and architectural decisions from an existing codebase and saves them as standards files. Use when the user asks to document standards, capture patterns, extract conventions, or onboard an AI agent to an existing project.
---

# Discovering Standards

## When to use this skill

- User asks to "document my standards", "capture my conventions", or "extract patterns from my code"
- Onboarding Blueprint OS to an existing project for the first time
- A recurring pattern has emerged that should be formalized
- Tribal knowledge exists in the code but not in writing
- Before brainstorming a new feature in an existing codebase — discover constraints first, then brainstorm within them

## Workflow

- [ ] Identify which area to document (ask user if unclear)
- [ ] Scan relevant files in that area
- [ ] Identify recurring patterns, naming conventions, and architectural decisions
- [ ] Draft a standards file and confirm with the user
- [ ] Save to `standards/<layer>/<category>.md` or `standards/<category>.md` for shared
- [ ] Update `standards/README.md` index if the file is new

## Instructions

### Discovery areas

Approach one area at a time. Save to the appropriate layer:

| Area | Layer | Path |
|------|-------|------|
| Tech stack | shared | `standards/tech-stack.md` |
| Folder structure | shared | `standards/folder-structure.md` |
| Naming conventions | shared | `standards/naming-conventions.md` |
| Testing approach | shared | `standards/testing-approach.md` |
| API design | backend | `standards/backend/api-design.md` |
| Data models | backend | `standards/backend/data-models.md` |
| Error handling | backend | `standards/backend/error-handling.md` |
| Authentication | backend | `standards/backend/authentication.md` |
| Component patterns | frontend | `standards/frontend/component-patterns.md` |
| State management | frontend | `standards/frontend/state-management.md` |
| Design system | design | `standards/design/design-system.md` |
| Accessibility | design | `standards/design/accessibility.md` |

### Extraction process

For each area:

1. Read 3–5 representative files
2. Identify what is consistent across them (naming, structure, patterns)
3. Note any exceptions — are they intentional or accidental?
4. Write only what the agent needs to replicate the pattern, not what is obvious

### Standards file format

Save to `standards/<layer>/<category>.md` (or `standards/<category>.md` for shared):

```markdown
# [Category] Standards

**Last updated:** [YYYY-MM-DD]

## Overview
[One paragraph: what this standard covers and why it exists]

## Conventions

### [Convention name]
[Description of the pattern]

**Example:**
[code example or file path showing the pattern]

**Avoid:**
[counter-example if useful]

## Exceptions
[Known deviations and why they exist]
```

### What to capture vs. skip

**Capture:**
- Decisions that aren't obvious from reading a single file
- Patterns that repeat across the codebase
- Choices that differ from common defaults (e.g., "we use tabs, not spaces")
- Architectural boundaries (e.g., "services never import from controllers")

**Skip:**
- Things the language or framework enforces automatically
- One-off implementations with no pattern
- Preferences with no clear rationale

### File naming

Use lowercase, hyphen-separated names. Place in the correct layer folder:

```
standards/tech-stack.md
standards/naming-conventions.md
standards/backend/api-design.md
standards/frontend/component-patterns.md
standards/design/design-system.md
```

## Resources

- Standards directory: `standards/` (shared at root, layer-specific in `backend/`, `frontend/`, `design/`)
- Standards guide: `standards/README.md`
- Next step for new feature work: `.agent/skills/brainstorming/SKILL.md` (brainstorm within discovered constraints)
- Next step before implementation: `.agent/skills/deploying-standards/SKILL.md`
