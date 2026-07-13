# Crossroads of the North Meats and Cellar — Build Spec

> **For:** Build subagent
> **Date:** 2026-07-03
> **Source of truth:** `/workspace/jjctech-outreach/crossroads-meats/research.md` (read this first)
> **Working dir:** `/workspace/jjctech-outreach/crossroads-meats/site/`

---

## Quick Brief

Build a **premium one-page site** for a brand-new **butcher + wine/cellar shop** in Spooner, WI.

- **Business:** Crossroads of the North Meats and Cellar
- **Owners:** Jen & Dennis Lutz (family-owned, hands-on)
- **Address:** 118 Ash Street, Spooner, WI 54801
- **Pre-launch** (opening soon — currently hiring store manager)
- **Logo:** `/workspace/jjctech-outreach/crossroads-meats/site/assets/logo.jpeg` (1320×712, black + red heart, Wisconsin outline)
- **No first-party website exists** — this is greenfield

---

## Brand Palette (verified via PIL — use these exact values)

```css
:root {
  /* Brand-locked colors from logo */
  --brand-red-logo:    #F50D0B;   /* logo heart pure red — for decorative use only */
  --brand-red-button:  #C0392B;   /* AA-passing red for buttons + accent text on white */
  --brand-red-dark:    #8C2A1F;   /* deeper red for hover + dark sections */
  --brand-red-glow:    rgba(192, 57, 43, 0.25);  /* button glow */

  --charcoal:          #0a0a0a;   /* near-black for navbar/hero/footer */
  --charcoal-2:        #1a1a1a;   /* secondary surface (slightly lighter) */
  --cream:             #F8F5F0;   /* warm off-white for light section bg */
  --white:             #ffffff;

  /* Text */
  --text-light:        #ffffff;   /* text on dark sections */
  --text-dark:         #0a0a0a;   /* text on light sections */
  --text-muted:        rgba(255,255,255,0.75);  /* muted on dark */
  --text-muted-dark:   rgba(10,10,10,0.65);    /* muted on light */
}
```

