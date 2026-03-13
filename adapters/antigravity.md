# Using Blueprint OS in Antigravity

Blueprint OS uses the `.agent/skills/` structure natively — the same format Antigravity expects. When installing from skills.sh, use `-a antigravity` so skills land here. No extra setup required.

---

## Setup

Copy the `.agent/` folder into the root of your project:

```
your-project/
├── .agent/
│   └── skills/
│       ├── brainstorming/
│       │   └── SKILL.md
│       ├── creating-skills/
│       │   └── SKILL.md
│       ├── shaping-specs/
│       │   └── SKILL.md
│       ├── discovering-standards/
│       │   └── SKILL.md
│       ├── deploying-standards/
│       │   └── SKILL.md
│       ├── quality-assurance/
│       │   └── SKILL.md
│       ├── security-audit/
│       │   └── SKILL.md
│       └── code-review/
│           └── SKILL.md
├── specs/
│   ├── brainstorming/<name>/design.md
│   └── shaped-specs/<name>/spec.md (+ quality-assurance.md, security-audit.md, code-review.md when run)
├── standards/
│   ├── backend/
│   ├── frontend/
│   └── design/
├── references/
└── ... your code
```

Antigravity will automatically detect skills in `.agent/skills/` and make them available to the agent.

---

## Triggering skills

Antigravity picks up skills by their `name` field (defined in YAML frontmatter). Trigger them naturally:

| What you say | Skill triggered |
|---|---|
| "Brainstorm [idea]" / "Think through [feature]" / "Explore [problem]" | `brainstorming` |
| "Create a skill for X" / "Find a skill for X" | `creating-skills` |
| "Shape a spec for X" / "Plan this feature" | `shaping-specs` |
| "Document my standards" / "Extract patterns" | `discovering-standards` |
| "Deploy standards" / "Inject context for X" | `deploying-standards` |
| "Add tests" / "QA" / "Validate acceptance criteria" | `quality-assurance` |
| "Security audit" / "SEC" / "Audit for vulnerabilities" | `security-audit` |
| "Review" / "REV" / "Final review" / "Ready for merge" | `code-review` |

---

## Typical workflow in Antigravity

**New product (no codebase yet):**

```
Brainstorm a [product idea] — I want to explore the problem and options before writing any code.
```

Then:

```
Shape a spec using the brainstorm document in specs/brainstorming/<name>/design.md.
```

**New feature in a legacy codebase:**

```
Document the standards for my codebase — start with the tech stack and naming conventions.
```

Then:

```
Brainstorm [feature] with the discovered standards loaded as constraints.
```

Then:

```
Shape a spec using the brainstorm document.
```

**Small feature or bug fix (approach already clear):**

```
Shape a spec for [feature].
```

**Before coding:**

```
Deploy standards relevant to building [task].
```

**After execution (quality gates):**

```
Add tests for [feature].
```

Then, if the change touches auth, API, or sensitive data:

```
Security audit for [feature].
```

Before merge:

```
Review [feature] before merge.
```

**Adding a skill:**

```
Find or create a skill for [task].
```

---

## Adding your own skills

Use the `creating-skills` skill — it searches skills.sh first, then creates from scratch if nothing suitable exists:

```
Find or create a skill for [task]
```

The agent will check [skills.sh](https://skills.sh) first (`npx skills add https://github.com/vercel-labs/skills --skill find-skills -a antigravity -y --copy`), install a community skill if available, or create a new one following `.agent/skills/creating-skills/SKILL.md`.

---

## Notes

- The `description` field in each `SKILL.md` controls when Antigravity auto-triggers a skill — keep it specific with clear keywords
- Brainstorm documents are saved to `specs/brainstorming/<name>/design.md` — reference them when shaping the spec
- Standards files in `standards/` (root for shared, `backend/`, `frontend/`, `design/` for layer-specific) are plain markdown — reference them in your prompts or let the `deploying-standards` skill load them automatically
- Reference designs and diagrams in `references/` — the spec lists applicable references; `deploying-standards` loads them before implementation
- Spec files are saved to `specs/shaped-specs/<name>/spec.md` — reference them in subsequent sessions to maintain continuity
- For skills.sh integration, see [skills-sh.md](skills-sh.md)
