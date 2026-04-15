---
title: Carrd Make Notion Stack
type: concept
tags: [tech-stack, carrd, make, notion, automation, utm, crm, gearhorse, no-code]
created: 2026-04-14
updated: 2026-04-14
sources: 3
---

# Carrd Make Notion Stack

The near-free marketing and CRM stack used by [[Gear Horse]] pre-launch. Built from free-tier and low-cost SaaS tools connected via Make (formerly Integromat) as automation middleware.

---

## Stack components

| Tool | Role | Cost |
|---|---|---|
| Carrd Pro Lite | Landing pages + forms | $19/year |
| Make | Automation middleware | Free (under 1k ops/mo) |
| Notion | CRM + database | Free |
| Brevo | Email marketing | Free (under 300 emails/day) |
| GA4 | Analytics | Free |
| Microsoft Clarity | Heatmaps + session recordings | Free |
| Stripe | Payments | Free until transactions (2.9% + 30¢) |
| Porkbun | Domain registrar | ~$10/year |
| ImprovMX | Email forwarding | Free tier |
| Gmail Send As | Branded email | Free |
| reCAPTCHA v3 | Bot protection | Free |

---

## Integration: Carrd → Make → Notion

### How it works
Carrd Pro form submits to a Make webhook URL. Make maps the form fields to Notion database properties and creates a new CRM row.

### Setup steps
1. Create Notion database with desired fields
2. Create Make account → new Scenario → Webhooks trigger → copy webhook URL
3. Paste webhook URL into Carrd form: Submissions → Webhook
4. In Make, add Notion module → map form fields to Notion properties
5. Share the Notion database with the Make integration (easy to forget)

### Common error
`[400] path failed validation: path.data_source_id should be a valid uuid`

Cause: a template variable like `{{1.email}}` was accidentally pasted into the Database ID field. The Database ID is auto-filled from a dropdown and should never be manually edited. Form field variables go in the Properties section below the Database ID.

---

## UTM tracking

See [[UTM Tracking]] for the full setup. Carrd hidden fields capture UTM parameters from URL query strings automatically. Make maps them to Notion CRM properties. A Notion Formula field produces a readable `source / medium` label.

---

## Spam protection

See [[Honeypot Spam Protection]]. Recommended combo: reCAPTCHA v3 (Carrd built-in) + honeypot field filtered in Make before the Notion write.

---

## Testing

See [[CI for No-Code Stack]]. GitHub Actions runs lychee (link checker) and Playwright (form-to-Notion end-to-end test) on a weekly schedule for free.

---

## Hosting upgrade path

Current: Carrd Pro Lite. If marketing posts/blog are needed before Kickstarter:
- **Lowest friction:** Framer (~$5/mo) — single platform with CMS, forms, UTM, product pages
- **Zero cost:** Astro + Cloudflare Pages — requires developer setup
- **Stay lean:** No blog needed until after Kickstarter launch

See [[summary: gearhorse-hosting-options]] for full evaluation.

---

## Why Make over Zapier

| | Make | Zapier |
|---|---|---|
| Free tier | 1,000 ops/month | 100 tasks/month |
| Multi-step flows | Free | Paid only |

For a multi-step flow (Carrd → filter → Notion), Make is the clear choice at free tier.

---

## Sources

- [[summary: gearhorse-tech-stack-setup]]
- [[summary: gearhorse-hosting-options]]
- [[summary: testing-carrd-make-notion-github-actions]]
