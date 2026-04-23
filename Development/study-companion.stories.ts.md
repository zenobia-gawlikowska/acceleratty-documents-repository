import {
  faBook,
  faCalculator,
  faCalendarDays,
  faCheck,
  faChevronRight,
  faFilter,
  faFlask,
  faGlobe,
  faPalette,
  faPaperclip,
  faPlus
} from '@fortawesome/pro-regular-svg-icons';
import '@sl-design-system/avatar/register.js';
import '@sl-design-system/badge/register.js';
import '@sl-design-system/button/register.js';
import '@sl-design-system/button-bar/register.js';
import '@sl-design-system/date-field/register.js';
import { Icon } from '@sl-design-system/icon';
import '@sl-design-system/icon/register.js';
import '@sl-design-system/listbox/register.js';
import '@sl-design-system/panel/register.js';
import '@sl-design-system/popover/register.js';
import '@sl-design-system/progress-bar/register.js';
import '@sl-design-system/search-field/register.js';
import '@sl-design-system/select/register.js';
import '@sl-design-system/tabs/register.js';
import '@sl-design-system/toggle-button/register.js';
import '@sl-design-system/tooltip/register.js';
import { type StoryObj } from '@storybook/web-components-vite';
import { html } from 'lit';

Icon.register(
  faBook,
  faCalculator,
  faCalendarDays,
  faCheck,
  faChevronRight,
  faFilter,
  faFlask,
  faGlobe,
  faPalette,
  faPaperclip,
  faPlus
);

type Story = StoryObj;

type Subject = {
  key: string;
  label: string;
  iconName: string;
  color: 'blue' | 'purple' | 'green' | 'orange' | 'red';
};

type Task = {
  id: string;
  title: string;
  subject: Subject;
  due: string;
  durationMin: number;
  priority: 'low' | 'normal' | 'high';
  status: 'todo' | 'in-progress' | 'done';
  progress: number;
  note?: string;
};

const subjects: Subject[] = [
  { key: 'math', label: 'Math', iconName: 'far-calculator', color: 'blue' },
  { key: 'history', label: 'History', iconName: 'far-book', color: 'orange' },
  { key: 'science', label: 'Science', iconName: 'far-flask', color: 'green' },
  { key: 'geography', label: 'Geography', iconName: 'far-globe', color: 'purple' },
  { key: 'art', label: 'Art', iconName: 'far-palette', color: 'red' }
];

const week: Array<{ dayLabel: string; dateLabel: string; tasks: Task[] }> = [
  {
    dayLabel: 'Monday',
    dateLabel: 'Apr 27',
    tasks: [
      {
        id: 'm1',
        title: 'Quadratic equations — test',
        subject: subjects[0],
        due: 'today',
        durationMin: 60,
        priority: 'high',
        status: 'in-progress',
        progress: 40,
        note: 'Chapter 4, exercises 1–12. Focus on discriminant and Vieta’s formulas.'
      },
      {
        id: 'h1',
        title: 'Partitions of Poland — essay outline',
        subject: subjects[1],
        due: 'today',
        durationMin: 45,
        priority: 'normal',
        status: 'todo',
        progress: 10
      }
    ]
  },
  {
    dayLabel: 'Tuesday',
    dateLabel: 'Apr 28',
    tasks: [
      {
        id: 's1',
        title: 'Cell biology — flashcards review',
        subject: subjects[2],
        due: 'tomorrow',
        durationMin: 30,
        priority: 'normal',
        status: 'todo',
        progress: 0
      }
    ]
  },
  {
    dayLabel: 'Wednesday',
    dateLabel: 'Apr 29',
    tasks: [
      {
        id: 'g1',
        title: 'Map quiz — European capitals',
        subject: subjects[3],
        due: 'Apr 29',
        durationMin: 30,
        priority: 'high',
        status: 'todo',
        progress: 20
      },
      {
        id: 'a1',
        title: 'Impressionism — bring portfolio',
        subject: subjects[4],
        due: 'Apr 29',
        durationMin: 20,
        priority: 'low',
        status: 'done',
        progress: 100
      }
    ]
  },
  {
    dayLabel: 'Thursday',
    dateLabel: 'Apr 30',
    tasks: [
      {
        id: 'm2',
        title: 'Fractions — worksheet',
        subject: subjects[0],
        due: 'Apr 30',
        durationMin: 45,
        priority: 'normal',
        status: 'todo',
        progress: 0
      }
    ]
  },
  {
    dayLabel: 'Friday',
    dateLabel: 'May 1',
    tasks: []
  }
];

const priorityBadge = (priority: Task['priority']) => {
  if (priority === 'high') {
    return html`<sl-badge color="red" emphasis="bold" size="sm">High priority</sl-badge>`;
  }
  if (priority === 'normal') {
    return html`<sl-badge color="blue" size="sm">Normal</sl-badge>`;
  }
  return html`<sl-badge color="neutral" size="sm">Low</sl-badge>`;
};

