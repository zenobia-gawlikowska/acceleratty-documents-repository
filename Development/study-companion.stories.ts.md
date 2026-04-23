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
  faPlus
} from '@fortawesome/pro-regular-svg-icons';
import {
  faBook as fasBook,
  faCalculator as fasCalculator,
  faFlask as fasFlask,
  faGlobe as fasGlobe,
  faPalette as fasPalette
} from '@fortawesome/pro-solid-svg-icons';
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
  faPlus,
  fasBook,
  fasCalculator,
  fasFlask,
  fasGlobe,
  fasPalette
);

type Story = StoryObj;

type Subject = {
  key: string;
  label: string;
  iconName: string;
  iconNamePressed: string;
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
  { key: 'math', label: 'Math', iconName: 'far-calculator', iconNamePressed: 'fas-calculator', color: 'blue' },
  { key: 'history', label: 'History', iconName: 'far-book', iconNamePressed: 'fas-book', color: 'orange' },
  { key: 'science', label: 'Science', iconName: 'far-flask', iconNamePressed: 'fas-flask', color: 'green' },
  { key: 'geography', label: 'Geography', iconName: 'far-globe', iconNamePressed: 'fas-globe', color: 'purple' },
  { key: 'art', label: 'Art', iconName: 'far-palette', iconNamePressed: 'fas-palette', color: 'red' }
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
        progress: 10,
        note: 'Draft a 5-paragraph outline covering the three partitions (1772, 1793, 1795) and their causes.'
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
        progress: 0,
        note: 'Go through the organelles deck twice and mark the cards you get wrong for a second pass.'
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
        progress: 20,
        note: 'Focus on Central and Eastern Europe — those are the ones you keep mixing up.'
      },
      {
        id: 'a1',
        title: 'Impressionism — bring portfolio',
        subject: subjects[4],
        due: 'Apr 29',
        durationMin: 20,
        priority: 'low',
        status: 'done',
        progress: 100,
        note: 'Portfolio is packed by the door. Don’t forget the sketchbook from last week.'
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
        progress: 0,
        note: 'Worksheet from Mr. Nowak — questions 1 to 15, show your working for the tricky ones.'
      }
    ]
  },
  {
    dayLabel: 'Friday',
    dateLabel: 'May 1',
    tasks: []
  }
];

// Status shown as a single subtle large badge in the panel suffix
// (matches Figma frame node 821:2537).
const statusBadge = (status: Task['status']) => {
  if (status === 'done') {
    return html`<sl-badge color="green" emphasis="subtle" size="lg">Done</sl-badge>`;
  }
  if (status === 'in-progress') {
    return html`<sl-badge color="blue" emphasis="subtle" size="lg">In progress</sl-badge>`;
  }
  return html`<sl-badge color="orange" emphasis="subtle" size="lg">To do</sl-badge>`;
};

// Spiciness = priority. Peppers match the difficulty select labels:
// low = 🌶️ (easy), normal = 🌶️🌶️ (medium), high = 🌶️🌶️🌶️ (hard).
const spice = (priority: Task['priority']) => {
  if (priority === 'high') return ' 🌶️🌶️🌶️';
  if (priority === 'normal') return ' 🌶️🌶️';
  return ' 🌶️';
};

// Design decision: no artwork/imagery represents an event (test/quiz/essay),
// so each task is rendered as an sl-panel instead of an sl-card.
// Layout mirrors the Figma frame `sl-panel-default` (Multitenant materials, 821:2537):
// icon prefix, title heading, subtle status badge suffix, divider, body with
// subject line + description, and a primary pill `Start` action in a button-bar.
// A task's "topic" is the part of the title before " — " (e.g. "Quadratic equations"
// from "Quadratic equations — test"). Used to populate the topic filter select.
const topicOf = (task: Task) => task.title.split(' — ')[0].trim();
// A task's "type" is the part after " — " (e.g. "test", "essay outline"). Shown
// as a subtitle line in the panel header, alongside the difficulty peppers.
const typeOf = (task: Task) => {
  const parts = task.title.split(' — ');
  if (parts.length < 2) return '';
  const type = parts.slice(1).join(' — ').trim();
  return type.charAt(0).toUpperCase() + type.slice(1);
};
const topicSlug = (topic: string) =>
  topic
    .toLowerCase()
    .replaceAll(/[^a-z0-9]+/g, '-')
    .replaceAll(/^-|-$/g, '');

const allTopics: Array<{ slug: string; label: string }> = Array.from(
  new Map(week.flatMap(d => d.tasks.map(t => [topicSlug(topicOf(t)), topicOf(t)] as const))).entries()
)
  .map(([slug, label]) => ({ slug, label }))
  .sort((a, b) => a.label.localeCompare(b.label));

