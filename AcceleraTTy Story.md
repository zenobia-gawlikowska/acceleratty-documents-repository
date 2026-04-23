# Study Companion — Development Story

## What is it?

Study Companion is a web app designed for students aged 12–15 to help them track upcoming exams, tests, and assignments without getting lost in the chaos of school planners, parent chats, and e-gradebooks. The core idea: one clear view of "what do I have, when, and from which subject" — built with accessibility as a first-class requirement from day one.

---

## Where it started (22 April, evening)

The project kicked off with a refinement session (a recorded transcript is in the repo), where the team — Gosia, Kasia, Jędrzej, Pati, Kacper, and Zenobia — worked through the problem space together. Gosia framed the human need: students spend roughly half their study time just *figuring out what to study*, leaving the other half for actual learning. Her own neurodivergent child was part of the inspiration.

From that conversation, Kasia quickly drafted a **PRD (Product Requirements Document)** — the written contract of what the app should do — while Gosia and Jędrzej produced the UX screen list and user flow in Figma. Zenobia opened the codebase and Kacper wrote the first automated test that same evening.

The first working prototype used cards (think: tiles) to show events. Within hours, the team made their first design decision together: cards are for things with imagery — events like exams have no natural image, so they switched to *panels* instead. This became **Design Decision D-01**, the first entry in a living design log Zenobia maintained throughout.

---

## 23 April — a full day of parallel work

The bulk of the development happened in a single intensive day, with everyone committing changes in rapid succession.

**Morning:** Zenobia rebuilt the story component significantly — replacing the old card-based layout with a cleaner panel-based weekly view, adding subject icons, difficulty peppers (🌶️🌶️🌶️ for hard, 🌶️ for easy — a real system used by teachers), and status badges. She also wrote a detailed "kid-friendly requirements" document arguing that the current design read too much like a corporate SaaS product, proposing warmer copy, rounder shapes, and a more playful tone.

Kacper, working in parallel, wrote the first batch of automated Playwright tests — code that navigates the app like a real user would and checks that filtering by subject, difficulty, and event type actually works.

Pati started documenting the design decisions in a separate file (`Decyzje/design#1.md`), capturing the rationale behind panel structure and subject icon choices so the team had a shared reference point.

**Late morning:** A significant refactor. Zenobia replaced what had been a tab-based view (Today / This Week / Month) with a simpler day-by-day chronological list. The reasoning: the Figma designs had evolved, and the tab structure was introducing complexity that didn't serve students. Events are now grouped by "Today", "Tomorrow", "Saturday", "Monday" — much more immediate.

Kasia updated the PRD to reflect this, adding status filtering, dark mode, and responsiveness as formal requirements. The filter spec got more precise: exam, test, oral exam, written assignment as the four event types.

**Afternoon:** A burst of new features landed:
- **Responsive design** from Zenobia — the toolbar and filter controls now reflow gracefully on mobile rather than overflowing
- **Dark mode support** — hardcoded white backgrounds replaced with design system color tokens so the app respects users' system preferences
- **Responsive dropdowns and date filters** — the four filter dropdowns now wrap fluidly on small screens, and the date range filters actually trigger filtering
- **Reset Filters button** — added after Pati flagged it in Figma and Kasia wrote it into the PRD as a user story: "I can quickly return to the full list without clearing each filter manually"
- **Empty state** — when filters produce no results, a friendly illustrated placeholder appears instead of a blank screen
- **Accessibility fixes** — Zenobia wrapped date fields and search in proper `sl-form-field` labels (a WCAG requirement), replaced tooltip-based button labels with direct `aria-label` attributes, and changed `<div>` to `<main>` for correct landmark semantics

Kacper updated his tests to match the new structure throughout the afternoon, tracking each interface change as it landed. GosiaTTy added the first research summary from a usability session with a 12-year-old girl (Agata), noting that she understood priority by reading pepper icons, expected tasks sorted by urgency, and struggled with the search/filter navigation.

**Evening:** Zenobia added the **Task Detail screen** — the destination when a student clicks "Start" or "Continue" on an event. It shows the scope of the assignment, three linked study resources (book, video, flashcards), and an "I'm ready" button that marks the task done and returns to the weekly view. Pati confirmed the Figma design for this screen was ready. A final spacing refinement commit closed the day.

---

## The team's working rhythm

The collaborative process is visible in the commit sequence. The team worked from a shared document repository (Acceleratty) that acted as their single source of truth — similar to how code works, but for design decisions, specifications, and research. Changes were announced on a dedicated Slack channel.

Work was divided naturally by role but not in isolation:
- **Gosia** drove UX research (including the live usability test with Agata) and design decisions
- **Kasia** owned the product spec — updating the PRD in real time as design evolved
- **Pati** documented design decisions and flagged changes from Figma to the rest of the team
- **Kacper** wrote and continuously updated automated tests, rewriting them each time the implementation changed
- **Jędrzej** maintained the Ways of Working document and contributed to the UX screen list
- **Zenobia** wrote the implementation code, the design log, and the kid-friendly UX analysis

What's notable is that Kacper's tests and Zenobia's code were moving in lockstep — tests were written, broke when the interface changed, and were rewritten immediately. This is a healthy sign: it means the tests were actually testing real behavior rather than being written after the fact.

---

## Where things landed

By end of day, the app had a working weekly event view with full filtering (subject, event type, status, difficulty, date range), a reset button, dark mode, responsive layout, an accessible empty state, and a task detail screen with study materials. The PRD had been updated to v2 with four non-functional requirement sections (accessibility, dark mode, responsiveness, and design system compliance). A real student had tested it.

The foundation is clearly built to grow: the code is structured for adding more tasks, the design system tokens are in place for theming, and the accessibility groundwork means screen reader users aren't an afterthought.