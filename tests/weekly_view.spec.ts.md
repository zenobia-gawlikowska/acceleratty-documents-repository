import { expect, test } from '@playwright/test';

test.describe('Study Companion Weekly View', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('http://localhost:6006/?path=/story/examples-study-companion--weekly-view', {
      waitUntil: 'networkidle'
    });
  });

  test('can use search in Study Companion weekly view', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    const searchField = preview.getByRole('textbox', { name: 'Search tasks' });
    await searchField.waitFor({ state: 'visible', timeout: 5000 });

    await searchField.fill('Math');
    await preview.getByRole('button', { name: 'Search' }).click();

    // Only Monday and Thursday have Math tasks; Friday has no tasks so its
    // (empty) day panel stays visible regardless of the filter.
    await expect(preview.locator('sl-panel[heading^="Monday"]')).toBeVisible();
    await expect(preview.locator('sl-panel[heading^="Tuesday"]')).toBeHidden();
    await expect(preview.locator('sl-panel[heading^="Wednesday"]')).toBeHidden();
    await expect(preview.locator('sl-panel[heading^="Thursday"]')).toBeVisible();

    const weekPanel = preview.locator('sl-tab-panel').nth(1);
    await expect(weekPanel.locator('sl-panel[data-subject-key="math"]').first()).toBeVisible();
    await expect(weekPanel.locator('sl-panel[data-subject-key="history"]')).toBeHidden();
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
});