const statusBadge = (status: Task['status']) => {
  if (status === 'done') {
    return html`<sl-badge color="green" size="sm">Done</sl-badge>`;
  }
  if (status === 'in-progress') {
    return html`<sl-badge color="purple" size="sm">In progress</sl-badge>`;
  }
  return html`<sl-badge color="neutral" size="sm">To do</sl-badge>`;
};

// Design decision: no artwork/imagery represents an event (test/quiz/essay),
// so each task is rendered as an sl-panel instead of an sl-card.
const taskPanel = (task: Task) => html`
  <sl-panel heading=${task.title} elevation="raised" collapsible>
    <sl-avatar
      slot="prefix"
      size="sm"
      shape="square"
      style=${`--sl-avatar-background-color: var(--sl-color-palette-${task.subject.color}-100);`}
    >
      <sl-icon slot="fallback" name=${task.subject.iconName}></sl-icon>
    </sl-avatar>

    <span slot="suffix" style="display: inline-flex; gap: 0.5rem; align-items: center; flex-wrap: wrap;">
      <sl-badge color=${task.subject.color} size="sm">${task.subject.label}</sl-badge>
      ${priorityBadge(task.priority)} ${statusBadge(task.status)}
    </span>

    <sl-tooltip slot="actions" id=${`tt-start-${task.id}`} position="top">Start studying this task</sl-tooltip>
    <sl-button slot="actions" variant="primary" aria-describedby=${`tt-start-${task.id}`}>Start</sl-button>

    <div style="display: grid; gap: 0.75rem;">
      <span style="color: var(--sl-color-foreground-muted); font: var(--sl-text-body-xs);">
        Due ${task.due} · ${task.durationMin} min · priority: ${task.priority}
      </span>
      <sl-progress-bar .value=${task.progress} label="Progress on this task">
        ${task.progress}% complete
      </sl-progress-bar>
      ${task.note ? html`<p style="margin: 0;">${task.note}</p>` : null}
      <sl-button-bar align="end">
        <sl-button fill="outline">
          <sl-icon name="far-paperclip"></sl-icon>
          Attach material
        </sl-button>
        <sl-button fill="outline">Mark as done</sl-button>
      </sl-button-bar>
    </div>
  </sl-panel>
`;

const dayPanel = (day: (typeof week)[number]) => html`
  <sl-panel heading=${`${day.dayLabel} · ${day.dateLabel}`} collapsible>
    <sl-badge slot="prefix" size="sm" color="neutral">${day.tasks.length}</sl-badge>
    <sl-tooltip slot="actions" id=${`tt-add-${day.dayLabel}`} position="top">Add task for ${day.dayLabel}</sl-tooltip>
    <sl-button
      slot="actions"
      fill="ghost"
      aria-label=${`Add task for ${day.dayLabel}`}
      aria-describedby=${`tt-add-${day.dayLabel}`}
    >
      <sl-icon name="far-plus"></sl-icon>
    </sl-button>

    ${day.tasks.length === 0
      ? html`<p style="margin: 0; color: var(--sl-color-foreground-muted);">No tasks planned.</p>`
      : html`<div style="display: grid; gap: 0.75rem;">${day.tasks.map(taskPanel)}</div>`}
  </sl-panel>
`;

export default {
  title: 'Examples/Study Companion'
};

/**
 * User story: _As a student, I want to see all tasks for the week in a clear view,
 * so I can plan my study time and priorities quickly._
 *
 * Composition:
 * - Tabs — switch the time horizon (Today / This week / Month)
 * - Progress bar — weekly preparation summary
 * - Search field + Select + Date picker + Toggle button group — filter & focus the week
 * - Panel — two roles:
 *     1. one per day, collapsible so the student sees only what matters right now
 *     2. one per task (design decision: no imagery for events → Panel replaces Card),
 *        carrying Avatar + Icon (subject) in the prefix, Badges (subject / priority / status)
 *        in the suffix, a primary Start action, and per-task Progress bar + actions inside
 * - Popover — quick-add popover anchored to the global "New task" button
 * - Tooltip — labels for icon-only controls (WCAG 2.2 target-size & non-visual context)
 */
