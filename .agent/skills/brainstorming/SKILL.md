---
name: brainstorming
description: Explores a problem through Socratic questioning before any spec or code is written. Surfaces real needs, compares approaches, and produces a design document. Use when the user wants to think through a new product, a new feature, a technical decision, or anything where the right approach is still undecided.
---

# Brainstorming

## When to use this skill

- User says "I want to build X" but hasn't decided how yet
- Starting a brand new product with no codebase yet
- Adding a significant feature to an existing codebase
- A technical or architectural decision needs exploration before committing
- The approach is unclear or multiple valid paths exist
- User asks to "brainstorm", "think through", "explore", or "figure out" something

## Workflow

- [ ] Identify which mode applies (see Instructions > Modes)
- [ ] Load relevant context for the mode (standards, existing files, or nothing)
- [ ] Ask the brainstorming questions — one section at a time, conversationally
- [ ] Present 2–3 alternative approaches with tradeoffs after gathering answers
- [ ] Validate the preferred direction with the user
- [ ] Save the design document to `specs/brainstorming/<name>/design.md`
- [ ] Hand off to `shaping-specs` with the design document as input

## Instructions

### Modes

Pick the mode based on what the user is starting from:

**Mode 1 — New product**
No codebase exists yet. Start with open-ended exploration. No standards to load. Focus on the problem, the user, and the product vision before any technical decisions.

**Mode 2 — New feature in a legacy codebase**
A codebase exists. Load `standards/` files (root and backend/, frontend/, design/) first to understand existing patterns and constraints. Brainstorm within those constraints — what fits the existing architecture, what would require deviation and why.

**Mode 3 — New feature with loaded context**
The user has specific files or modules in mind. Load those files before brainstorming. Keep exploration scoped to the relevant area.

### Brainstorming questions

Ask these in a natural, conversational flow — one section at a time. Do not ask all at once. Wait for responses before moving on.

**1. What is the real problem?**
- What are you actually trying to solve? (not the solution — the problem)
- Who experiences this problem and when?
- What happens today without this? What's the workaround?

**2. What does success look like?**
- If this works perfectly, what changes for the user?
- How would you know it worked? What's measurable?
- What would make this a failure even if it ships?

**3. What are the constraints?**
- Are there technical boundaries (existing stack, performance, compatibility)?
- Are there product constraints (scope, timeline, dependencies)?
- What can we NOT change, no matter what?

**4. What approaches could work?**
- What's the most obvious solution? What are its downsides?
- Is there a simpler version that solves 80% of the problem?
- Is there an existing tool, library, or pattern that does this?

**5. What are the risks?**
- What could go wrong during implementation?
- What edge cases will be hardest to handle?
- What assumption, if wrong, would invalidate the whole approach?

### Presenting alternatives

After gathering answers, present 2–3 distinct approaches:

```
## Approach A: [Name]
[One paragraph description]

Pros:
- [Pro 1]
- [Pro 2]

Cons:
- [Con 1]
- [Con 2]

Best if: [condition where this wins]

---

## Approach B: [Name]
...
```

Ask the user which direction resonates before writing the design document.

### Design document format

Save to `specs/brainstorming/<name>/design.md`:

```markdown
# Brainstorm: [Topic]

**Date:** [YYYY-MM-DD]
**Mode:** New product | Legacy codebase | Feature in context
**Chosen approach:** [Approach name]

## The problem
[What we're actually solving and for whom]

## Success criteria
- [What done looks like]

## Constraints
- [Technical and product constraints]

## Approaches considered
### [Approach A]
[Summary + why it was not chosen, or why it was chosen]

### [Approach B]
[Summary + tradeoffs]

## Chosen direction
[Rationale for the selected approach]

## Open questions
- [Anything still unresolved that shaping-specs needs to address]

## Context loaded
- `standards/[layer/][file].md` — [what was relevant]
- `path/to/file` — [why it was included]
```

### Hand-off to shaping-specs

After saving the design document, tell the user:

```
Brainstorm complete. Load specs/brainstorming/<name>/design.md and run the shaping-specs skill to formalize this into an implementation spec.
```

The `shaping-specs` skill will use the design document as its starting context, so the shaping questions can be answered faster and with less ambiguity.

### What to avoid

- Do not propose implementation details during brainstorming — that belongs in the spec
- Do not skip to a single approach without exploring alternatives
- Do not skip the problem questions and jump to solutions
- Do not ask all questions at once — keep the conversation flowing

## Resources

- Design document output: `specs/brainstorming/<name>/design.md`
- Next step: `.agent/skills/shaping-specs/SKILL.md`
- Existing standards (Mode 2): `standards/README.md`
- Superpowers brainstorming reference: `npx skills add obra/superpowers -a antigravity -y --copy`
