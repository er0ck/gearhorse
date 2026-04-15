---
title: Gearhorse — Hosting Options Research
type: source
tags: [gearhorse, hosting, landing-page, utm, make, carrd, framer, webflow, astro]
created: 2026-04-14
context: Chat with Claude (claude.ai) — Q&A about cheapest hosting with custom domain for a marketing site
---

# Gearhorse — Hosting Options Research

**Source type:** Chat Q&A  
**Date:** 2026-04-14  
**Context:** Evaluating hosting options for a Gearhorse marketing site with custom domain, marketing posts, product page, signup forms, and UTM param forwarding to Make.

---

## Prompt

> Cheapest hosting with custom domain?

> A landing page with marketing posts, product page, custom domain and signup forms that forward utm params to make et al

---

## Response Summary

### Requirements identified

- Landing page
- Marketing posts / blog-style content (CMS)
- Product page
- Custom domain
- Signup forms
- UTM parameter forwarding → Make (and downstream: Notion CRM, Brevo)

### Options evaluated

#### Framer (free → $5–10/mo)
- Best overall fit for the requirements
- CMS built in for blog/marketing posts
- Forms and product pages supported
- UTM passthrough possible with custom JS
- Custom domain requires paid tier (~$5/mo starter)

#### Webflow (free → $14/mo)
- Overkill unless pixel-perfect design control is needed
- CMS for posts, forms, custom domain all supported
- UTM → Make via webhook is well-documented
- Gets expensive fast

#### Carrd Pro (current stack, $19/yr) + Ghost or Beehiiv
- Keep Carrd for landing/product pages — UTM already wired
- Add Ghost (free self-host or Ghost Pro starter) or Beehiiv (free tier) for marketing posts
- Two subdomains of gearhorse.camp (e.g. `gearhorse.camp` + `blog.gearhorse.camp`)
- Low additional cost; leverages existing Make/Brevo setup

#### Astro + Cloudflare Pages (free)
- Zero hosting cost
- Markdown-based blog posts, static product pages
- Custom domain free
- Forms via serverless function → Make webhook
- Most setup effort; full ownership of stack

### Recommendation given current context

**Option A (stay lean, pre-Kickstarter):** Stay on Carrd Pro, skip marketing posts for now. No blog needed before Kickstarter launch.

**Option B (lowest-effort upgrade):** Move to Framer at ~$5/mo — single platform covering CMS posts + product pages + forms + UTM in one place.

### Existing stack context

- Carrd Pro Lite — landing page at gearhorse.camp
- Make — automation middleware
- Notion — CRM
- Brevo — email
- GA4 + Microsoft Clarity — analytics
- UTM params already wired via hidden Carrd fields → Make

---

## Key decisions / open questions

- Is a blog/marketing posts needed before Kickstarter? If no → stay on Carrd.
- If yes → Framer is lowest-friction path at ~$5/mo.
- Astro + Cloudflare Pages is the zero-cost ceiling option but requires dev work.