const taskAction = (task: Task) => {
  if (task.status === 'done') return null;
  if (task.status === 'in-progress') {
    return html`
      <sl-tooltip id=${`tt-continue-${task.id}`} position="top">Pick up where you left off</sl-tooltip>
      <sl-button fill="link" aria-describedby=${`tt-continue-${task.id}`}>Continue</sl-button>
    `;
  }
  return html`
    <sl-tooltip id=${`tt-start-${task.id}`} position="top">Jump in and start this one</sl-tooltip>
    <sl-button variant="primary" shape="pill" aria-describedby=${`tt-start-${task.id}`}>Start</sl-button>
  `;
};

const taskPanel = (task: Task) => html`
  <sl-panel
    class="task-panel"
    data-subject=${task.subject.label}
    data-subject-key=${task.subject.key}
    data-priority=${task.priority}
    data-topic=${topicSlug(topicOf(task))}
    divider
  >
    <sl-icon slot="prefix" name=${task.subject.iconName}></sl-icon>
    <div slot="heading" class="task-heading">
      <span class="task-heading__title">${topicOf(task)}</span>
      <span class="task-heading__subtitle">${typeOf(task)}${spice(task.priority)}</span>
    </div>
    <span slot="aside" class="task-aside">${statusBadge(task.status)}</span>

    <div style="display: grid; gap: 1rem;">
      <div style="display: grid; gap: 0.5rem;">
        <p style="margin: 0;">
          <sl-icon name=${task.subject.iconName} style="vertical-align: -0.15em;"></sl-icon>
          ${task.subject.label}
        </p>
        ${task.note ? html`<p style="margin: 0;">${task.note}</p>` : null}
      </div>

      <sl-button-bar align="end"> ${taskAction(task)} </sl-button-bar>
    </div>
  </sl-panel>
`;

