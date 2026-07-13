# Crossroads of the North Meats & Cellar — Brand Guide

> Living brand document for the JJCTech engagement with **Jen & Dennis Lutz**.
> Last updated: **2026-07-04** (initial draft, awaiting client sign-off)
> Companion to `/workspace/clients/crossroadsmeatsandcellar/RUNBOOK.md`.
> Owner: Josh (JJCTech) for ops; brand authority flows from Jen & Dennis (final say).

This file is the source of truth for brand voice, visual identity, photography direction, and tagline options. Anything not explicitly approved by Jen & Dennis is marked **DRAFT** or **TBD**.

---

## 1. Brand brief (owner-approved)

> **Source:** Josh (JJCTech, owner) confirmed verbally 2026-07-04.
> **Status:** Approved direction. Jen & Dennis have not signed off on the wording yet — they own the final say.

**Crossroads of the North Meats & Cellar is a chique, clean, modern retail meat market in a cozy Northland tourist lake town.**

It is a one-stop shop for **hand-cut meats, Wisconsin cheese, wine and craft beer, and curated merchandise**.

It is in **Spooner, Wisconsin**.

### 1.1 — Expanded descriptors

| Descriptor | What it means here | What it does NOT mean |
|---|---|---|
| Chique | Refined without being precious. Editorial typography, generous whitespace, considered details. | Not Parisian patisserie, not sterile boutique, not minimalist-to-the-point-of-cold. |
| Clean | Layout breathes. Few competing elements per view. No visual clutter. | Not sparse-to-the-point-of-empty. Not corporate-templated. |
| Modern | Editorial type pairings, contemporary color usage, current web norms (mobile-first, fast, accessible). | Not futurist, not techy, not cold-blue-and-gray. Not trendy-for-trend's-sake. |
| Retail | Customer-facing shopfront. Approachable. Curated. | Not warehouse, not bulk, not industrial. |
| Cozy | Warm cream backgrounds, soft red accent, friendly type pairing, real photos of real place. | Not folksy-cute, not country-kitsch, not overcrowded. |
| Northland tourist lake town | Located in NW Wisconsin — cabin country, lakes, rivers, weekend visitors. Use "Spooner," "Northwoods," "Washburn County" deliberately. Geography is part of the appeal. | Not generic rural. Not "country." Use the actual place names. |
| One-stop shop | Save a stop. Get meat, cheese, wine, gifts at one place. The trip is the point. | Not "everything store." Not bulk. Curated selections, not a thousand SKUs. |

### 1.2 — What we sell (in brand-priority order)

1. **Hand-cut meats** — flagship. Cut fresh in-house. Wisconsin-sourced where possible. 30-day aged steaks, 30+ house-made brat flavors, snack sticks, jerky.
2. **Wisconsin cheese** — co-flagship. Curds, blocks, specialty. Local sourcing.
3. **Wine & craft beer** — the "Cellar" half. Curated not encyclopedic. Class A liquor license approved June 2, 2025.
4. **Merchandise** — gifts, pet food, kitchen accessories, locally-themed items.

---

## 2. Visual identity

> **Source:** Logo file (`/workspace/jjctech-outreach/crossroads-meats/site/assets/logo.jpeg`, 1320×712) + brand-color values verified via PIL in `BUILD_SPEC.md`.
> **Status:** Locked by client (logo exists). Color tokens below are extracted from the logo. Final shadow / light values derived analytically.

### 2.1 — Logo

| | |
|---|---|
| **File** | `assets/logo.jpeg` |
| **Format** | JPEG (could use a vector SVG/EPS for crisp print; not produced yet) |
| **Composition** | Wisconsin state outline (black line art) + dotted lines converging on a red heart in the NW corner + bold serif wordmark "CROSSROADS MEATS" with secondary line containing the Spooner address (118 Ash St) and tagline line |
| **Mood** | Heritage, small-town Wisconsin, established. Black + red + white only. |

**Logo usage rules (DRAFT — confirm with Dennis):**
- **Clear space:** minimum clear space equal to the height of the "C" in "Crossroads" on all sides
- **Minimum size:** 96px wide on screen; 1 inch wide in print
- **Reproducibility:** prefer the original asset; rebuild as SVG if scaling is needed for signage
- **Don'ts:** do NOT place over busy photography at small sizes; do NOT recolor; do NOT outline the heart separately; do NOT stretch non-uniformly

