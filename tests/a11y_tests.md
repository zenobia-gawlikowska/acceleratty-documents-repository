import AxeBuilder from '@axe-core/playwright';
import { expect, test } from '@playwright/test';

let axe: AxeBuilder;
let results: AxeResults;

function getPreviewFrame(page) {
  const frames = page.frames();
  return frames.find(f => f.url().includes('iframe.html') || f.url().includes('storybook-preview'));
}

function createNumberedList<T>(items: T[]): string {
  return items.map((item, index) => `${index + 1}. ${item}`).join('\n');
}

test.describe('A11y: Study Companion weekly view tabs', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('http://localhost:6006/?path=/story/examples-study-companion--weekly-view', {
      waitUntil: 'networkidle'
    });
    axe = new AxeBuilder({ page });
  });

  test.afterEach(async ({ page }) => {
    if (!results || !results.violations) {
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
    const frame = getPreviewFrame(page);
    await frame.click('sl-tab:has-text("Today")');
    await expect(frame.locator('sl-tab:has-text("Today")')).toHaveAttribute('selected', '');
    results = await axe.analyze();
    expect(results.violations.length, 'Accessibility violations found, see details above').toEqual(0);
  });

  test('This week tab panel has no detectable a11y violations', async ({ page }) => {
    const frame = getPreviewFrame(page);
    await frame.click('sl-tab:has-text("This week")');
    await expect(frame.locator('sl-tab:has-text("This week")')).toHaveAttribute('selected', '');
    results = await axe.analyze();
    expect(results.violations.length, 'Accessibility violations found, see details above').toEqual(0);
  });

  test('Later tab panel has no detectable a11y violations', async ({ page }) => {
    const frame = getPreviewFrame(page);
    await frame.click('sl-tab:has-text("Later")');
    await expect(frame.locator('sl-tab:has-text("Later")')).toHaveAttribute('selected', '');
    results = await axe.analyze();
    expect(results.violations.length, 'Accessibility violations found, see details above').toEqual(0);
  });
});
