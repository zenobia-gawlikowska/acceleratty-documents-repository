// paste this into your .storybook/tests/weekly_view.spec.ts file

import { expect, test } from '@playwright/test';

test('can switch to Today tab in Study Companion weekly view', async ({ page }) => {
  await page.goto('http://localhost:6006/?path=/story/examples-study-companion--weekly-view', {
    waitUntil: 'networkidle'
  });

  await expect(
    page.locator('iframe[title="storybook-preview-iframe"]').contentFrame().locator('#sl-tab-group-1-tab-2 > div')
  ).toBeVisible();
  await expect(
    page.locator('iframe[title="storybook-preview-iframe"]').contentFrame().getByText('Monday · Apr')
  ).toBeVisible();
  await expect(
    page.locator('iframe[title="storybook-preview-iframe"]').contentFrame().getByText('Tuesday · Apr')
  ).toBeVisible();
  await expect(
    page.locator('iframe[title="storybook-preview-iframe"]').contentFrame().getByText('Wednesday · Apr')
  ).toBeVisible();
  await expect(
    page.locator('iframe[title="storybook-preview-iframe"]').contentFrame().getByText('Thursday · Apr')
  ).toBeVisible();
  await expect(
    page.locator('iframe[title="storybook-preview-iframe"]').contentFrame().getByText('Friday · May')
  ).toBeVisible();
  await page
    .locator('iframe[title="storybook-preview-iframe"]')
    .contentFrame()
    .locator('#sl-tab-group-1-tab-1 > div')
    .click();
});
