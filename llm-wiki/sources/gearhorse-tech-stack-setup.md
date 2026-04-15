---
title: Gearhorse Tech Stack Setup
type: source
tags: [gearhorse, carrd, notion, make, utm, crm, email, spam-protection, landing-page]
created: 2026-04-14
updated: 2026-04-14
domain: personal/gearhorse
---

# Gearhorse Tech Stack Setup

> **Source type:** Conversation transcript  
> **Date:** 2026-04-14  
> **Topic:** End-to-end free-tier retail web stack for Gearhorse — landing pages, CRM, email collection, UTM tracking, and spam protection.

---

## Prompt

Design a plan for a lightweight retail website, marketing landing pages, CRM, and traffic analytics, mostly for free. Then detail: DIY CRM options, Carrd Pro → Notion free integration, email campaigns from Notion, Carrd form setup, UTM tag capture, honeypot spam protection, and reCAPTCHA setup without domain-matched Google account.

---

## Recommended Free-Tier Stack

### Retail Website
- **Medusa.js** (open source) + **Vercel** (free tier) — full e-commerce engine
- **Stripe** for payments — no monthly fee, 2.9% + 30¢ per transaction
- Lightweight alternative: Carrd storefront + Stripe payment links

### Marketing Landing Pages
- **Framer** — free tier, fast, drag-and-drop, publishes to `.framer.app` or custom domain
- **Carrd** — ultra-lightweight, free tier generous, Pro Lite at $19/year unlocks forms + webhooks

### CRM
- **HubSpot CRM** — free tier: unlimited contacts, deal pipelines, email tracking, basic automation
- **Notion** — lightweight DIY CRM via database with filtering, tags, custom views

### Traffic Analytics
- **Google Analytics 4 (GA4)** — free forever, pageviews, sessions, conversions, funnels
- **Microsoft Clarity** — free forever, adds heatmaps and session recordings
- **Vercel Analytics** — free tier includes core web vitals and real-time traffic

### Full Stack Connection
```
Landing Pages (Framer / Carrd)
        ↓
  Retail Site (Medusa / WooCommerce)
        ↓
   Payments (Stripe)
        ↓
  CRM (Notion / HubSpot) ← captures leads + customers
        ↓
  Email Marketing (Brevo / Mailchimp)

Analytics layer (GA4 + Clarity) on everything
```

---

## DIY CRM Options (Ranked by Ease)

### 1. Notion (1–2 hours)
Database with columns: Name, Email, Phone, Status, Last Contact, Notes, Deal Value. Add Kanban view for pipeline. Free tier fully supports this. Pre-built templates available in Notion gallery.

### 2. Airtable (2–4 hours)
Multiple linked tables (Contacts → Deals → Communications), form views, Kanban pipeline. Free tier: 1,000 records, 5 editors.

### 3. Google Sheets + AppSheet (1 hour)
Sheets with conditional formatting + dropdown status fields + Google Form for data entry. AppSheet (free, Google-owned) wraps it in a mobile app with filters and push notifications. No code required.

### 4. Glide (2–3 hours)
Connects to Google Sheet, wraps it in a mobile-first app UI. Free tier usable for small teams.

---

## Carrd Pro → Notion CRM Integration

**Stack:** Carrd Pro form → Make (free) → Notion database

### Why Make over Zapier
| | Make | Zapier |
|---|---|---|
| Free tier | 1,000 ops/month | 100 tasks/month |
| Multi-step flows | ✅ Free | ❌ Paid only |
| **Verdict** | Better for this | Too limited |

### Setup Steps (30–45 min)
1. Create Notion database with target fields
2. Create Make account → new Scenario → add **Webhooks** trigger → copy the webhook URL
3. Paste webhook URL into Carrd form settings → Submissions → Webhook
4. In Make, add Notion module → map form fields to Notion properties
5. **Critical:** Share your Notion database with the Make integration (one-click in Notion) — easy to forget

