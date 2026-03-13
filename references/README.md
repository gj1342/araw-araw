# References

This folder holds design documents, flowcharts, diagrams, and other reference materials the AI workflow uses to understand what to build. Unlike standards (which document patterns from your codebase), references are inputs — designs, flows, and specs that guide implementation.

---

## What goes here

- **Reference designs** — UI mockups, wireframes, design specs
- **Flowcharts** — Process flows, user journeys (Mermaid, PNG, etc.)
- **Architecture diagrams** — System diagrams, sequence diagrams
- **External links** — URLs to design tools, Figma, Notion, etc.

Organize by domain or type as needed: `references/frontend/`, `references/backend/`, `references/diagrams/`. The `agent-workflow/` subfolder holds meta-references for the Blueprint OS framework itself (links to agent-os, superpowers, skill creators).

---

## File naming

Use lowercase, hyphen-separated names:

```
references/
├── agent-workflow/         # Meta: Blueprint OS framework links
│   ├── agent-os-link.md
│   ├── superpowers-link.md
│   ├── Skills.md
│   └── blast.md
├── checkout-flow.mmd
├── auth-sequence.png
├── component-hierarchy.md
└── design-system-link.md
```

---

## References index

When you add a new reference, add it to the table below so agents can discover it:

| File | Covers |
|---|---|
| `agent-workflow/agent-os-link.md` | Link to agent-os |
| `agent-workflow/superpowers-link.md` | Link to superpowers |
| `agent-workflow/Skills.md` | Skill creator reference |
| `agent-workflow/blast.md` | Skill creator reference |

---

## How the workflow uses references

**During spec shaping** — The shaping-specs skill asks which references apply. Specs include an "Applicable references" section.

**Before implementation** — The deploying-standards skill checks `references/` and loads any referenced in the spec or relevant to the task.

**Manual reference** — In any chat, use `@references/checkout-flow.mmd` to include a reference in context.

---

## Related

For codebase patterns and conventions (naming, structure, API design), see `standards/README.md`.
