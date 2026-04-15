---
title: "Summary: Domain Registration, Email Hosting & Web Platform Infrastructure"
type: source
tags: [infrastructure, domain, email, carrd, shopify, improvmx, zoho, porkbun, namecheap, cloudflare, gearhorse]
created: 2026-04-15
updated: 2026-04-15
sources: 1
---

# Summary: Domain Registration, Email Hosting & Web Platform Infrastructure

**Source file:** `sources/domain-email-infrastructure.md`
**Source type:** Q&A conversation
**Date captured:** 2026-04-15

---

## What this source covers

Options and trade-offs for domain registration, email hosting, and DNS integration with Carrd and Shopify, evaluated in the context of [[Gear Horse]]'s pre-launch stack.

---

## Key takeaways

### Domain registrars
- Porkbun and Namecheap are the best options for new registrations: cheap (~$9–14/yr), free WHOIS privacy, broad TLD catalogs
- Cloudflare Registrar is best for transfers (zero markup, no ongoing premium), but can't register new domains and has a limited TLD catalog
- Always check renewal pricing before registering: .io renews at ~$40–50/yr at many registrars

### Email hosting
The standard pattern for any pre-launch brand: third-party email provider via MX records, independent of the web host.

- **Free, zero-friction:** ImprovMX (forwarding only) + Gmail "Send As" — inbound forwarded to Gmail, outbound sent with custom domain address. During setup, check "Treat as an alias" so Gmail auto-selects the right reply-from address.
- **Free full mailbox:** Zoho Mail free tier (1 domain, 5 users, 5GB). Better when email credibility matters or client correspondence is involved.
- **Multi-domain best value:** Migadu ($4/mo flat, unlimited domains and users)
- **Overkill for solo/pre-launch:** Google Workspace ($6/user/mo)

### TLD notes
- Email hosts, Carrd, and Shopify are all TLD-agnostic (DNS-based)
- Spam-risk TLDs to avoid: .xyz, .top, .click, .loan — may be filtered by recipient mail servers
- .us prohibits WHOIS privacy protection

### DNS integration
Carrd and Shopify both accept any externally registered domain via A/CNAME records. Neither provides email hosting; MX records are set separately for whichever email provider is used.

### Workspace orgs
For users on Google Workspace (not personal Gmail), try the standard alias verification flow first before touching admin settings. Outbound through Google's servers usually works without admin changes.

---

## Recommended starter stack

| Layer | Choice | Cost |
|---|---|---|
| Domain | Porkbun | ~$9/yr |
| Email | ImprovMX free + Gmail Send As | $0 |
| Website | Carrd Pro | $19/yr |
| **Total** | | **~$28/yr** |

---

## Pages created or updated from this source

- [[Domain and Email Infrastructure]] (concept) — new
- [[Gear Horse]] (entity) — infrastructure section updated
- [[Carrd Make Notion Stack]] (concept) — DNS/email layer clarified
