---
title: Domain and Email Infrastructure
type: concept
tags: [infrastructure, domain, email, dns, porkbun, namecheap, cloudflare, improvmx, zoho, migadu, carrd, shopify, gearhorse]
created: 2026-04-15
updated: 2026-04-15
sources: 1
---

# Domain and Email Infrastructure

How to set up cost-effective domain registration and email for a pre-launch brand. This is the infrastructure layer beneath [[Carrd Make Notion Stack]].

---

## Domain registrars

| Registrar | Best for | Notes |
|---|---|---|
| **Porkbun** | New registrations | Very cheap, free WHOIS privacy, broad TLD catalog |
| **Namecheap** | New registrations | Similar pricing, clean UI, broad TLD catalog |
| **Cloudflare Registrar** | Transfers only | Zero markup forever; can't register new domains; limited TLD catalog |
| Squarespace Domains | Simplicity | ~$12/yr, slightly pricier, limited appeal |

For [[Gear Horse]]: Porkbun is the recommended registrar (gearhorse.camp, .camp TLD, confirmed available).

### TLD watch-outs
- **.io** registers cheap but renews ~$40–50/yr at many registrars — always check renewal price before registering
- **.xyz, .top, .click, .loan** — spam-associated, may be filtered by recipient mail servers
- **.us** — WHOIS privacy officially prohibited
- **Some ccTLDs** (.de, .ca, .au) are registry-locked and can only be registered through authorized local registrars

---

## Email hosting options

All options below work via MX record delegation, independent of the web host (Carrd, Shopify, etc.).

| Service | Cost | Type | Best for |
|---|---|---|---|
| **ImprovMX** | Free / $4/mo | Forwarding + alias only | Solo, pre-launch |
| **Zoho Mail** | Free (1 domain, 5 users) | Full mailbox | When email credibility matters |
| **Zoho Mail Lite** | ~$1/user/mo | Full mailbox | Light upgrade from free |
| **Migadu** | $4/mo flat | Full mailbox, unlimited domains | Multi-domain / multi-project |
| Fastmail | $3–5/user/mo | Full mailbox | Privacy-focused |
| Google Workspace | $6/user/mo | Full mailbox | Premium; overkill solo/pre-launch |

---

## ImprovMX + Gmail "Send As" setup

The zero-cost path for solo/pre-launch use:

1. Add ImprovMX MX records at registrar — inbound to custom domain forwards to Gmail
2. Gmail → Settings → Accounts → "Add another email address" → enter custom domain address
3. **Check "Treat as an alias"** — makes Gmail treat both addresses as the same person; auto-selects correct reply-from; this is almost always the right setting
4. Gmail sends verification email → ImprovMX forwards it to Gmail → click confirm

Limitation: raw email headers may show "sent via Gmail." Not visible to most recipients.

---

## When to upgrade from ImprovMX to Zoho

Use Zoho Mail free tier when:
- Email credibility matters (customer support, client-facing correspondence)
- You want a clean mailbox with no Gmail fingerprints in headers
- You anticipate migration — cleaner to start there

---

## DNS integration with Carrd and Shopify

Both platforms accept any externally registered domain:
- Point domain via A/CNAME records at registrar → Carrd or Shopify handles web
- Set MX records separately → email handled by ImprovMX, Zoho, etc.
- Neither platform provides email hosting

This means registrar, web platform, and email provider are fully independent choices.

---

## Google Workspace: alias setup

For users on a Workspace org (not personal Gmail):
- Try the standard alias verification flow first — it works without admin changes in most configurations
- Outbound via Google's servers (default) usually requires no admin changes
- Outbound via external SMTP requires admin to enable: Admin Console → Gmail → End User Access → "Allow per-user outbound gateways"
- If alias addition is blocked: Admin Console → Gmail → End User Access → look for restrictions on external addresses

---

## Recommended starter stack

| Layer | Choice | Cost |
|---|---|---|
| Domain | Porkbun | ~$9/yr |
| Email | ImprovMX free + Gmail Send As | $0 |
| Website | Carrd Pro | $19/yr |
| **Total** | | **~$28/yr** |

Next step-up: swap ImprovMX for Migadu ($48/yr flat) when managing multiple domains or when clean sending matters.

---

## Sources

- [[summary: domain-email-infrastructure]]
