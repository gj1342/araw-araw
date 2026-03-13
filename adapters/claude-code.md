# Using Blueprint OS in Claude Code

Claude Code works with markdown files natively. Blueprint OS skills are referenced via `@file` syntax or through custom slash commands you define.

---

## Option A — @file reference (simplest)

Reference any skill directly in your prompt using the `@` file reference:

```
@.agent/skills/brainstorming/SKILL.md — brainstorm a [product or feature idea]
```

Claude Code will read the file and follow the skill's instructions for the rest of the session.

---

## Option B — Custom slash commands (recommended for daily use)

Claude Code supports custom slash commands defined in `.claude/commands/`. Create one command per skill.

**Create `.claude/commands/brainstorm.md`:**

```markdown
Read the file `.agent/skills/brainstorming/SKILL.md` and follow all instructions in it for the current task.
```

**Create `.claude/commands/shape-spec.md`:**

```markdown
Read the file `.agent/skills/shaping-specs/SKILL.md` and follow all instructions in it for the current task.
```

**Create `.claude/commands/deploy-standards.md`:**

```markdown
Read the file `.agent/skills/deploying-standards/SKILL.md` and follow all instructions in it.
```

**Create `.claude/commands/discover-standards.md`:**

```markdown
Read the file `.agent/skills/discovering-standards/SKILL.md` and follow all instructions in it.
```

**Create `.claude/commands/create-skill.md`:**

```markdown
Read the file `.agent/skills/creating-skills/SKILL.md` and follow all instructions in it.
```

**Create `.claude/commands/qa.md`:**

```markdown
Read the file `.agent/skills/quality-assurance/SKILL.md` and follow all instructions in it.
```

**Create `.claude/commands/security-audit.md`:**

```markdown
Read the file `.agent/skills/security-audit/SKILL.md` and follow all instructions in it.
```

**Create `.claude/commands/code-review.md`:**

```markdown
Read the file `.agent/skills/code-review/SKILL.md` and follow all instructions in it.
```

Then invoke them with:

```
/brainstorm — explore the idea for [product or feature]
/shape-spec — formalize the spec using specs/brainstorming/<name>/design.md
/deploy-standards — I'm about to build a REST API
/discover-standards — extract naming conventions
/create-skill — find or create a skill for database migrations
/qa — add tests for [feature]
/security-audit — audit [feature] for vulnerabilities
/code-review — review [feature] before merge
```

---

## Typical workflow in Claude Code

**New product (no codebase yet):**

```
/brainstorm — explore the idea for [product]
/shape-spec — formalize specs/brainstorming/<name>/design.md into an implementation spec
/deploy-standards — inject relevant standards for [first task]
```

**New feature in a legacy codebase:**

```
/discover-standards — extract patterns from the existing codebase
/brainstorm — explore [feature] within the existing constraints
/shape-spec — formalize specs/brainstorming/<name>/design.md
/deploy-standards — inject standards before implementation
```

**Small feature or bug fix (approach already clear):**

```
/shape-spec — plan [feature]
/deploy-standards — inject relevant standards
```

**Adding a skill:**

```
/create-skill — find or create a skill for [task]
```

---

## CLAUDE.md integration

Add a section to your project's `CLAUDE.md` to make Blueprint OS always available:

```markdown
## Blueprint OS

This project uses Blueprint OS for agent workflows.

- Skills: `.agent/skills/`
- Standards: `standards/` (shared at root; `backend/`, `frontend/`, `design/` for layer-specific)
- Specs: `specs/brainstorming/<name>/design.md`, `specs/shaped-specs/<name>/spec.md`
- References: `references/` (design docs, flowcharts, diagrams)

### Workflow

For new or complex features:
1. Run `/brainstorm` to explore the problem and options
2. Run `/shape-spec` using the brainstorm document
3. Run `/deploy-standards` to load relevant standards
4. Proceed with implementation

For small tasks where the approach is clear:
1. Run `/shape-spec` or skip straight to `/deploy-standards`
2. Proceed with implementation

Before merge (quality gates):
1. Run `/qa` to add or update tests
2. Run `/security-audit` when changes touch auth, API, or sensitive data
3. Run `/code-review` as the final gate
```

---

## Notes

- Standards files in `standards/` can be referenced with `@standards/api-design.md` or `@standards/backend/api-design.md` to include them directly in context
- Reference designs and diagrams in `references/` with `@references/checkout-flow.mmd` or `@references/agent-workflow/superpowers-link.md`
- Brainstorm documents (`specs/brainstorming/<name>/design.md`) and spec files (`specs/shaped-specs/<name>/spec.md`) persist across sessions — always reference them when resuming work
- The `@file` approach works in any Claude Code session without any setup
- For skills.sh integration, see [skills-sh.md](skills-sh.md)
