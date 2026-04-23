## Why it reads "adult-corporate" right now

Looking at the current story, it hits several patterns that feel enterprise, not teen:

1. **Dense compliance-style language** — "Weekly preparation · 42% ready · 5 of 12 tasks done", "In progress", "Attach material", "Mark as done". Reads like a project management tool.
2. **Abstract progress metrics** — raw percentages with no emotional payoff.
3. **Neutral / muted subject colors** — blue/orange/green/purple/red badges, but used in a small, administrative way.
4. **No reward / celebration moments** — completing a task is a flat state change.
5. **Tabs labelled "Today / This week / Month"** — calendar-speak, not student-speak.
6. **Avatar + name in header** — mirrors SaaS dashboards (Gmail, Jira).
7. **No personality** — no illustrations, no mascot, no playful copy, no variation between subjects beyond a color chip.
8. **Task titles are formal** — "Quadratic equations — test", "Partitions of Poland — essay outline".

## What 12–15 year olds respond to (research signals)

- **Ownership & identity** (customization, avatars, themes) — they're building identity, not just using a tool.
- **Progress framed as growth, not compliance** — XP, streaks, levels, "you're on a 3-day roll" rather than "42% ready".
- **Short, concrete, conversational copy** — "Let's do this", "Nice, one down", "Tomorrow you've got…".
- **Visual hierarchy driven by color & illustration**, not by typography alone.
- **Micro-rewards** — confetti / animation / badge unlock when a task completes.
- **Low-stakes entry points** — "5 min warm-up" framing beats "Start 40-min study block".
- **Safety / low-pressure tone** — especially important for neurodivergent users; no red "OVERDUE" alarms.

## Concrete changes, grouped by effort

### A. Copy & framing (no component changes — do this first)

| Current | Kid-friendly |
|---|---|
| "Weekly preparation · 42% ready" | "This week's quest · 5 of 12 done 🎯" (or without emoji: "5 down, 7 to go — you're crushing it") |
| "New task" | "Add something" / "Add to list" |
| "Mark as done" | "Done!" |
| "Attach material" | "Add notes or files" |
| "In progress" badge | "Working on it" |
| "High priority" | "Do first" |
| "Today / This week / Month" | "Today / This week / Later" |
| "Quadratic equations — test" | "Math test: quadratic equations" |
| Avatar initials "AK" | First-name only ("Hi, Ada 👋") |

### B. Visual — stays within existing SL components

1. **Bigger, warmer subject presence.** Right now subjects are a small badge + avatar. Make the subject icon the *dominant* visual of each task panel (large square avatar tile, saturated color). Panel still does the job.
2. **Swap the global "Weekly preparation" progress bar for a streak / level line** — same `sl-progress-bar`, different label: "Level 3 · 60 XP to next level" or "3-day streak 🔥".
3. **Celebration state on completion** — when "Done!" is pressed, the task panel collapses with a brief transform/scale animation (CSS only, respecting `prefers-reduced-motion`). No new component needed.
4. **Rounder, softer density** — increase spacing between task panels; larger avatar size on panels.
5. **Use `sl-avatar` for subjects with a real illustration/emoji**, not just a FA icon — e.g. 📐 Math, 🌍 Geography, 🧪 Science. (Falls back to current icons for WCAG — emoji is decorative, label carries meaning.)

### C. Structural additions (new stories, not breaking D-01..D-06)

1. **"Today" as the default tab**, not "This week" — reduces overwhelm.
2. **"Next up" hero card** at the top: one single task framed as "Start with this — 15 min" with a big primary button. Students do better with one next action than a list.
3. **Mood / energy check-in** as a row of `sl-toggle-button` (😴 🙂 ⚡) — informs which task is surfaced as "Next up". Very supportive for ADHD users.
4. **Rewards row** — row of `sl-badge` showing earned badges ("5-day streak", "Math master", "Early bird").
5. **Parent-view toggle** — since parents are secondary users, one `sl-toggle-button` that switches copy from "you" to "your child" could be a later story.

### D. Theming

If the design system has a theme layer in themes, a **"Study Companion" theme** with saturated primaries, larger border-radius tokens, and a playful heading font would do a lot of the heavy lifting without touching component structure.

## Design decisions this would add to the log

- **D-07** — Use first-name-only greeting; drop formal avatar initials in header.
- **D-08** — Replace "Weekly preparation %" with streak / level framing; same component, new semantic.
- **D-09** — Default tab is "Today" + single "Next up" hero, not the full-week planner.
- **D-10** — Subject encoding upgraded to illustrated avatar + color + label (keeps D-04's non-color-alone rule).
- **D-11** — Copy tone: second-person, short, supportive; no red/alarm language on overdue.
- **D-12** — Celebratory micro-animation on task completion, gated by `prefers-reduced-motion`.

## Risks / things to watch

- **Don't gamify the stress in.** XP and streaks can backfire for anxious students — make streaks forgiving (skip days, freeze tokens) or make them opt-in.
- **Avoid emoji-as-only-label** in interactive controls (WCAG 1.1.1). Keep text labels; emoji is supplementary.
- **Teen aesthetic ≠ childish.** 12–15 is middle school / early high school; avoid primary-color "kids' app" visuals — they'll reject it as babyish. Aim closer to Duolingo / Notion-for-students / Quizlet tone.