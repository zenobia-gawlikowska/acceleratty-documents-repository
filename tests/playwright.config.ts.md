// paste this into your playwright.config.ts file

import { defineConfig } from '@playwright/test';

export default defineConfig({
  timeout: 30 * 1000,

  projects: [
    {
      testDir: './.storybook/tests',
      name: 'storybook',
      use: {
        baseURL: 'http://localhost:6006'
      },
      testMatch: /storybook\/.*\.spec\.ts/
    },
    {
      name: 'website',
      testDir: './website/tests',
      use: {
        baseURL: 'http://localhost:8000'
      },
      testMatch: /website\/.*\.spec\.ts/
    }
  ]
});

