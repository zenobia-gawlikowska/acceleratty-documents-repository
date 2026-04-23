import { expect, test } from '@playwright/test';

test.describe('Study Companion Weekly View', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('http://localhost:6006/?path=/story/examples-study-companion--weekly-view', {
      waitUntil: 'networkidle'
    });
  });

test('can use search in Study Companion weekly view', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    // Wait for the search input to render
    await preview.locator('input#sl-text-field-1').waitFor({ state: 'visible', timeout: 5000 });
    // Type a search query
    await preview.locator('input#sl-text-field-1').fill('Math');
    await preview.getByRole('button', { name: /Search/i }).click();

    await preview.getByText('Monday · Apr 27').waitFor({ state: 'visible', timeout: 5000 });
    await expect(preview.getByText('Math').nth(4)).toBeVisible();
    await expect(preview.getByText('Math').nth(5)).toBeVisible();
    await expect(preview.getByText('No tasks planned.')).not.toBeVisible();
  });

  test('can use filter by Difficulty', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    const select = preview.getByRole('combobox', { name: 'Filter by difficulty' });
    await select.waitFor({ state: 'visible', timeout: 5000 });

    await select.click();
    await preview.getByRole('option', { name: /^Easy/ }).click();

    // Only the "Impressionism — bring portfolio" task (priority=low → 🌶️) should remain.
    await expect(preview.locator('sl-panel[data-priority="low"]').first()).toBeVisible();
    await expect(preview.locator('sl-panel[data-priority="normal"]').first()).toBeHidden();
    await expect(preview.locator('sl-panel[data-priority="high"]').first()).toBeHidden();
  });

  test('can use filter by Subject', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    const select = preview.getByRole('combobox', { name: 'Filter by subject' });
    await select.waitFor({ state: 'visible', timeout: 5000 });

    await select.click();
    await preview.getByRole('option', { name: 'Science' }).click();

    // Only science tasks should remain visible.
    await expect(preview.locator('sl-panel[data-subject-key="science"]').first()).toBeVisible();
    await expect(preview.locator('sl-panel[data-subject-key="math"]').first()).toBeHidden();
    await expect(preview.locator('sl-panel[data-subject-key="history"]').first()).toBeHidden();
    await expect(preview.locator('sl-panel[data-subject-key="geography"]').first()).toBeHidden();
    await expect(preview.locator('sl-panel[data-subject-key="art"]').first()).toBeHidden();
  });

  test('can use filter by Topic', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    const select = preview.getByRole('combobox', { name: 'Filter by topic' });
    await select.waitFor({ state: 'visible', timeout: 5000 });

    await select.click();
    await preview.getByRole('option', { name: 'Fractions' }).click();

    // Only the "Fractions — worksheet" task should remain in the week view.
    const weekPanel = preview.locator('sl-tab-panel').nth(1);
    await expect(weekPanel.locator('sl-panel[data-topic="fractions"]')).toBeVisible();
    await expect(weekPanel.locator('sl-panel[data-topic="quadratic-equations"]')).toBeHidden();
    await expect(weekPanel.locator('sl-panel[data-topic="cell-biology"]')).toBeHidden();
  });

  test('can use From date and To date filters', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');
    // Wait for the search input to render
    await preview.locator('input[label="From date"]').waitFor({ state: 'visible', timeout: 5000 });
    await preview.locator('input[label="To date"]').waitFor({ state: 'visible', timeout: 5000 });
    // Type a search query
    await preview.locator('input[label="From date"]').fill('24-04-2026');
    await preview.locator('input[label="To date"]').fill('25-04-2026');
    await preview.getByRole('button', { name: /Search/i }).press('Enter');
    // Verify search results are visible
    await expect(preview.getByText('Monday (24.04.2026)')).toBeVisible();
    await expect(preview.getByText('Tuesday (25.04.2026)')).toBeVisible();
    await expect(preview.getByText('Wednesday (27.04.2026)')).not.toBeVisible();
    await expect(preview.getByText('Thursday (28.04.2026)')).not.toBeVisible();
    await expect(preview.getByText('Friday (29.04.2026)')).not.toBeVisible();
  });
});
