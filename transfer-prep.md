# Crossroads Domain Transfer — Prep for Dennis Call

> Document to bring to your call with Dennis.
> Goal: leave the call with everything unlocked and the EPP code on the way.

---

## TL;DR (what to tell Dennis)

"Hey, the website is built and audit-clean. To get it live on `crossroadsmeatsandcellar.com`, we need to transfer the domain from Squarespace to Cloudflare. It's a 30-min process total. Squarespace has the domain locked by default — I need you to log into Squarespace and do 4 things. After that, the domain flips to the real site in under an hour."

---

## Why transfer (the "why" — only say this if he asks)

- Squarespace currently serves a "Coming Soon" parking page on the domain
- The real site (built and ready) lives at `crossroads-meats.pages.dev` but is invisible until the domain points at it
- Cloudflare Registrar is cheaper long-term (~$10–14/yr vs Squarespace's $18–20/yr)
- One dashboard for everything (DNS, SSL, hosting, future email if needed)
- Squarespace auto-renew bundles with their site builder we don't use — JJCTECH absorbs the cost as customer acquisition
- Domain is registered through Jun 2029, so we're not at risk of losing it

---

## Domain facts (reference)

| | |
|---|---|
| Domain | `crossroadsmeatsandcellar.com` |
| Registrar | Squarespace Domains LLC |
| Registered | Jun 9, 2026 |
| Expires | Jun 9, 2029 |
| Current status | `client delete prohibited`, `client transfer prohibited` (default lock) |
| Current DNS | A records → 198.49.23.144 (Squarespace parking) |
| Nameservers | nsd1–4.squarespacedns.com |

---

## What Dennis needs to do in Squarespace (4 steps, ~10 min)

I'll write this out so he has it on hand. He can screenshot this section or you can walk him through it on the call.

### Step 1 — Log into Squarespace
- URL: https://account.squarespace.com/
- Use whatever email owns the domain registration (likely Dennis's personal email)

### Step 2 — Unlock the domain for transfer
- Domains panel → click `crossroadsmeatsandcellar.com`
- Look for the **"Registration & Privacy"** or **"Domain Lock"** section
- Toggle **"Domain Lock"** OFF (or **"Unlock for transfer"**)
- This removes the `client transfer prohibited` flag

### Step 3 — Disable WHOIS privacy (optional but recommended)
- In the same domain settings
- Turn off Squarespace's WHOIS privacy proxy
- **Why**: when the domain transfers, the registrant email needs to be reachable for confirmation. Squarespace's proxy hides it.
- Note: Cloudflare Registrar uses its own WHOIS privacy by default, so no info leaks

### Step 4 — Disable auto-renew / bundle (cleanup)
- Domains panel → click the domain
- Settings → **"Auto-Renew"** → turn off
- If Squarespace is bundling with a "Website" or "Site Builder" plan, untangle
- **Why**: clean handoff. After transfer, JJCTECH owns the renewal calendar (currently expires Jun 2029, so 3 years of buffer).

### Step 5 — Request the EPP / auth code
- Domains panel → click the domain
- Look for **"Transfer Domain Away"** or **"Get Auth Code"**
- Squarespace emails the EPP code to the registrant contact (typically within minutes)
- **This is what I need from you most** — forward me the email with the code, or send me the code over text

---

## What JJCTECH (Josh) does after the call

Once Dennis has the EPP code, the rest is mine. ~30 min total:

1. Log into Cloudflare dashboard
2. Registrar → **Transfer domains** → paste EPP code
3. Approve the transfer-confirmation email when it arrives
4. Wait 5 min – 24 hrs (usually < 1 hr) for transfer to complete
5. In Cloudflare Pages → `crossroads-meats` project → **Settings** → **Custom domains** → add both:
   - `crossroadsmeatsandcellar.com`
   - `www.crossroadsmeatsandcellar.com`
6. SSL auto-provisions (Let's Encrypt)
7. Run the audit: `~/.hermes/skills/site-audit/audit.sh https://crossroadsmeatsandcellar.com/`
8. Calendar reminder for renewal (~Jun 2027 first auto-renew)

---

## Risk questions Dennis might ask (with answers)

**"Does this cost anything?"**
- Squarespace's 60-day free-transfer window: the domain was registered Jun 9, so we have until ~Aug 8, 2026 for free transfer. After that, ~$10–14/yr Cloudflare renewal (cheaper than Squarespace's $18–20). Either way, JJCTECH covers it.

**"What if something goes wrong?"**
- Domain transfer is reversible for 60 days after initiation. If anything breaks, the domain stays at Squarespace.
- The site stays at `crossroads-meats.pages.dev` the entire time — nothing to lose.

**"Do I lose anything?"**
- No email is currently set up on the domain (we'd have to set up forwarding anyway).
- No Squarespace site is being deleted (the parking page just goes away when DNS flips).

**"Will the 'Coming Soon' page go away?"**
- Yes, replaced by the real site. Anyone with the link will see the actual shop page.

**"How long is the Squarespace parking page visible?"**
- After transfer + DNS flip, the parking page stops. Some DNS resolvers take 5–60 min to update (rarely up to 4 hrs).

---

## If Dennis says no / wants to keep Squarespace as registrar

Tell me — I'll handle DNS via Cloudflare-for-SaaS mode (no transfer needed). It's worse long-term but works.

---

## What I need from you on the call

1. **Yes/no on the transfer itself** (recommended: yes)
2. **Squarespace login or admin access** (or Dennis does the unlock himself and sends me the EPP code)
3. **Best email to send the EPP code to** (or have Dennis forward the email to me)

If he can do the unlock steps (2, 3, 4) before we get to step 5, we're already 80% there. The EPP code is the actual blocker.

---

## Backup plan (if EPP request fails or stalls)

- Cloudflare can attempt transfer via WHOIS-based lookup as fallback
- Squarespace transfer-confirmation email is sent to registrant email — make sure Dennis checks spam
- If 60-day free window lapses, transfer still works at $10–14/yr renewal (vs Squarespace's $18–20/yr) — JJCTECH still saves money long-term