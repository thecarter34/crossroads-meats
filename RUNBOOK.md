# Crossroads of the North Meats & Cellar — Client Runbook

> **Status:** Maintenance
> **Next:** Dennis feedback: (1) Spooner = pre-packaged from Amery sister facility — sourcing copy updated across all pages; (2) "CRN Meats & Cellar" short name adopted in nav, footers, titles, meta; (3) Venison service removed from Spooner — /venison-processing is now a pointer page to Amery Meat Market. Awaiting Dennis review on dev preview.
> **Blocker:** 🟡 Awaiting Dennis review of CRN name + Amery sourcing + venison-pivot changes on dev preview.
> **Last touched:** 2026-07-14

> Living document for the JJCTech engagement with **Jen & Dennis Lutz**
> Owner: Josh (JJCTech)
> Status detail: **DNS flip complete · apex live · www cancelled · Registrar transfer deferred (3-yr Squarespace registration) · GBP base optimization live (16 photos via API) · 1 client-side dependency (branding assets) + 1 ops follow-up (GBP UI-only via Dennis screen-share)**

> This file is the **active** source of truth. Historical content lives in `appendix-history.md`. Edit in place; append changes rather than rewriting.

---

## 1. Quick facts

| | |
|---|---|
| **Business** | Crossroads of the North Meats & Cellar |
| **Owners** | Jen Lutz & Dennis Lutz (family-owned, hands-on) |
| **Address** | 118 Ash Street, Spooner, WI 54801 |
| **Phone** | `(715) 416-0989` (on GBP; pending Dennis confirmation) |
| **Vertical** | Butcher + wine/beer/cellar shop (WI cheese, gifts, pet food, snack sticks/jerky) |
| **Status** | **Open — soft opening Jul 9 2026, prod apex live Jul 13 2026** |
| **Production URL** | https://crossroadsmeatsandcellar.com/ — **LIVE** via CF Pages |
| **Liquor license** | ✅ Class A combination; valid through Jun 30 2027 |
| **Currently hiring** | Store Manager + cleaning specialist |
| **Source of truth (comms)** | Personal — Josh knows Jen & Dennis personally |

---

## 2. Source-of-truth folders

All project files consolidated into a single folder (Jul 13, 2026):

| Purpose | Path |
|---|---|
| **Everything for this project** | `/workspace/clients/crossroads_meats_and_cellar/` |
| **Live site** | https://crossroadsmeatsandcellar.com/ |
| **GitHub repo** | https://github.com/thecarter34/crossroads-meats (`dev` working, `prd` production) |

Contents at the project root: docs (RUNBOOK, BRAND, audits, transfer-prep, BUILD_SPEC, research, discord-approval, draft-message, appendix-history, squarespace-transfer-steps, TEMPLATE_IMPROVEMENTS), site code (6 HTML pages, 404, robots, sitemap), `assets/` (images), `.hermes/plans/` + `desktop-attachments/`, `.git/`, `.gitignore`, `.wrangler/` (CF local cache).

> Folder naming note: original outreach build folder was `/workspace/jjctech-outreach/crossroads-meats/site/`. Briefly landed at `/workspace/hermes-workspace/clients/crossroads_meats_and_cellar/` on 2026-07-13 (one-step mistake) before being corrected to the proper clients root here at `/workspace/clients/crossroads_meats_and_cellar/` later that day. Matches the flat-layout convention used by the other 18 sibling client folders (beauty_and_blendz, hickory-hills-gc, carter_quality_construction, etc.).

---

## 3. Live site status (Cloudflare Pages)

| Check | Value | Last verified |
|---|---|---|
| HTTP 200 (apex prod) | ✅ | 2026-07-13 |
| WCAG 2.1 AA (axe-core) prod | ✅ 0/0/0/0 | 2026-07-13 |
| Console errors | 0 | 2026-07-13 |
| Broken links | 0 | 2026-07-13 |
| FCP (prod apex, warm cache) | 288ms | 2026-07-13 |
| Production `/sitemap.xml` | ✅ HTTP 200 `application/xml` (2 URLs) | 2026-07-13 |
| Production `/robots.txt` | ✅ sitemap directive | 2026-07-13 |
| Production unknown URL | ✅ HTTP 404 (real, not SPA fallback) | 2026-07-13 |
| Production JSON-LD | ✅ Valid | 2026-07-13 |
| Last deploy | `prd` commit `9eaaa3a` — PR #6 (sitemap, canonicals, real 404s, schema cleanup) | 2026-07-13 |

**Re-audit:** `~/.hermes/skills/site-audit/audit.sh https://crossroadsmeatsandcellar.com/`

---

## 4. Domain status — apex LIVE on CF Pages

