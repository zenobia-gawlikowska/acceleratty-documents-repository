// paste this into your .storybook/tests/weekly_view.spec.ts file

import { expect, test } from '@playwright/test';

test.describe('Study Companion Weekly View', () => {
  test.beforeEach('renders the weekly view with correct tabs', async ({ page }) => {
    await page.goto('http://localhost:6006/?path=/story/examples-study-companion--weekly-view', {
      waitUntil: 'networkidle'
    });
  });
  test('can switch to Today tab in Study Companion weekly view', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    // Wait for the tabs to render
    await preview.getByRole('tab', { name: /This week/i }).waitFor({ state: 'visible', timeout: 5000 });

    // Verify day panels are visible
    await expect(preview.getByText(/Monday · Apr/)).toBeVisible();
    await expect(preview.getByText(/Tuesday · Apr/)).toBeVisible();
    await expect(preview.getByText(/Wednesday · Apr/)).toBeVisible();
    await expect(preview.getByText(/Thursday · Apr/)).toBeVisible();
    await expect(preview.getByText(/Friday · May/)).toBeVisible();

    // Click the "Today" tab
    await preview.getByRole('tab', { name: /Today/i }).click();
    // Verify day panels are visible
    await expect(preview.getByText(/Monday · Apr/)).not.toBeVisible();
    await expect(preview.getByText(/Tuesday · Apr/)).not.toBeVisible();
    await expect(preview.getByText(/Wednesday · Apr/)).not.toBeVisible();
    await expect(preview.getByText(/Thursday · Apr/)).not.toBeVisible();
    await expect(preview.getByText(/Friday · May/)).not.toBeVisible();
  });

  test('can use search in Study Companion weekly view', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    // Wait for the search input to render
    await preview.locator('input#sl-text-field-1').waitFor({ state: 'visible', timeout: 5000 });
    // Type a search query
    await preview.locator('input#sl-text-field-1').fill('Math');
    await preview.getByRole('button', { name: /Search/i }).press('Enter');
    // Verify search results are visible
    await expect(preview.getByText(/Monday · Apr/)).toBeVisible();
    await expect(preview.getByText(/Tuesday · Apr/)).not.toBeVisible();
    await expect(preview.getByText(/Wednesday · Apr/)).not.toBeVisible();
    await expect(preview.getByText(/Thursday · Apr/)).toBeVisible();
    await expect(preview.getByText(/Friday · May/)).not.toBeVisible();

    await expect(preview.locator('sl-panel#Quadratic equations — test')).toBeVisible();
    await expect(preview.locator('sl-panel#Partitions of Poland — essay outline')).not.toBeVisible();
    await expect(preview.locator('sl-panel#Quadratic equations — test')).toBeVisible();
    await expect(preview.locator('sl-panel#Fractions — worksheet')).toBeVisible();
  });
});
