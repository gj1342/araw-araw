# Spec: Araw-araw Habit Tracker

**Date:** 2026-03-14  
**Status:** Ready

---

## What we're building

Araw-araw is a personal habit tracker web app where users register, define habits to track for a month, mark daily completions, view progress via a line graph and GitHub-style contribution calendar, manage custom lists (e.g., movies watched), and interact with an AI chatbot for motivation and insights. Built with React + Vite + TypeScript, Tailwind CSS, shadcn/ui, and Supabase. Design is clean, minimalist, and responsive from 320px to 2560px.

---

## Users and context

**Primary user:** Individuals who want to build and maintain habits (exercise, reading, meditation) and/or keep custom lists (movies, books) in one place. They need visibility into progress over time without daily manual task entry.

**Secondary:** None. Single-user, personal use. No sharing or collaboration in v1.

---

## Success criteria

- [ ] User can register (email + password) and log in without Google Sign-In
- [ ] User can add, edit, and remove habits; habits are defined once per month, not per day
- [ ] User marks habit completion per day; data persists in Supabase
- [ ] Line graph shows daily completion count over the current month (or selected month)
- [ ] GitHub-style contribution calendar shows activity intensity (color-coded cells) for the month
- [ ] Custom lists page: user creates lists (e.g., "Movies watched"), adds/edits/removes items with full control
- [ ] AI chatbot responds with habit coach messages and simple insights based on user data
- [ ] All hardcoded text lives in `src/config/constants.ts`
- [ ] All colors defined in `src/styles/colors.css` (or `index.css`) and wired into Tailwind
- [ ] Responsive across: 320px, 375px, 425px, 768px, 1024px, 1440px, 2560px
- [ ] Animations: hover, subtle text/cursor effects to avoid flatness

---

## Scope

**In scope:**

- Email/password auth (register, login, logout)
- Habit CRUD (create, read, update, delete) scoped to a month
- Daily completion tracking (mark habit done for a date)
- Line graph (1-month duration, daily completion count)
- Contribution calendar (GitHub-style grid, color intensity by completion count)
- Custom lists: create list, add/edit/remove items
- AI chatbot (Mistral or Gemini) with habit coach + insights
- Responsive layout, Tailwind + shadcn, TypeScript
- Supabase RLS for data isolation

**Out of scope:**

- Google Sign-In
- Offline / PWA
- Sharing, collaboration, or multi-user features
- List assistant via AI (Phase 2)
- Dark mode (can add later via existing color variables)

---

## Relevant files and modules

**Net new.** No existing codebase. Create:

| Path | Purpose |
|------|---------|
| `src/config/constants.ts` | All hardcoded text (labels, messages, errors) |
| `src/config/routes.ts` | Route path constants |
| `src/styles/colors.css` | CSS variables for colors; import in `index.css` |
| `src/lib/supabase.ts` | Supabase client init |
| `src/hooks/useAuth.ts` | Auth state and actions |
| `src/hooks/useHabits.ts` | Habit CRUD + completions (React Query) |
| `src/hooks/useLists.ts` | Custom list CRUD (React Query) |
| `src/pages/Login.tsx`, `Register.tsx` | Auth pages |
| `src/pages/Habits.tsx` | Habit list, add/edit, completion toggle |
| `src/pages/Lists.tsx` | Custom lists management |
| `src/pages/Chat.tsx` | AI chatbot UI |
| `src/components/charts/LineGraph.tsx` | 1-month line chart |
| `src/components/charts/ContributionCalendar.tsx` | GitHub-style grid |
| `src/components/chat/ChatWindow.tsx` | Chat UI + API integration |
| `src/App.tsx` | Router, layout, protected routes |

---

## Supabase schema

**Tables:**

