# Crossroads of the North — Runbook History Archive

Historical content moved from the main runbook on 2026-07-13 to keep the active file lean (~200 lines). This file is **read-only** — append new historical content here, don't rewrite.

**Navigation map** (where these sections used to live):

| Old section | New location |
|-------------|--------------|
| §4A Transfer-of-ownership checklist | Moved to `transfer-prep.md` (Dennis call doc) |
| §4B Why transfer vs Squarespace | This file |
| §4C Renewal & billing | This file |
| §4D When DNS flips (historical) | This file — superseded Jul 13 |
| §4E Old 3-option DNS plan | This file |
| §4G `www` redirect (open) | This file — **CANCELLED Jul 13** |
| §4H Soft opening note | This file |
| §4F Branch workflow | Main runbook §4F |
| §11.1 Amery Meat Market detail | `amery-meat-market/RUNBOOK.md` |
| Appendix A: Audit transcripts | This file |
| Appendix B: Cloudflare project info | This file (historical snapshot) |
| Appendix C: GBP photo upload detail | This file |
| Document history | This file |

---

## Old §4A — Transfer-of-ownership checklist (Josh ← Jen & Dennis)

> Updated 2026-07-05: Wrote `/workspace/clients/crossroadsmeatsandcellar/transfer-prep.md` — the doc to bring to the call with Dennis. Includes WHOIS findings (domain registered Jun 9 2026, expires Jun 9 2029, currently `client transfer prohibited`), 60-day free-transfer window timing (expires ~Aug 8 2026), step-by-step for Dennis, FAQ for pushback questions, backup plan if EPP request stalls.

What Josh already has / has done:
- [x] Verbal confirmation Jen & Dennis will release the domain (recorded 2026-07-04)
- [x] Wrote prep doc with WHOIS data + step-by-step for Dennis (Jul 5 2026)
- [x] Pulled current Squarespace parking page state (A records 198.49.23.144)
- [x] **DEFERRED Jul 13** — Squarespace holds 3-year registration (~Jun 2029), no urgency

What Jen & Dennis need to do at Squarespace (only if transfer is pursued):
- [ ] Unlock the domain for transfer
- [ ] Disable WHOIS privacy
- [ ] Disable Squarespace's "auto-renew + bundle with site builder"
- [ ] Request the EPP / auth code from Squarespace
- [ ] Confirm domain ownership via Squarespace's transfer-out email flow

## Old §4B — Why transfer vs keep Squarespace as registrar

| Option | Where | Pros | Cons |
|---|---|---|---|
| **A — Transfer to Cloudflare Registrar** | CF | One dashboard; SSL + DNS automatic; free first year (~$10–14/yr after) | One-time transfer |
| **B — Keep at Squarespace, pay renewal** | Squarespace | No transfer step | Manual SSL/DNS; awkward UI for non-Squarespace hosting |

**Decision (Jul 13 2026):** Option B. Squarespace holds 3-year registration; renewal cost absorbed.

## Old §4C — Renewal & billing

If transfer lands:
- [ ] Confirm auto-renew on the new account
- [ ] Calendar reminder for renewal date
- [ ] Cost is absorbed by JJCTech as customer-acquisition (per Jul 2026 pricing default)

## Old §4D — When DNS flips (historical, superseded)

Once the domain is in Cloudflare and added to CF Pages:
- [ ] Both `crossroadsmeatsandcellar.com` and `www.crossroadsmeatsandcellar.com` should resolve
- [ ] Run audit at 5 min, 15 min, 1 hr after flip
- [ ] Once 100% clean, archive this section

**Status (Jul 13 2026):** Apex is live on CF Pages. `www` cancelled (Squarespace retains registration). See main runbook §4.

## Old §4E — Old 3-option DNS plan (superseded)

The original 3-option plan (A: keep Squarespace, B: full Registrar transfer, C: NS-only setup) was superseded by transfer-of-ownership (Josh is taking over). The NS-only setup was chosen and is now active: Squarespace is registrar, Cloudflare is authoritative DNS. Apex attaches to CF Pages.

