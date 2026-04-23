import AxeBuilder from '@axe-core/playwright';
import { expect, test } from '@playwright/test';
import type { AxeResults } from 'axe-core';

const STORY_URL = 'http://localhost:6006/iframe.html?id=examples-study-companion--weekly-view&viewMode=story';

let axe: AxeBuilder;
let results: AxeResults;

function createNumberedList<T>(items: T[]): string {
  return items.map((item, index) => `${index + 1}. ${item}`).join('\n');
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

test.describe('A11y: Study Companion weekly view', () => {
  test.beforeEach(async ({ page }) => {
    // Navigate directly to the preview iframe so axe only analyzes the story,
    // not the Storybook manager UI (which has its own banner and landmarks).
    await page.goto(STORY_URL, { waitUntil: 'networkidle' });
    // Wait for the dashboard to render before running axe.
    await page.locator('.study-dashboard').waitFor({ state: 'visible' });
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

  test('weekly view has no detectable a11y violations', async () => {
    results = await runAxe();
    expect(results.violations.length, 'Accessibility violations found, see details above').toEqual(0);
  });
});
