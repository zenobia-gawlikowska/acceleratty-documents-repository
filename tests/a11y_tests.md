import AxeBuilder from '@axe-core/playwright';
import { type Page, expect, test } from '@playwright/test';
import type { AxeResults } from 'axe-core';

const STORY_URL = 'http://localhost:6006/iframe.html?id=examples-study-companion--weekly-view&viewMode=story';

let axe: AxeBuilder;
let results: AxeResults;

function createNumberedList<T>(items: T[]): string {
  return items.map((item, index) => `${index + 1}. ${item}`).join('\n');
}

async function selectTab(page: Page, name: string): Promise<void> {
  await page.click(`sl-tab:has-text("${name}")`);
  await expect(page.locator(`sl-tab:has-text("${name}")`)).toHaveAttribute('selected', '');
}

async function runAxe(): Promise<AxeResults> {
  // Storybook's addon-a11y may run axe in the preview; retry while it is busy.
  let lastError: unknown;
  for (let i = 0; i < 10; i++) {
    try {
      return await axe.analyze();
    } catch (err) {
      lastError = err;
      if (!(err instanceof Error) || !err.message.includes('Axe is already running')) {
        throw err;
      }
      await new Promise(resolve => setTimeout(resolve, 250));
    }
  }
  throw lastError;
}

test.describe('A11y: Study Companion weekly view tabs', () => {
  test.beforeEach(async ({ page }) => {
    // Navigate directly to the preview iframe so axe only analyzes the story,
    // not the Storybook manager UI (which has its own banner and landmarks).
    await page.goto(STORY_URL, { waitUntil: 'networkidle' });
    axe = new AxeBuilder({ page });
  });

  test.afterEach(() => {
    if (!results?.violations) {
      return;
    }

    if (results.violations.length > 0) {
      const violationDetails = results.violations
        .map(violation => {
          const nodeDetails = createNumberedList(violation.nodes.flatMap(node => node.target));
          return `${violation.id} (${violation.impact}) \n${violation.description}\n${nodeDetails} \n`;
        })
        .join('\n\n');
      console.error(`Accessibility violations found:\n\n${violationDetails}`);
    }
  });

  test('Today tab panel has no detectable a11y violations', async ({ page }) => {
    await selectTab(page, 'Today');
    results = await runAxe();
    expect(results.violations.length, 'Accessibility violations found, see details above').toEqual(0);
  });

  test('This week tab panel has no detectable a11y violations', async ({ page }) => {
    await selectTab(page, 'This week');
    results = await runAxe();
    expect(results.violations.length, 'Accessibility violations found, see details above').toEqual(0);
  });

  test('Later tab panel has no detectable a11y violations', async ({ page }) => {
    await selectTab(page, 'Later');
    results = await runAxe();
    expect(results.violations.length, 'Accessibility violations found, see details above').toEqual(0);
  });

  test('should have correct tab order', async ({ page }) => {
    const activeElements = [
      'Search',
      'Search',
      'Subject',
      'Event type',
      'Status',
      'Difficulty',
      'From date',
      'Select from date',
      'To date',
      'Select to date',
      'Continue Title of the element',
      'Start Title of the element',
      'Start Title of the element'
    ] as const;

    await page.getByRole('button', { name: 'Collapse navigation' }).click();

    for (const activeElement of activeElements) {
      await page.keyboard.press('Tab');
      const focusedOn = await getFocusedElement(page);
      expect(focusedOn).toBe(activeElement);
    }
  });
});