export const WeeklyView: Story = {
  render: () => html`
    <style>
      .study-dashboard {
        display: grid;
        gap: 1rem;
        max-inline-size: 960px;
        margin: 0 auto;
        padding: 1rem;
        font: var(--sl-text-body-md);
      }
      .dashboard-header {
        display: flex;
        align-items: center;
        gap: 0.75rem;
        flex-wrap: wrap;
      }
      .dashboard-header h1 {
        margin: 0;
        font: var(--sl-text-heading-lg);
        flex: 1 1 auto;
      }
      .dashboard-toolbar {
        display: grid;
        grid-template-columns: 1fr auto auto;
        gap: 0.75rem;
        align-items: end;
      }
      .subject-filters {
        display: flex;
        gap: 0.5rem;
        flex-wrap: wrap;
      }
      .week-summary {
        display: grid;
        gap: 0.25rem;
      }
      sl-progress-bar {
        max-inline-size: 33.3333%;
      }
      @media (max-inline-size: 720px) {
        .dashboard-toolbar {
          grid-template-columns: 1fr;
        }
      }
    </style>

    <div class="study-dashboard">
      <header class="dashboard-header">
        <sl-avatar display-name="Ada Kowalska" size="md"></sl-avatar>
        <h1>This week</h1>

        <sl-tooltip id="tt-add-global" position="bottom">Add a new task</sl-tooltip>
        <sl-button id="btn-new-task" variant="primary" aria-describedby="tt-add-global">
          <sl-icon name="far-plus"></sl-icon>
          New task
        </sl-button>
        <sl-popover anchor="btn-new-task" position="bottom-end">
          <div style="display: grid; gap: 0.5rem; max-inline-size: 280px;">
            <strong>Quick add</strong>
            <sl-search-field aria-label="Task title" placeholder="What do you need to do?"></sl-search-field>
            <sl-select aria-label="Subject" placeholder="Subject">
              ${subjects.map(s => html`<sl-option value=${s.key}>${s.label}</sl-option>`)}
            </sl-select>
            <sl-date-field aria-label="Due date"></sl-date-field>
            <sl-button-bar align="end"><sl-button variant="primary" size="sm">Add task</sl-button></sl-button-bar>
          </div>
        </sl-popover>
      </header>

      <section class="week-summary" aria-label="Weekly preparation">
        <sl-progress-bar .value=${42} label="Weekly preparation">
          <strong>42% ready</strong> · 5 of 12 tasks done this week
        </sl-progress-bar>
      </section>

      <div class="dashboard-toolbar">
        <sl-search-field aria-label="Search tasks" placeholder="Search tasks, subjects, materials…"></sl-search-field>

        <sl-select aria-label="Filter by subject" placeholder="All subjects">
          <sl-option value="all">All subjects</sl-option>
          ${subjects.map(
            s => html`
              <sl-option value=${s.key}>
                <sl-icon name=${s.iconName} slot="prefix"></sl-icon>
                ${s.label}
              </sl-option>
            `
          )}
        </sl-select>

        <sl-date-field aria-label="Jump to date"></sl-date-field>
      </div>

      <div class="subject-filters" role="group" aria-label="Quick subject filters">
        ${subjects.map(
          s => html`
            <sl-tooltip id=${`tt-${s.key}`} position="top">Show only ${s.label}</sl-tooltip>
            <sl-toggle-button fill="outline" shape="pill" aria-label=${s.label} aria-describedby=${`tt-${s.key}`}>
              <sl-icon name=${s.iconName} slot="default"></sl-icon>
              <sl-icon name="far-check" slot="pressed"></sl-icon>
            </sl-toggle-button>
          `
        )}
        <sl-tooltip id="tt-more-filters" position="top">More filters (status, priority, duration)</sl-tooltip>
        <sl-toggle-button fill="outline" shape="pill" aria-label="More filters" aria-describedby="tt-more-filters">
          <sl-icon name="far-filter" slot="default"></sl-icon>
          <sl-icon name="far-check" slot="pressed"></sl-icon>
        </sl-toggle-button>
      </div>

      <sl-tab-group>
        <sl-tab>
          Today
          <sl-badge color="red" size="sm" style="margin-inline-start: 0.25rem;">2</sl-badge>
        </sl-tab>
        <sl-tab selected>
          This week
          <sl-badge color="blue" size="sm" style="margin-inline-start: 0.25rem;">6</sl-badge>
        </sl-tab>
        <sl-tab>
          Month
          <sl-badge color="neutral" size="sm" style="margin-inline-start: 0.25rem;">14</sl-badge>
        </sl-tab>

        <sl-tab-panel>
          <div style="display: grid; gap: 0.75rem;">${week[0].tasks.map(taskPanel)}</div>
        </sl-tab-panel>

        <sl-tab-panel>
          <div style="display: grid; gap: 0.75rem;">${week.map(dayPanel)}</div>
        </sl-tab-panel>

        <sl-tab-panel>
          <p style="color: var(--sl-color-foreground-muted);">
            Month view is not part of this user story — see the Today and This week tabs.
          </p>
        </sl-tab-panel>
      </sl-tab-group>
    </div>
  `
};
