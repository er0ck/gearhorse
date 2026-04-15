---
title: Honeypot Spam Protection
type: concept
tags: [spam, security, forms, carrd, make, honeypot, recaptcha, gearhorse]
created: 2026-04-14
updated: 2026-04-14
sources: 1
---

# Honeypot Spam Protection

A free, no-friction spam protection technique used in the [[Carrd Make Notion Stack]]. Combined with reCAPTCHA v3 as a two-layer defense.

---

## How it works

A hidden field is added to the Carrd form. Real users never see it, so they never fill it. Bots read raw HTML and fill every field they find. Make checks the field on receipt: if it's populated, the submission is discarded. If it's empty, the submission is a real human and proceeds to Notion.

---

## Setup: Carrd

1. Add a Text field to the form
2. Set the name to something bot-tempting: `website`, `url`, or `phone`
3. Add a Custom Attribute: `style` → `display:none`
4. Mark as not required

---

## Setup: Make filter

Between the Webhook trigger and the Notion module:
1. Click the dotted line between modules → Add a Filter
2. Configure:
   - Label: `Honeypot check`
   - Condition: `{{1.website}}` is empty

Only proceeds to create the Notion row if the honeypot field is empty.

---

## Flow

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

---

## Important note

The field name in Carrd must exactly match the variable reference in Make. Verify by checking the Make execution log after the first test submission.

---

## Combined protection pattern

```
reCAPTCHA v3 (Carrd built-in)  ← stops most bots at the door
       +
Honeypot field in Make          ← catches anything that slips through
```

reCAPTCHA v3 is invisible (no user friction, background scoring). Site keys are registered to a Google account, not verified against domain ownership — any Google account can register gearhorse.camp.

---

## Sources

- [[summary: gearhorse-tech-stack-setup]]
