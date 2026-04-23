# Study Companion — design & scope log

Living reference for the Storybook example at `Examples / Study Companion / Weekly view`
(source: [.storybook/stories/study-companion.stories.ts](./study-companion.stories.ts)).

---

## 1. Product context

**Product:** Study Companion (PRD v1.0, 2026-04-22)

Tool that helps students reduce information chaos around assignments, tests,
and study materials, and build consistency in studying. WCAG 2.2 AA is a
first-class requirement.

### Target users

- Primary: **students** (incl. neurodivergent — ADHD, autism spectrum)
- Secondary: **parents** (serve as "external brain" for younger students)
- Not: teachers / LMS integrations (out of scope in v1)

### Core problems (from refinement 2026-04-22)

1. "What, by when, from which subject" is scattered across e-gradebooks,
   parent chats, and verbal announcements.
2. Students struggle to prioritize and plan (especially neurodivergent users).
3. Study material is spread across flashcards, quizzes, outdated textbooks.
4. Stress about whether they are prepared enough.

### Success metrics (from PRD)

| Goal                                  | Metric                                     |
| ------------------------------------- | ------------------------------------------ |
| Reduce "figuring out what to do" time | −30 % in 4 weeks (pilot)                   |
| More on-time completion               | +15 pp in 6 weeks                          |
| More consistency                      | +20 % weeks with X study blocks in 6 weeks |
| WCAG 2.2 AA                           | 0 critical issues; keyboard-only + SR pass |

---

## 2. User story implemented in Storybook

> As a student, I want to see all tasks for the week in a clear view,
> so I can plan my study time and priorities quickly.

Other PRD user stories (add task, attach materials, filter/sort, track
progress To do / In progress / Done) are represented structurally in the UI
but are not yet wired to real data.

---

## 3. Component scope

Designers narrowed the SL Design System catalogue down to **15 components**:

Button · Button bar · Toggle button · Date picker (`sl-date-field`) ·
Badge · Progress bar · Search field · Select · Card · Panel ·
Avatar · Icon · Tabs · Overlay (`sl-popover`) · Tooltip

### Design decisions

| #    | Date       | Decision                                                                                                                    | Rationale                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ---- | ---------- | --------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| D-01 | 2026-04-22 | Use `sl-panel` instead of `sl-card` for event items                                                                         | No artwork/imagery represents events (test, quiz, essay); `sl-card` media slot would stay empty. `sl-panel` better matches the text-only, list-like nature of tasks.                                                                                                                                                                                                                                                                                                                           |
| D-02 | 2026-04-22 | `sl-progress-bar` max width = ⅓ of container                                                                                | Avoids over-dominating the layout; keeps progress signal compact next to other content.                                                                                                                                                                                                                                                                                                                                                                                                        |
| D-03 | 2026-04-22 | `sl-popover` powers quick-add ("New task"), not per-task details                                                            | Lightweight add flow on mobile; per-task details live inline inside the collapsible task panel — no modal takeover.                                                                                                                                                                                                                                                                                                                                                                            |
| D-04 | 2026-04-22 | Subject is encoded with **color + icon + text badge** (never color alone)                                                   | WCAG 1.4.1 Use of Color + neurodivergent clarity.                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| D-05 | 2026-04-22 | Every icon-only control carries `aria-label` + `sl-tooltip` via `aria-describedby`                                          | WCAG 2.2 Target Size + non-visual context.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| D-06 | 2026-04-23 | Subject toggle-buttons use the regular (outline) subject icon in `default` and the solid (filled) subject icon in `pressed` | Preserves the subject symbol in both states so users always recognize the category, while still conveying selection through two reinforcing cues: a solid selected background (`fill="outline"` + `[pressed]` → `--sl-color-background-selected-bold`) and an outline→solid icon weight shift. The component requires different icon names for the two slots, so the `far-*` / `fas-*` pair also satisfies that constraint. The generic "More filters" toggle still uses a check in `pressed`. |
| D-07 | 2026-04-23 | Task panels are **not collapsible**; show full task details inline                                                          | Figma frame 821:2537 shows an always-expanded panel; reduces interaction cost and keeps key info visible for neurodivergent users.                                                                                                                                                                                                                                                                                                                                                             |
| D-08 | 2026-04-23 | Single **status badge** (subtle, lg) in the panel `aside` slot, right-aligned                                               | Matches Figma frame; avoids the earlier three-badge stack (subject/priority/status) which crowded the header.                                                                                                                                                                                                                                                                                                                                                                                  |
| D-09 | 2026-04-23 | Priority encoded as **pepper emoji** appended to the task title (🌶️🌶️ / 🌶️)                                                 | Playful, student-friendly spiciness metaphor; mirrors Figma frame's `Test 🌶️🌶️` title.                                                                                                                                                                                                                                                                                                                                                                                                         |
| D-10 | 2026-04-23 | Simplified task body: **subject line + description only**; no per-task progress bar / meta / attach-file action             | Reduces cognitive load per task; keeps the card scannable. Detailed progress/attachments can return later in a drill-down view.                                                                                                                                                                                                                                                                                                                                                                |

### Components removed from initial broader recommendation

- `sl-card` (replaced by `sl-panel`, decision D-01)
- `sl-checkbox`, `sl-callout`, `sl-inline-message`, `sl-tree`, `sl-tag` —
  not in the narrowed list; can revisit if the scope grows.
