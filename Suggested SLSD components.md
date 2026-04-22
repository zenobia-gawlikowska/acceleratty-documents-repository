# Suggested SLSD components

Based on the transcript, the product is a student dashboard for managing assignments, tests, and study materials with:
- Visual representation of "what, when, from which subject"
- Time perspectives (today / this week / longer)
- Subject differentiation (color + icon coding)
- Cards per event/assignment (expandable to show material)
- Progress tracking (% of week prepared)
- Mobile-friendly, accessible (neurodivergent-friendly)
- Filtering


## Component recommendations for the student study dashboard

Based on the refinement, the product needs a **visual, mobile-friendly dashboard** that shows students *what* to study, *when*, and *from which subject*, with color/icon subject coding, time horizons (today / week / longer), per-assignment cards expandable to learning material, filtering, and progress tracking — plus neurodivergent-friendly UX.

### Primary (core building blocks for the dashboard)

| # | Component | Role in the product |
|---|-----------|---------------------|
| 1 | card.png — **Card** (stable)  | The main "event card": one per sprawdzian/kartkówka/lektura. Can hold image/icon, title, tag, description, primary action ("Ucz się teraz"). This is the heart of the dashboard. |
| 2 | panel.png — **Panel** (draft) | Wraps grouped sections ("Dziś", "Ten tydzień", "Później"). With title + action slot. |
| 3 | accordion.png — **Accordion** (stable) | Collapsible grouping by day or by subject; perfect for "zjedziałość" — expand only what's now. |
| 4 | tabs.png — **Tabs** (stable) | Switch the time horizon: *Dziś / Tydzień / Miesiąc*. |
| 5 | progress-bar.png — **Progress bar** (preview) | "Jak bardzo jesteś przygotowany na ten tydzień" — % done per subject or overall. Strong stress-reduction signal. |

### Supporting (semantic / visual encoding)

| # | Component | Role |
|---|-----------|------|
| 6 | tag-list.png — **Tag / Tag list** (preview) | Color-coded subject labels (matematyka, historia, polski…) — exactly the "kolor + ikona per przedmiot" requirement. |
| 7 | badge.png — **Badge** (stable) | Small status indicator: "nowe", "jutro", "pilne", or a count of items per day. |
| 8 | callout.png — **Callout** (preview) | Strong "here and now" message: "Masz sprawdzian jutro — zacznij teraz". |
| 9 | inline-message.png — **Inline message** (stable) | Dismissable reminders inside a card/panel ("Materiał zaktualizowany przez nauczyciela"). |
| 10 | checkbox.png — **Checkbox** (stable) | Mark a subtask / flashcard / topic as "opanowane". Feeds the progress bar. |

### Optional (filter / detail / hierarchy)

- **Tree** (tree.png, preview) — if you want to break a big topic into subtopics ("Rozbiory → I rozbiór, II rozbiór…") that the student (or the tool) can split.
- **Select / Combobox** — filter by subject.
- **Search field** — find a topic or flashcard.
- **Dialog / Drawer** — open full card details on mobile (drawer is especially good on phones).
- **Avatar / Icon** — subject avatar/icon inside cards and tags.
- **Tooltip** — hints, since the audience includes neurodivergent users.
- **Button / Button bar** — actions: "Ucz się", "Podziel materiał", "Oznacz jako gotowe".

### Suggested dashboard composition
- `sl-tabs` (Dziś / Tydzień / Miesiąc)
  - `sl-panel` "Dziś" → grid of `sl-card`s, each with `sl-tag` (subject), `sl-badge` (priority), `sl-progress-bar` (how much of this material done), `sl-checkbox` list for subtopics
  - `sl-accordion` "Później" grouped by day
- Top-level `sl-callout` for the most urgent item
- `sl-progress-bar` at the top = "Przygotowanie na ten tydzień: 35%"

All screenshots are saved in the workspace root (card.png, panel.png, accordion.png, tabs.png, progress-bar.png, tag-list.png, badge.png, callout.png, inline-message.png, checkbox.png, tree.png).

## Screenshots

### Card

![screenshot-20260422191844293](/api/file/raw?path=screenshot-20260422191844293.png)

### Panel

![screenshot-20260422191918304](/api/file/raw?path=screenshot-20260422191918304.png)

### Accordion

![screenshot-20260422191945649](/api/file/raw?path=screenshot-20260422191945649.png)

### Tabs

![screenshot-20260422192009933](/api/file/raw?path=screenshot-20260422192009933.png)

### Progress Bar

![screenshot-20260422192030881](/api/file/raw?path=screenshot-20260422192030881.png)

### Tag list

![screenshot-20260422192056439](/api/file/raw?path=screenshot-20260422192056439.png)

### Badge

![screenshot-20260422192136123](/api/file/raw?path=screenshot-20260422192136123.png)

### Callout

![screenshot-20260422192201886](/api/file/raw?path=screenshot-20260422192201886.png)

### Inline Message


![screenshot-20260422192228291](/api/file/raw?path=screenshot-20260422192228291.png)

### Checkbox


![screenshot-20260422192245606](/api/file/raw?path=screenshot-20260422192245606.png)

### Tree 

![screenshot-20260422192326940](/api/file/raw?path=screenshot-20260422192326940.png)
