# Research — Crossroads of the North Meats and Cellar

> **Date:** 2026-07-03
> **Source:** Facebook page (no first-party website exists)
> **Pipeline stage:** 1 (Research)

---

## Business Basics

| Field | Value |
|---|---|
| Business name | Crossroads of the North Meats and Cellar |
| Industry | Butcher Shop / Wine & Beer Store / Specialty Grocery |
| Owners | **Jen Lutz & Dennis Lutz** |
| Address | 118 Ash Street, Spooner, WI 54801 |
| Phone | **Not publicly listed anywhere** (no Google Business, no Yelp, no website — only Facebook Messenger / DM) |
| Hours | **Not yet posted** — status: "Closed now" (pre-launch) |
| Facebook | https://www.facebook.com/people/Crossroads-of-the-North-Meats-and-Cellar/61590342133702/ |
| Website | **None — pipeline target** |
| Status | Pre-launch / opening soon (hiring store manager and cleaning specialist, June 2025) |
| Liquor license | ✅ Approved June 2, 2025 by Spooner City Council (Class A combination, through June 30, 2027) |

---

## Brand Identity (extracted from logo)

**Logo file:** `assets/crossroads-meats-logo.jpeg` (1320×712)
- Wisconsin state outline (black line art, with dotted routes converging on a red heart in the NW corner = Spooner)
- Bold serif typography (Trajan-family), stacked
- **Colors:** black `#0a0a0a`, white `#ffffff`, accent red `#d62828` (will sample more precisely during build)
- Address baked into the wordmark — signature of a designed-from-scratch identity
- **Vibe:** heritage, established, small-town Wisconsin pride, hand-crafted

---

## Product / Service Mix

- **Hand-cut meats** (signature offering)
- **30-day aged steaks**
- **30+ house-made brat flavors**
- **Snack sticks**
- **Jerky**
- **Wisconsin cheese** (cheese curds + specialty)
- **Wine** (the "Cellar")
- **Craft beer**
- **Pet food & treats** (bonus category, not all butchers carry this)
- **Gifts**

**FB page categories (3):** Butcher Shop · Wine, Beer & Spirits Store · Specialty Grocery Store

---

## Industry Context — Washburn County, WI

- **Spooner, WI** — pop. ~2,500, Washburn County seat, NW Wisconsin
- Local culture: rodeo, dairy, lake country, railroad history, county fairs
- Competition: only **2-3 small grocers** in the county; closest "real" butcher is 30+ min drive (Hayward, Rice Lake, or Amery)
- Spooner has **no specialty butcher + wine shop combo** — this is a greenfield market

---

## Hiring Post Analysis (Store Manager JD, June 16 2025)

**Position:** Store Manager (Full-Time, Exempt)
**Reports to:** Jen & Dennis (Owners)
**Schedule:** ~40 hrs/week, weekdays + weekends, flexible

**Key responsibilities confirm business model:**
- "Open and close the store; maintain daily operational checklists"
- "Oversee meat case presentation, product rotation, labeling, and quality control"
- "Manage inventory counts, ordering, and vendor relationships in coordination with owners"
- "Maintain compliance with health, food safety, and sanitation regulations"
- "Operate POS system, process transactions, and ensure cash drawer accuracy"

**Owner philosophy from JD (last paragraph, Page 2):**
> "Jen and Dennis are committed to being hands-on partners. The manager will have strong owner support and room to grow with the business."

→ **Tone for the site copy:** family-owned, hands-on, room to grow, not a corporate chain.

---

## Liquor License (from apg-wi.com, June 2 2025)

- **Class A combination liquor license** — fermented malt beverages AND intoxicating liquors for off-premises consumption (i.e. retail sales of wine + beer for the Cellar)
- Two licenses approved (one through June 30, one through June 30, 2027) — likely to keep options open or to cover both entities
- Source: https://www.apg-wi.com/spooner_advocate/news/local/spooner-city-council-approves-liquor-licenses-park-permits-and-more/article_8ce0a0cf-21d6-43e7-a3eb-26aa4bddb2cd.html

