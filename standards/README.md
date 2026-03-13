# Standards

This folder contains your project's documented coding standards. Each file captures patterns and conventions from your actual codebase so AI agents can replicate them consistently.

---

## What goes here

Standards files are extracted from your codebase — not invented. They document what you already do, not what you wish you did.

**A good standards file answers:**
- What pattern is used here?
- Why was this decision made?
- What does it look like in practice?
- What should be avoided?

---

## Structure

Standards are organized by layer. Shared standards (cross-cutting) live at root; layer-specific standards go in subfolders:

```
standards/
├── tech-stack.md           # shared
├── naming-conventions.md   # shared
├── folder-structure.md     # shared
├── testing-approach.md    # shared
├── backend/
│   ├── api-design.md
│   ├── data-models.md
│   ├── error-handling.md
│   └── authentication.md
├── frontend/
│   ├── component-patterns.md
│   └── state-management.md
└── design/
    ├── design-system.md
    └── accessibility.md
```

**Backend** — API, data, auth, server-side patterns.  
**Frontend** — Components, state, client-side patterns.  
**Design** — UI system, accessibility, visual conventions.  
**Root** — Tech stack, naming, folder structure, testing (apply across layers).

Use lowercase, hyphen-separated names. Keep one category per file.

---

## File format

Each standards file follows this structure:

```markdown
# [Category] Standards

**Last updated:** YYYY-MM-DD

## Overview
One paragraph describing what this standard covers and why it matters.

## Conventions

### [Convention name]
Description of the pattern.

**Example:**
[code example or path to a representative file]

**Avoid:**
[counter-example, if useful]

## Exceptions
Known deviations from the standard and the reason they exist.
```

---

## Standards index

When you add a new standards file, add it to the table below so agents can discover it. Group by layer (shared, backend, frontend, design):

| File | Covers |
|---|---|
| *(none yet — run the `discovering-standards` skill to create your first one)* | |

---

## How to create standards

Use the `discovering-standards` skill:

```
Read .agent/skills/discovering-standards/SKILL.md and extract standards for [area]
```

The skill will scan your codebase, identify patterns, draft a standards file, and save it here.

---

## How to use standards

**Before brainstorming a new feature** (existing codebase):

```
Read .agent/skills/brainstorming/SKILL.md and brainstorm [feature] with the existing standards loaded
```

The brainstorming skill will load relevant standards files as constraints, so ideas stay grounded in the existing architecture.

**Before implementation** (after a spec is ready):

```
Read .agent/skills/deploying-standards/SKILL.md and inject relevant standards for [task]
```

The agent will read the appropriate files from this folder and apply them throughout the session.

---

## Related

For design docs, flowcharts, and diagrams that guide implementation, see `references/README.md`. Standards document patterns from your codebase; references are inputs (designs, flows) that define what to build.

---

## Maintenance

- Update standards files when patterns change in the codebase
- Delete or archive standards that no longer apply
- Add an **Exceptions** section rather than removing a standard when a one-off deviation is needed
- Standards should reflect reality, not aspiration
