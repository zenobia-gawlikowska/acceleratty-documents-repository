# Product Requirements Document

**Product:**
Study Companion
**Version:** 1.0
**Date:** 4/22/2026
**Owner:**

---

## 1. Overview
This project aims to create a tool that supports learning organization through task & deadline visualization and study planning. The solution is intended to reduce information chaos and strengthen students’ consistency.
A key requirement from the start is digital accessibility compliant with WCAG 2.2 AA level.

## 2. Goals & Success Metrics
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
|    Student     |      see all tasks for the week in a clear view       |  I can plan my study time and priorities quickly          |
|    Student     |      add a task with a due date, priority, and estimated time       |  I know what’s urgent and how long it will take          |
|    Student     |      attach materials to a task (link/file/note)       |  everything is in one place and I don’t have to search          |
|    Student     |      filter and sort tasks (subject/status/date/priority)       |  I can focus on what matters most right now          |
|    Student     |      track progress (To do / In progress / Done)       |  I can see real progress and reduce stress         |

## 4. Requirements

### Functional Requirements
[ ] FR-01: Task views (list + weekly planner)
- Provide a task list view and a weekly view (calendar/planner).
- Each task shows: title, due date, category/subject, status, priority, estimated time.
[ ] FR-02: Create and edit tasks
- Task form: title (required), due date (required), description, category, priority, estimated time, optional checklist steps.
- Validation and error messages.
[ ] FR-03: Filtering and sorting
- Filters: subject/category, status, date range (today/week/month), priority.
- Sorting: due date, priority, estimated time.
[ ] FR-04: Study block planning
- Assign tasks to days and time blocks (e.g., 30/45/60 minutes).
- Indicate overload conflicts (e.g., “planned 4h vs limit 2h”).
[ ] FR-05: Progress and basic stats
- Change task status (To do / In progress / Done).
- Basic summaries: completed this week, overdue, remaining estimated time.
[ ] FR-06: Reminders/notifications
- Remind 24h before the deadline and on the due date.
- Allow configuration or disabling.
[ ] FR-07: Drag & drop alternative (if drag & drop is introduced)
If planning/reordering uses drag & drop, provide an equivalent method: “move up/down” buttons, “assign to day” dropdown, keyboard shortcuts.

### Non-Functional Requirements
[ ] NFR-01: Accessibility (WCAG 2.2 AA)
- Full keyboard support (no focus traps), visible focus and compliant focus appearance.
- No reliance on color alone; text and UI component contrast meets AA requirements.
- Supports text resizing up to 200% and responsive layouts.
- Supports “prefers-reduced-motion”.
- Touch/Click target sizes meet WCAG 2.2 (Target Size).
- Alternative to dragging (Dragging Movements).
- Correct semantics and compatibility with screen readers on key flows (NVDA/VoiceOver depending on platform).


## 5. Out of Scope
- Integrations with e-gradebooks / LMS (automatic assignment import) — later phase.
- Integration of educational materials

