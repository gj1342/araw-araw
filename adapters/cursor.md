# Using Blueprint OS in Cursor

Cursor reads rules from `.cursor/rules/` and skills from a skills directory. Blueprint OS skills live in `.agent/skills/` (singular) as portable markdown files. Cursor's native skills path is `.agents/skills/` (plural) — Blueprint OS uses `.agent/` and connects via rules and `@` references. Do not rename `.agent` to `.agents`; keep the Blueprint OS layout. This guide shows how to connect them.

---

## Option A — Reference skills via Cursor Rules (recommended)

Create a rule file that tells Cursor's agent where to find Blueprint OS skills.

**Create `.cursor/rules/blueprint-os.mdc`:**

```markdown
---
description: Blueprint OS skill loader. Apply when the user mentions a skill or asks to brainstorm, shape a spec, discover standards, deploy standards, create a skill, add tests, security audit, or code review.
globs:
alwaysApply: false
---

# Blueprint OS

Blueprint OS skills are located in `.agent/skills/`. When the user invokes a skill, read the corresponding `SKILL.md` and follow its instructions.

## Available skills

- **Find skills** → `.agent/skills/find-skills/SKILL.md` (pre-installed)
- **Brainstorming** → `.agent/skills/brainstorming/SKILL.md`
- **Creating skills** → `.agent/skills/creating-skills/SKILL.md`
- **Shaping specs** → `.agent/skills/shaping-specs/SKILL.md`
- **Discovering standards** → `.agent/skills/discovering-standards/SKILL.md`
- **Deploying standards** → `.agent/skills/deploying-standards/SKILL.md`
- **Quality assurance** → `.agent/skills/quality-assurance/SKILL.md`
- **Security audit** → `.agent/skills/security-audit/SKILL.md`
- **Code review** → `.agent/skills/code-review/SKILL.md`

## How to trigger

Say: "Use the [skill-name] skill" or "Read the [skill-name] SKILL.md and follow it."
```

---

## Option B — Use Cursor Skills (`.cursor/skills/`)

If you have a Cursor skills directory set up, you can create stub skills that reference the Blueprint OS `SKILL.md` files.

**Create `.cursor/skills/brainstorming/SKILL.md`:**

```markdown
---
name: brainstorming
description: Explores a problem through Socratic questioning before any spec or code is written. Delegates to Blueprint OS. Use when the user wants to think through a new product or feature.
---

# Brainstorming

Read `.agent/skills/brainstorming/SKILL.md` and follow all instructions there.
```

Repeat this pattern for each Blueprint OS skill. The stub keeps your Cursor skills directory tidy while the actual logic lives in the portable `.agent/skills/` files.

---

## Option C — Direct file reference

In any Cursor chat, reference a skill directly with `@`:

```
@.agent/skills/brainstorming/SKILL.md — brainstorm [product or feature idea]
```

Cursor will include the file content in context and the agent will follow the skill's instructions.

---

## Typical workflow in Cursor

**New product (no codebase yet):**

```
@.agent/skills/brainstorming/SKILL.md — brainstorm [product idea]
@.agent/skills/shaping-specs/SKILL.md — shape a spec using specs/brainstorming/<name>/design.md
@.agent/skills/deploying-standards/SKILL.md — inject standards for [task]
```

**New feature in an existing codebase:**

```
@.agent/skills/discovering-standards/SKILL.md — document [area] standards
@.agent/skills/brainstorming/SKILL.md — brainstorm [feature] with existing standards loaded
@.agent/skills/shaping-specs/SKILL.md — shape a spec using specs/brainstorming/<name>/design.md
@.agent/skills/deploying-standards/SKILL.md — inject standards for [task]
```

**Small feature or bug fix (approach already clear):**

```
@.agent/skills/shaping-specs/SKILL.md — shape a spec for [feature]
@.agent/skills/deploying-standards/SKILL.md — inject relevant standards for [task]
```

**Extracting patterns from existing code:**

```
@.agent/skills/discovering-standards/SKILL.md — document the API design standards
```

**Adding a new skill:**

```
@.agent/skills/creating-skills/SKILL.md — find or create a skill for [task]
```

**Before merge (quality gates):**

```
@.agent/skills/quality-assurance/SKILL.md — add tests for [feature]
@.agent/skills/security-audit/SKILL.md — audit [feature] for vulnerabilities
@.agent/skills/code-review/SKILL.md — review [feature] before merge
```

Run QA after implementation. Run SEC when changes touch auth, API, or sensitive data. Run REV as the final gate.

---

## Notes

- Standards files in `standards/` can be referenced with `@standards/tech-stack.md` or `@standards/backend/api-design.md`
- Reference designs and diagrams in `references/` with `@references/checkout-flow.mmd` or `@references/agent-workflow/superpowers-link.md`
- Brainstorm documents (`specs/brainstorming/<name>/design.md`) and spec files (`specs/shaped-specs/<name>/spec.md`) are readable the same way
- The `.agent/` folder is invisible to most file trees by default — open it explicitly if needed
- For skills.sh integration, see [skills-sh.md](skills-sh.md). Use `-a antigravity -y --copy` when installing so skills land in `.agent/skills/` as real files. Without `--copy`, the CLI may symlink from `.agents/`, and deleting `.agents` breaks the skill.
