# test_strategy

## Scope: 
Complete product described in PRD-2.md plus example-study-companion-weekly.md (WeeklyView). Includes UI (Today/Week/Month), panels/cards, popovers, toolbar (search/select/date), progress/metrics, filters, start/pause/resume/finish learning flow, offline/undo, parent view.

## Goals: 
Verify correctness of core flows, accessibility (WCAG AA), responsiveness (mobile-first), reliability under interruptions, acceptable performance for large lists, and identify automation candidates.


## Test Levels & Types

### Integration: 
Component composition (Panel → Card → Popover → Button actions) in isolation (Storybook stories with mocked data).
### E2E: 
Playwright (recommended) covering primary user flows: login → identify closest deadline → sort/filter → start → pause/resume → finish → verify progress; additional flows for divide task, set date, undo.
### Accessibility: 
Axe (automated) + manual keyboard & screen-reader checks for critical flows and icon-only controls.


### Dev/Storybook: 
component tests and visual verification.
### Staging: 
E2E and integration tests against seeded test accounts (StudentA, StudentB, Parent).
### Mobile devices: 
Chromium, WebKit, Firefox via Playwright.


### Seed sets:
Empty: no tasks.
Typical week: mix of statuses and priorities (from example file).

## Acceptance Criteria

Core E2E flows pass ≥95% on staging.
No critical a11y violations on main flow (automated Axe + manual checks).
E2E flakiness < 5%.
Automation Plan

Priority E2E scripts (Playwright):
List load + verify Progress bar + list present.
Identify closest-deadline card → open Popover → start study → pause → resume → finish → assert progress update.
Sort/filter/search scenarios and assert visible list.
Split task into sub-tasks → assert sub-tasks created with deadlines.
Undo after delete/mark-done → assert restore.
Offline-resume: start session → go offline → come back online → resume state.
Component tests: Storybook-driven snapshots + interaction tests for Card, Popover, Date picker, Button bar.

A11y gate: run Axe during CI for stories covering main flow; fail build on critical violations.
CI cadence: Run component & Axe on PR, run E2E nightly + on main-branch merges.
Manual Testing Checklist (for QA sessions)

Verify keyboard-only navigation across Tabs → Panels → Card actions → Popover.
Ensure tooltips and aria-describedby exist for icon-only controls.
Validate Date picker prevents past dates; edge-case dates (month boundaries).
Confirm progress propagation (card → weekly progress).
Test empty states messaging and CTA.
Test parent view is read-only and shows accurate statuses.
Visual checks for color contrast and label redundancy (no color-only meaning).
Metrics & Reporting

Track E2E pass rate, flakiness, a11y violations, average dashboard load time, memory footprint on heavy lists.
Weekly QA report with critical/major bugs and regression checks.