### Common Error: UUID Validation
**Error:** `[400] path failed validation: path.data_source_id should be a valid uuid`

**Cause:** The `{{1.email}}` variable was accidentally pasted into the **Database ID** field instead of the property mapping fields.

**Fix:**
- Database ID field = UUID only (auto-filled when you select database from dropdown)
- Properties section below = where `{{1.email}}`, `{{1.name}}` etc. go
```
Database ID:  a1b2c3d4-e5f6-7890-abcd-ef1234567890  ← never touch
Properties:
  Email    →  {{1.email}}       ← variables go HERE
  Name     →  {{1.name}}
```

---

## Email Campaigns from Notion

Notion has no built-in sending. Pattern: **Notion stores contacts → sync via Make → send from a dedicated tool.**

### Recommended Free Senders
| Tool | Free Tier | Best For |
|---|---|---|
| **Brevo** (formerly Sendinblue) | 300 emails/day, unlimited contacts | Larger lists |
| **Mailchimp** | 500 contacts, 1k emails/month | Small lists |
| **MailerLite** | 1,000 contacts, 12k emails/month | Cleanest UI |

### SMS
- **Twilio** — pay-as-you-go, ~$0.01/text, no monthly fee. Connect via Make.

### Social Retargeting
- Export CSV from Notion → upload as Custom Audience in Meta Ads or Google Ads

### Recommended Free Stack
```
Notion CRM (free)
      ↓
   Make (free tier)
   /         \
Brevo        Twilio
(email)      (SMS, pay-as-you-go)
```

---

## Carrd Landing Page + Email Form Setup

### Requirements
- Carrd **Pro Lite** ($19/year) — minimum plan for forms and webhooks

### Page Structure (High-Converting)
- Headline — what you're offering and who it's for
- Subheadline — one sentence on value
- Optional image — product photo or hero graphic
- Form — email field + submit button
- Social proof — "Join 200+ customers" if available

### Form Setup
1. Add Form element → delete Name field if email-only (lower friction, higher conversion)
2. Set email field type to `email`, mark required
3. Customize submit button label: "Get Early Access", "Join the List", etc.
4. Set Success message: "You're in! Check your inbox."
5. Set Submissions → Webhook → paste Make webhook URL

### Conversion Tips
| Do | Don't |
|---|---|
| One clear CTA | Multiple competing buttons |
| Benefit-focused headline | Feature-focused headline |
| Email field only | Long forms |
| Specific value prop | Vague "Sign up for updates" |

---

## UTM Tag Capture in Carrd + Notion

### UTM Parameters
```
yoursite.com/landing?utm_source=instagram
                     &utm_medium=paid_social
                     &utm_campaign=summer_sale
                     &utm_content=carousel_ad
                     &utm_term=buy_shoes
```

### Step 1: Notion CRM Fields to Add
| Property | Type | Captures |
|---|---|---|
| UTM Source | Text | `instagram`, `google`, `newsletter` |
| UTM Medium | Text | `paid_social`, `email`, `cpc` |
| UTM Campaign | Text | `summer_sale`, `launch_2024` |
| UTM Content | Text | `carousel_ad`, `banner_v2` |
| UTM Term | Text | Paid keyword |
| Landing Page | URL | Full URL |
| Lead Date | Date | Auto-set on row creation |

### Step 2: Carrd Hidden Fields
Add one hidden field per UTM parameter:
```
Hidden field name: utm_source    → value: {utm_source}
Hidden field name: utm_medium    → value: {utm_medium}
Hidden field name: utm_campaign  → value: {utm_campaign}
Hidden field name: utm_content   → value: {utm_content}
Hidden field name: utm_term      → value: {utm_term}
```
Carrd reads these from URL parameters automatically. Users never see them.

