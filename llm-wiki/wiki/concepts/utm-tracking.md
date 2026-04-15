---
title: UTM Tracking
type: concept
tags: [analytics, utm, carrd, make, notion, gearhorse, marketing]
created: 2026-04-14
updated: 2026-04-14
sources: 1
---

# UTM Tracking

Pattern for capturing UTM parameters from landing page URLs and persisting them to the [[Carrd Make Notion Stack]] CRM.

---

## What UTM parameters are

Query string parameters appended to URLs to identify traffic sources:

```
gearhorse.camp?utm_source=instagram
               &utm_medium=paid_social
               &utm_campaign=launch_2026
               &utm_content=carousel_ad
               &utm_term=campfire
```

---

## Setup: Carrd hidden fields

Add one hidden field per UTM parameter to the Carrd form. Carrd reads these from URL query strings automatically. Visitors never see them.

```
Hidden field name: utm_source    → value: {utm_source}
Hidden field name: utm_medium    → value: {utm_medium}
Hidden field name: utm_campaign  → value: {utm_campaign}
Hidden field name: utm_content   → value: {utm_content}
Hidden field name: utm_term      → value: {utm_term}
```

---

## Setup: Notion CRM fields

Add these properties to the Notion CRM database:

| Property | Type |
|---|---|
| UTM Source | Text |
| UTM Medium | Text |
| UTM Campaign | Text |
| UTM Content | Text |
| UTM Term | Text |
| Landing Page | URL |
| Lead Date | Date |

Add a Formula property called `Source Label`:
```
prop("UTM Source") + " / " + prop("UTM Medium")
```

This produces a readable `instagram / paid_social` label in the main CRM view.

---

## Setup: Make mapping

```
Notion: UTM Source    → {{1.utm_source}}
Notion: UTM Medium    → {{1.utm_medium}}
Notion: UTM Campaign  → {{1.utm_campaign}}
Notion: UTM Content   → {{1.utm_content}}
Notion: UTM Term      → {{1.utm_term}}
Notion: Landing Page  → {{1.pageurl}}
Notion: Lead Date     → {{now}}
```

---

## Gear Horse signature UTM links

[[Eric Thompson]]'s email signature includes UTM-tagged links:
- Homepage: `?utm_source=email&utm_medium=signature&utm_campaign=brand&utm_content=website`
- Trail Maul: `?utm_source=email&utm_medium=signature&utm_campaign=product&utm_content=trail-maul`
- Ember Lid: `?utm_source=email&utm_medium=signature&utm_campaign=product&utm_content=ember-lid`
- Chair Armor: `?utm_source=email&utm_medium=signature&utm_campaign=product&utm_content=chair-armor`

---

## URL builder

Google's free UTM builder: `ga-dev-tools.google/campaign-url-builder`

---

## Sources

- [[summary: gearhorse-tech-stack-setup]]
