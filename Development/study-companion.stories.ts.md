import {
  faBook,
  faCalculator,
  faFilePen,
  faFlask,
  faGlobe,
  faHeadSideSpeak,
  faPalette,
  faPenField,
  faTimer,
  faXmark
} from '@fortawesome/pro-regular-svg-icons';
import '@sl-design-system/avatar/register.js';
import '@sl-design-system/badge/register.js';
import '@sl-design-system/button/register.js';
import '@sl-design-system/button-bar/register.js';
import '@sl-design-system/date-field/register.js';
import '@sl-design-system/form/register.js';
import { Icon } from '@sl-design-system/icon';
import '@sl-design-system/icon/register.js';
import '@sl-design-system/listbox/register.js';
import '@sl-design-system/panel/register.js';
import '@sl-design-system/search-field/register.js';
import '@sl-design-system/select/register.js';
import '@sl-design-system/tooltip/register.js';
import { type StoryObj } from '@storybook/web-components-vite';
import { html } from 'lit';

Icon.register(
  faBook,
  faCalculator,
  faFilePen,
  faFlask,
  faGlobe,
  faHeadSideSpeak,
  faPalette,
  faPenField,
  faTimer,
  faXmark
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
    dayLabel: 'Today',
    dateLabel: 'Thursday (23/04/2026)',
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
        title: 'Partitions of Poland — written assignment',
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
    dayLabel: 'Tomorrow',
    dateLabel: 'Friday (24/04/2026)',
    tasks: [
      {
        id: 's1',
        title: 'Cell biology — exam',
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
    dayLabel: 'Saturday',
    dateLabel: 'Saturday (25/04/2026)',
    tasks: [
      {
        id: 'g1',
        title: 'European capitals — oral exam',
        subject: subjects[3],
        due: '25/04/2026',
        durationMin: 30,
        priority: 'high',
        status: 'todo',
        progress: 20,
        note: 'Focus on Central and Eastern Europe — those are the ones you keep mixing up.'
      },
      {
        id: 'a1',
        title: 'Impressionism — written assignment',
        subject: subjects[4],
        due: '25/04/2026',
        durationMin: 20,
        priority: 'low',
        status: 'done',
        progress: 100,
        note: 'Portfolio is packed by the door. Don’t forget the sketchbook from last week.'
      }
    ]
  },
  {
    dayLabel: 'Monday',
    dateLabel: 'Monday (27/04/2026)',
    tasks: [
      {
        id: 'm2',
        title: 'Fractions — test',
        subject: subjects[0],
        due: '27/04/2026',
        durationMin: 45,
        priority: 'normal',
        status: 'todo',
        progress: 0,
        note: 'Worksheet from Mr. Nowak — questions 1 to 15, show your working for the tricky ones.'
      }
    ]
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
// Icon per event-type category (shown next to the title in the panel prefix).
// Unknown types fall back to the task's subject icon.
const eventTypeIcons: Record<string, string> = {
  exam: 'far-pen-field',
  test: 'far-timer',
  'oral-exam': 'far-head-side-speak',
  'written-assignment': 'far-file-pen'
};

// Extract an ISO date string (YYYY-MM-DD) from a day.dateLabel like
// "Thursday (23/04/2026)". Used to filter tasks by the From/To date pickers.
const parseDayDate = (dateLabel: string): string => {
  const match = /\((\d{2})\/(\d{2})\/(\d{4})\)/.exec(dateLabel);
  if (!match) return '';
  const [, day, month, year] = match;
  return `${year}-${month}-${day}`;
};

const topicSlug = (topic: string) =>
  topic
    .toLowerCase()
    .replaceAll(/[^a-z0-9]+/g, '-')
    .replaceAll(/^-|-$/g, '');

const allEventTypes: Array<{ slug: string; label: string }> = Array.from(
  new Map(
    week.flatMap(d =>
      d.tasks.map(t => {
        const label = typeOf(t);
        return [topicSlug(label), label] as const;
      })
    )
  ).entries()
)
  .filter(([slug]) => slug !== '')
  .map(([slug, label]) => ({ slug, label }))
  .sort((a, b) => a.label.localeCompare(b.label));

const taskAction = (task: Task) => {
  if (task.status === 'done') return null;
  const topic = topicOf(task);
  if (task.status === 'in-progress') {
    return html` <sl-button fill="link" aria-label=${`Continue ${topic}`}>Continue</sl-button> `;
  }
  return html` <sl-button variant="primary" shape="pill" aria-label=${`Start ${topic}`}>Start</sl-button> `;
};

const taskPanel = (task: Task, isoDate: string) => {
  const typeSlug = topicSlug(typeOf(task));
  const prefixIcon = eventTypeIcons[typeSlug] ?? task.subject.iconName;

  return html`
    <sl-panel
      class="task-panel"
      data-subject=${task.subject.label}
      data-subject-key=${task.subject.key}
      data-priority=${task.priority}
      data-status=${task.status}
      data-topic=${topicSlug(topicOf(task))}
      data-type=${typeSlug}
      data-date=${isoDate}
      divider
    >
      <sl-icon slot="prefix" name=${prefixIcon}></sl-icon>
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
};

const daySection = (day: (typeof week)[number]) => {
  const isoDate = parseDayDate(day.dateLabel);
  return day.tasks.length === 0
    ? null
    : html`
        <section class="day-section" data-day=${day.dayLabel} data-date=${isoDate}>
          <header class="day-section__header">
            <span class="day-section__label">${day.dayLabel}</span>
            <span class="day-section__date">${day.dateLabel}</span>
          </header>
          <div class="day-section__tasks">${day.tasks.map(t => taskPanel(t, isoDate))}</div>
        </section>
      `;
};

export default {
  title: 'Examples/Study Companion'
};

/**
 * User story: _As a student, I want to see my upcoming events grouped by day,
 * so I can plan my study time and priorities quickly._
 *
 * Composition:
 * - Search field + Button — look up events by subject or description
 * - Select (x4) — filter by subject, event type, status and difficulty
 * - Date field (x2) — narrow the range with From/To dates
 * - Day section — a simple header (day label + date) followed by a list of event panels
 * - Panel — one per event (design decision: no imagery for events → Panel replaces Card),
 *     with a subject icon prefix, title + event-type subtitle, status badge suffix,
 *     divider, a body with the subject line + description, and a primary pill Start action
 * - Popover — quick-add popover anchored to the global "Add something" button
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
      if (!toolbar) return;

      const searchField = toolbar.querySelector<HTMLInputElement>('sl-search-field');
      const query = (searchField?.value ?? '').toString().trim().toLowerCase();

      const selects = toolbar.querySelectorAll<HTMLElement & { value?: string }>('sl-select');
      const subjectValue = selects[0]?.value ?? 'all';
      const eventTypeValue = selects[1]?.value ?? 'all';
      const statusValue = selects[2]?.value ?? 'all';
      const difficultyValue = selects[3]?.value ?? 'all';

      const dateFields = toolbar.querySelectorAll<HTMLElement & { value?: Date }>('sl-date-field');
      const toIso = (d?: Date) => {
        if (!d) return '';
        const y = d.getFullYear(),
          m = String(d.getMonth() + 1).padStart(2, '0'),
          day = String(d.getDate()).padStart(2, '0');
        return `${y}-${m}-${day}`;
      };
      const fromIso = toIso(dateFields[0]?.value);
      const toIsoStr = toIso(dateFields[1]?.value);

      const taskMatches = (t: HTMLElement) => {
        const heading = (t.querySelector('[slot="heading"]')?.textContent ?? '').toLowerCase();
        const subjectLabel = (t.dataset.subject ?? '').toLowerCase();
        const subjectKey = t.dataset.subjectKey ?? '';
        const priority = t.dataset.priority ?? '';
        const status = t.dataset.status ?? '';
        const type = t.dataset.type ?? '';
        const date = t.dataset.date ?? '';

        if (query && !heading.includes(query) && !subjectLabel.includes(query)) return false;
        if (subjectValue !== 'all' && subjectKey !== subjectValue) return false;
        if (eventTypeValue !== 'all' && type !== eventTypeValue) return false;
        if (statusValue !== 'all' && status !== statusValue) return false;
        if (difficultyValue !== 'all' && priority !== difficultyToPriority[difficultyValue]) return false;
        if (fromIso && date && date < fromIso) return false;
        if (toIsoStr && date && date > toIsoStr) return false;

        return true;
      };

      const taskPanels = root.querySelectorAll<HTMLElement>('.day-section sl-panel[data-subject-key]');
      const dayVisibility = new Map<HTMLElement, boolean>();

      taskPanels.forEach(t => {
        const ok = taskMatches(t);
        t.style.display = ok ? '' : 'none';
        const day = t.closest<HTMLElement>('.day-section');
        if (day) dayVisibility.set(day, (dayVisibility.get(day) ?? false) || ok);
      });

      dayVisibility.forEach((visible, day) => {
        day.style.display = visible ? '' : 'none';
      });

      // Show the empty state when no task panel is currently visible.
      const anyVisible = Array.from(taskPanels).some(t => t.style.display !== 'none');
      const emptyState = root.querySelector<HTMLElement>('.empty-state');
      if (emptyState) emptyState.hidden = anyVisible;
    };

    const applyFilters = (event: Event) => {
      const root = (event.target as HTMLElement).closest('.study-dashboard');
      if (root) applyFiltersToRoot(root);
    };

    const resetFilters = (event: Event) => {
      const root = (event.target as HTMLElement).closest('.study-dashboard');
      if (!root) return;
      const toolbar = root.querySelector('.dashboard-toolbar');
      if (!toolbar) return;

      const searchField = toolbar.querySelector<HTMLInputElement & { value: string }>('sl-search-field');
      if (searchField) searchField.value = '';

      toolbar.querySelectorAll<HTMLElement & { value?: string }>('sl-select').forEach(s => {
        s.value = 'all';
      });

      toolbar.querySelectorAll<HTMLElement & { value?: Date }>('sl-date-field').forEach(d => {
        d.value = undefined;
      });

      applyFiltersToRoot(root);
    };

    return html`
      <style>
        .top-bar {
          display: flex;
          align-items: center;
          justify-content: space-between;
          gap: 1rem;
          padding: 8px clamp(1rem, 4vw, 64px);
          background: var(--sl-color-background-plain);
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
          max-inline-size: 100%;
          object-fit: contain;
          object-position: left center;
        }
        .top-bar__profile {
          display: flex;
          align-items: center;
          gap: 8px;
          color: var(--sl-color-foreground-plain);
          font: var(--sl-text-body-md);
        }
        .study-dashboard {
          display: grid;
          gap: 1.5rem;
          max-inline-size: 720px;
          margin: 0 auto;
          padding: clamp(1rem, 3vw, 2rem) clamp(0.75rem, 3vw, 1rem);
          font: var(--sl-text-body-md);
        }
        .dashboard-header {
          display: flex;
          justify-content: center;
          margin-block-end: 0.5rem;
        }
        .dashboard-header h1 {
          margin: 0;
          /* Figma 'Heading/xl' (node 834:31854): 32px / 40px, semiBold. */
          font: var(--sl-text-new-heading-xl);
          font-weight: var(--sl-text-new-typeset-fontWeight-semiBold);
          font-size: clamp(1.5rem, 4.5vw, 2rem);
          line-height: 1.25;
          color: var(--sl-color-foreground-plain);
          text-align: center;
        }
        .dashboard-toolbar {
          display: grid;
          gap: 1rem;
          container-type: inline-size;
        }
        .toolbar-search {
          display: grid;
          grid-template-columns: 1fr auto;
          gap: 0.5rem;
          align-items: end;
        }
        .toolbar-selects {
          display: flex;
          flex-wrap: wrap;
          gap: 0.5rem;
          align-items: end;
        }
        .toolbar-selects > sl-form-field {
          flex: 1 1 9rem;
          min-inline-size: 9rem;
        }
        .toolbar-selects sl-select {
          inline-size: 100%;
          min-inline-size: 0;
        }
        .reset-filters {
          align-self: end;
          flex: 0 0 auto;
        }
        .toolbar-dates {
          display: flex;
          flex-wrap: wrap;
          gap: 0.5rem;
          align-items: end;
        }
        .toolbar-dates > sl-form-field {
          flex: 1 1 12rem;
          min-inline-size: 10rem;
        }
        .toolbar-dates sl-date-field {
          inline-size: 100%;
          min-inline-size: 0;
        }
        .toolbar-dates > :last-child {
          display: none;
        }
        .day-section {
          display: grid;
          gap: 0.75rem;
        }
        .day-section__header {
          display: flex;
          align-items: baseline;
          justify-content: space-between;
          flex-wrap: wrap;
          column-gap: 0.75rem;
          row-gap: 0.25rem;
          margin: var(--sl-size-250) 0;
          font-weight: var(--sl-text-new-typeset-fontWeight-semiBold, 600);
          color: var(--sl-color-foreground-bold);
        }
        .day-section__date {
          color: var(--sl-color-foreground-bold);
        }
        .day-section__tasks {
          display: grid;
          gap: 1rem;
        }
        /* Empty state shown when search/filters produce no matching events.
           Figma: Multitenant materials, node 834:20132. */
        .empty-state {
          display: flex;
          align-items: center;
          justify-content: center;
          gap: 20px;
          padding-block: 20px;
          padding-inline: 20px;
          background: var(--sl-color-background-plain);
          border: 1px solid var(--sl-color-border-plain);
          border-radius: 4px;
          color: var(--sl-color-foreground-plain);
          text-align: center;
          font: var(--sl-text-body-md);
          line-height: 20px;
        }
        .empty-state[hidden] {
          display: none;
        }
        .empty-state__image {
          inline-size: 92px;
          block-size: 80px;
          object-fit: contain;
          flex: 0 0 auto;
        }
        @media (prefers-color-scheme: dark) {
          .empty-state__image {
            filter: invert(1) hue-rotate(180deg);
          }
        }
        :root[style*='color-scheme: dark'] .empty-state__image {
          filter: invert(1) hue-rotate(180deg);
        }
        /* Kid-friendly: rounder panels, softer shadows, chunkier spacing */
        .study-dashboard sl-panel {
          --sl-panel-border-radius: 0.75rem;
          border-radius: 0.75rem;
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
        }
        .task-aside sl-badge {
          white-space: nowrap;
          inline-size: fit-content;
          flex: 0 0 auto;
        }
        /* Figma body padding: 16px 24px 24px 24px (spacing below the divider). */
        .task-panel::part(content) {
          padding: 16px 16px 24px 24px;
        }
        .day-section__date {
          color: var(--sl-color-foreground-bold);
        }
        @media (max-inline-size: 720px) {
          .task-panel::part(header) {
            padding: 12px 12px 12px 16px;
          }
          .task-panel::part(content) {
            padding: 12px 16px 16px 16px;
          }
          .task-aside {
            padding-block-start: 12px;
            padding-inline-end: 12px;
          }
        }
        @media (max-inline-size: 520px) {
          .top-bar__wordmark {
            display: none;
          }
          .toolbar-search {
            grid-template-columns: 1fr;
          }
          .reset-filters {
            flex: 1 1 100%;
          }
          .task-heading__title {
            font-size: 15px;
          }
          .task-heading__subtitle {
            font-size: 13px;
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
          <h1>Your upcoming events</h1>
        </header>

        <div class="dashboard-toolbar">
          <div class="toolbar-search">
            <sl-form-field label="Search events by subject or description">
              <sl-search-field
                placeholder="ex. biology"
                @sl-change=${applyFilters}
                @keydown=${(e: KeyboardEvent) => {
                  if (e.key === 'Enter') applyFilters(e);
                }}
              ></sl-search-field>
            </sl-form-field>
            <sl-button aria-label="Search" variant="primary" fill="outline" @click=${applyFilters}>Search</sl-button>
          </div>

          <div class="toolbar-selects">
            <sl-form-field label="Subject">
              <sl-select aria-label="Filter by subject" placeholder="Subject" @sl-change=${applyFilters}>
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
            </sl-form-field>

            <sl-form-field label="Event type">
              <sl-select aria-label="Filter by event type" placeholder="Event type" @sl-change=${applyFilters}>
                <sl-option value="all">All types</sl-option>
                ${allEventTypes.map(t => html`<sl-option value=${t.slug}>${t.label}</sl-option>`)}
              </sl-select>
            </sl-form-field>

            <sl-form-field label="Status">
              <sl-select aria-label="Filter by status" placeholder="Status" @sl-change=${applyFilters}>
                <sl-option value="all">All statuses</sl-option>
                <sl-option value="todo">To do</sl-option>
                <sl-option value="in-progress">In progress</sl-option>
                <sl-option value="done">Done</sl-option>
              </sl-select>
            </sl-form-field>

            <sl-form-field label="Difficulty">
              <sl-select aria-label="Filter by difficulty" placeholder="Difficulty" @sl-change=${applyFilters}>
                <sl-option value="all">All difficulties</sl-option>
                <sl-option value="easy">Easy 🌶️</sl-option>
                <sl-option value="medium">Medium 🌶️🌶️</sl-option>
                <sl-option value="hard">Hard 🌶️🌶️🌶️</sl-option>
              </sl-select>
            </sl-form-field>

            <sl-button
              class="reset-filters"
              aria-label="Reset filters"
              variant="primary"
              fill="outline"
              @click=${resetFilters}
            >
              <sl-icon slot="prefix" name="far-xmark"></sl-icon>
              Reset filters
            </sl-button>
          </div>

          <div class="toolbar-dates">
            <sl-form-field label="From date">
              <sl-date-field @sl-change=${applyFilters}></sl-date-field>
            </sl-form-field>
            <sl-form-field label="To date">
              <sl-date-field @sl-change=${applyFilters}></sl-date-field>
            </sl-form-field>
            <span aria-hidden="true"></span>
          </div>
        </div>

        <div class="week-days">${week.map(daySection)}</div>

        <div class="empty-state" role="status" aria-live="polite" hidden>
          <p>No events match your search</p>
          <img class="empty-state__image" src="/images/study-companion/charco-pet.png" alt="" aria-hidden="true" />
        </div>
      </main>
    `;
  }
};
