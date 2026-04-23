import { expect, test } from '@playwright/test';

test.describe('Study Companion Weekly View', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('http://localhost:6006/?path=/story/examples-study-companion--weekly-view', {
      waitUntil: 'networkidle'
    });
  });

  test('can use search in Study Companion weekly view', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    const searchField = preview.locator('sl-search-field input');
    await searchField.waitFor({ state: 'visible', timeout: 5000 });

    await searchField.fill('Math');
    await preview.getByRole('button', { name: 'Search' }).click();

    // Only math tasks (Quadratic equations, Fractions) should remain; other
    // subjects are hidden. Their day sections stay visible because they still
    // contain at least one matching task.
    await expect(preview.locator('sl-panel[data-subject-key="math"]').first()).toBeVisible();
    await expect(preview.locator('sl-panel[data-subject-key="history"]').first()).toBeHidden();
    await expect(preview.locator('sl-panel[data-subject-key="science"]').first()).toBeHidden();
    await expect(preview.locator('sl-panel[data-subject-key="geography"]').first()).toBeHidden();
    await expect(preview.locator('sl-panel[data-subject-key="art"]').first()).toBeHidden();

    // Day sections with no matching tasks should be hidden.
    await expect(preview.locator('.day-section[data-day="Today"]')).toBeVisible();
    await expect(preview.locator('.day-section[data-day="Tomorrow"]')).toBeHidden();
    await expect(preview.locator('.day-section[data-day="Saturday"]')).toBeHidden();
    await expect(preview.locator('.day-section[data-day="Monday"]')).toBeVisible();
  });

  test('can use filter by Difficulty', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    const select = preview.getByRole('combobox', { name: 'Filter by difficulty' });
    await select.waitFor({ state: 'visible', timeout: 5000 });

    await select.click();
    await preview.getByRole('option', { name: /^Easy/ }).click();

    // Only the "Impressionism — written assignment" task (priority=low → 🌶️) should remain.
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

  test('can use filter by Event type', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    const select = preview.getByRole('combobox', { name: 'Filter by event type' });
    await select.waitFor({ state: 'visible', timeout: 5000 });

    await select.click();
    await preview.getByRole('option', { name: 'Test' }).click();

    // Only "— test" tasks (Quadratic equations, Fractions) should remain.
    await expect(preview.locator('sl-panel[data-type="test"]').first()).toBeVisible();
    await expect(preview.locator('sl-panel[data-type="exam"]').first()).toBeHidden();
    await expect(preview.locator('sl-panel[data-type="oral-exam"]').first()).toBeHidden();
    await expect(preview.locator('sl-panel[data-type="written-assignment"]').first()).toBeHidden();
  });

  test('can change to dark mode', async ({ page }) => {
    await page.getByText('Quadratic equations').waitFor({ state: 'visible', timeout: 5000 });
    await expect(page.locator('body')).toHaveCSS('background-color', 'rgb(255, 255, 255)');
    // Navigate with dark mode global
    await page.goto(
      'http://localhost:6006/iframe.html?id=examples-study-companion--weekly-view&viewMode=story&globals=mode:dark',
      { waitUntil: 'networkidle' }
    );
    await expect(page.locator('body')).not.toHaveCSS('background-color', 'rgb(22, 22, 22)');
  });
});
