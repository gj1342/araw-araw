---
name: deploying-standards
description: Injects relevant project standards into the agent's current context before implementation work begins. Use when the user is about to start coding, asks the agent to build something, or says "inject standards", "load standards", or "deploy standards".
---

# Deploying Standards

## When to use this skill

- User is about to start implementation on a task or feature
- User asks to "inject standards", "load context", or "deploy standards"
- Starting a new conversation with context that needs to carry forward
- After shaping a spec and before executing it

## Workflow

- [ ] Check if a spec exists for the current task in `specs/shaped-specs/<name>/spec.md`
- [ ] Read `standards/README.md` to see what standards files are available
- [ ] Read `references/README.md` to see what reference files exist
- [ ] Identify which standards and references are relevant to the current task (spec may list applicable references)
- [ ] Read those standards and reference files and internalize the content
- [ ] Confirm to the user which standards and references were loaded
- [ ] Proceed with the task using those standards and references actively applied

## Instructions

### Relevance mapping

Match the task type to the standards files most likely to apply. Load from the correct layer:

| Task type | Standards to load |
|---|---|
| Building a UI component | `frontend/component-patterns`, `naming-conventions`, `tech-stack` |
| Creating an API endpoint | `backend/api-design`, `backend/error-handling`, `naming-conventions` |
| Writing database queries | `backend/data-models`, `naming-conventions` |
| Adding authentication | `backend/authentication`, `backend/error-handling` |
| Writing tests | `testing-approach`, `naming-conventions` |
| Refactoring | `naming-conventions`, `folder-structure`, relevant domain |
| General feature | `tech-stack`, `naming-conventions`, `folder-structure` |

When in doubt, always load `tech-stack` and `naming-conventions` at minimum. Shared standards are at root; layer-specific are in `backend/`, `frontend/`, `design/`.

### Auto-suggest mode

If the user says "deploy standards" without specifying which:

1. Ask what they are about to build (one sentence)
2. Use the relevance map above to select the appropriate files
3. Load those files and confirm: "Loaded: `standards/backend/api-design.md`, `standards/naming-conventions.md`"

### Manual mode

If the user specifies standards explicitly (e.g., "inject the API standards"):

1. Locate the matching file in `standards/` (check root and `backend/`, `frontend/`, `design/` subfolders)
2. Read it and confirm it is loaded

### References

If the spec lists "Applicable references" or the task involves UI flows, architecture, or design, check `references/` and load relevant files (design docs, flowcharts, diagrams). Reference `references/README.md` for the index.

### After loading

State which standards and references were loaded, then proceed with the task. Keep standards active throughout the session — do not ignore them mid-task.

If a decision during execution conflicts with a loaded standard, surface the conflict explicitly:

```
This would normally follow [standard X], but [situation]. Do you want me to follow the standard or make an exception here?
```

### Standards not found

If `standards/` is empty or does not exist:

```
No standards files found. To create standards for this project, use the discovering-standards skill.
```

## Resources

- Standards directory: `standards/` (shared at root; `backend/`, `frontend/`, `design/` for layer-specific)
- References directory: `references/`
- Discover new standards: `.agent/skills/discovering-standards/SKILL.md`
- Shape a spec first: `.agent/skills/shaping-specs/SKILL.md`