| | |
|---|---|
| **Domain** | `crossroadsmeatsandcellar.com` |
| **Registrar** | Squarespace Domains (3-yr registration through ~Jun 2029 — transfer **deferred**) |
| **DNS authority** | Cloudflare (NS flipped 2026-07-13 16:58 UTC) |
| **Apex** | Served by CF Pages (verified HTTP 200) |
| **`www`** | **Cancelled Jul 13 2026** — apex is canonical |
| **SSL** | Auto via CF (Google CA) |
| **CF Pages project** | `crossroads-meats` · default branch `dev` · production branch `prd` |

**Why defer the Registrar transfer:** Squarespace holds the registration for 3 years. No renewal pressure. Apex works fine without it. Revisit ~Jun 2029 if cost matters then.

---

## 4F — Branch workflow (locked 2026-07-08)

- `dev` = working branch, CF Pages auto-builds preview URLs (`dev.<domain>.pages.dev`)
- `prd` = production, CF Pages auto-deploys when pushed via PR merge
- Squash-merge only (`git-ship dev prd` or `gh pr merge --squash --admin`)
- Branch protection on `prd`: linear history, 1 PR approval, no force-push, no deletions
- Single-contributor repo → use `--admin` for self-merge (documented precedent; revisit if multi-contributor review becomes standard)

---

## 4V — Local dev server for change verification (Jul 14)

**Use a local Python HTTP server to verify HTML changes before pushing.** Do NOT rely on CF Pages preview URLs (`dev.crossroads-meats.pages.dev`) — they alias to old builds and lag the actual `dev` branch by several minutes. Run the server locally, hit it with `curl` or the browser, and confirm changes are correct before `git push`.

### Start the server

```bash
# Background, bound to localhost only
python3 -m http.server 8765 --bind 127.0.0.1 &
# From /workspace/clients/crossroads_meats_and_cellar/
```

### Smoke test all pages

```bash
for page in "" custom-cutting cabin-provisions gift-baskets venison-processing hiring 404; do
  if [ -z "$page" ]; then url="http://127.0.0.1:8765/"; else url="http://127.0.0.1:8765/$page.html"; fi
  code=$(curl -s -o /dev/null -w "%{http_code}" "$url")
  echo "$page: $code"
done
# Expected: index/custom-cutting/cabin-provisions/gift-baskets/venison-processing/hiring/404 all 200
```

### Visual review

Load `http://127.0.0.1:8765/` in a browser (the Hermes browser tool works) and check the changes you're working on. No build, no deploy, no CF API call needed.

### Ad-hoc content checks

```bash
# Confirm no "hand-cut" / "cut in-house" claims slip back into body copy
grep -rn "hand-cut\|cut in-house\|butcher-cut\|on the block" /workspace/clients/crossroads_meats_and_cellar/*.html \
  | grep -v "sister facility in Amery\|alt=\"Hand-cut ribeye"

# Confirm all 5 sub-page nav links exist on every page
for f in /workspace/clients/crossroads_meats_and_cellar/*.html; do
  echo "=== $(basename $f) ==="
  awk '/<nav class="navbar/,/<\/nav>/' "$f" | grep -oE 'href="[^"]+"' | sort -u
done
```

### Stop the server

```bash
# Find and kill the python http.server
pkill -f "http.server 8765"
```

### Why this matters

On Jul 14, three rounds of CF Pages previews (`b93cb669`, `4b5a9e46`, `6999b439`) all showed stale content because:
- `dev.crossroads-meats.pages.dev` aliases to the wrong build
- Each push triggers a new build, but the alias doesn't update until the build finishes
- The build ID URL is the right one but takes a minute to become reachable

A local server is instant, always correct (serves from the actual working tree), and never out of sync. **Use it for every iteration before pushing.**

---

## 5. Content / open items

