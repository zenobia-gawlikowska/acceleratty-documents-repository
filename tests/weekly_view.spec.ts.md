import { expect, test } from '@playwright/test';

test.describe('Study Companion Weekly View', () => {
  test.beforeEach('renders the weekly view with correct tabs', async ({ page }) => {
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
    await preview.getByRole('button', { name: /Search/i }).press('Enter');
    // Verify search results are visible
    await expect(preview.getByText(/Monday · Apr/)).toBeVisible();
    await expect(preview.getByText(/Tuesday · Apr/)).not.toBeVisible();
    await expect(preview.getByText(/Wednesday · Apr/)).not.toBeVisible();
    await expect(preview.getByText(/Thursday · Apr/)).toBeVisible();
    await expect(preview.getByText(/Friday · May/)).not.toBeVisible();

    const weekPanel = preview.locator('sl-tab-panel').nth(1);
    await expect(weekPanel.locator('sl-panel[heading="Quadratic equations — test 🌶️🌶️"]')).toBeVisible();
    await expect(weekPanel.locator('sl-panel[heading="Partitions of Poland — essay outline 🌶️"]')).not.toBeVisible();
    await expect(weekPanel.locator('sl-panel[heading="Fractions — worksheet 🌶️"]')).toBeVisible();
  });

  test('can use filter by Status', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    // Wait for the search input to render
    await preview.locator('sl-select-button[label=Status]').waitFor({ state: 'visible', timeout: 5000 });
    // Type a search query
    await preview.locator('sl-select-button[label=Status]').click();
    await page.locator('sl-option[value=english]').click();
    await preview.getByRole('button', { name: /Search/i }).press('Enter');
    // Verify search results are visible
    await expect(preview.getByText(/🏴󠁧󠁢󠁥󠁮󠁧󠁿 English\s*Description for the student, what they need to do/i)).toBeVisible();
    await expect(
      preview.getByText(/🌍 Geography\s*Description for the student, what they need to do/i)
    ).not.toBeVisible();
  });

  test('can use filter by Difficulty', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    // Wait for the search input to render
    await preview.locator('sl-select-button[label=Difficulty]').waitFor({ state: 'visible', timeout: 5000 });
    // Type a search query
    await preview.locator('sl-select-button[label=Difficulty]').click();
    await page.locator('sl-option[value=easy]').click();
    await preview.getByRole('button', { name: /Search/i }).press('Enter');
    // Verify search results are visible
    await expect(
      preview.getByText(/🏴󠁧󠁢󠁥󠁮󠁧󠁿 English\s*Description for the student, what they need to do/i)
    ).not.toBeVisible();
    await expect(
      preview.getByText(/🌍 Geography\s*Description for the student, what they need to do/i)
    ).not.toBeVisible();
    await expect(preview.getByText(/🧬 Biology\s*Description for the student, what they need to do/i)).toBeVisible();
  });

  test('can use filter by Subject', async ({ page }) => {
    const preview = page.frameLocator('iframe[title="storybook-preview-iframe"]');

    // Wait for the search input to render
    await preview.locator('sl-select-button[label=Subject]').waitFor({ state: 'visible', timeout: 5000 });
    // Type a search query
    await preview.locator('sl-select-button[label=Subject]').click();
    await page.locator('sl-option[value=biology]').click();
    await preview.getByRole('button', { name: /Search/i }).press('Enter');
    // Verify search results are visible
    await expect(
      preview.getByText(/🏴󠁧󠁢󠁥󠁮󠁧󠁿 English\s*Description for the student, what they need to do/i)
    ).not.toBeVisible();
    await expect(
      preview.getByText(/🌍 Geography\s*Description for the student, what they need to do/i)
    ).not.toBeVisible();
    await expect(preview.getByText(/🧬 Biology\s*Description for the student, what they need to do/i)).toBeVisible();
  });
});
