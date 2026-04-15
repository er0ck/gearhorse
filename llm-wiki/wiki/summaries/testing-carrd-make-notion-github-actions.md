---
title: "Summary: Testing Carrd / Make / Notion Stack with GitHub Actions"
type: source
tags: [gearhorse, testing, github-actions, playwright, notion-api, ci-cd, carrd, make]
created: 2026-04-14
updated: 2026-04-14
sources: 1
---

# Summary: Testing Carrd / Make / Notion Stack with GitHub Actions

**Source file:** `sources/testing-carrd-make-notion-github-actions.md`
**Source type:** Conversation transcript
**Date captured:** 2026-04-14

---

## What this source covers

How to test [[Gear Horse]]'s no-code stack (Carrd forms, Make automations, Notion CRM) using free GitHub Actions workflows with lychee (link checking) and Playwright (end-to-end form testing).

---

## Key takeaways

### Test layer map

| Concern | Tool |
|---|---|
| Links resolve correctly | `lychee` via `lycheeverse/lychee-action` |
| Forms submit correctly | Playwright |
| Make automation fires | Playwright + wait |
| Notion CRM populates | Notion JS client + assertions |

### End-to-end flow

```
GitHub Actions → link checker → Playwright form fill → wait 10–15s → Notion API assertion
```

### Critical design pattern
[[CI for No-Code Stack]]: use a **timestamped unique email per test run** (e.g. `test+${Date.now()}@domain.com`). This prevents false positives from old Notion records and makes each run fully isolated without requiring database cleanup.

### Cost
Well within GitHub Actions' free tier (2,000 min/month). Weekly runs use ~15 minutes/month total.

### Workflow structure
Two separate workflow files:
- `links.yml` — weekly lychee link check, no secrets needed
- `integration.yml` — weekly Playwright form-to-Notion test, requires `NOTION_TOKEN` and `NOTION_DB_ID` secrets

---

## Pages created or updated from this source

- [[CI for No-Code Stack]] (concept)
- [[Carrd Make Notion Stack]] (concept) — testing section
- [[Gear Horse]] (entity) — testing infrastructure
