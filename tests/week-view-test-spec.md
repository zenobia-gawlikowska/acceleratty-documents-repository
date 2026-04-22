import { test, expect } from '@playwright/test';

test('can switch to Today tab in Study Companion weekly view', async ({ page }) => {
  await page.goto('http://localhost:6006/?path=/story/examples-study-companion--weekly-view', { waitUntil: 'networkidle' });

  const preview = page.frameLocator('#storybook-preview-iframe');

  // Ensure story content has loaded
  await preview.locator('sl-tab:has-text("This week")').first().waitFor({ state: 'visible', timeout: 5000 });

  // Click the "Today" tab
  await preview.locator('sl-tab:has-text("Today")').click();

  // Verify a task from Today is visible
  await expect(preview.locator('text=Quadratic equations — test')).toBeVisible();
});