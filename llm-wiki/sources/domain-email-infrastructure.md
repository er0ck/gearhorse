---
title: Domain Registration, Email Hosting & Web Platform Infrastructure
type: source
tags: [infrastructure, domain, email, carrd, shopify, google-workspace, improvmx, zoho, porkbun, namecheap, cloudflare]
created: 2026-04-15
source_type: conversation
---

# Domain Registration, Email Hosting & Web Platform Infrastructure

> Source type: Q&A conversation  
> Date: 2026-04-15  
> Context: Evaluating cost-effective infrastructure options for a pre-launch brand (Gearhorse) using Carrd Pro for landing pages and potentially Shopify for e-commerce.

---

## Domain Registration

Cost-effective registrar options, roughly $9–14/yr for common TLDs:

- **Porkbun** — very cheap, free WHOIS privacy, good for bulk; supports broad TLD catalog
- **Namecheap** — similar pricing, clean UI, solid support, broad TLD catalog
- **Cloudflare Registrar** — at-cost (no markup), free WHOIS privacy; transfer-only (can't register new domains); limited TLD catalog, especially weak on ccTLDs
- **Squarespace Domains** (formerly Google Domains) — ~$12/yr, simple but slightly pricier

**Best for new registrations:** Porkbun or Namecheap.  
**Best for transfers:** Cloudflare (zero markup forever).

---

## Email Hosting (3rd Party, domain-agnostic)

All services below work via MX record delegation — independent of web/app host.

| Service | Cost | Notes |
|---|---|---|
| ImprovMX | Free / $4/mo | Forwarding/alias only — no mailbox. Pairs with Gmail "Send As". |
| Zoho Mail (Free) | $0 | 1 domain, up to 5 users, 5GB. Best free full mailbox option. |
| Zoho Mail (Lite) | ~$1/user/mo | Removes free tier limits. |
| Migadu | $4/mo flat | Unlimited domains and users. Excellent for multi-project setups. |
| Fastmail | $3–5/user/mo | Clean, reliable, privacy-focused. |
| Google Workspace | $6/user/mo | Premium; overkill for solo/small use. |

### ImprovMX + Gmail "Send As" (recommended starter path)

Zero-cost setup for solo use:
1. Add ImprovMX MX records at registrar → inbound to custom domain forwarded to Gmail
2. Gmail → Settings → Accounts → "Add another email address" → enter custom domain address
3. **Check "Treat as an alias"** — this makes Gmail treat both addresses as the same person; auto-selects the right reply-from address; correct for this use case
4. Gmail sends verification email → ImprovMX forwards it to Gmail → click confirm

Limitation: Raw email headers may show "sent via Gmail." Not visible to most recipients.

### When to use Zoho instead

Use Zoho Mail free tier (not ImprovMX) when:
- Email credibility matters (client-facing, customer support)
- You want a clean separate mailbox with no Gmail fingerprints
- You anticipate needing to migrate later — better to start clean

---

## TLD Compatibility Notes

- **Email hosts and web platforms (Carrd, Shopify) are TLD-agnostic** — they operate via DNS records regardless of TLD
- **Registrar TLD availability varies** — Cloudflare's catalog is notably limited; Porkbun/Namecheap are broader
- **Some ccTLDs are registry-locked** — only authorized (often local) registrars can sell them (.de, .ca, .au, etc.)
- **Deliverability risk TLDs** — .xyz, .top, .click, .loan are spam-associated and may be filtered by recipient mail servers
- **WHOIS privacy restrictions** — .us officially prohibits WHOIS privacy protection
- **Renewal pricing traps** — .io registers cheap but renews ~$40–50/yr at many registrars; always check renewal price before registering

---

## Carrd & Shopify DNS Integration

Both platforms accept any externally registered domain:
- Point domain via A/CNAME records at registrar → Carrd or Shopify handles web
- Set MX records separately at registrar → email handled by Zoho, ImprovMX, etc.
- Neither platform provides email hosting; 3rd-party email is the standard path for both

### Recommended starter stack (lowest viable cost)

| Layer | Choice | Cost |
|---|---|---|
| Domain | Porkbun | ~$9/yr |
| Email | ImprovMX free + Gmail Send As | $0 |
| Website | Carrd Pro | $19/yr |
| **Total** | | **~$28/yr** |

Next step-up: swap ImprovMX → Migadu ($48/yr) for multi-domain or cleaner sending.

---

## Google Workspace: Enabling Gmail "Send As" for Aliases

For users on a Google Workspace org (not personal Gmail), sending from a custom domain alias may require admin configuration.

### Two outbound send paths

1. **Send through Google's servers (default, simpler)**  
   Usually works without admin changes. Gmail sends a verification email to the alias address; ImprovMX forwards it to Gmail; user clicks confirm. Recommended first attempt.

2. **Send through external SMTP**  
   Requires admin to enable: Admin Console → Apps → Google Workspace → Gmail → End User Access → **"Allow per-user outbound gateways"**  
   Only needed if routing outbound mail through a specific external mail server.

### If alias addition is blocked entirely

Check: Admin Console → Apps → Google Workspace → Gmail → End User Access  
Look for restrictions on adding external addresses or sending as non-Workspace addresses. Options visible depend on Workspace tier.

**Practical note:** Try the standard verification flow first — it works without admin changes in most Workspace configurations.

---

## Key Decisions & Recommendations (Summary)

1. **Register at Porkbun or Namecheap** for broad TLD support and free WHOIS privacy
2. **Use ImprovMX + Gmail "Send As"** for zero-cost custom email on solo/pre-launch projects; check "Treat as an alias" during setup
3. **Upgrade to Zoho Mail free** when email credibility matters or client correspondence begins
4. **Migadu ($4/mo flat)** is the best value if managing multiple domains/projects
5. **Carrd and Shopify are fully compatible** with any registrar and any 3rd-party email host via standard DNS
6. **For Workspace orgs**, try standard alias setup first before touching admin settings
