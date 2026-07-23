# Crossroads of the North Meats & Cellar — Client Runbook

> **Status:** Maintenance
> **Next:** Wait for Josh's review of pending GBP category change (Gourmet grocery store → Meat products store as primary, on screen now); ship hours edits on dev → prd when he says so. Also: GBP screen-share item #1 still open (Dennis owes ~30 min for serviceItems + MON/TUE/WED hours confirmation + phone verification).
> **Blocker:** None
> **Last touched:** 2026-07-23

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
| 1a | **Ship 4 grand-opening assets to prd**: `gbp-grand-opening-shirt.jpg`, `gbp-grand-opening-poster.jpg`, `gbp-cover-storefront-1920x1080.jpg`, `gbp-storefront-portrait-1080x1350.jpg`. All sourceUrl on dev with 24h TTL — re-pushing to prd makes them permanent on apex. | 🟡 awaiting ship-it | Low |
| 1b | **Review storefront cover + portrait in GBP dashboard** | Cover photo determines the search-result hero. Verify Google picked the COVER as the primary, not an earlier photo. | 🟡 optional check | Low |
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
| 2026-07-23 | **GBP primary category change queued in dashboard** (per Josh review of GBP edit screen). Currently `Gourmet grocery store` primary + Gift/Beer/Wine/Cheese shop as additional. Pending change swaps primary to **`Meat products store`** (`gcid:meat_products`) with the same 4 additional. Applied Google's official "IS a" test from https://support.google.com/business/answer/3038177#Categories: "Crossroads IS a meat products store" ✅, vs rejected candidates `butcher_shop` (fails CRN fabrication rule — Amery does the cutting), `gourmet_grocery_store` (too generic), `deli` (no prepared food), `farmers_market` (wrong business model). Whitespark ranking-factor study puts primary category as the #1 GBP-owned local-pack signal — highest-leverage GBP change. Additional 4 stay as keyword associations for long-tail searches ("cheese shop near me", "wine shop Spooner") even though they fail the strict "IS a" test — additional slot is a different signal than primary. `local-seo` skill updated with the full "IS a" framework + worked examples + Crossroads application. Awaiting Google's category-edit review (typically a few minutes, sometimes longer). |
| 2026-07-23 | **Sunday hours changed to 10am-3pm** (per Dennis via Josh). Site + GBP + SEO all updated: index.html (meta/og/twitter description, JSON-LD `openingHoursSpecification` split into 2 periods, FAQ JSON-LD + visible accordion, Hours location card, footer); 4 sub-page footers; 404 footer; seo-technical-audit.md (HIGH gap → resolved). GBP PATCH via `mybusinessbusinessinformation` v1 `updateMask=regularHours` against `locations/12257474292180026211`. Token file `~/.hermes/gbp_tokens.json` re-authed via PKCE+OOB flow (using skill `google-oauth-pkce-oob`, prior `invalid_grant` was token revocation) — 3-check live verification passed (accounts.list, locations.list, refresh_token round-trip). Committed on dev as `ed541a2`. Pushed to origin/dev; awaiting Josh's ship approval for dev → prd. |
| 2026-07-20 | **Recovered content lost during the Jul 18 merge.** Audit of `9248e51..HEAD` confirmed the four original image blobs (`cheese-cooler.jpg`, `jerky-cheese-shelf.jpg`, `dry-goods-condiments-shelf.jpg`, `knives-victorinox.jpg`) survived byte-for-byte, but merge commit `b978872` removed 283 lines from `index.html`: all `Inside the Shop` CSS/HTML, both product-card image swaps, and all four additions from both marquee tracks. Restored the missing implementation from `9248e51`, retained the later Crossroads-first SEO metadata from `62f584b`, and corrected stale `in-house` / `hand-packaged` / `working kitchen` language so Spooner remains accurately described as retail-only. Verified locally: HTML structure clean, all local assets present, target image hashes match `9248e51`, four-column desktop grid renders with four distinct loaded photos, and both marquee tracks contain the restored assets. Production untouched pending explicit approval. |
| 2026-07-14 | **OG metadata overhaul + favicon fix (per Josh: "do a full OG audit. I don't see the logo or image on the google search page. can that be fixed" + screenshot showing GSC dark cube placeholder for missing favicon).** OG fix: per-page og:image swapped from `assets/logo.jpeg` to hero/product photos (hero-bg.jpg on index/cabin/venison, stock-handcut.jpeg on custom-cutting, stock-cheese.jpg on gift-baskets, shop-interior-1.jpg on hiring). Full OG metadata added: og:image:secure_url + og:image:type + og:image:width + og:image:height + og:image:alt on every active page. Twitter image updated to match, twitter:image:alt added everywhere. Cabin-provisions.html meta/og/twitter descriptions + schema description + hero lede + content-intro all updated "house-made brats" → "Amery brat recipes grilled fresh Friday through Labor Day" (6 places) — subagent caught 3 in meta tags but missed schema description, hero lede, and content-intro, parent sweep caught + fixed. Favicon fix: generated 6 favicon variants from logo.jpeg via PIL (center-crop 712×712, then resize to 16/32/180/192/512 + multi-size .ico with 16+32+48 embedded). Copied to project root for browser auto-discovery of /favicon.ico. Updated favicon link tags on all 6 active HTML pages (1 was 2-line block of logo.jpeg refs; now 6-line block referencing new variants). GSC should now show the brand mark instead of dark cube, browser tabs will show the favicon. Plan: `.hermes/plans/og-metadata-overhaul.md`. Dispatched to M2.7 subagent `deleg_784a14c2`; Stage 1 + Stage 2 audit caught + fixed 3 missed "house-made brats" instances + favicon link tags. Verified: 154/154 ad-hoc checks, live server 200s on 6 pages + 6 favicon URLs. |
| 2026-07-14 | **In-house language + Friday grill + Amery pedigree (per Dennis via Josh).** Spooner is retail-only today; future processing will include slicing summer sausage, cheese, snack sticks, and Friday brat grills (Memorial Day → Labor Day). Brand strategy: cross-promote both stores; Amery's value is 20+ years of proven recipes + business longevity. Six copy patches on index.html: (1) hero h1 "House-made brats" → "Brats grilled fresh" (the recipes are Amery's, the grilling is Spooner's); (2) hero h2 adds "(20+ years)" to Amery mention, "House-made brats in Spooner" → "Brats grilled fresh in Spooner"; (3) intro paragraph "house-made brats" → "brats grilled fresh Friday through Labor Day"; (4) about paragraph "brats are made in-house" → "we grill Amery's brats fresh Friday through Labor Day"; (5) "What we'll have on the counter" section sub adds "(20+ years)"; (6) new FAQ added in both JSON-LD and visible accordion: "Do you grill brats at the store?" → "Yes — Friday through Labor Day, weather permitting. We grill Amery's brats fresh around lunchtime. It's a good reason to time your visit, and a good way to meet Jen and Dennis if you haven't yet." Plan: `.hermes/plans/inhouse-grill-fixes.md`. Dispatched to M2.7 subagent `deleg_dcf77d6a`; Stage 1 + Stage 2 audit passed. |
| 2026-07-14 | Navbar consistency: "Venison Processing" → "Venison & Wild Game" on 5 pages (index, custom-cutting, cabin-provisions, hiring, gift-baskets). Affected 15 link texts (desktop nav + mobile menu + footer Explore list per file). Venison page itself was already correct. Aligns nav labels with what the page actually does (full wild game, not just venison). Site-wide copy sweep: all CRN-context "butcher" references (10 across index.html + hiring.html) replaced with "meat market". Title, meta, OG, Twitter, schema description, intro paragraph, about H2, about paragraph, hiring JSON-LD, hiring hero, hiring summary all updated. Amery Meat Market sister-card on venison page still correctly calls Amery a "full-service butcher shop" — they're the actual butcher. Bonus sweep: 32 instances of "sister facility in Amery" → "sister store Amery Meat Market" across 6 HTML files (Josh's voice). Curbside pickup REVERSED (FB page is source of truth, it lists curbside) — index FAQ changed back from "no, walk-in only" to "yes", hero-sub re-added "Curbside pickup available", gift-baskets + cabin-provisions curbside copy kept intact. Wrong image alt fixed: index.html line 1881 was "Butcher counter with a selection of fresh cuts" but actual image is rolled pork belly on a board (verified via vision_analyze) — now correctly says "Rolled pork belly cuts on a wooden board". Plan: `.hermes/plans/crn-meat-market-rebrand.md`. |
| 2026-07-14 | **Custom-cutting page clarified (per Josh: "same idea as the venison processing").** Unlike venison, custom cutting has a real Spooner-side workflow (intake + coordination + pickup), so the page is kept — just fixed to honestly disclose that the cutting happens at Amery. H1 changed from "Custom Meat Cutting in Spooner" → "Custom cuts, coordinated through our Amery facility". Page title, meta, OG, Twitter all mirror the new framing. Hero lede now explicitly splits "We handle the order here in Spooner — the cutting happens at our sister facility in Amery — and your order is ready for pickup at 118 Ash Street." Intro paragraph reframed with "same Lutz family, same Wisconsin standards" (matches venison sister-card language). How-it-works steps made explicit: Step 3 names "vacuum-seal at the Amery facility", Step 4 moves timing detail ("typically ready within a few days"). Schema.org `Service` block DROPPED entirely (Crossroads doesn't perform the service — it coordinates it). FAQ "Can you wrap and freeze it for me?" replaced with "How is my order packaged?" — drops the wrong "We can freeze it for you" claim, adds practical "bring a cooler" tip. Plan: `.hermes/plans/custom-cutting-pivot.md`. |
| 2026-07-14 | **Hero + intro cleanup (per Josh review of dev preview).** H1 swapped from sourcing-led "Pre-packaged from our sister facility. House-made brats. Spooner's newest butcher and cellar." to offerings-led "30-day aged steaks. House-made brats. Wisconsin cheese, wine, and craft beer." H2 reworded to put sourcing one level down: "All cut and packaged at our sister facility in Amery · House-made brats in Spooner · 30-day dry-aged · Wisconsin-sourced first". Hero-sub drops "Curbside pickup available" + fluff; adds "just off Hwy 63" anchor. Intro section tightened from a giant redundant paragraph to a short what-and-where sentence. About pillar "Wisconsin-sourced first" drops "Look for the local tag on the shelf." FAQ "local products" drops the shelf-tag instruction. FAQ "curbside pickup" reframed from "yes, we'll bring your order out" to "no, walk-in retail only — call ahead to confirm stock." Josh picked option A (soft reframe) from clarify. Plan: `.hermes/plans/hero-pivot.md`. |
| 2026-07-14 | **Venison service removed from Spooner (per Dennis via Josh).** `venison-processing.html` rewritten as a pointer page to Amery Meat Market (116 Central St, 715-268-7515, amerymeatmarket.com). Index FAQ answers corrected for both custom-meat-cutting and wild-game questions (both JSON-LD and visible accordion). `hiring.html` Weekly Cleaning Specialist description drops "meat counter, prep area" (neither exists at Spooner). `custom-cutting.html` intro rephrased for clarity. `sitemap.xml` adds `<priority>0.5</priority>` to /venison-processing to demote. Schema.org `Service` for venison processing removed. Plan: `.hermes/plans/venison-pivot.md`. Verified on local http.server 8765 — NOT pushed to dev. |
| 2026-07-14 | Sourcing copy corrected across all pages: Spooner is pre-packaged from sister facility in Amery. Hero rewritten ("Pre-packaged from our sister facility. House-made brats. Spooner's newest butcher and cellar."), 6-card "Hand-Cut Meats" renamed "Hand-Cut in Amery", "Cut in-house" pillar renamed "Cut in Amery, finished in Spooner", schema product updated, meta/OG/Twitter updated. House-made brats/snack sticks/jerky still attributed to Spooner (correct). |
| 2026-07-14 | CRN Meats & Cellar short name rolled out across all pages (logo, title, preloader, footer, mobile menu). Sub-page navs unified to 7 items each: Home, What We Carry, Custom Cutting, Cabin Provisions, Gift Baskets, Venison Processing, Now Hiring, Visit Us. Sub-page footers unified to same Explore set. |
| 2026-07-14 | **Local dev server workflow adopted for change verification** (`python3 -m http.server 8765 --bind 127.0.0.1`). Faster than CF Pages preview URLs, never out of sync with working tree, no API calls. See §4V. |
| 2026-07-13 | GBP photo upload via API (16 photos, uniform 1920×1080). Revises the `google-business-profile-optimization` skill pitfall #4: `mybusiness.googleapis.com/v4/.../media` IS alive, single-call upload works. |
| 2026-07-13 | `www` redirect CANCELLED; Registrar transfer DEFERRED. Squarespace holds 3-yr registration (~Jun 2029). Apex is canonical. |
| 2026-07-13 | NS flipped to Cloudflare; prod apex LIVE. CF Pages custom domain attached, SSL via Google CA, audit clean. |
| 2026-07-18 | **Attempted "Inside the Shop" rollout — corrected 2026-07-20.** Commit `9248e51` added four in-store merchandising photos and wired them into a new four-tile section, the Wisconsin Cheese + Snack Sticks product cards, and both marquee tracks. The image blobs survived, but follow-up merge `b978872` merged `prd` into `dev` and resolved `index.html` to the older production version, deleting the implementation. Contrary to the original entry, this was **not live on prd**. See the 2026-07-20 recovery entry above. |
| 2026-07-08 | Branch workflow locked: `dev` working, `prd` production, squash-merge via `git-ship`, single-contributor `--admin` override. |
| 2026-07-14 | **GBP COVER + ADDITIONAL storefront photos uploaded.** Source: iPhone HEIC (`IMG_5497.heic`, 5712×4284) showing the actual storefront on opening day — stone facade, painted "NOW OPEN — CROSSROADS OF THE NORTH — MEATS & CELLAR" window signage with meat graphic + JR's Sweet Red wine bottle, American-flag bunting, potted flowers next to entry. Two variants pushed to GBP: (1) **1920×1080 (16:9) COVER** — dropped top 17% (sky) + bottom 8% (walkway) to fit, scaled down with PIL LANCZOS, under the 2120×1192 max. mediaName `accounts/.../media/AF1QipN9GjvMHQaNv8NsyXo3P_SEiRlfmcyaadA2PBp_`. (2) **1080×1350 (4:5) ADDITIONAL** — biased horizontally at 0.55 of source to keep meat graphic + wine bottle + bunting in frame, loses road context but keeps signage as anchor. mediaName `accounts/.../media/AF1QipOOTFej2ryJ0iaKwsOgL7QBNQp7cW7rJeFZte2x`. Both pushed to dev branch via commits (`e2ca9e7`). |
| 2026-07-14 | **GBP grand opening BIG POST LIVE (brand-style poster).** Second post for the grand opening — full brand treatment using Playfair Display + Inter on warm cream `#F5EFE2`, `--accent #C0392B` for rules. Poster: cropped logo (white→transparent with PIL), eyebrow `SPOONER · WISCONSIN`, logo lockup with hairline separators top + bottom, 92pt Playfair Black `Grand Opening` (mixed case), Saturday July 18 / 10 AM–6 PM / 118 Ash Street / "Pull up a stool." (serif quote from BRAND.md §3.4 sample phrasings) / product line `30-day aged steaks · 30+ house-made brat flavors · Wisconsin cheese` / footer URL. 1080×1080, 100KB, image served from `dev.crossroads-meats.pages.dev/assets/gbp-grand-opening-poster.jpg`. Post `name=accounts/.../localPosts/4700139011320040348`, `state=LIVE` at 2026-07-14T16:36:32Z. Canonical URL: `https://local.google.com/place?id=15501986993814392536&use=posts&lpsid=CIHM0ogKEIXfjYqag9XfVw`. Pitfalls hit: (a) `Media.Create category=COVER` enforces 16:9 (1920×1080) — subErrorCode 101 (Invalid aspect ratio) on a 1:1 image. Switched to `ADDITIONAL` so the 1:1 poster composites into the post body correctly. (b) Cropping logo JPEG to its tight bbox leaves a white plate; fixed with PIL mask `(R,G,B all > 230) → alpha=0` so padding composes transparently onto the cream canvas. Open follow-up: build a true 1920×1080 cover-format variant for future use as the GBP profile cover. |
| 2026-07-14 | **GBP grand opening post LIVE (free t-shirt giveaway).** First-25-customers free t-shirt promo, Saturday July 18 10AM–6PM at 118 Ash St. Post body: 407-char STANDARD topicType with LEARN_MORE CTA → apex; t-shirt image attached (1080×1080, 172KB, cropped from `assets/gbp-grand-opening-shirt.jpg` on dev). Posted via `/tmp/hermes-deploy-gbp-grand-opening.py` (full My Business API flow: refresh token, list locations, Media.Create from URL, localPosts create, verify state). Post `name=accounts/.../localPosts/7494464812631420451`, `state=LIVE` at 2026-07-14T16:23:47Z. Canonical URLs: `https://search.google.com/local/posts/7494464812631420451` and `https://local.google.com/place?id=15501986993814392536&use=posts&lpsid=CIHM0ogKEIOB9rDv3_zz5QE`. Asset pushed to dev (commit `9c169b1`) only — to keep sourceUrl permanent on apex, the asset needs ship-it to prd. Tokens re-authed (old refresh token revoked). Pitfalls hit: (a) bare-urllib HEAD got 403 from CF Pages WAF, fixed with Mozilla UA; (b) `Media.Create` returns a `mediaName` resource path that is NOT a valid sourceUrl for localPosts — must use the `googleUrl` (`https://lh3.googleusercontent.com/...`) instead; docs in `gbp-photo-upload.md` and `gbp-localposts-recipe.md` could note this — flag for skill patch. Crossroads screen-share (#1) not blocked: grand opening is time-sensitive public announcement, distinct from the open Q&A/hours/serviceItems work. |
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