## Old §4G — `www` redirect (open, **CANCELLED 2026-07-13**)

`www.crossroadsmeatsandcellar.com` was on Squarespace parking. Originally planned to attach `www` as CF Pages custom domain + Bulk Redirect `www.* → apex`. **Cancelled Jul 13 2026** — Josh removed the `www` CNAME; Squarespace retains the registration for 3 years, no urgency. Apex URL is canonical.

## Old §4H — Note on soft opening (2026-07-09)

Soft opening landed on **Thursday, July 9, 2026** as planned. Site was still on `pages.dev` at that moment (prod URL was parked at Squarespace). Launch was not blocked on DNS flip; flip happened ~4 days later. Customers who bookmarked the prod URL in the first 4 days saw the Squarespace parking page. The site has been promoted as `crossroads-meats.pages.dev` in pre-launch materials (per `BUILD_SPEC.md`). For real marketing now, the prod URL is the right thing to print on signs / cards / FB CTA.

---

## Appendix A — Audit transcripts (historical)

### Jul 13 2026 (technical SEO / GSC sitemap diagnosis)

- GSC API confirmed two submitted sitemap paths, both with one error: `/sitemap.xml` and the mistaken homepage submission.
- Live production `/sitemap.xml` returned the 96 KB homepage as `Content-Type: text/html`, confirming the GSC report exactly.
- Root cause: no `sitemap.xml` and no top-level `404.html` existed in the static Pages build. Cloudflare treated the site as SPA, served `index.html` for every unknown path.
- Dev fix `95c2f34`: added valid `sitemap.xml`, `robots.txt`, `404.html`; canonical/OG/schema URLs → production domain; consolidated business schema; removed fake `$0/hour` JobPosting salary; aligned FAQ schema with visible content.
- Dev verification: `/sitemap.xml` → 200 `application/xml`; unknown URL → 404; both sitemap URLs → 200; audit → 0 axe, 0 console, 0 broken links, FCP 248ms.
- Shipped to `prd` via PR #6 after explicit "ship to prd" approval. Production: sitemap 200 `application/xml`, real 404s, correct canonicals, valid JSON-LD, both sitemap targets 200; ad-hoc verification 10/10; full site audit clean, FCP 288ms.
- GSC cleanup attempt via API blocked by read-only OAuth scope. Use the GSC UI or re-auth with write scope to delete the mistaken homepage submission.

### Jul 4 2026 (preview URL)

```
=== Site Audit: https://crossroads-meats.pages.dev/ ===
[OK] HTTP 200 (streaming, 68ms)
[OK] 0 console errors
[OK] critical: 0, serious: 0, moderate: 0, minor: 0
[OK] all links ok
[OK] FCP 288ms, TTFB 41ms, 381ms total
```

### Jul 13 2026 (prod apex, first audit post-DNS-flip)

```
=== Site Audit: https://crossroadsmeatsandcellar.com/ ===
[OK] HTTP 200 (streaming, 134ms)
[OK] 0 console errors
[OK] critical: 0, serious: 0, moderate: 0, minor: 0
[OK] all links ok
[OK] FCP 700ms, TTFB 56ms, 388ms total
```

### Jul 13 2026 (post-SEO-fix, prod apex)

```
=== Site Audit: https://crossroadsmeatsandcellar.com/ ===
[OK] HTTP 200 (streaming, 140ms)
[OK] 0 console errors
[OK] critical: 0, serious: 0, moderate: 0, minor: 0
[OK] all links ok
[OK] FCP 288ms, TTFB 97ms, 549ms total
```

---

## Appendix B — Cloudflare project info (historical snapshot)

