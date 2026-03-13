# Brainstorm: Araw-araw Habit Tracker

**Date:** 2026-03-14  
**Mode:** New product  
**Chosen approach:** React + Vite + Supabase (see rationale below)

---

## The problem

People struggle to build and maintain habits. They either forget to track, lose motivation, or lack visibility into their progress over time. Existing habit trackers often feel cluttered, require daily manual task entry, or don't offer flexibility for different types of lists (e.g., movies watched, books read).

**Who experiences this:** Individuals who want to build consistency in habits (exercise, reading, meditation) and/or maintain custom lists (movies, books, etc.) in one place.

**Without this:** Users rely on scattered notes, spreadsheets, or multiple apps that don't give a unified view of their progress.

---

## Success criteria

- User can register, log in, and manage habits without friction
- Habits are defined once per month (not per day) — user marks completion daily
- Line graph shows daily completion count over 1 month
- GitHub-style contribution calendar shows streak/activity at a glance
- Custom lists (movies, books, etc.) are fully user-controlled
- AI chatbot adds tangible value (suggestions, insights, or motivation)
- Design is responsive across 320px–2560px
- Code is clean, typed, and maintainable

---

## Constraints

**Technical:**
- TypeScript required
- Tailwind CSS + shadcn/ui
- Supabase for auth + database
- No Google Sign-In
- AI: Mistral preferred (free tier); alternatives acceptable

**Product:**
- Clean, minimalist design
- Animations (hover, cursor, text) to avoid flatness
- All hardcoded text in one config file
- Global color definitions in one place

---

## Approaches considered

### Framework: React + Vite vs Next.js

**Approach A: React + Vite + React Router**

- **What it is:** Vite is a build tool that compiles your React app. React Router handles navigation (e.g., `/habits`, `/lists`, `/chat`). Everything runs in the browser; Supabase is called directly from the client.
- **Pros:** You already know it. Faster dev experience. Simpler mental model. No server to manage. Supabase handles auth and data — no need for a backend.
- **Cons:** All logic runs client-side. SEO is weaker (but for a personal habit tracker, SEO is rarely critical).
- **Best if:** You want to ship fast, learn Supabase deeply, and avoid learning Next.js at the same time.

**Approach B: Next.js (App Router)**

- **What it is:** React framework with built-in routing, server components, and optional API routes. Can render pages on the server for better SEO and performance.
- **Pros:** Industry standard for production React apps. Great DX. Server components can reduce client bundle size. Built-in API routes if you ever need a backend.
- **Cons:** Steeper learning curve. More concepts (Server vs Client components, layouts, etc.). Overkill for a habit tracker that doesn't need SEO or complex server logic.
- **Best if:** You explicitly want to learn Next.js as part of upskilling.

**Chosen direction:** **React + Vite + React Router.** You're already comfortable with it, and the product doesn't require Next.js features. Learning Supabase + TypeScript + shadcn is enough new surface area. You can migrate to Next.js later if needed.

---

### AI chatbot utilization

**Approach A: Habit coach / motivator**

- Suggests which habit to focus on based on streaks and gaps
- Sends encouraging messages when user is on a streak
- Explains why consistency matters (e.g., "You've done X 5 days in a row — that's how habits form")

**Approach B: Insights and analytics**

- "You complete more habits on weekdays than weekends — consider lighter goals on Saturday"
- "Your best streak was 12 days in February — here's what was different"
- Summarizes progress in natural language

**Approach C: List assistant**

- Helps add items to custom lists (e.g., "Add 'Inception' to my movies list")
- Suggests similar items or categories
- Can search or filter lists via natural language

**Chosen direction:** Start with **Approach A + B** (coach + insights). Approach C is nice but adds complexity. Phase 1: chatbot that can read user's habit data (via Supabase) and respond with motivation + simple insights. Phase 2: add list assistant if valuable.

**AI API recommendation:** Mistral is a solid choice (free tier, good quality). **Google Gemini** also has a generous free tier and is worth considering. Both work well for chat-style use cases.

---

## Design and vibe

