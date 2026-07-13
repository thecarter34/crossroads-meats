# Squarespace → Cloudflare Domain Transfer (Self-Service)

> Use once you have Squarespace login credentials from Dennis.

---

## Login
https://account.squarespace.com/

---

## Step 1 — Unlock the domain
1. Click the Squarespace logo (top-left) → **Domains** (in the sidebar under "Sales & Marketing" or similar)
2. Click `crossroadsmeatsandcellar.com`
3. Look for **"Registration & Privacy"** or scroll to find the lock toggle
4. Toggle **"Domain Lock"** OFF
   - If you see "This domain is locked", click it → confirm unlock
5. Wait ~5 minutes for WHOIS to update (you can verify at https://rdap.verisign.com/com/v1/domain/crossroadsmeatsandcellar.com — the `client transfer prohibited` flag should clear)

## Step 2 — Disable WHOIS privacy (optional)
1. Same domain settings page
2. Look for "WHOIS Privacy" or "Privacy Proxy"
3. Turn OFF
   - Squarespace will show a warning — confirm
   - Reason: when the domain transfers to Cloudflare, the registrant email needs to be reachable for confirmation. Cloudflare sets its own privacy by default after transfer, so nothing leaks permanently.

## Step 3 — Disable auto-renew
1. Same domain settings
2. **Auto-Renew** → OFF
3. Save
   - This prevents Squarespace from charging for a year we're not using.

## Step 4 — Get the EPP / auth code
1. Same domain settings, scroll to bottom
2. Look for **"Transfer Domain Away"** or **"Get Auth Code"** button
2. Click → Squarespace sends the EPP code to the **registrant email** (probably Dennis's)
3. Check the email — code is usually 30+ chars, format like `ABC123XYZ...`
4. **Forward the email to me** (or text the code) — I need it to initiate the Cloudflare transfer

---

## Step 5 — Confirm transfer-out via email
- Squarespace may also send a separate "confirm transfer out" email to the registrant
- Dennis (or whoever owns the registrant email) needs to click the confirmation link
- This is the second blocker after the EPP code — both must happen

---

## After you have the EPP code (hand to me)

I take over from here:
1. Cloudflare dashboard → Registrar → Transfer domains → paste EPP code
2. Pay first-year (FREE if within 60-day window — domain registered Jun 9 2026, window closes ~Aug 8 2026)
3. Approve Cloudflare's transfer-confirmation email
4. Wait 5 min – 24 hrs (usually < 1 hr) for transfer to land
5. Add `crossroadsmeatsandcellar.com` + `www` to CF Pages `crossroads-meats` custom domains
6. SSL auto-provisions
7. Run site audit
8. Calendar reminder for renewal

---

## What can go wrong

| Issue | Fix |
|---|---|
| Squarespace login doesn't have domain access | Ask Dennis to add you as admin: Account → Permissions → invite your email |
| Domain Lock toggle grayed out | Account might not have transfer privileges; ask Dennis to do it |
| EPP email doesn't arrive | Check spam. If still nothing after 30 min, contact Squarespace support |
| EPP code rejected by Cloudflare | Re-request code (each one is single-use, sometimes typos happen) |
| 60-day window lapsed | Transfer still works at $10–14/yr (cheaper than Squarespace $18–20 anyway) |

---

## Time estimate
- Steps 1-4: 10-15 min
- EPP email arrives: 5-30 min
- Step 5: 2 min (click email link)
- Hand to me: instant
- Total: ~1 hour to fully transferred and live

---

## Verify your work
After step 1, confirm the unlock worked:
```bash
# Check WHOIS — should NOT show 'client transfer prohibited'
curl -s https://rdap.verisign.com/com/v1/domain/crossroadsmeatsandcellar.com | python3 -c "
import json, sys
d = json.load(sys.stdin)
print('Status:', d.get('status'))
"
```
Expected: `Status: ['client delete prohibited']` (only the delete lock remains, transfer lock is gone)

---

## Send to me when you have it
- The EPP code (text is fine), or
- A screenshot of the email with the code

I'll handle the rest.