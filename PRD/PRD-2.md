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
- Supports text resizing up to 200% and responsive layouts.
- Supports “prefers-reduced-motion”.
- Touch/Click target sizes meet WCAG 2.2 (Target Size).
- Correct semantics and compatibility with screen readers on key flows (NVDA/VoiceOver depending on platform).
- Filtering, sorting, searching, empty states, and status updates are fully accessible (proper labels, keyboard operability, and announced changes).


## 5. Out of Scope
- Integrations with e-gradebooks / LMS (automatic assignment import) — later phase.
- Possibility of adding external (non-Sanoma) educational materials — later phase.
- Progress bar (visual indicator of task/study completion progress) — later phase.
- Manual event creation by students (adding custom events/tasks) — later phase.