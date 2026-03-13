# Using skills.sh with Blueprint OS

[skills.sh](https://skills.sh) is an open registry of reusable agent skills. Blueprint OS uses `.agent/skills/` (singular). The skills CLI installs to different paths per agent — use the Antigravity target so skills land in `.agent/skills/` and work with Blueprint OS.

---

## Paths: `.agent` vs `.agents`

The skills CLI uses agent-specific paths. Cursor, Codex, and other "Universal" agents use `.agents/skills/` (plural). Antigravity uses `.agent/skills/` (singular). Blueprint OS uses `.agent/skills/` for all adapters — Cursor reads from there via rules and `@` references, not from `.agents`. Always target Antigravity when installing so skills land in the correct folder.

---

## Symlinks: use `--copy` for Cursor

The CLI defaults to symlinks. One location holds the canonical copy; others point to it. If Cursor is detected, the canonical copy may live in `.agents/`. Deleting `.agents` then breaks the skill in `.agent/`. Use `--copy` so `.agent/skills/` gets real files. You can safely remove `.agents` if the CLI creates it.

---

## How it works

```bash
npx skills add <owner/repo> -a antigravity -y --copy
```

The `-a antigravity` flag installs to `.agent/skills/`, matching Blueprint OS. The `--copy` flag creates real files (not symlinks) so deleting `.agents` won't break the skill. The `-y` flag skips prompts. Installed skills are immediately available via `@.agent/skills/<skill-name>/SKILL.md`.

Skills.sh supports Cursor, Antigravity, Claude Code, Codex, Cline, Windsurf, and more — the same tools Blueprint OS targets.

---

## Discovering skills

**Option 1 — Use the find-skills skill:**

```bash
npx skills add https://github.com/vercel-labs/skills --skill find-skills -a antigravity -y --copy
```

This installs a meta-skill that can search the registry for you. Then ask your agent:

```
Use the find-skills skill to find a skill for [task]
```

**Option 2 — Browse the registry directly:**

Visit [skills.sh](https://skills.sh) and search by keyword or browse by category.

**Option 3 — Search by GitHub org:**

Many popular tool maintainers publish official skills. Append `-a antigravity -y` for Blueprint OS:

| Publisher | Install command | What's inside |
|---|---|---|
| Vercel | `npx skills add vercel-labs/agent-skills` | React, Next.js, Vercel deployment |
| Supabase | `npx skills add supabase/agent-skills` | Postgres, auth, storage best practices |
| Anthropic | `npx skills add anthropics/skills` | PDF, PPTX, DOCX, frontend design, MCP |
| Expo | `npx skills add expo/skills` | React Native, EAS, native APIs |
| Obra | `npx skills add obra/superpowers` | Planning, debugging, git worktrees, subagents |
| wshobson | `npx skills add wshobson/agents` | API design, testing, architecture patterns |

---

## Installing skills

```bash
# Single repo (may contain multiple skills)
npx skills add supabase/agent-skills -a antigravity -y --copy

# Install a specific skill by name
npx skills add obra/superpowers --skill systematic-debugging -a antigravity -y --copy
```

Skills land in `.agent/skills/<skill-name>/SKILL.md`. Open them to inspect what was installed.

---

## Evaluating a skill after install

After installing, read the skill's `SKILL.md` and ask:

1. Does the workflow match your needs exactly? → Use as-is
2. Does it cover 70–80% of your need? → Customize it
3. Does it cover a completely different use case? → Delete it and build from scratch

---

## Customizing an installed skill

Open `.agent/skills/<skill-name>/SKILL.md` and:

- **Keep** the core workflow steps and any low-freedom (exact command) sections
- **Update** paths, naming conventions, and tool references to match your project
- **Add** a `## Project context` section at the bottom with project-specific notes
- **Remove** sections that don't apply to your stack

When your changes are substantial, rename the folder to distinguish your fork from upstream:

```
.agent/skills/supabase-postgres/     ← upstream name
.agent/skills/postgres-migrations/   ← your customized version
```

---

## Publishing your own skills

If you've built a Blueprint OS skill that others would benefit from, you can publish it to skills.sh.

**Requirements:**
- Skill must be in a public GitHub repository
- Must follow the standard `SKILL.md` format with valid YAML frontmatter
- `name` must be unique and in gerund form
- `description` must be written in third person with clear triggers

**Publishing steps:**

1. Push your `.agent/skills/<skill-name>/` folder to a public GitHub repo
2. Submit your skill at [skills.sh](https://skills.sh) (follow the site's submission process)
3. Others can then install it with `npx skills add <your-github-username>/<repo> -a antigravity -y --copy`

**Tip:** Structure your repo so each skill is its own folder at the root — that's the convention the `npx skills add` command expects.

---

## Recommended skills for Blueprint OS workflows

These community skills complement Blueprint OS directly:

| Skill | Install | Pairs with |
|---|---|---|
| `systematic-debugging` | `npx skills add obra/superpowers -a antigravity -y --copy` | Any execution phase |
| `writing-plans` | `npx skills add obra/superpowers -a antigravity -y --copy` | `shaping-specs` |
| `executing-plans` | `npx skills add obra/superpowers -a antigravity -y --copy` | After `deploying-standards` |
| `requesting-code-review` | `npx skills add obra/superpowers -a antigravity -y --copy` | After execution |
| `api-design-principles` | `npx skills add wshobson/agents -a antigravity -y --copy` | `discovering-standards` for API projects |
| `test-driven-development` | `npx skills add obra/superpowers -a antigravity -y --copy` | Before writing new features |

---

## Notes

- skills.sh skills use the same `SKILL.md` format as Blueprint OS — no conversion needed
- Always use `-a antigravity -y --copy` when installing. `-a antigravity` puts skills in `.agent/skills/`. `--copy` creates real files (not symlinks) so deleting `.agents` won't break the skill. Without it, the CLI may symlink from `.agents/`, and removing `.agents` breaks `.agent/`
- If `npx skills add` is not available, clone the repo and copy the skill folder manually into `.agent/skills/`
- Community skills may not follow all Blueprint OS conventions — review before use
- Pin to a specific commit if you need a stable, reproducible skill version