**WCAG verification (already done — don't re-verify):**
- `#C0392B` on white = 5.44:1 (AA pass) ✓
- White on `#C0392B` button = 5.44:1 (AA pass) ✓
- `#C0392B` on charcoal = 3.64:1 → **use larger font (≥18pt) for accent text on dark** OR use `#E84855` (5.18:1) for body text on dark
- Logo red `#F50D0B` only for the heart icon decorative element

**Buttons:** Use `--brand-red-button` (`#C0392B`) — passes WCAG AA.
**Decorative red (heart, accents on light bg):** Logo red `#F50D0B` is fine for non-text elements.

---

## Fonts (Google Fonts CDN)

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

- **Display (headlines):** Playfair Display 700/900 — classic serif, matches the Trajan-family logo typography
- **Body:** Inter 300-700 — clean modern sans, pairs well with serif

---

## Section-by-Section Content

### NAVBAR
- Logo: `assets/logo.jpeg` (full mark, ~140-160px wide, with light/white version if needed)
- Links: `What We Carry` · `Our Story` · `Visit Us` · `Reviews`
- CTA button: "Message Us" → `https://m.me/61590342133702` (FB Messenger)
- Dark sophisticated nav: `var(--charcoal)` bg, `backdrop-filter: blur(20px)`, white links

### HERO
- **Eyebrow:** `Now Opening in Spooner` (or similar — "Coming soon to Ash Street" if pre-launch)
- **Headline:** `Hand-cut. House-made. Spooner's newest butcher and cellar.`
- **Subheadline:** `30-day aged steaks, 30+ brat flavors, Wisconsin cheese, and a curated wine and craft beer selection — opening at 118 Ash Street.`
- **CTAs:** Primary "Send us a message" → `https://m.me/61590342133702` | Secondary "See what we carry" → `#what-we-carry`
- **Hero visual:** Logo + maybe a small Wisconsin map icon echo (or just the logo, large)
- **Trust bar:** `Family-Owned` · `30+ Brat Flavors` · `30-Day Aged` · `Local Wisconsin`

### ABOUT — "Our Story"
- **Headline:** `A family-owned butcher and cellar, rooted in Spooner.`
- **Body:** Write 2-3 paragraphs about Jen & Dennis, family-owned, hands-on philosophy, the "Crossroads" name origin (Spooner = junction of railroads + logging + lake country), the cellar concept, opening soon
- **Pull quote:** `We're hands-on partners with our team and our community. We can't wait to open our doors.`
- **About photo:** Use a stock meat/butcher photo from Pexels/Unsplash since the FB photos are CDN-blocked. Try "raw steak on cutting board" or "butcher counter" — rustic, warm, dark.

### WHAT WE CARRY (replaces default `.services` 4-card grid)
- **Headline:** `What we'll have on the counter.`
- **Intro:** `Hand-cut in-house, sourced local where we can, and aged with care. Here's what you'll find when we open.`
- **6 product cards in 3x2 grid:**
  1. **Hand-Cut Meats** — "Steaks, chops, roasts, and specialty cuts cut fresh in-house every day."
  2. **30-Day Aged Steaks** — "Premium dry-aged for thirty days for deep, beef-forward flavor."
  3. **30+ House-Made Brats** — "From classic Wisconsin to our own creations — there's a brat for every taste."
  4. **Snack Sticks & Jerky** — "Hand-seasoned and smoked. Stock-up cuts for the cabin, the road, and the tailgate."
  5. **Wisconsin Cheese** — "Curds, blocks, and specialty cheeses from cheesemakers across the state."
  6. **Wine, Craft Beer & Gifts** — "A curated cellar of bottles, six-packs, and locally-made gifts. Pet food and treats too."
- **Visual:** Each card gets a stock photo (use Pexels/Unsplash for meat/cheese/wine — no emoji)
- **No prices** — it's a pre-launch site, owner hasn't set them yet

### STATS (kept from template, customize)
- `2 OWNERS` — Jen & Dennis
- `30+ BRAT FLAVORS`
- `30-DAY AGED`
- `OPENING SOON` — at 118 Ash Street

### REVIEWS (kept from template, but mark as "Be the first to review" since 0 reviews exist)
- **Headline:** `Word's still spreading.`
- **Subhead:** `We just opened — be the first to leave a review on Facebook.`
- **CTA:** "Leave a review on Facebook" → `https://www.facebook.com/people/Crossroads-of-the-North-Meats-and-Cellar/61590342133702/`
- **Or:** Show 2-3 placeholder cards that say something like "Pre-launch — reviews coming soon" with the 5-star UI but soft content
- **Recommended:** Just keep the section but make it a single centered "Be the first to review" CTA card — don't fake testimonials

### FAQ (5 questions — relevant to a NEW butcher)
1. **When are you opening?** — `We're putting the finishing touches on 118 Ash Street and will announce our opening day soon. Follow us on Facebook for the latest.`
2. **Do you take special orders?** — `Yes — call or message us about custom cuts, gift baskets, and party trays.`
3. **Do you carry local products?** — `Absolutely. We source from Wisconsin farms and producers whenever we can. Look for the local tag on shelves.`
4. **Can I bring my own container?** — `Yes. Bring your own clean container for meat or cheese and we'll weigh it out.`
5. **Do you deliver?** — `Not at launch, but ask us in-store about catering and large orders.`

### LOCATION / VISIT US
- **Address card:** 118 Ash Street, Spooner, WI 54801
- **Hours card:** TBD — Opening soon
- **Map:** Google Maps embed centered on the address
- **Phone:** "Phone coming soon" or omit (no public number)
- **Side note:** "We're right in the heart of downtown Spooner — easy to find, easy to park."

### FINAL CTA
- **Headline:** `Stay in the loop.`
- **Subhead:** `Follow us on Facebook for opening day announcements, new brat flavors, and what's on the smoker.`
- **Buttons:** "Message us on Facebook" (FB Messenger) + "Like our page" (FB page)

### FOOTER
- Logo (small)
- Tagline: `Crossroads of the North — Hand-cut meats, 30-day aged steaks, and a curated cellar in Spooner, Wisconsin.`
- Columns: Visit (118 Ash Street, Spooner, WI 54801) · Hours (Opening soon) · Connect (FB link)
- Bottom: `© 2026 Crossroads of the North Meats and Cellar · Site by JJC Tech`

---

## METADATA (fill into <head>)

| Field | Value |
|---|---|
| SITE_NAME | Crossroads of the North Meats and Cellar |
| SHORT_NAME | Crossroads of the North |
| TAGLINE | Hand-cut meats, house-made brats, and a curated cellar |
| META_DESCRIPTION | Family-owned butcher and wine shop in Spooner, WI. Hand-cut meats, 30-day aged steaks, 30+ brat flavors, Wisconsin cheese, and a curated cellar. Opening soon at 118 Ash Street. |
| SITE_URL | (use relative for now, or `https://crossroads-meats.pages.dev`) |
| CITY | Spooner |
| STATE | WI |
| ZIP | 54801 |
| STREET_ADDRESS | 118 Ash Street |
| LAT | 45.8225 |
| LNG | -91.8899 |
| OPEN_TIME | Opening Soon |
| CLOSE_TIME | TBD |
| PHONE / PHONE_DISPLAY | (not public — use a `tel:` link with no number, or omit; CTA routes to FB Messenger) |
| EMAIL | info@jjctech.net (forms-worker fallback — real email can be added post-launch) |
| TWITTER_HANDLE | @crossroadsnorth (placeholder) |
| LEAVE_REVIEW_URL | https://www.facebook.com/people/Crossroads-of-the-North-Meats-and-Cellar/61590342133702/ |
| SEE_REVIEWS_URL | (same as above) |
| ACCENT_COLOR | #C0392B |
| ACCENT_DARK | #8C2A1F |
| ACCENT_LIGHT | #E84855 |
| ACCENT_GLOW | rgba(192, 57, 43, 0.25) |
| BG_PRIMARY | #0a0a0a |
| BG_SECONDARY | #1a1a1a |
| BG_CARD | #141414 |
| BG_DARK | #0a0a0a |
| BORDER_COLOR | rgba(255,255,255,0.08) |
| TEXT_PRIMARY | #ffffff |
| TEXT_SECONDARY | rgba(255,255,255,0.85) |
| TEXT_MUTED | rgba(255,255,255,0.65) |
| BG_RGB | "10, 10, 10" |
| OG_IMAGE | /assets/logo.jpeg |
| OG_IMAGE_ALT | Crossroads of the North Meats and Cellar logo |
| YEAR | 2026 |
| FOUNDER_NAME | Jen & Dennis Lutz |
| FOUNDER_TITLE | Owners |
| BUSINESS_TYPE | Butcher Shop · Wine & Beer Store |
| PRICE_RANGE | $$ |
| RATING | 5.0 (placeholder — no reviews yet) |
| REVIEW_COUNT | 0 (will show "Opening soon" instead) |
| CREDENTIALS | Class A Liquor License · Wisconsin Food Retail |

---

## TRUTH-OR-FABRICATION RULES — CRITICAL

1. **No fake testimonials.** Pre-launch site — 0 reviews exist. Either omit the reviews section, show a single "Be the first" CTA, or use clear placeholder text saying "Reviews coming soon."
2. **No fake hours.** Status is pre-launch. Show "Opening Soon" or TBD honestly.
3. **No fake phone number.** None exists publicly. Use FB Messenger for contact, and only mention email if real.
4. **No fake photos.** The FB photos are CDN-blocked. Use **real stock from Pexels/Unsplash** (free, no attribution required for our use). Search:
   - Butcher/cutting board: https://www.pexels.com/search/butcher/
   - Cheese: https://www.pexels.com/search/cheese/
   - Wine cellar: https://www.pexels.com/search/wine%20cellar/
   - Bratwurst/grilling: https://www.pexels.com/search/bratwurst/
   - **DOWNLOAD the stock images with curl/wget** to `site/assets/` (don't reference the external URL — CF Pages will block it)
   - Rename them: `stock-butchercut.jpg`, `stock-cheese.jpg`, `stock-wine.jpg`, `stock-brats.jpg`, `stock-snack-sticks.jpg`, `stock-gifts.jpg`
5. **Owner photos:** Skip — owners haven't published any.
6. **No emoji anywhere.** Use Phosphor icons only (`<i class="ph ph-ICON">`) from `https://cdn.jsdelivr.net/npm/@phosphor-icons/web@2.1.1/src/regular/style.css`

---

## ECT CROSS-BUILD DEFAULTS (Jul 3 2026) — APPLY THESE

- ❌ **NO** `.services` 4-card grid (services, contact-form, contact) — REPLACE with "What We Carry" product grid (already specified above)
- ❌ **NO** "Our Services" eyebrow
- ❌ **NO** "Let's Talk" contact form — REPLACE with FB Messenger button + email form via `https://forms.jjctech.net`
- ❌ **NO** "What We Do" or generic "Get a quote" copy
- ❌ **NO** `.hero-grid` animated grid pattern (already removed from template)
- ❌ **NO** `body::before` grain noise (already removed from template)
- ❌ **NO** emoji
- ❌ **NO** navy/blue default template colors
- ✅ Use Playfair Display + Inter (NOT template defaults)
- ✅ Charcoal/cream/red palette (Wisconsin heritage feel)
- ✅ "Visit Us" location section with map embed

---

## CONTACT FORM (replace template default)

The template's `.contact` form should be DELETED. Replace with a final CTA section that has:
- Headline: `Stay in the loop.`
- Subhead: `Send us a note and we'll get back to you.`
- A 3-field form (name, email, message) posting to `https://forms.jjctech.net/crossroads-meats` (or similar — just make it look professional)
- Plus a `Send us a message on Facebook` button (FB Messenger link)

Use the existing form styles from template CSS, just rebrand the labels/CTAs.

---

## STRUCTURED DATA (JSON-LD)

Add a `ButcherShop` + `FoodEstablishment` schema in the `<head>`:
```json
{
  "@context": "https://schema.org",
  "@type": "FoodEstablishment",
  "name": "Crossroads of the North Meats and Cellar",
  "image": "https://crossroads-meats.pages.dev/assets/logo.jpeg",
  "@id": "https://crossroads-meats.pages.dev",
  "url": "https://crossroads-meats.pages.dev",
  "telephone": "",
  "priceRange": "$$",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "118 Ash Street",
    "addressLocality": "Spooner",
    "addressRegion": "WI",
    "postalCode": "54801",
    "addressCountry": "US"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 45.8225,
    "longitude": -91.8899
  },
  "openingHoursSpecification": {
    "@type": "OpeningHoursSpecification",
    "description": "Opening soon"
  },
  "sameAs": [
    "https://www.facebook.com/people/Crossroads-of-the-North-Meats-and-Cellar/61590342133702/"
  ]
}
```

---

## ITERATION PLAN (3 iterations)

### Iteration 1 — Build the site
1. Read this spec + `research.md`
2. Copy template is already done
3. Fill ALL 71 placeholders
4. Apply brand palette to `:root` CSS variables
5. Replace `.services` 4-card grid with "What We Carry" 6-card product grid
6. Replace `.contact` form with the simplified FB-Messenger + email form
7. Add Phosphor icons (CDN, with `crossorigin` and `rel="preconnect"`)
8. Add the JSON-LD structured data
9. Add stock photos (download from Pexels/Unsplash, save to `site/assets/`)
10. Take desktop + mobile screenshots using `screenshot_with_observer_fire()`
11. Run `scripts/content-presence-gate.py` from local-web-design skill — must PASS

### Iteration 2 — Fix any gate failures + vision critique
- If gate fails → fix the issue → re-run
- If gate passes → M3 vision critique using `local-web-design` skill's premium mockup audit prompt
- Fix any 🔴 blockers

### Iteration 3 — Polish + final
- Fix any 🟡 polish issues
- Final desktop + mobile screenshots
- Verify all 71 placeholders replaced (grep for `{{` should return 0)
- Verify no emoji (grep for common emoji codepoints)

---

## DELIVERABLE

When done, return:
```
CROSSROADS_MEATS_BUILD_DONE
final_desktop: /tmp/crossroads-meats-final-desktop.png
final_mobile: /tmp/crossroads-meats-final-mobile.png
existing_desktop: N/A (no existing site)
existing_mobile: N/A
notion_page_id: 392a5829-4490-8106-9965-c935516c9ff6
business_name: Crossroads of the North Meats and Cellar
site_url: https://crossroads-meats.pages.dev
iteration_summary: <2-3 sentence summary>
gate_status: PASS/FAIL
contrast_min: <float>
```

---

## LOCAL FILES (paths subagent will use)

- Working dir: `/workspace/jjctech-outreach/crossroads-meats/site/`
- HTML: `/workspace/jjctech-outreach/crossroads-meats/site/index.html`
- Assets: `/workspace/jjctech-outreach/crossroads-meats/site/assets/`
- Research: `/workspace/jjctech-outreach/crossroads-meats/research.md`
- Logo: `/workspace/jjctech-outreach/crossroads-meats/site/assets/logo.jpeg`
- Content gate script: `/root/.hermes/skills/productivity/local-web-design/scripts/content-presence-gate.py`
- Template: `/workspace/premium-template/`

---

## DON'T FORGET

- Read `local-web-design` skill BEFORE iterating (load with `skill_view(name='local-web-design')`)
- Use `screenshot_with_observer_fire()` from the skill — never bare `page.screenshot()`
- Every external image must be downloaded to `site/assets/` and referenced with a relative path
- Phosphor icons CDN: `https://cdn.jsdelivr.net/npm/@phosphor-icons/web@2.1.1/src/regular/style.css` (NOT unpkg — Edge TP false positive)
- Run the gate before any vision critique
- M3 vision critique: use the prompt from local-web-design skill, NOT a "what's working" pre-bucket
- Stay 100% truthful — no fake reviews, no fake hours, no fake phone, no fake photos
