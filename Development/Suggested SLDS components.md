# Suggested SLSD components

## How the narrowed components combine into the student study dashboard

Using only the 15 components the designers approved: **Button, Button bar, Toggle button, Date picker, Badge, Progress bar, Search field, Select, Card, Panel, Avatar, Icon, Tabs, Overlay (Popover), Tooltip.**

---

### 1. Page-level layout — "What's on my plate"

**`Tabs`** drives the time horizon at the top of the dashboard:
`Dziś` · `Tydzień` · `Miesiąc`

Each tab hosts a stack of **`Panel`**s, one per day or per subject group (e.g. "Poniedziałek", "Wtorek", or "Matematyka", "Historia"). `Panel` is collapsible, which supports the "pokaż tylko to, co teraz" need for neurodivergent users.

Inside each `Panel` content slot → a grid of **`Card`**s, one per assignment/test/lektura.

---

### 2. The event `Card` — the atomic unit

Every sprawdzian / kartkówka / lektura is a `Card` composed of:

- **`Avatar`** (square variant) or **`Icon`** on the left — the subject's visual identity (color + symbol for matematyka, historia, polski…). Solves the "uczeń musi odróżnić przedmioty" requirement.
- **`Badge`** next to the title — status/priority: `Jutro`, `Pilne`, `Nowe`, or a numeric countdown ("za 2 dni").
- **Title + short description** (the "co" and "z czego").
- **`Progress bar`** inside the card footer — how much of this topic the student has already covered ("3 z 5 tematów opanowanych"). This is the anti-stress signal.
- **`Button bar`** at the bottom with the primary **`Button`** ("Ucz się teraz") and secondary buttons ("Podziel materiał", "Oznacz jako gotowe").

---

### 3. Global dashboard header

Above the `Tabs`:

- Large **`Progress bar`** = "Przygotowanie na ten tydzień: 35%" (the headline metric from the refinement: *"ile z tego, co miałaś zaplanowane na ten tydzień, zrobiłaś"*).
- **`Avatar`** of the logged-in student (top-right), opening a profile **`Popover`**.

---

### 4. Filtering & navigation bar

A second row of controls, below the header:

- **`Search field`** — "znajdź temat / fiszkę / przedmiot".
- **`Select`** — filter by subject (matematyka, historia, …). With the icons slot, options carry the same subject `Icon` used in cards.
- **`Date picker`** — jump to a specific day; also used in "Planner" view to set custom deadlines when the student splits a big topic.
- **`Toggle button`** group (acting as view switcher): `Lista` / `Kalendarz` / `Tablica`. Also used as a subject-pill filter row ("Pokaż tylko: Matematyka · Historia · Polski") — each pill is a `Toggle button` with subject `Icon`.

---

### 5. Progressive disclosure — details without leaving the page

Clicking a `Card` opens a **`Popover`** (the "Overlay" from the designers' list) anchored to the card. It holds:

- Full description of the material.
- A nested **`Progress bar`** per subtopic.
- A **`Button bar`** with "Rozpocznij naukę", "Podziel na części", "Przełóż termin" (opens an inline `Date picker`).

This keeps the mobile-first constraint — no route change, no modal takeover.

---

### 6. Helpful hints everywhere

**`Tooltip`** wraps every icon-only control (the `Toggle button` subject pills, icon-only `Button`s in the `Button bar`, subject `Avatar`s). The refinement specifically called out accessibility and clarity for neurodivergent students — tooltips carry the labels for icon-only UI.

**`Badge`** is also placed on navigation/tab labels to show counts: `Dziś (3)`, `Tydzień (11)`.

---

### 7. Concrete wiring (ASCII sketch)

```
┌─ Header ─────────────────────────────────────────────┐
│ [Avatar]  Cześć, Ada!       [Progress bar 35%]       │
├─ Toolbar ────────────────────────────────────────────┤
│ [Search field]  [Select: Przedmiot]  [Date picker]   │
│ [Toggle button group: Lista | Kalendarz | Tablica]   │
├─ Tabs ───────────────────────────────────────────────┤
│  Dziś(3)  Tydzień(11)  Miesiąc(27)                   │
├─ Panel "Dziś" ───────────────────────────────────────┤
│  ┌ Card ─────────────────────────────────────────┐   │
│  │ [Avatar HIST] Konfederacja barska  [Badge ⚠]  │   │
│  │ Sprawdzian · jutro 8:00                       │   │
│  │ [Progress bar 2/5]                            │   │
│  │ [Button bar: Ucz się | Podziel | Gotowe (?) ] │   │
│  └───────────────────────────────────────────────┘   │
│  ┌ Card ──────────────────────────── ... ─────── ┐   │
│                                                      │
│     (Click a Card → Popover with details +          │
│      Date picker to reschedule + Button bar)        │
└──────────────────────────────────────────────────────┘
```

---

### Coverage check against the refinement needs

| Refinement need | Covered by |
|---|---|
| Wizualne pokazanie "co na kiedy" | `Tabs` + `Panel` + `Card` |
| Różne przedmioty = kolor + ikona | `Avatar` / `Icon` + `Select` with icons + `Toggle button` pills |
| Priorytet / pilność | `Badge` on cards and tabs |
| "Tu i teraz" vs planowanie na tydzień | `Tabs` (Dziś/Tydzień/Miesiąc) |
| % przygotowania, anti-stress | `Progress bar` (global + per card) |
| Podział dużego materiału na części | Action in card's `Button bar` → `Popover` with `Date picker` |
| Szybkie znalezienie tematu | `Search field` + `Select` |
| Mobile-friendly, bez pełnych modali | `Popover` zamiast dialogu |
| Dostępność / neurodivergent | `Tooltip` wszędzie na icon-only, kolapsujące `Panel` |

## Screenshots

### Button

![screenshot-20260422193004515](/api/file/raw?path=screenshot-20260422193004515.png)

### Button Bar

![screenshot-20260422193104944](/api/file/raw?path=screenshot-20260422193104944.png)

### Toggle Button

![screenshot-20260422193127434](/api/file/raw?path=screenshot-20260422193127434.png)

### Date Picker

![screenshot-20260422193153507](/api/file/raw?path=screenshot-20260422193153507.png)

### Badge

![screenshot-20260422193208042](/api/file/raw?path=screenshot-20260422193208042.png)

### Progress Bar

![screenshot-20260422193232002](/api/file/raw?path=screenshot-20260422193232002.png)

### Search Bar

![screenshot-20260422193835370](/api/file/raw?path=screenshot-20260422193835370.png)

### Select

![screenshot-20260422193924239](/api/file/raw?path=screenshot-20260422193924239.png)

### Card


![screenshot-20260422193951677](/api/file/raw?path=screenshot-20260422193951677.png)

### Panel

![screenshot-20260422193939631](/api/file/raw?path=screenshot-20260422193939631.png)

### Avatar

![screenshot-20260422194014802](/api/file/raw?path=screenshot-20260422194014802.png)

### Icon

![screenshot-20260422194034149](/api/file/raw?path=screenshot-20260422194034149.png)

### Tabs

![screenshot-20260422194235790](/api/file/raw?path=screenshot-20260422194235790.png)

### Popover

![screenshot-20260422194108334](/api/file/raw?path=screenshot-20260422194108334.png)

### Tooltip


![screenshot-20260422194328239](/api/file/raw?path=screenshot-20260422194328239.png)