### 2.2 — Color tokens

> Color values verified directly from the supplied logo file via PIL sample, then reconciled with the live site's CSS custom properties (verified via Playwright `getComputedStyle` on 2026-07-05).
>
> **2026-07-05 reconciliation:** the live site ships a slightly different cream (`#FAF8F4` vs the brand-doc spec of `#EDE9E2`). The site version is the source of truth going forward — it reads warmer and editorial. Tokens below reflect the served CSS.

| Token | Hex | RGB | Used for | Source |
|---|---|---|---|---|
| `--ink` | `#0A0A0A` | 10,10,10 | Body type, headings, default text on cream | Wordmark sample + live CSS |
| `--ink-body` | `#2A2A2A` | 42,42,42 | Long-form paragraph copy | Live CSS |
| `--ink-muted` | `#6B6660` | 107,102,96 | Captions, helpers, secondary text — verified 5.6x on white | Live CSS |
| `--ink-faint` | `#6B6660` | 107,102,96 | Same as muted (alias) | Live CSS |
| `--bg-primary` | `#FFFFFF` | 255,255,255 | Default page background | Live CSS |
| `--bg-alt` | `#FAF8F4` | 250,248,244 | Parchment sections (warm cream) — **updated from `#EDE9E2` to match live site** | Live CSS (2026-07-05) |
| `--rule` | `#E5E0D5` | 229,224,213 | Hairlines, dividers | Live CSS |
| `--rule-soft` | `#F0EBE0` | 240,235,224 | Soft dividers (added in live CSS) | Live CSS |
| `--bg-dark` | *deprecated 2026-07-05* | Email signup band no longer uses dark background — see audit fix #4. Replaced with `--bg-alt` + `--accent` 2px rules top + bottom. The `--bg-dark` CSS var is still defined but unused on the live site. Safe to remove in a future cleanup pass. | Audit fix #4 |
| `--brand-red-logo` | `#F50D0B` | 245,13,11 | **Logo only.** Pure red — for decorative brand-mark use, NOT for body text or buttons (4.16x on cream fails AA). | Logo heart |
| `--accent` | `#C0392B` | 192,57,43 | Primary CTAs, accent text, section dividers | Live CSS (`--accent`) — previously labeled `--brand-red-button` |
| `--accent-dark` | `#8C2A1F` | 140,42,31 | Hover states | Live CSS (`--accent-dark`) — previously labeled `--brand-red-dark` |