### Site content (current state)
- Hero with opening badge, "What We Carry" 6-card grid, "About" story, "Three Rooms" feature tiles (Jen's 3 photos), marquee gallery (8 photos), hiring CTA, FAQ, location/map
- All on-site schema valid: `GroceryStore/LiquorStore`, `WebSite`, `BreadcrumbList`, `FAQPage`, `JobPosting` (with correct identifier, no fake salary)

### Meta / socials — Dennis raised this (DEFERRED)
- Don't pitch ads before 30 days post-opening
- Circle back after soft opening settles
- JJCTech recommendation: organic-only first, ads later

### Branding assets (waiting on client)
- Currently using Josh-supplied `assets/logo.jpeg` placeholder
- Real logo / hero photos / color palette refinements → swap when delivered
- Low impact — site runs fine as-is

---

## 6. Pending action items

### Open (2026-07-13)

| # | Ask | Why | Status | Impact |
|---|---|---|---|---|
| 1 | **GBP screen-share with Dennis** (~30 min) | Q&A, serviceItems, verification status, MON/TUE/WED hours, phone confirmation | 🟡 waiting on Dennis | Medium |
| 2 | **Branding assets** (logo, photos, palette) | Replace placeholder | 🟡 waiting on client | Low |
| 3 | **GSC sitemap cleanup** | Manually delete mistaken homepage sitemap entry; resubmit `/sitemap.xml` | 🟡 GSC OAuth is read-only — UI only | Low |
| 4 | **Re-audit at 2 weeks post-launch** | Confirm warm-cache FCP ~300ms | 🟢 scheduled via cron `b38c43bff207` for 2026-07-27 | Low |
| 5 | ~~CF Pages preview URL pattern (Jul 14)~~ | — | 🟢 RESOLVED 2026-07-14 — replaced by local dev server (§4V) | — |
| 5 | ~~Squarespace login~~ | — | 🟢 RESOLVED 2026-07-13 (NS flipped) | — |
| 6 | ~~Google account for GBP~~ | — | 🟢 RESOLVED 2026-07-13 (Dennis created GBP, JJCTech is Manager) | — |
| 7 | ~~`www` redirect~~ | — | 🟢 CANCELLED 2026-07-13 (3-yr Squarespace registration, no urgency) | — |
| 8 | ~~Registrar transfer~~ | — | 🟢 DEFERRED 2026-07-13 (3-yr registration, no urgency) | — |

### Scheduled reminders

- **2026-07-27** (cron `b38c43bff207`) — 2-week post-launch check: GBP screen-share status, branding assets, GSC cleanup, site re-audit
- **2029-06-01** (manual reminder, not scheduled) — Revisit Registrar transfer decision before Squarespace renewal

---

## 7. Decisions log (recent)

| Date | Decision |
|---|---|
| 2026-07-14 | **Venison service removed from Spooner (per Dennis via Josh).** `venison-processing.html` rewritten as a pointer page to Amery Meat Market (116 Central St, 715-268-7515, amerymeatmarket.com). Index FAQ answers corrected for both custom-meat-cutting and wild-game questions (both JSON-LD and visible accordion). `hiring.html` Weekly Cleaning Specialist description drops "meat counter, prep area" (neither exists at Spooner). `custom-cutting.html` intro rephrased for clarity. `sitemap.xml` adds `<priority>0.5</priority>` to /venison-processing to demote. Schema.org `Service` for venison processing removed. Plan: `.hermes/plans/venison-pivot.md`. Verified on local http.server 8765 — NOT pushed to dev. |
| 2026-07-14 | Sourcing copy corrected across all pages: Spooner is pre-packaged from sister facility in Amery. Hero rewritten ("Pre-packaged from our sister facility. House-made brats. Spooner's newest butcher and cellar."), 6-card "Hand-Cut Meats" renamed "Hand-Cut in Amery", "Cut in-house" pillar renamed "Cut in Amery, finished in Spooner", schema product updated, meta/OG/Twitter updated. House-made brats/snack sticks/jerky still attributed to Spooner (correct). |
| 2026-07-14 | CRN Meats & Cellar short name rolled out across all pages (logo, title, preloader, footer, mobile menu). Sub-page navs unified to 7 items each: Home, What We Carry, Custom Cutting, Cabin Provisions, Gift Baskets, Venison Processing, Now Hiring, Visit Us. Sub-page footers unified to same Explore set. |
| 2026-07-14 | **Local dev server workflow adopted for change verification** (`python3 -m http.server 8765 --bind 127.0.0.1`). Faster than CF Pages preview URLs, never out of sync with working tree, no API calls. See §4V. |
| 2026-07-13 | GBP photo upload via API (16 photos, uniform 1920×1080). Revises the `google-business-profile-optimization` skill pitfall #4: `mybusiness.googleapis.com/v4/.../media` IS alive, single-call upload works. |
| 2026-07-13 | `www` redirect CANCELLED; Registrar transfer DEFERRED. Squarespace holds 3-yr registration (~Jun 2029). Apex is canonical. |
| 2026-07-13 | NS flipped to Cloudflare; prod apex LIVE. CF Pages custom domain attached, SSL via Google CA, audit clean. |
| 2026-07-08 | Branch workflow locked: `dev` working, `prd` production, squash-merge via `git-ship`, single-contributor `--admin` override. |
| 2026-07-04 | Josh is taking over ownership of `crossroadsmeatsandcellar.com`. Plan: NS-only setup (chosen over full Registrar transfer for now). |

---

## 8. Contact

- Jen Lutz — open via personal relationship
- Dennis Lutz — open via personal relationship
- Internal (Josh's): `littlecarter21@gmail.com`

---

## 9. Related projects (same owner)

Dennis & Jen Lutz also own Amery Meat Market. Engagement is confirmed but no scope kickoff yet — see `/workspace/clients/amery-meat-market/RUNBOOK.md`. **Do not start Amery design/build work until after Crossroads soft opening settles.**

---

## 10. Brand guide

See `/workspace/clients/crossroads_meats_and_cellar/BRAND.md` for visual identity (color tokens, AA pair ratios), voice/tone (DRAFT, pending client sign-off), tagline options, photography direction, and client-confirmation checklist.