- `sl-dialog`, `sl-drawer`, `sl-message-dialog` — mobile-first decision
  favors inline panel + popover.

---

## 4. Composition in the Storybook page

```
Header
  ├─ Avatar (student)
  ├─ H1
  └─ Button "New task" (primary) + Tooltip + Popover (quick-add)
        └─ Popover body: Search field + Select (subject) + Date picker + Button-bar

Section "Weekly preparation"
  └─ Progress bar (max ⅓ width)

Toolbar
  ├─ Search field (tasks/subjects/materials)
  ├─ Select (filter by subject, icons in options)
  └─ Date picker (jump to date)

Subject filters (role="group")
  └─ Toggle button × N (subject pills, icon-only + Tooltip)
  └─ Toggle button "More filters"

Tab group: Today / This week / Month (each tab has a Badge count)
  ├─ Today panel     → list of task panels
  ├─ This week panel → list of day panels (one per weekday),
  │                    each containing task panels
  └─ Month panel     → out of scope placeholder

Day panel (sl-panel)
  ├─ prefix: Badge (task count)
  ├─ actions: Button (add task for day) + Tooltip
  └─ default: task panels

Task panel (sl-panel, divider)  ← D-01, D-07, mirrors Figma 821:2537
  ├─ heading: task title + spiciness peppers (priority → D-09)
  ├─ prefix: sl-icon (subject)
  ├─ aside:  sl-badge (status, subtle/lg, right-aligned → D-08)
  └─ default:
        ├─ subject line (icon + label)
        ├─ description / note
        └─ Button-bar: primary pill "Start"
```

---

## 5. Functional-requirement coverage (PRD §4)

| Req                              | Status in story                                     | Notes                                    |
| -------------------------------- | --------------------------------------------------- | ---------------------------------------- |
| FR-01 task views (list + weekly) | ✅ Today tab = list; This-week tab = weekly planner | Month tab placeholder                    |
| FR-02 create/edit task           | 🟡 Quick-add Popover only                           | Full form + validation later             |
| FR-03 filtering & sorting        | 🟡 Search + Select + Toggle-button pills            | No sort control yet                      |
| FR-04 study-block planning       | ❌                                                  | Later phase                              |
| FR-05 progress & basic stats     | 🟡 Progress bar global + per task; status badge     | Stats panel later                        |
| FR-06 reminders / notifications  | ❌                                                  | Out of scope for the story               |
| FR-07 DnD alternative            | n/a                                                 | No DnD used; keep as guidance when added |

### Non-functional (WCAG 2.2 AA) status

- ✅ Color not sole encoding (D-04)
- ✅ Icon-only buttons labelled + tooltipped (D-05)
- ✅ Built-in SL components keyboard- & SR-accessible
- ✅ Responsive breakpoint at 720 px
- 🟡 Text resize 200 %, prefers-reduced-motion — rely on design-system
  defaults; needs verification in audit
- 🟡 Focus appearance — inherited from components; needs audit

---

## 6. Out of scope (current iteration)

- Authentication, persistence, multi-device sync
- LMS / e-gradebook integrations (Librus, Vulcan, etc.)
- Automatic material ingestion (flashcards, quizzes)
- Teacher- or parent-facing views
- Notifications / reminders infrastructure
- Monthly/yearly long-range planning views
- Drag-and-drop reordering

---

## 7. Open questions

- How is "weekly preparation %" calculated — per task weighted by duration,
  or flat task count?
- Should the student be able to split a large topic into subtasks themselves
  (manual) or via AI suggestion (auto)?
- Subject color palette — fixed set of 5, or user-assignable?
- Quick-add popover vs. full-screen form on narrow viewports?
- Integration points for externally authored materials (link, file, note)
  — purely user-uploaded in v1?

---

## 8. Change log

| Date       | Change                                                                                                                                                                     |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-04-22 | Initial story created; `Card` used for tasks                                                                                                                               |
| 2026-04-22 | D-01 applied: task items switched from `Card` to `Panel`                                                                                                                   |
| 2026-04-22 | D-02 applied: `Progress bar` capped at ⅓ container width                                                                                                                   |
| 2026-04-22 | D-03 applied: `Popover` moved from per-task details to global quick-add                                                                                                    |
| 2026-04-23 | Implemented Figma frame 821:2537 into the weekly view                                                                                                                      |
| 2026-04-23 | D-07 applied: removed `collapsible` from task and day panels                                                                                                               |
| 2026-04-23 | D-08 applied: single subtle/lg status badge, right-aligned in `aside`                                                                                                      |
| 2026-04-23 | D-09 applied: priority shown as pepper emoji in the task title                                                                                                             |
| 2026-04-23 | D-10 applied: simplified task body (no per-task progress bar / meta)                                                                                                       |
| 2026-04-23 | Added sample `note` text to every task                                                                                                                                     |
| 2026-04-23 | Wrapped body + button-bar in a grid to fix narrow-width collision                                                                                                          |
| 2026-04-23 | D-06 revised: subject toggle-buttons keep the subject icon when pressed                                                                                                    |
| 2026-04-23 | D-06 refined: use solid (`fas-*`) icon variant in pressed slot so the button shows a true solid-vs-outline distinction and satisfies the "different icon name" requirement |