**Pair rules (verified):**
- `--ink` on `--bg-primary` (white): 18.0x ✓ AAA
- `--ink-body` on white: 12.5x ✓ AAA
- `--ink-muted` on white: 5.6x ✓ AA
- `--ink-muted` on `--bg-alt` (#FAF8F4): 5.7x ✓ AA
- `--accent` on white: 5.83x ✓ AA
- `--accent` on `--bg-alt`: 5.12x ✓ AA
- `--brand-red-logo` on white: 4.16x ✗ — **logo red is decorative only**

### 2.3 — Typography

|> **Status:** Locked on the live site. Not yet confirmed with Jen & Dennis.
>
> **2026-07-05 reconciliation:** Google Fonts request in the served HTML only loads `Playfair+Display:wght@700;900`. Inter is **not** loaded from Google Fonts — it falls back to system stack (`Inter, -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif`). On machines without Inter installed, the body renders in the OS default sans (San Francisco on Mac, Segoe UI on Windows, Roboto on Android). This is acceptable for an editorial site but worth flagging if Inter is meant to be a brand requirement rather than a preference.

| Role | Family | Source | Notes |
|---|---|---|---|
| **Display (headings)** | Playfair Display (700, 900) | Google Fonts (loaded) | Editorial serif. Used for h1, h2, hero lockup |
| **Body** | Inter (300, 400, 500, 600, 700) | **System stack only** (not loaded from Google Fonts) | Modern humanist sans. UI + long-form copy. Renders in OS default on machines without Inter installed |

**Type pair rules:**
- Display at 700+ weight, generous letter-spacing on display only
- Body at 400 weight, 1.6–1.8 line-height for readability
- Eyebrows / labels in Inter 500, uppercase, letter-spaced 0.18–0.24em
- No emoji. Real Phosphor / Lucide icons only (per JJCTech default)

### 2.4 — Iconography

- **Phosphor Icons** (regular + fill) from CDN. Already in use on the live site for products, contact, location.
- No emoji anywhere.
- Stroke icons, never chunky/filled.

---

## 3. Voice & tone

> **Source:** Derived from FB posts (research.md), the store-manager job description (FB), and Josh's brief (2026-07-04).
> **Status:** Inferred. Confirm with Jen & Dennis before publishing voice rules in copy deck.

### 3.1 — Brand voice in three words

**Warm. Honest. Locally-rooted.**

### 3.2 — Voice principles

| Principle | In practice |
|---|---|
| **Warm without folksy** | Friendly, refer to the owners as "the Lutz family" in customer-facing copy (not "Jen and Dennis" / "Jen & Dennis" — those are internal references). Not "y'all" or "folks" or backwoods metaphor. |
| **Honest about what we are** | "We're new" is on the page; "we're still handpicking vendors" is real. Don't pretend to history we don't have yet. |
| **Locally rooted** | "Spooner" appears where it adds locality. "Wisconsin" appears on cheese. "Northwoods" or "Washburn County" works for cabin-country framing. |
| **Confident without grandiose** | "Hand-cut every day." Not "the absolute finest cuts anywhere." No superlatives, no "premier," no "the best in the region." |
| **Specific over vague** | Name products, not categories. "30-day aged ribeye" beats "premium beef." "30+ brat flavors" beats "wide selection." |
| **Active verbs, short sentences** | "Cut fresh." "Pulled from the case this morning." "Pull up a stool." Not "offerings are available." |
| **Humor when natural** | Butcher puns are fine in moderation. Don't lean on them. |

### 3.3 — Vocabulary

| Prefer | Avoid |
|---|---|
| Hand-cut, house-made, in-house | Artisanal, craft, small-batch (overused) |
| Spooner, Washburn County, Northwoods, Wisconsin | The country, up north, the North (vague) |
| Wisconsin-sourced, local | Farm-to-table, locally-grown (jargon) |
| Jen & Dennis, the family | The team, our staff (we're small enough to name them) |
| The counter, the case, the cooler | Inventory terminology |
| Curated (wine, gifts) | Hand-picked, selection (vague) |
| Cabin, weekend, cookout | Backwoods, rustic (kitsch) |

### 3.4 — Sample phrasings (DRAFT — these are illustrative, not final)

**Hero / headline:**
- "Hand-cut. House-made. Spooner's newest butcher and cellar." (already on live site — verified)
- "Northwoods, well-fed."
- "From the counter to your kitchen."

**Section intros:**
- "What's at the counter today."
- "Worth the drive."
- "Cut fresh. Pulled from the case."

**About / owner voice:**
- "The Lutz family is at the counter, not in a back office."
- "We're putting the finishing touches on 118 Ash Street." *(real draft currently on site)*

**Closing:**
- "Stop in. Bring a cooler."
- "We're at 118 Ash Street, Spooner."

### 3.5 — Tone do / don't

| DO | DON'T |
|---|---|
| Refer to the owners as "the Lutz family" | Refer to them as "the owners," "the proprietors," or by full first names in customer copy |
| Use "we" naturally | Use third-person "Crossroads offers…" |
| Reference Spooner specifically | Generic "local town," "small community" |
| Say "opening July 9" (real date) | "Coming soon" indefinitely |
| Show real products with specificity | "Wide variety of meats" |
| Acknowledge what we don't have yet | Pretend to be a full operation before we are |

---

## 4. Tagline options (DRAFT — for Jen & Dennis to react to)

> **Important:** These are options, not final. None should ship without client approval. They reflect different directions in the brand voice — desc, modern, warm, place-driven — so Jen & Dennis can pick what feels like them.

| # | Tagline | Direction | Notes |
|---|---|---|---|
| 1 | "Hand-cut. House-made. Spooner-made." | Place + craft. Rhythm. Triple-beat structure mirrors the existing site hero. | Echoes the live-site hero phrasing. Strongest for editorial-modern direction. |
| 2 | "Spooner's table." | Two-word. Place + function. Quiet, confident. | Works at any scale — small as a hashtag, large as a sign. Risk: vague if taken alone. |
| 3 | "Butcher. Cellar. Curated." | Three-word. Product + atmosphere. Minimal. | Reads almost like a label set. Modern. Could pair with "Crossroads of the North" as the lockup. |
| 4 | "Cut fresh. Curated daily." | Active-verb pair. Operational honesty. | Doubles as a promise — what they'll see when they walk in. |
| 5 | "Northwoods, well-fed." | Place + sensory. Plays on the cabin-country frame. | Warm without being folksy. Most poetic of the set. |
| 6 | "One stop. Spooner." | Two phrases. Staccato. Stops, then place. | Conveys the one-stop-shop value prop in a glance. |

**My recommendation if forced:** Start with **#1** for editorial lockups (mirrors the live site already in production) and **#5** for FB post headers / seasonal promos. Defer the final decision until Jen & Dennis have been asked.

---

## 5. Photography direction

> **Source:** Zero-fabrication doctrine (Jul 4 2026) and Josh's brief.
> **Status:** Implied. Confirm with Jen & Dennis before kicking off a photo shoot.

### 5.1 — Rules

| DO | DON'T |
|---|---|
| Real photos of the actual shop, the actual cuts, Jen and Dennis in the shop | Stock food photography (especially obvious Getty-style butcher shots) |
| Real labels on shelves and products | Generic fake labels |
| Hands at work (cutting, slicing, plating, ringing up) | Over-styled "food blogger" overheads |
| Warm natural light when possible | Harsh flash, sterile-warehouse lighting |
| Outdoor photos if there's a useful setting (front of shop, Spooner streetscape) | B-roll that looks nothing like NW Wisconsin |
| Family in the frame when natural and wanted | Staged stock-photo families |

### 5.2 — Shot list (suggested for when the shop is ready)

| Subject | Use |
|---|---|
| Exterior — 118 Ash Street storefront | Hero, contact section |
| Interior — counter, case, retail floor | Hero, about |
| Cut being made — hands on the board | "Hand-cut daily" supporting image |
| Brats on the grill | "30+ flavors" support |
| Wisconsin cheese on a board | Cheese section |
| Wine wall / cellar wall | Cellar section |
| Jen and Dennis at the counter | About / hero |
| Family / community behind-the-counter | About |
| Wine + meat + cheese + gift together | "One-stop shop" / gift section |

### 5.3 — Existing site status (Jul 4 2026)

- **Photos currently in site:** Mostly stock food photos (placeholders). Per zero-fabrication doctrine, these are acknowledged as placeholders, not as the shop itself.
- **Action:** Replace with real photos as Jen & Dennis ship them. Tied to scope of ongoing retainer.

### 5.4 — Site section map (locked 2026-07-08)

> **Visual hierarchy:** what / who / where / sneak-peek / careers / socials / contact.

| # | Section | Visual treatment | Content |
|---|---|---|---|
| 1 | Hero | Cream bg, large Playfair title, italic subhead, two CTAs | Date, address, intro |
| 1a | `.hero-trust` | Editorial upper/lowercase with red dot separators | Family-Owned · 30+ Brat Flavors · 30-Day Aged · Local Wisconsin |
| 1b | **Hiring banner** (NEW Jul 8) | Sticky red strip above navbar, z-1001, dismissible (sessionStorage) | "We're hiring — Store Manager & Cleaning Specialist positions open" + See Open Positions CTA |
| 2 | What We Carry (products) | 3-col image grid, no card chrome | 6 product cards |
| 3 | About / Our Story | 2-col with pull quote + 3 pillars | Jen & Dennis story |
| 4 | **Three Rooms, One Shop** (NEW Jul 8) | 3-col feature tiles, 4:5 portrait, italic red 01/02/03 numerals | Cellar / Apparel / Floor — showcases Jen's photos with copy |
| 5 | A Look Inside (gallery marquee) | Auto-scrolling ~100 px/sec, 8 photos duplicated for seamless loop, 64px edge fade, left/right arrow buttons on desktop (hidden ≤720px) | Sneak-peek shop photos |
| 6 | **Now Hiring band** (upgraded Jul 8) | Full-width accent-red band, 2-col desktop, briefcase eyebrow, two role pills, white CTA button, italic quote with attribution | Store Manager + Weekly Cleaning Specialist — soft-opening team |
| 7 | Reviews placeholder | Empty-state | (populated post-launch) |
| 8 | Visit Us / Location | Map + hours + address | 118 Ash Street, Spooner |
| 9 | Final CTA | Two buttons | Send a message / See what we carry |

---

## 6. Brand do / don't (summary poster)

| DO | DON'T |
|---|---|
| Say "Spooner" | Say "small community" |
| Name Jen & Dennis | Say "the team" |
| List specific products | "Wide selection" |
| Use cream + red + ink palette | Add new accent colors |
| Use Playfair + Inter | Substitute typefaces |
| Phosphor icons | Emoji |
| Real photos of the real shop | Stock photos without disclosure |
| Active, short sentences | Passive / corporate phrasing |
| Be honest about being new | Pretend to heritage we don't have |
| Link to FB page for current updates | Pretend to have hours we haven't set |

---

## 7. Open items — confirm with Jen & Dennis

> **Status:** NOT YET CONFIRMED. Listed so the scope conversation has a checklist.

### Voice / tone
- [ ] Does the "warm, honest, locally-rooted" framing feel right? Any descriptors that miss or that we should add?
- [ ] Are the sample phrasings in §3.4 landing? Which ones, if any, sound like them?
- [ ] Puns okay in moderation? Or keep them out entirely?
- [ ] Should Jen & Dennis be referenced by first name always, or sometimes differently?

### Tagline
- [ ] Review the 6 options in §4. Pick 1–2 to refine.
- [ ] Or — do they already have a tagline in their head we haven't captured?

### Visual identity
- [ ] Confirm they want the existing logo as primary mark on the website (no redesign).
- [ ] Logo SVG conversion — want it? (Useful for signage + print.)
- [ ] Confirm color tokens as listed; any house colors we should add or remove?

### Photography
- [ ] Are Jen & Dennis comfortable being on camera/in photos?
- [ ] When can the real shop be shot?
- [ ] Any professional photo shoot budget, or strictly iPhone-natural-light?
- [ ] Permissions for family photos / community / staff?

### Naming / referencing
- [ ] "Crossroads Meats" — short form acceptable in copy, social handles, badges? Or always "Crossroads of the North Meats & Cellar" full?
- [ ] FB page name vs web name: FB is "Crossroads of the North Meats and Cellar" — we should match in copy.

---

## 8. Companion documents

| File | What it covers |
|---|---|
| `/workspace/clients/crossroadsmeatsandcellar/RUNBOOK.md` | Client ops, domain transfer, ongoing engagement state |
| `/workspace/clients/crossroadsmeatsandcellar/draft-message.md` | Pitch draft for Jen & Dennis |
| `/workspace/jjctech-outreach/crossroads-meats/BUILD_SPEC.md` | Build spec for the live site (pre-supersedes §2/§3 of this file for in-page styling only) |
| `/workspace/jjctech-outreach/crossroads-meats/site/index.html` | Live site source — implicit brand execution |

---

## Appendix A — Document history

- **2026-07-05 (reconciled)** — Brand audit landed. Doc synced to live site:
  - §2.2 color tokens updated to reflect served CSS (live site is source of truth):
    - `--bg-alt` corrected from `#EDE9E2` → `#FAF8F4` (live)
    - Added `--rule-soft: #F0EBE0` (was missing from doc)
    - Renamed `--brand-red-button` → `--accent`, `--brand-red-dark` → `--accent-dark` (matches live CSS var names)
    - **Audit fix #4:** `--bg-dark` deprecated — email signup band now uses `--bg-alt` + 2px `--accent` rules top + bottom. Live CSS still defines the var but it is unused.
  - §2.2 pair rules updated to use `--accent` (renamed from `--brand-red-button`)
  - §2.3 typography corrected: Google Fonts request only loads Playfair Display. Inter is **system-stack only** — not loaded from Google Fonts. Flagged for owner decision: brand requirement vs. preference.
  - §2.2 pair rules updated for new `--bg-alt` value (`--ink-muted` on `#FAF8F4` = 5.7x ✓ AA)

- **2026-07-05 (live fix)** — Audit fix #4: dark email signup band removed
  - File: `/workspace/jjctech-outreach/crossroads-meats/site/index.html` (CSS block at original lines 1132-1151)
  - Replaced `background: var(--bg-dark)` + white text with `background: var(--bg-alt)` + ink text + 2px `--accent` rules top/bottom
  - Outline button colors flipped (was white-on-dark, now ink-on-cream)
  - 0 console errors, contrast unchanged (AAA on cream)
  - Vision audit on local preview: "fix raised the register; reads as editorial masthead not website footer"

- **2026-07-05 (live fix)** — Audit fix #7: trustbar styled as editorial colophon
  - File: `/workspace/jjctech-outreach/crossroads-meats/site/index.html`
  - Original state: `<section class="trustbar">` existed in markup but had **no CSS rule** at all — rendered as 5 unstyled `<span>` items on white, height 28px, plain text
  - Audit had misread this as "scrolling marquee" — it was actually plain unstyled text, not animated
  - Fix: added full `.trustbar` + `.trustbar-item` + `.trustbar-item::after` rules
    - Cream bg (`--bg-alt`), 22px padding, 1px hairline rules top/bottom
    - Icons in `--accent` red, uppercase tracked Inter at 0.7rem
    - Dot separators (`::after` `·`) between items, color `--ink-muted` 60% opacity
  - Mobile breakpoint (≤768px):
    - Font-size 0.6rem, letter-spacing 0.1em, tighter padding
    - **Dots hidden** (`.trustbar-item:not(:last-child)::after { display: none }`) — pseudo-elements become flex items and orphan themselves on row wrap
    - Layout naturally wraps to 2+2+1 (acceptable responsive pattern)
  - 0 console errors, 0 a11y violations on local audit
  - Vision audit on local preview @ 2x DPR: "polished, brand-aware trust strip; successfully elevates from utility-bar to editorial-colophon"

- **2026-07-05 (live fix)** — Audit fix #6: "Word's still spreading" re-framed + moved
  - File: `/workspace/jjctech-outreach/crossroads-meats/site/index.html`
  - **Move:** section moved from between About and FAQ to between FAQ and Location (out of prime mid-page real estate)
  - **Copy refresh:** heading changed from "Word's still spreading." to **"Be the first to stop in."**; sub-copy changed from date-specific ("Soft opening Thursday, July 9...") to opening-day-agnostic so it doesn't need a content flip on Jul 9
  - **Hidden bug found during fix:** `.stars { display: none }` was hiding the 5-star placeholder row because the page was only loading Phosphor `regular` weight stylesheet — `ph-fill` icons weren't rendering, so the empty container was hidden
  - **Fix:** added `<link>` for `https://cdn.jsdelivr.net/npm/@phosphor-icons/web@2.1.1/src/fill/style.css` alongside the existing regular link
  - **Visual:** stars now render as faint outline (color `--rule`, opacity 0.45, font-size 1.4rem) — functions as visual anchor without faking a rating
  - **Copy detail:** changed "your note" to "your review" in sub-copy to avoid repetition with the "Leave us a note on Facebook." h3

- **2026-07-05 (live fix)** — Audit fix #8: Sneak Peek gallery polish
  - File: `/workspace/jjctech-outreach/crossroads-meats/site/index.html`
  - Audit said "give the real shop photos more room" — reality was photos were already at usable size (482px wide). Actual fix was polish, not resize
  - **Photo height:** 360px → 420px desktop (60% of figures now render at 562px wide × 505px tall)
  - **Mobile height:** 260px → 300px
  - **Caption styling:** italic display serif 0.95rem muted gray → upright Inter 0.92rem body-grey with 1px hairline border-top separator. Much more legible
  - **Added scroll affordance:** new `<p class="interior-hint">Scroll for more →</p>` pill (cream bg, hairline border, animated arrow pulse)
  - **Mobile:** scroll hint hidden on mobile (≤720px) since horizontal scroll is more discoverable via touch
  - **Thin scrollbar:** added `scrollbar-width: thin` + `scrollbar-color` for Firefox
  - 0 console errors, 0 a11y violations on local audit
  - Vision audit @ 2x DPR: "section feels intentional, curated, and welcoming — exactly the tone a soft-opening announcement should have"

- **2026-07-04 (initial)** — Created. Captured:
  - Brand brief from Josh's 2026-07-04 verbal (owner-approved direction)
  - Visual identity extracted from logo file + `BUILD_SPEC.md` color values
  - Voice & tone inferred from FB content + JD + Josh's brief — labeled as inferred, awaiting client sign-off
  - 6 tagline options as DRAFT options for Jen & Dennis to react to
  - Photography direction aligned with zero-fabrication doctrine
  - 4 explicit client-confirmation areas in §7 (no fabrication in final lockup)

- **2026-07-13 (folder rename)** — Parent folder renamed `spooner-meat-market` → `crossroadsmeatsandcellar` to match the production domain. Path-only change; brand content unchanged. Updated companion-pointer line at top of file and the §8 docs table to use the new path. Historical references to the old path elsewhere in the doc history are kept as-is (they are accurate for the date they describe).
