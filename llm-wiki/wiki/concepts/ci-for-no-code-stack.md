---
title: CI for No-Code Stack
type: concept
tags: [testing, ci-cd, github-actions, playwright, notion-api, lychee, carrd, make, gearhorse]
created: 2026-04-14
updated: 2026-04-14
sources: 1
---

# CI for No-Code Stack

Pattern for testing a no-code marketing stack (Carrd forms, Make automations, Notion CRM) using free GitHub Actions workflows.

---

## Why test a no-code stack

Even though none of the tools require code to configure, they can break silently:
- Carrd form submissions fail if the Make webhook URL changes
- Make scenarios stop if the Notion integration token expires
- Notion field mappings break if a database property is renamed
- Links on the Carrd site can go stale

---

## Test layer map

| Concern | Tool |
|---|---|
| Links resolve correctly | `lychee` via `lycheeverse/lychee-action` |
| Forms submit correctly | Playwright |
| Make automation fires | Playwright + `waitForTimeout` |
| Notion CRM populates | Notion JS client + assertions |

---

## End-to-end test flow

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

## Critical pattern: timestamped unique email

Use `test+${Date.now()}@yourdomain.com` as the test email on every run. This:
- Prevents false positives from old Notion records
- Makes each run fully isolated and auditable
- Allows safe repeated execution without manual database cleanup

This is the most important design decision in the test setup.

---

## GitHub Actions cost

Well within free tier (2,000 min/month). Weekly runs use approximately 15 minutes/month total.

---

## Required secrets

| Secret | Value |
|---|---|
| `NOTION_TOKEN` | Notion integration token |
| `NOTION_DB_ID` | Database ID from Notion URL |

---

## Future enhancements

- Screenshot on failure: `use: { screenshot: 'only-on-failure' }` in `playwright.config.js`
- Failure alerts: notification step on job failure (Slack, email, etc.)
- Smoke test on deploy: trigger `workflow_dispatch` manually after any Carrd update

---

## Connection to Gear Horse

[[Gear Horse]] uses this pattern. The `gearhorse` GitHub repo contains the two workflow files. See [[summary: testing-carrd-make-notion-github-actions]] for full workflow code.

---

## Sources

- [[summary: testing-carrd-make-notion-github-actions]]
