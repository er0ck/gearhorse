---
title: "Summary: Gearhorse Tech Stack Setup"
type: source
tags: [gearhorse, carrd, make, notion, utm, crm, spam-protection, recaptcha, honeypot]
created: 2026-04-14
updated: 2026-04-14
sources: 1
---

# Summary: Gearhorse Tech Stack Setup

**Source file:** `sources/gearhorse-tech-stack-setup.md`
**Source type:** Conversation transcript
**Date captured:** 2026-04-14

---

## What this source covers

End-to-end setup guide for [[Gear Horse]]'s free-tier marketing stack: landing pages, CRM integration, email collection, [[UTM Tracking]], and spam protection via [[Honeypot Spam Protection]] and reCAPTCHA.

---

## Key takeaways

### Stack recommendation
The recommended free-tier stack uses [[Carrd Make Notion Stack]] as its core, with Stripe for payments and Medusa.js + Vercel as a potential full e-commerce path if needed.

### Carrd → Make → Notion integration
The integration pattern is: Carrd Pro form → Make webhook trigger → Notion database row creation. The Make free tier (1,000 ops/month) is significantly better than Zapier (100 tasks/month) for multi-step flows.

**Common error:** The UUID validation error (`path.data_source_id should be a valid uuid`) happens when a template variable like `{{1.email}}` gets pasted into the Database ID field in Make. The Database ID is auto-filled from a dropdown and should never be edited manually.

### UTM tracking pattern
Carrd hidden fields capture UTM params from URL query strings automatically. Make maps them to Notion properties. A Formula field in Notion produces a readable `source / medium` label for the CRM view.

See [[UTM Tracking]] for full setup detail.

### Spam protection
Recommended combo: reCAPTCHA v3 (Carrd built-in, invisible, no user friction) + honeypot field filtered in Make. See [[Honeypot Spam Protection]].

**Key note:** reCAPTCHA site keys are registered to a Google account, not verified against domain ownership. Any Google account can register `gearhorse.camp` as a domain. Use a personal, long-term account, not an employer account.

### Email marketing options
- Brevo (300 emails/day, unlimited contacts) — best for larger lists
- MailerLite (1k contacts, 12k emails/month) — cleanest UI
- Mailchimp (500 contacts, 1k emails/month) — smallest free tier

---

## Pages created or updated from this source

- [[Carrd Make Notion Stack]] (concept)
- [[UTM Tracking]] (concept)
- [[Honeypot Spam Protection]] (concept)
- [[Gear Horse]] (entity) — tech stack section