**Font suggestions (professional, clean, minimalist):**
- **Primary:** [DM Sans](https://fonts.google.com/specimen/DM+Sans) — geometric, readable, modern
- **Alternative:** [Plus Jakarta Sans](https://fonts.google.com/specimen/Plus+Jakarta+Sans) — slightly warmer, still clean
- **Monospace (for numbers/dates):** [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) — for contribution grid labels

**Color strategy:**

Two valid approaches — pick one and stick to it:

1. **CSS variables in `src/index.css`** (recommended for "one file for all colors"):
   - Define `:root { --color-primary: ...; --color-surface: ...; }`
   - Reference in Tailwind via `tailwind.config.js` → `theme.extend.colors`
   - Or use directly: `style={{ backgroundColor: 'var(--color-primary)' }}` or `bg-[var(--color-primary)]`

2. **Tailwind theme in `tailwind.config.js`**:
   - Extend `theme.colors` with your palette
   - Use as `bg-primary`, `text-muted`, etc.
   - Keeps colors in one config file

**Recommendation:** Use `src/styles/colors.css` (or a `:root` block in `index.css`) for CSS variables, then wire them into Tailwind. This gives you a single source of truth and makes dark mode easier later.

**Animation ideas:**
- Hover: subtle scale or shadow on cards/buttons
- Cursor: custom cursor on interactive elements (optional, can feel gimmicky — use sparingly)
- Text: staggered fade-in on page load; subtle underline animation on links
- Contribution grid: cell hover shows tooltip with date + count; gentle pulse on today's cell

---

## Project structure (high level)

```
src/
├── config/
│   ├── constants.ts      # All hardcoded text (labels, messages, etc.)
│   └── routes.ts         # Route paths
├── styles/
│   └── colors.css        # Or in index.css — all color variables
├── components/
│   ├── ui/               # shadcn components
│   ├── habits/
│   ├── lists/
│   ├── charts/
│   └── chat/
├── hooks/
├── lib/
│   └── supabase.ts
├── pages/
└── App.tsx
```

---

## Software development lifecycle (roleplay)

**Suggested phases:**

1. **Discovery / Spec** — Brainstorm (done) → Shaping spec (next)
2. **Sprint 0** — Project setup, Supabase schema, auth flow
3. **Sprint 1** — Auth (register, login), habit CRUD, basic UI
4. **Sprint 2** — Line graph, contribution calendar
5. **Sprint 3** — Custom lists
6. **Sprint 4** — AI chatbot integration
7. **Sprint 5** — Polish, responsive testing, QA

**Skills to use:**
- `shaping-specs` — After this brainstorm, run it to formalize the spec
- `deploying-standards` — Before coding each sprint, load standards
- `quality-assurance` — Before considering a sprint "done"
- `security-audit` — After auth and any API integration
- `code-review` — Before merge / handoff

---

## shadcn/ui components to install

Install these as you need them (shadcn uses `npx shadcn@latest add <component>` — install one at a time):

| Component | Use case |
|-----------|----------|
| `button` | CTAs, form submit, habit toggle |
| `input` | Text fields (habit name, list item) |
| `card` | Habit cards, list cards, dashboard sections |
| `dialog` | Add/edit habit modal, add list item modal |
| `form` | Register, login, habit form (with react-hook-form + zod) |
| `table` | Contribution calendar (grid), custom list table |
| `calendar` | Date picker for viewing specific month |
| `dropdown-menu` | Habit actions (edit, delete), list actions |
| `toast` / `sonner` | Success/error feedback |
| `skeleton` | Loading states |
| `tabs` | Switch between Habits / Lists / Chat |
| `avatar` | User profile (optional) |
| `badge` | Streak count, habit count |
| `tooltip` | Contribution cell hover (date + count) |
| `separator` | Visual dividers |
| `scroll-area` | Long lists, mobile scroll |

**Install order suggestion:** Start with `button`, `input`, `card`, `form`, `dialog`. Add others as you build each feature.

---

## Other standards (optimization, security, caching)

**Security:**
- Supabase Row Level Security (RLS): Every table must have policies so users only read/write their own data. Think of it as "each row has a bouncer — only the owner gets in."
- Never expose Supabase anon key in client code for sensitive ops — for a habit tracker, anon key + RLS is fine. Service role key must stay server-side only (you won't need it for v1).
- Sanitize user input: Use Zod or similar for form validation. Supabase parameterized queries prevent SQL injection.

**Caching:**
- Supabase client caches nothing by default. Use React Query (TanStack Query) or SWR to cache habit/list data and avoid refetching on every navigation.
- For the line graph: fetch completion data once per month view; cache by month key.

**Optimization:**
- Lazy-load routes: `React.lazy()` for pages (Habits, Lists, Chat) so the initial bundle is smaller.
- Virtualize long lists: If a user has 100+ habits or list items, use `@tanstack/react-virtual` or similar so only visible rows render.
- Images: If you add avatars or icons, use Next.js Image or a CDN — for v1 you may not need any.

**Load balancing:**
- Not applicable for a client-only app. Supabase handles backend scaling. If you add a custom API later, consider Vercel/Netlify serverless or a small Node server.

---

## Open questions

- [ ] Exact Supabase schema (tables for users, habits, completions, custom_lists, list_items)
- [ ] How custom lists relate to habits (separate feature or unified?)
- [ ] Rate limiting / abuse prevention for AI chatbot
- [ ] Offline support (PWA?) — out of scope for v1?

---

## Context loaded

- `.agent/skills/brainstorming/SKILL.md` — Workflow and design doc format
- User requirements (features, standards, preferences) — Provided in prompt