---

## Build Plan Notes (for Stage 3)

**Template:** premium-template (logo-derived design system, scroll-reveal animations, dark sophisticated nav)

**Brand palette (to confirm in build):**
- `--bg`: `#0a0a0a` (charcoal — matches the logo's bold black)
- `--text`: white / cream
- `--accent`: `#d62828` (heart red — verify WCAG AA against dark + white)
- `--accent-dark`: darker red for hover states
- Hero will use logo + Wisconsin map element as graphic anchor

**Industry-mismatch removals (per local-web-design skill, July 2026):**
- ❌ Delete `.services` 4-card grid — butcher is a product catalog, not "services" (use a Products/What We Carry section instead)
- ❌ Delete `.contact` form — no phone number exists, will use forms.jjctech.net → email + Facebook DM CTA
- ✅ Keep: hero, about (owner story — Jen & Dennis), stats, reviews (placeholder for now — they'll get real ones post-open), FAQ

**Sections the butcher site needs (custom-built, not in template):**
1. Hero (logo + "Now Open" / "Coming Soon" status + tagline)
2. About (Jen & Dennis — family-owned, hands-on)
3. **What We Carry** — product grid (hand-cut meats, 30-day aged steaks, 30+ brats, snack sticks, jerky, WI cheese, wine, craft beer, pet food, gifts)
4. Stats (years in dream, products, partnerships, etc.)
5. FAQ (hours TBD, custom cuts, gift baskets, walk-ins)
6. Location (118 Ash Street, Spooner, WI + map embed)
7. Final CTA (Facebook DM button + email form via forms.jjctech.net)
8. Footer (FB link, address, copyright)

**Critical — NO EMOJI / NO "Our Services" / NO "Let's Talk" form / NO "What We Do" eyebrow** (per ECT cross-build defaults, July 3 2026).

**Brand color for accent:** Will need to sample the heart red with PIL and run WCAG candidate table — logo red is the source of truth, not a guess.

---

## Photos Available

| File | Source | Use |
|---|---|---|
| `assets/crossroads-meats-logo.jpeg` | Josh-supplied (FB profile pic, 1320×712) | Logo, hero graphic, OG card |
| `assets/fb-launch-hero.jpg` | FB launch post (FB CDN, 960×632, 2.8KB only — blocked) | Failed to download — fallback: regenerate or use stock |
| `assets/fb-hiring-flyer.jpg` | FB hiring flyer (FB CDN, 526×526, 2.8KB only — blocked) | Failed to download — fallback: skip |

> **Note:** FB's CDN blocks direct curl downloads. Subagent will need to grab the original images via Playwright (browser-based, with cookies) if we want to use real photos. Otherwise we use stock meat/butcher imagery from Pexels/Unsplash during build.

---

## Sources

- Facebook page: https://www.facebook.com/people/Crossroads-of-the-North-Meats-and-Cellar/61590342133702/
- Hiring post: https://www.facebook.com/61590342133702/posts/were-hiring-our-store-manager-join-us-as-we-open-crossroads-of-the-north-meats-a/122111303181344737/
- Cleaning post: https://www.facebook.com/61590342133702/posts/were-hiring-a-weekly-cleaning-specialist-one-scheduled-day-a-week-to-help-keep-o/122112265215344737/
- City Council article: https://www.apg-wi.com/spooner_advocate/news/local/spooner-city-council-approves-liquor-licenses-park-permits-and-more/article_8ce0a0cf-21d6-43e7-a3eb-26aa4bddb2cd.html
- Google Drive JD: https://drive.google.com/file/d/1ku48g4bTL73nScy4NnVduDxUE3OFLxhv/view
- Logo (Josh-supplied, image_cache): `/root/.hermes/image_cache/img_f4c9c77ae8d0.jpeg`
