# Product Requirements Document

**Product:**
Study Companion
**Version:** 2.0
**Date:** 4/22/2026
**Owner:**

---

## 1. Overview
This project aims to create a tool that supports learning organization through task &amp; deadline visualization and study planning. The solution is intended to reduce information chaos and strengthen students’ consistency. **Users' ages: 12–15.**
A key requirement from the start is digital accessibility compliant with WCAG 2.2 AA level.

## 2. Goals &amp; Success Metrics
- **Goal 1:** Reduce the time students spend “figuring out what needs to be done” before studying.
- **Metric:** Average self-reported time in a pre/post survey or telemetry time from opening the app to creating a plan/selecting today’s tasks — ≥ 30% decrease within 4 weeks after MVP rollout (pilot group).
- **Goal 2:** Increase on-time task completion.
- **Metric:** Percentage of tasks marked “Done” before the deadline — ≥ 15 pp increase within 6 weeks.
- **Goal 3:** Improve study consistency (regularity).
- **Metric:** Share of weeks in which a student planned and completed at least X study blocks — ≥ 20% increase within 6 weeks.
- **Goal 4:** Ensure WCAG 2.2 AA compliance and eliminate blocking accessibility barriers.
- **Metric:** Accessibility audit results (0 critical/blocking issues) + passing keyboard-only and screen-reader tests for key user journeys.

## 3. User Stories
| As a... | I want to... | So that... |
|---------|-------------|------------|
|    Student     |      see an empty view when there are no tasks today / in the upcoming week       |  I immediately know there is nothing to do          |
|    Student     |      see an empty view when all tasks are completed       |  I know I’m fully up to date          |
|    Student     |      browse tasks in a short perspective (today–tomorrow)       |  I can focus on what’s next right away         |
|    Student     |      browse tasks in a long perspective (next week)       |  I can anticipate workload and deadlines         |
|    Student     |      filter and sort events by subject, difficulty, date, event type, and status       |  I can quickly narrow down to what matters         |
|    Student     |      reset all applied filters at once       |  I can quickly return to the full list of events without clearing each filter manually         |
|    Student     |      search events by text (subject and description fragment)       |  I can find specific items without scrolling         |
|    Student     |      mark a task as done and see the list refresh       |  my current workload is always up to date         |
|    Student     |      access educational materials from Sanoma products linked to events/tasks       |  I can study the relevant content without searching elsewhere         |
|    Student     |      have my events/tasks automatically imported from my e-gradebook       |  I don’t have to re-enter assignments manually         |

## 4. Requirements

### Functional Requirements
[ ] FR-01: Views (empty + short + long)
- Provide empty states:
  - no tasks today / in the upcoming week
  - all tasks completed
- Provide browsing perspectives:
  - short perspective (today–tomorrow)
  - long perspective (next week)

[ ] FR-02: Filtering and sorting (events)
- Filtering and sorting supports:
  - subject
  - difficulty
  - date
  - event type (exam, test, oral exam, written assignment)
  - status (to do, in progress, after deadline, done)
- Provide a "Reset filters" action that clears all applied filters at once and restores the default (unfiltered) view.
  - The action is only enabled when at least one filter is active.

[ ] FR-03: Search (text)
- Enable text search for events by:
  - subject
  - description or any fragment of the description

[ ] FR-04: Completing a task updates the view
- Completing a task changes its status and refreshes the list/view immediately.

[ ] FR-05: Integration of educational materials (Sanoma products only)
- Materials are accessible from the event/task detail view.
- Materials open in an accessible manner (proper labels, keyboard operability).
- External (non-Sanoma) materials are not supported in this phase.

[ ] FR-06: Integration with e-gradebooks
- Automatically import events/tasks (e.g., exams, tests, assignments, deadlines) from supported e-gradebook systems.
- Imported items include at minimum: subject, event type, date/deadline, and description (when available).
- Imported events are kept in sync (additions, updates, and removals reflected in the app).
- Handle authentication/authorization with the e-gradebook provider securely.
- Provide clear error handling and user feedback when sync fails or credentials expire.

### Non-Functional Requirements
[ ] NFR-01: Accessibility (WCAG 2.2 AA)
- Full keyboard support (no focus traps), visible focus and compliant focus appearance.
- No reliance on color alone; text and UI component contrast meets AA requirements.
- Supports text resizing up to 200%.
- Supports “prefers-reduced-motion”.
- Touch/Click target sizes meet WCAG 2.2 (Target Size).
- Correct semantics and compatibility with screen readers on key flows (NVDA/VoiceOver depending on platform).
- Filtering, sorting, searching, empty states, and status updates are fully accessible (proper labels, keyboard operability, and announced changes).