```sql
-- Users: handled by Supabase Auth (auth.users). We use public.profiles for extra data if needed.
-- For v1, auth.users is sufficient.

-- Habits: one row per habit per month (user defines habits for a month)
create table habits (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users(id) on delete cascade not null,
  name text not null,
  month date not null,  -- first day of month, e.g. '2026-03-01'
  created_at timestamptz default now(),
  updated_at timestamptz default now(),
  unique(user_id, name, month)
);

-- Completions: one row per habit per day when user marks it done
create table completions (
  id uuid primary key default gen_random_uuid(),
  habit_id uuid references habits(id) on delete cascade not null,
  completed_date date not null,
  created_at timestamptz default now(),
  unique(habit_id, completed_date)
);

-- Custom lists: user-defined lists (e.g. "Movies watched")
create table custom_lists (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users(id) on delete cascade not null,
  name text not null,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

-- List items: items in a custom list
create table list_items (
  id uuid primary key default gen_random_uuid(),
  list_id uuid references custom_lists(id) on delete cascade not null,
  content text not null,
  sort_order int default 0,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

-- RLS policies: users can only access their own data
alter table habits enable row level security;
alter table completions enable row level security;
alter table custom_lists enable row level security;
alter table list_items enable row level security;

-- Habits: user can CRUD own habits
create policy "Users manage own habits" on habits
  for all using (auth.uid() = user_id);

-- Completions: user can CRUD via habit ownership
create policy "Users manage completions via habits" on completions
  for all using (
    exists (select 1 from habits where habits.id = completions.habit_id and habits.user_id = auth.uid())
  );

-- Custom lists: user can CRUD own lists
create policy "Users manage own lists" on custom_lists
  for all using (auth.uid() = user_id);

-- List items: user can CRUD via list ownership
create policy "Users manage list items via lists" on list_items
  for all using (
    exists (select 1 from custom_lists where custom_lists.id = list_items.list_id and custom_lists.user_id = auth.uid())
  );
```

**Custom lists vs habits:** Separate. Habits have completions and drive the graph/calendar. Custom lists are free-form collections with no completion tracking.

---

## Risks and edge cases

- **Auth token expiry:** Supabase handles refresh. Ensure `onAuthStateChange` is used to update UI when session changes.
- **AI rate limits:** Mistral/Gemini free tiers have limits (~1 req/s, ~30/min). Implement client-side throttle (e.g., debounce, disable send while loading) and surface a friendly message if rate-limited.
- **Empty states:** New user has no habits, no completions, no lists. Design empty states with clear CTAs.
- **Month boundaries:** Habits are scoped to a month. When viewing March, show March habits only. Provide month selector for graph and calendar.
- **Timezone:** Store `completed_date` as date (no time). Use user's local date for "today" to avoid timezone confusion.
- **Long lists:** 100+ habits or list items may cause lag. Consider virtualization (e.g., `@tanstack/react-virtual`) in a later sprint.

---

## Applicable standards

- `standards/README.md` — Standards structure (no project-specific standards yet; this spec defines them)
- Project conventions from brainstorm: constants in one file, colors in one file, TypeScript, Tailwind, shadcn

---

## Applicable references

- `specs/brainstorming/araw-araw/design.md` — Design decisions, fonts, colors, component list, lifecycle

---

## Implementation notes

### Sprint order

1. **Sprint 0:** Project setup (Vite, React, TS, Tailwind, shadcn init), Supabase project + schema, `lib/supabase.ts`, `config/constants.ts`, `styles/colors.css`
2. **Sprint 1:** Auth (Register, Login, protected routes), habit CRUD, completion toggle, basic Habits page
3. **Sprint 2:** Line graph (Recharts or Nivo), contribution calendar, month selector
4. **Sprint 3:** Custom lists CRUD, Lists page
5. **Sprint 4:** AI chatbot (Mistral or Gemini), Chat page, habit coach + insights
6. **Sprint 5:** Responsive polish, animations, QA

### Tech stack

- **Framework:** React 18 + Vite + React Router
- **Styling:** Tailwind CSS, shadcn/ui
- **Database/Auth:** Supabase
- **State/cache:** TanStack React Query
- **Forms:** react-hook-form + zod + @hookform/resolvers
- **Charts:** Recharts (simpler) or @nivo/line + @nivo/calendar
- **AI:** Mistral API or Google Gemini API

### Route structure

```
/                 → Redirect to /habits if logged in, else /login
/login            → Login page
/register         → Register page
/habits           → Habits dashboard (protected)
/lists            → Custom lists (protected)
/chat             → AI chatbot (protected)
```

### AI chatbot flow

1. User sends message.
2. Client fetches user's habits + completions for current month from Supabase (or use cached React Query data).
3. Client calls Mistral/Gemini API with: system prompt (habit coach persona), user message, and JSON summary of habit data.
4. API returns response; display in chat UI.
5. Store conversation in component state only (no persistence in v1).

### Responsive breakpoints (Tailwind defaults)

- `sm`: 640px  
- `md`: 768px  
- `lg`: 1024px  
- `xl`: 1280px  
- `2xl`: 1536px  

Add custom `3xl` or `4k` in `tailwind.config.js` for 2560px if needed.