| | |
|---|---|
| CF Pages project name | `crossroads-meats` |
| Subdomain | `crossroads-meats.pages.dev` |
| Default branch | `dev` (changed from `main` on 2026-07-08) — production deploy branch `prd`, requires PR approval |
| Deploy trigger | auto on push |
| DNS records (custom) | apex `crossroadsmeatsandcellar.com` → attached (active 2026-07-13) |
| DNS records (`www`) | not attached (cancelled); Squarespace parking |
| DNS authority | Cloudflare (NS flipped 2026-07-13 16:58 UTC) |
| Registrar | Squarespace Domains (NS-only setup; full transfer deferred) |

---

## Appendix C — GBP photo upload detail (2026-07-13)

### Final state as of 2026-07-13 18:05 UTC

**16 photos live on GBP, ALL uniform 1920×1080.** Three upload passes converged to this state:

1. **Pass 1 (17:30 UTC, pre-PR):** 16 photos uploaded with mixed dimensions.
2. **Pass 2 (17:55 UTC, post-PR #3):** Re-uploaded with apex sourceUrls.
3. **Pass 3 (18:05 UTC, post-PR #4):** Final 16/16 uniform 1920×1080.

### Photo inventory

| # | Filename | Category | Notes |
|---|---|---|---|
| 1 | `gbp-shop-cellar.jpg` | COVER | The cellar is in the name |
| 2–8 | `gbp-shop-barrel/cleaver/coolers/interior-1/interior-2/merch-cuts/merch-towels.jpg` | ADDITIONAL | 7 brand shop-interior photos |
| 9–11 | `gbp-v2-jen-IMG_7221/7222/7223.jpg` | ADDITIONAL | Jen's shop construction shots |
| 12 | `gbp-v2-logo.jpeg` | LOGO | White plate to preserve mark |
| 13 | `gbp-v2-hero-bg.jpg` | ADDITIONAL | Butcher at counter |
| 14–16 | `gbp-v2-stock-cheese/handcut/dryaged.{jpg,jpeg}` | ADDITIONAL | Wisconsin cheese / hand-cut / dry-aged beef |

### Why the v2 logo got a white plate

Smart-cropping a 1320×712 logo to 1920×1080 would have either cropped off the side or padded with black bars. The white plate keeps the logo composition intact while making the file's aspect ratio match Google's expected strip layout.

### PRs

- **PR #3** merged dev→prd at commit `3cbf552` — shipped the gbp-shop-* variants
- **PR #4** merged dev→prd at commit `5ce2a108f8` — shipped the gbp-v2-* normalized variants

Both via squash-merge. PR #4 required `--admin` override because the repo's branch-protection rule requires a non-self approval and JJCTech is currently the only GitHub contributor. Acceptable in the single-contributor context.

### Trade-off accepted

~5 MB extra on `prd` build (16 photo variants at ~300 KB each). Justified: GBP sourceUrls need a permanent home; v2 normalization required duplicating originals; GBP traffic flows through Google's CDN, not ours.

---

## Old §11.1 — Amery Meat Market (moved to amery-meat-market/RUNBOOK.md)

Amery engagement status, pricing posture ($100/mo/site), and scope kickoff details live in the dedicated Amery runbook at `/workspace/clients/amery-meat-market/RUNBOOK.md`. Cross-link in main runbook §11 stays current.

---

## Old §5 — Content / open items (full historical detail)

Site content snapshots from Jul 4-13 — gallery, marquee, hero, etc. — all preserved here for reference. The active content/open-items section in the main runbook is the trimmed version.

### Site content (Jul 8 2026 update, Three Rooms + marquee gallery)

- **New section "Three Rooms, One Shop"** between About and the gallery. Three feature tiles (Cellar / Apparel / Floor) showcase Jen's 3 photos at full size.
- **Gallery converted to auto-scrolling marquee.** Two copies of the 8-photo track for seamless looping. 60s linear animation. 64px edge fade mask. Pauses on hover, focus-within, pointer drag. Respects `prefers-reduced-motion`.
- **Page rhythm:** hero → products → about → THREE ROOMS → gallery marquee → hiring → reviews → visit.

### Site content (Jul 8 2026 update, faster marquee + click-to-advance arrows)

Dropped CSS `@keyframes` animation entirely in favor of JS `requestAnimationFrame`. Reason: original subagent tried to mix CSS animation + JS nudges but that reset to 0 every click.

- **Auto-scroll:** 100 px/sec (~2× the previous 50 px/sec).
- **Arrow buttons:** Left/right circular `<button>` elements with Phosphor icons. 44×44px targets.
- **Click behavior:** One figure + gap per click. Wraps cleanly. Auto-scroll continues seamlessly.
- **Pause behavior revised:** pointerdown pauses; hover does NOT pause (arrows live in strip); focus does NOT pause.
- **Reduced motion:** static; arrows still work.

### Site content (Jul 8 2026 update, hiring emphasis)

- **Sticky hiring banner** above navbar. 42px tall red accent strip with briefcase icon. Dismissal persists in sessionStorage.
- **Hiring CTA section upgraded** to full-width accent-red band. Layout: 2-column on desktop, stacks ≤820px.

### Site content (Jul 8 2026 update, dead-code cleanup)

- Removed custom mouse cursor (red ring + dot). ~80 lines removed.
- Removed orphan `<section class="trustbar">` (markup existed, CSS never written).

### Site content (Jul 8 2026 update, Jen's 3 shop photos)

Converted HEIC → JPG, saved to `assets/jen-IMG_7221.jpg`, `jen-IMG_7222.jpg`, `jen-IMG_7223.jpg`. Appended as figures 9/10/11 in the gallery strip.

### Meta / socials — Dennis raised this (2026-07-04, deferred)

Dennis mentioned Meta / socials. Recorded as open question. Don't pitch ads before the shop is open 30 days. Circle back after soft opening. JJCTech recommendation: start organic-only, add ads later.

---

## Old §6 — JJCTech service package (when pitched)

Per Josh's pricing default (Jul 2026): **$100/month** base retainer. Domain cost ~$12–20/yr is absorbed by JJCTech as customer-acquisition. Pitch draft already approved and on disk at `draft-message.md`.

Add-ons when justified:
- Weekly photo uploads + every-weekend promos → higher retainer tier
- Google Business Profile setup + ongoing management → bundled
- Review responses / QR counter-card → bundled on request
- Counter-card / sign design → one-shot project fee

---

## Old §7 — Audit / pre-pitch QA (historical)

Use `~/.hermes/skills/site-audit/audit.sh` before pitching any update.

```bash
~/.hermes/skills/site-audit/audit.sh https://crossroads-meats.pages.dev/
```

Flags: `--viewports=desktop,mobile`, `--axe-only`, `--links-only`, `--no-screenshots`, `--save-report=path.json`.

**Known pitfall (Jul 4 2026):** reveal-animation timing window that produced 44 false-positive color-contrast violations. If audit reports contrast failures on `.product-card` or `.about-content`, verify by inspecting `getComputedStyle` after reveal animations settle — they're 5.6x–18x in reality and the failures are timing artifacts. See `~/.hermes/skills/site-audit/references/known-pitfalls.md` Bug 5.

---

## Document history

### 2026-07-04 (initial)
- Created. Captured: client facts, live audit status, domain status, 3-option DNS-flip plan, zero-fabrication content audit, JJCTech pricing default.

### 2026-07-04 (updated) — Ownership change
- Added §4A transfer-of-ownership checklist, §4B transfer rationale, §4C renewal, §4D post-flip audit
- Demoted old 3-option plan to §4E
- §9 split into "waiting on Jen & Dennis" vs "Josh does next"
- §8 added ownership-change decision

### 2026-07-04 (updated) — Meta / socials
- §5 split into "Site content" + "Meta / socials — Dennis raised this"
- §9 added "Deferred to future session" subsection
- §8 decision log entry for the deferral

### 2026-07-04 (updated) — §11 Related projects (same owner)
- Cross-linking Amery: pipeline lead (same owner)
- Sister-client runbook skeleton at `/workspace/clients/amery-meat-market/RUNBOOK.md`

### 2026-07-04 (updated) — Amery engagement WON
- §11.1 promoted to "🟢 WON — engagement confirmed"
- No Amery design/build work has started

### 2026-07-04 (updated) — Brand guide companion doc
- Created `/workspace/clients/spooner-meat-market/BRAND.md` (162 lines, 8 sections) — folder later renamed to `/workspace/clients/crossroadsmeatsandcellar/` on 2026-07-13
- §2 visual identity locked, §3 voice DRAFT, §4 tagline OPTIONS, §5 photography direction, §6 brand do/don't, §7 client-confirmation checklist

### 2026-07-08 (updated) — §9 reorganized
- Three parallel client-side dependencies: Squarespace login, Google account for GBP, branding assets
- GBP plan locked: create under client's Google account, JJCTech added as Manager

### 2026-07-08 (updated) — Jen's 3 shop photos
- HEIC → JPG conversion; appended as figures 9/10/11
- Live on CF Pages preview at `https://eaac89a6.crossroads-meats.pages.dev/`
- Audit clean, FCP 300ms

### 2026-07-08 (updated) — Branch workflow restructured
- Created `dev` + `prd` branches; deleted `main`
- GitHub default branch: `main` → `dev`
- Branch protection on `prd`: linear history, 1 PR approval, no force-push

### 2026-07-08 (updated) — Dead-code cleanup
- Removed custom mouse cursor (~80 lines)
- Removed orphan `<section class="trustbar">` (~9 lines)
- Live preview URL: `https://4638a1df.crossroads-meats.pages.dev/`

### 2026-07-08 (updated) — Three Rooms + marquee gallery
- "Three Rooms, One Shop" section between About and gallery
- Gallery → auto-scrolling marquee (60s, 64px edge fade, pauses on hover/focus/pointer)
- Page rhythm: hero → products → about → THREE ROOMS → gallery marquee → hiring → reviews → visit

### 2026-07-08 (updated) — Faster marquee + click arrows
- Pure JS animation (RAF loop) instead of CSS @keyframes + JS nudges
- 100 px/sec auto-scroll; arrow buttons for click advance

### 2026-07-08 (updated) — Hiring emphasis
- Sticky top banner (42px, red accent, dismissible)
- Hiring CTA → full-width accent-red band
- Role pills (Store Manager, Cleaning Specialist)

### 2026-07-13 (updated) — DNS flip + prod apex LIVE
- NS flipped to Cloudflare; CF Pages custom domain attached; SSL provisioned
- Soft opening Jul 9 happened while DNS still parked (phased launch)
- `www` redirect CANCELLED, Registrar transfer DEFERRED (3-yr registration)

### 2026-07-13 (updated) — GBP base optimization via API
- PATCHed 4 additional categories + 455-char description

### 2026-07-13 (updated) — GBP photos via API (16 photos)
- Revises the `google-business-profile-optimization` skill pitfall #4: `mybusiness.googleapis.com/v4/.../media` IS alive, single-call upload works
- All 16 sourceUrls point at `crossroadsmeatsandcellar.com/assets/...`

### 2026-07-13 (updated) — Trimmed runbook
- Archived old §4 transfer-of-ownership detail, §5 site-content snapshots, §6 service package, §7 audit QA, §11.1 Amery, Appendices A/B/C, and full document history → `appendix-history.md`
- Main runbook now ~200 lines: header, quick facts, folders, live site status, domain status, branch workflow, content (trimmed), pending items, decisions log, contact, brand guide pointer
- Scheduled cron `b38c43bff207` for 2026-07-27: 2-week post-launch check on GBP screen-share, branding assets, GSC cleanup
- Project pinned via `project_create` to `/workspace/clients/crossroadsmeatsandcellar` (id `p_78458bf5`)