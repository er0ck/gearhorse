---
title: Testing Carrd / Make / Notion Stack with GitHub Actions
type: source
tags: [gearhorse, tech-stack, testing, github-actions, carrd, make, notion, ci-cd, playwright]
created: 2026-04-14
project: Gearhorse
source_type: conversation
---

# Testing Carrd / Make / Notion Stack with GitHub Actions

## Original Prompt

> Given my initial tech stack of Carrd, Make, and Notion, how can I be sure some of my features work, links go to the right place, and the CRM is populating with the right fields? How can I run tests for free in GitHub Actions?

---

## What to Test (Test Layer Map)

| Concern | What to test | Tool |
|---|---|---|
| Links resolve correctly | HTTP status codes on Carrd URLs | `lychee` (via `lycheeverse/lychee-action`) |
| Forms submit properly | Fill + submit a form on Carrd | Playwright |
| Make automation fires | Webhook receives payload, scenario runs | Playwright + wait |
| Notion CRM populates | Query Notion API for the new record | Notion JS client + assertions |

---

## End-to-End Test Flow

```
GitHub Actions
    │
    ├── 1. Run link checker against Carrd site
    │
    ├── 2. Playwright: fill out lead/contact form
    │       └── submits → triggers Make webhook
    │
    ├── 3. Wait ~10–15s for Make scenario to run
    │
    └── 4. Query Notion API: assert new row exists
            └── check name, email, source field, timestamp, etc.
```

---

## GitHub Actions Workflows

### Link Checker (`links.yml`)

Crawls the Carrd site weekly and flags broken links or bad redirects.

```yaml
name: Link Check
on:
  schedule:
    - cron: '0 9 * * 1'   # every Monday
  workflow_dispatch:

jobs:
  links:
    runs-on: ubuntu-latest
    steps:
      - uses: lycheeverse/lychee-action@v1
        with:
          args: >
            --verbose
            --no-progress
            https://yoursite.carrd.co
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Integration Test (`integration.yml`)

Runs the Playwright form-to-Notion test weekly (or on demand).

```yaml
name: Integration Test
on:
  workflow_dispatch:
  schedule:
    - cron: '0 10 * * 1'

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install deps
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      - name: Run integration tests
        env:
          NOTION_TOKEN: ${{ secrets.NOTION_TOKEN }}
          NOTION_DB_ID: ${{ secrets.NOTION_DB_ID }}
          TEST_EMAIL: ${{ secrets.TEST_EMAIL }}
        run: npx playwright test
```

---

## Playwright Test: Form → Make → Notion

Uses a **timestamped unique email per run** to avoid matching old Notion records and allow safe repeated execution without cleanup.

```js
// tests/form-to-notion.spec.js
import { test, expect } from '@playwright/test';
import { Client } from '@notionhq/client';

const notion = new Client({ auth: process.env.NOTION_TOKEN });
const testEmail = `test+${Date.now()}@yourdomain.com`; // unique per run

test('form submission populates Notion CRM', async ({ page }) => {
  // 1. Fill out the Carrd form
  await page.goto('https://yoursite.carrd.co');
  await page.fill('input[name="email"]', testEmail);
  await page.fill('input[name="name"]', 'CI Test User');
  await page.click('button[type="submit"]');

  // 2. Assert success state on the page
  await expect(page.locator('text=Thank you')).toBeVisible();

  // 3. Wait for Make to run
  await page.waitForTimeout(15000);

  // 4. Query Notion for the record
  const response = await notion.databases.query({
    database_id: process.env.NOTION_DB_ID,
    filter: {
      property: 'Email',
      email: { equals: testEmail }
    }
  });

  // 5. Assert the record exists and fields are correct
  expect(response.results.length).toBe(1);
  const record = response.results[0];

  const name = record.properties['Name'].title[0]?.plain_text;
  const source = record.properties['Source']?.select?.name;

  expect(name).toBe('CI Test User');
  expect(source).toBe('Website');   // whatever Make sets this field to
});
```

---

## Secrets Setup

In GitHub repo → **Settings → Secrets and variables → Actions**:

| Secret | Value |
|---|---|
| `NOTION_TOKEN` | Notion integration token |
| `NOTION_DB_ID` | Database ID from Notion URL |
| `TEST_EMAIL` | Optional — or generate dynamically with `Date.now()` as shown |

---

## GitHub Free Tier Cost

| Resource | Free allowance | This setup uses |
|---|---|---|
| Actions minutes | 2,000/month | ~3 min/run |
| Weekly cadence | — | ~4–5 runs/month ≈ 15 min total |

Well within free limits. Could run daily without issue.

---

## Future Enhancements

- **Screenshot on failure** — enable via `use: { screenshot: 'only-on-failure' }` in `playwright.config.js`
- **Failure alerts** — add a notification step (email, Slack, etc.) on job failure
- **Smoke test on deploy** — trigger `workflow_dispatch` manually after any Carrd site update

---

## Key Design Insight

Using a **timestamped unique email per test run** is the critical pattern. It ensures:
- No false positives from old Notion records
- Safe repeated runs without manual database cleanup
- Each CI run is fully isolated and auditable