[ ] NFR-02: Dark mode
- Provide both light and dark themes for the entire application UI.
- Respect the user's system preference via `prefers-color-scheme` by default.
- Allow users to manually override the theme (light / dark / system) with the choice persisted across sessions.
- Both light and dark themes must satisfy the accessibility requirements defined in NFR-01 (contrast, focus indicators, no color-only information).
- Images, icons, and status indicators remain legible and distinguishable in both themes.

[ ] NFR-03: Responsiveness (multi-device support)
- Fully responsive layout supporting mobile, tablet, and desktop viewports.
- Mobile-first approach: core user journeys (browsing, filtering, searching, completing tasks) are fully usable on small screens without horizontal scrolling.
- Layout adapts gracefully to portrait and landscape orientations.
- No loss of functionality or content across supported viewport sizes.
- Tested on latest two versions of major browsers (Chrome, Edge, Firefox, Safari) on desktop and mobile.

[ ] NFR-04: Sanoma Learning Design System
- Use Sanoma Learning Design System (SLDS) components as the primary UI building blocks for all screens and flows.
- Follow SLDS design tokens (colors, typography, spacing, elevation, radius) for both light and dark themes rather than defining ad-hoc values.
- Use SLDS patterns and guidelines for common UI elements: buttons, inputs, forms, lists, cards, dialogs, navigation, empty states, and status indicators.
- Custom components are only allowed when an equivalent SLDS component does not exist; custom components must visually and behaviorally align with SLDS guidelines and meet the accessibility requirements defined in NFR-01.
- Icons, illustrations, and typography follow SLDS asset libraries and branding guidelines.
- Keep the SLDS dependency up to date (within the supported version range) to benefit from accessibility and visual consistency improvements.

## 5. Out of Scope
- Possibility of adding external (non-Sanoma) educational materials — later phase.
- Progress bar (visual indicator of task/study completion progress) — later phase.
- Manual event creation by students (adding custom events/tasks) — later phase.

## 6. Future Requirements (later phases)

[ ] FUT-01: Self-marking as "Prepared"
- Allow students to manually mark themselves as prepared for an upcoming event (separate from "Done").
- The status is user-driven; the app does not infer preparedness automatically.

[ ] FUT-02: Auto-hide completed and past events
- Once a task is marked as "Done" or a user self-marks as "Prepared" (per FUT-01), the event is removed from the main list.
- Exam/test events automatically disappear from the list on/after the exam day.
- Hidden events should still be accessible via a dedicated "Archive" or "History" view for reference.

[ ] FUT-03: Teacher-driven changes visible to students
- When a teacher reschedules an event, changes its date, or modifies its scope/description, the change is reflected in the student's view.
- Changed events are highlighted (e.g., badge "Updated") so students immediately notice the difference.
- Provide a change indicator with details of what changed (date, scope, type).

[ ] FUT-04: Teacher-defined difficulty
- Event difficulty (easy / medium / hard) is set by the teacher, not the student.
- Difficulty is imported together with the event (from e-gradebook (e.g., Vulcan) or teacher input) and displayed read-only to students.

[ ] FUT-05: Progress celebration (gamification)
- When a student reaches 100% completion for a given day/week, show a celebratory animation (e.g., confetti).
- Respect "prefers-reduced-motion" (per NFR-01) — provide a non-animated visual alternative (e.g., a static celebratory badge, illustration, or success banner).
- Provide a non-visual alternative so the achievement is perceivable by users who cannot see the animation:
  - Announce the achievement to assistive technologies via an ARIA live region (e.g., "Well done! You've completed all tasks for today.").
  - Optionally play a short, non-intrusive success sound; the sound must be user-configurable (on/off) and must not be the only way the achievement is communicated.
  - Provide a haptic feedback option on supported devices (also user-configurable).
- All alternatives (animation, static visual, sound, haptics) must be independently toggleable in user preferences and respect system-level accessibility settings.
- Celebration is purely feedback and does not block any user action.

[ ] FUT-06: Progress bar
- Visual indicator showing task/study completion progress for the day and/or week.
- Accessible (proper ARIA semantics, not color-only; per NFR-01).

[ ] FUT-07: Manual event creation
- Allow students to add custom events/tasks not imported from the e-gradebook (e.g., personal study blocks, extracurricular assignments).
- Custom events support the same fields as imported ones: subject, event type, date/deadline, description, and (student-assigned) difficulty.
- Custom events are clearly distinguishable from imported ones.

[ ] FUT-08: External (non-Sanoma) educational materials
- Allow linking external learning resources (e.g., URLs, documents) to events/tasks.
- External links open in an accessible manner with clear indication that they lead outside the app.