### Step 3: Make Mapping
```
Notion Property: UTM Source    → {{1.utm_source}}
Notion Property: UTM Medium    → {{1.utm_medium}}
Notion Property: UTM Campaign  → {{1.utm_campaign}}
Notion Property: UTM Content   → {{1.utm_content}}
Notion Property: UTM Term      → {{1.utm_term}}
Notion Property: Landing Page  → {{1.pageurl}}
Notion Property: Lead Date     → {{now}}
```

### Bonus: Notion Formula Field
Add a **Formula** property called `Source Label`:
```
prop("UTM Source") + " / " + prop("UTM Medium")
```
Produces readable `instagram / paid_social` label in main CRM view.

### URL Builder
Use Google's free UTM builder at `ga-dev-tools.google/campaign-url-builder`.

---

## Spam / Bot Protection

### Free Options

**1. Google reCAPTCHA v3 (recommended)**
- Free, forever, no limits
- Invisible — scores submissions in background, no user friction
- Carrd Pro has **built-in reCAPTCHA support** — paste site key into Site Settings → Forms
- Register at `google.com/recaptcha`

**2. Cloudflare Turnstile**
- Free, forever, privacy-friendly (less user tracking than reCAPTCHA)
- No native Carrd support — requires small HTML embed

**3. Honeypot Fields**
- Free, no third party
- Add hidden field that bots fill but humans never see
- Filter in Make before Notion write

### Recommended Combo for Gearhorse
```
reCAPTCHA v3 (Carrd built-in)  ← stops most bots at the door
       +
Honeypot field in Make          ← catches anything that slips through
```

---

## Honeypot Field Setup (Carrd + Make)

### How It Works
Hidden field added to form. Real users never see it, never fill it. Bots read raw HTML and fill every field automatically. Make checks the field — if populated, discard the submission.

### Step 1: Carrd Setup
1. Add a **Text** field to the form
2. Set Name to something bot-tempting: `website`, `url`, `phone`
3. Add Custom Attribute: `style` → `display:none`
4. Mark as **not required**

### Step 2: Make Filter
Between Webhook trigger and Notion module:
1. Click dotted line → **Add a Filter**
2. Configure:
```
Label:      Honeypot check
Condition:  {{1.website}}    →    is empty
```
Only proceeds to Notion if honeypot is empty (real human).

### Flow
```
Carrd form submitted
        ↓
Make Webhook receives it
        ↓
   Filter: is {{1.website}} empty?
        ↓              ↓
       YES             NO
        ↓              ↓
  Create Notion     Stop. Discard.
  CRM row           Bot detected.
```

### Important
Field name in Carrd must exactly match Make variable reference. Verify by checking Make execution log after first test submission.

---

## reCAPTCHA Without Domain-Matched Google Account

**Yes, any Google account works.** reCAPTCHA site keys are registered to a Google account, not verified against domain ownership. Google does not check DNS or registrar records.

### Steps
1. Log into any Google account (personal Gmail is fine)
2. Go to `google.com/recaptcha`
3. Register `gearhorse.camp` as the domain
4. Copy the site key
5. In Carrd → Site Settings → Forms → enable reCAPTCHA → paste site key

### Caveats
- The reCAPTCHA dashboard (bot stats, settings) lives in whichever Google account registered it
- Use a long-term account — avoid work/employer accounts you might lose access to

---

## Gearhorse-Specific UTM URL (Launch Announcement)

```
https://gearhorse.camp?utm_source=friends&utm_medium=personal&utm_campaign=launch_announcement
```

---

## Cost Summary

| Tool | Cost |
|---|---|
| Notion | Free |
| Make | Free (under 1k ops/mo) |
| Brevo | Free (under 300 emails/day) |
| Carrd Pro Lite | $19/year |
| GA4 + Clarity | Free forever |
| Stripe | Free until transactions (2.9% + 30¢) |
| Twilio SMS | ~$1 per 100 texts |
| reCAPTCHA | Free forever |