const dayPanel = (day: (typeof week)[number]) => html`
  <sl-panel heading=${`${day.dayLabel} · ${day.dateLabel}`}>
    <sl-badge slot="prefix" size="sm" color="neutral">${day.tasks.length}</sl-badge>
    <sl-tooltip slot="actions" id=${`tt-add-${day.dayLabel}`} position="top"
      >Add something for ${day.dayLabel}</sl-tooltip
    >
    <sl-button
      slot="actions"
      fill="ghost"
      shape="pill"
      aria-label=${`Add something for ${day.dayLabel}`}
      aria-describedby=${`tt-add-${day.dayLabel}`}
    >
      <sl-icon name="far-plus"></sl-icon>
    </sl-button>

    ${day.tasks.length === 0
      ? html`<p style="margin: 0; color: var(--sl-color-foreground-muted);">No tasks planned.</p>`
      : html`<div style="display: grid; gap: 1.5rem;">${day.tasks.map(taskPanel)}</div>`}
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
  render: () => {
    const difficultyToPriority: Record<string, Task['priority']> = {
      easy: 'low',
      medium: 'normal',
      hard: 'high'
    };

    const applyFiltersToRoot = (root: Element) => {
      const toolbar = root.querySelector('.dashboard-toolbar');
      const subjectFilters = root.querySelector('.subject-filters');
      if (!toolbar || !subjectFilters) return;

      const searchField = toolbar.querySelector<HTMLInputElement>('sl-search-field');
      const query = (searchField?.value ?? '').toString().trim().toLowerCase();

      const selects = toolbar.querySelectorAll<HTMLElement & { value?: string }>('sl-select');
      const subjectValue = selects[0]?.value ?? 'all';
      const difficultyValue = selects[1]?.value ?? 'all';
      const topicValue = selects[2]?.value ?? 'all';

      const pressedSubjectKeys = new Set<string>();
      subjectFilters
        .querySelectorAll<HTMLElement & { pressed?: boolean }>('sl-toggle-button[data-subject-key]')
        .forEach(tb => {
          if (tb.pressed) pressedSubjectKeys.add(tb.dataset.subjectKey ?? '');
        });

      const taskMatches = (t: HTMLElement) => {
        const heading = (t.querySelector('[slot="heading"]')?.textContent ?? '').toLowerCase();
        const subjectLabel = (t.dataset.subject ?? '').toLowerCase();
        const subjectKey = t.dataset.subjectKey ?? '';
        const priority = t.dataset.priority ?? '';
        const topic = t.dataset.topic ?? '';

        if (query && !heading.includes(query) && !subjectLabel.includes(query)) return false;
        if (subjectValue !== 'all' && subjectKey !== subjectValue) return false;
        if (difficultyValue !== 'all' && priority !== difficultyToPriority[difficultyValue]) return false;
        if (topicValue !== 'all' && topic !== topicValue) return false;
        if (pressedSubjectKeys.size > 0 && !pressedSubjectKeys.has(subjectKey)) return false;

        return true;
      };

      const taskPanels = root.querySelectorAll<HTMLElement>('sl-tab-panel sl-panel[data-subject-key]');
      const dayVisibility = new Map<HTMLElement, boolean>();

      taskPanels.forEach(t => {
        const ok = taskMatches(t);
        t.style.display = ok ? '' : 'none';
        const day = t.parentElement?.closest<HTMLElement>('sl-panel:not([data-subject-key])');
        if (day) dayVisibility.set(day, (dayVisibility.get(day) ?? false) || ok);
      });

      dayVisibility.forEach((visible, day) => {
        day.style.display = visible ? '' : 'none';
      });
    };

    const applyFilters = (event: Event) => {
      const root = (event.target as HTMLElement).closest('.study-dashboard');
      if (root) applyFiltersToRoot(root);
    };

    return html`
      <style>
        .top-bar {
          display: flex;
          align-items: center;
          justify-content: space-between;
          padding: 8px 64px;
          background: #fff;
          box-shadow:
            0 1px 1px rgba(0, 0, 0, 0.09),
            0 3px 2px rgba(0, 0, 0, 0.05);
        }
        .top-bar__logo {
          display: flex;
          align-items: center;
          gap: 1.6px;
          height: 30px;
        }
        .top-bar__logomark {
          width: 22.849px;
          height: 25.044px;
          display: flex;
          align-items: center;
          justify-content: center;
        }
        .top-bar__logomark img {
          width: 21.104px;
          height: 16px;
          transform: rotate(-75deg);
          display: block;
        }
        .top-bar__wordmark {
          width: 201.356px;
          height: 22px;
          display: block;
        }
        .top-bar__profile {
          display: flex;
          align-items: center;
          gap: 8px;
          color: #222;
          font: var(--sl-text-body-md);
        }
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
        /* Kid-friendly: rounder panels, softer shadows, chunkier spacing */
        .study-dashboard sl-panel {
          --sl-panel-border-radius: 1.25rem;
          border-radius: 1.25rem;
          overflow: hidden;
        }
        /* Task panel header layout matches Figma (node 869:10300):
           padding 14px 16px 14px 24px, 6px gap between prefix/title,
           icon aligned with the title's first line. */
        .task-panel::part(header) {
          padding: 14px 16px 14px 24px;
          min-block-size: 0;
          gap: 6px;
        }
        .task-panel::part(wrapper) {
          align-items: start;
          gap: 6px;
        }
        .task-panel > sl-icon[slot='prefix'] {
          /* Visually center the 16px icon on the title glyph (not just the line box). */
          block-size: 20px;
          display: inline-flex;
          align-items: center;
          padding-block-start: 8px;
        }
        /* Stacked title + event-type subtitle in the task panel header
           (Figma: Multitenant materials, node 869:10300). */
        .task-heading {
          display: grid;
          gap: 4px;
          /* Override the panel's default heading padding so the title aligns
             with the prefix icon at the top of the header. */
          padding-block: 0 !important;
        }
        .task-heading__title {
          font-size: 16px;
          font-weight: var(--sl-text-new-typeset-fontWeight-semiBold, 500);
          line-height: 20px;
          color: var(--sl-color-foreground-bold);
        }
        .task-heading__subtitle {
          font-size: 14px;
          font-weight: var(--sl-text-new-typeset-fontWeight-regular, 400);
          line-height: 20px;
          color: var(--sl-color-foreground-plain);
        }
        /* Top-align the status badge with the title (not centered on the stacked heading). */
        .task-aside {
          display: flex;
          justify-content: flex-end;
          align-self: start;
          padding-block-start: 14px;
          padding-inline-end: 16px;
        }
        /* Figma body padding: 16px 24px 24px 24px (spacing below the divider). */
        .task-panel::part(content) {
          padding: 16px 24px 24px 24px;
        }
        .study-dashboard sl-search-field,
        .study-dashboard sl-select,
        .study-dashboard sl-date-field {
          --sl-input-border-radius: 999px;
        }
        @media (max-inline-size: 720px) {
          .dashboard-toolbar {
            grid-template-columns: 1fr;
          }
        }
      </style>

      <header class="top-bar" role="banner">
        <div class="top-bar__logo" aria-label="Study Companion">
          <span class="top-bar__logomark" aria-hidden="true">
            <img src="/images/study-companion/logomark.svg" alt="" />
          </span>
          <img class="top-bar__wordmark" src="/images/study-companion/wordmark.svg" alt="Study Companion" />
        </div>
        <div class="top-bar__profile">
          <sl-avatar display-name="Ada Kowalska" size="md" picture-url="/images/avatar-3.jpg">
            <sl-badge
              slot="badge"
              color="red"
              emphasis="bold"
              size="sm"
              role="status"
              aria-label="3 unread notifications"
            ></sl-badge>
          </sl-avatar>
        </div>
      </header>

      <main class="study-dashboard">
        <header class="dashboard-header">
          <sl-avatar display-name="Ada Kowalska" size="md"></sl-avatar>
          <h1>Hi, Ada — this is your week</h1>

          <sl-tooltip id="tt-add-global" position="bottom">Add something new</sl-tooltip>
          <sl-button id="btn-new-task" variant="primary" shape="pill" size="lg" aria-describedby="tt-add-global">
            <sl-icon name="far-plus"></sl-icon>
            Add something
          </sl-button>
          <sl-popover anchor="btn-new-task" position="bottom-end">
            <div style="display: grid; gap: 0.5rem; max-inline-size: 280px;">
              <strong>What do you want to add?</strong>
              <sl-search-field aria-label="Task title" placeholder="e.g. Math test on Friday"></sl-search-field>
              <sl-select aria-label="Subject" placeholder="Which subject?">
                ${subjects.map(s => html`<sl-option value=${s.key}>${s.label}</sl-option>`)}
              </sl-select>
              <sl-date-field aria-label="Due date"></sl-date-field>
              <sl-button-bar align="end"
                ><sl-button variant="primary" shape="pill" size="sm">Add it</sl-button></sl-button-bar
              >
            </div>
          </sl-popover>
        </header>

        <section class="week-summary" aria-label="Your week so far">
          <sl-progress-bar .value=${42} label="Your week so far">
            <strong>5 down, 7 to go</strong> · you're doing great 🎯
          </sl-progress-bar>
        </section>

        <div class="dashboard-toolbar">
          <sl-search-field
            aria-label="Search tasks"
            placeholder="Search tasks, subjects, materials…"
            @sl-change=${applyFilters}
            @keydown=${(e: KeyboardEvent) => {
              if (e.key === 'Enter') applyFilters(e);
            }}
          ></sl-search-field>
          <sl-button aria-label="Search" variant="primary" @click=${applyFilters}>Search</sl-button>

          <sl-select aria-label="Filter by subject" placeholder="All subjects" @sl-change=${applyFilters}>
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

          <sl-select aria-label="Filter by difficulty" value="all" @sl-change=${applyFilters}>
            <sl-option value="all">All</sl-option>
            <sl-option value="easy">Easy 🌶️</sl-option>
            <sl-option value="medium">Medium 🌶️🌶️</sl-option>
            <sl-option value="hard">Hard 🌶️🌶️🌶️</sl-option>
          </sl-select>

          <sl-select aria-label="Filter by topic" value="all" @sl-change=${applyFilters}>
            <sl-option value="all">All topics</sl-option>
            ${allTopics.map(t => html`<sl-option value=${t.slug}>${t.label}</sl-option>`)}
          </sl-select>

          <sl-date-field aria-label="Jump to date"></sl-date-field>
        </div>

        <div class="subject-filters" role="group" aria-label="Quick subject filters">
          ${subjects.map(
            s => html`
              <sl-tooltip id=${`tt-${s.key}`} position="top">Show only ${s.label}</sl-tooltip>
              <sl-toggle-button
                fill="outline"
                shape="pill"
                data-subject-key=${s.key}
                aria-label=${s.label}
                aria-describedby=${`tt-${s.key}`}
                @sl-toggle=${applyFilters}
              >
                <sl-icon name=${s.iconName} slot="default"></sl-icon>
                <sl-icon name=${s.iconNamePressed} slot="pressed"></sl-icon>
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
            <sl-badge color="orange" size="sm" style="margin-inline-start: 0.25rem;">2</sl-badge>
          </sl-tab>
          <sl-tab selected>
            This week
            <sl-badge color="blue" size="sm" style="margin-inline-start: 0.25rem;">6</sl-badge>
          </sl-tab>
          <sl-tab>
            Later
            <sl-badge color="neutral" size="sm" style="margin-inline-start: 0.25rem;">14</sl-badge>
          </sl-tab>

          <sl-tab-panel>
            <div style="display: grid; gap: 1.5rem;">${week[0].tasks.map(taskPanel)}</div>
          </sl-tab-panel>

          <sl-tab-panel>
            <div style="display: grid; gap: 1.5rem;">${week.map(dayPanel)}</div>
          </sl-tab-panel>

          <sl-tab-panel>
            <p style="color: var(--sl-color-foreground-muted);">
              The "Later" view isn't built yet — check "Today" or "This week" for what's coming up.
            </p>
          </sl-tab-panel>
        </sl-tab-group>
      </main>
    `;
  }
};
