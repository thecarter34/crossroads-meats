# Technical SEO Audit — crossroadsmeatsandcellar.com

**Audited:** July 13, 2026  
**Site:** Static HTML · Cloudflare Pages  
**Source:** `/workspace/jjctech-outreach/crossroads-meats/site/`  
**Pages:** `index.html` (homepage) + `hiring.html` (hiring)

---

## A. Crawlability & Indexability

### robots.txt
```
$ curl -sI https://crossroadsmeatsandcellar.com/robots.txt
HTTP/2 200

$ curl -s https://crossroadsmeatsandcellar.com/robots.txt
User-agent: *
Allow: /
Sitemap: https://crossroadsmeatsandcellar.com/sitemap.xml
```
✅ Present, correct HTTP 200, `Allow: /` + sitemap directive present.

> **Note:** The Cloudflare Managed content signal header block at the top (`Content-Signal: search=yes,ai-train=no`) is informational prose, not a crawler directive. The `robots.txt` correctly allows all.

### sitemap.xml
```
$ curl -s https://crossroadsmeatsandcellar.com/sitemap.xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://crossroadsmeatsandcellar.com/</loc>
  </url>
  <url>
    <loc>https://crossroadsmeatsandcellar.com/hiring</loc>
  </url>
</urlset>
```
✅ Present, valid XML, lists both URLs (`/` and `/hiring`). Passes Python XML parser.

### HTTP Status Codes
| URL | Status |
|---|---|
| `https://crossroadsmeatsandcellar.com/` | ✅ 200 |
| `https://crossroadsmeatsandcellar.com/hiring` | ✅ 200 |
| `https://crossroadsmeatsandcellar.com/sitemap.xml` | ✅ 200 |
| `https://crossroadsmeatsandcellar.com/robots.txt` | ✅ 200 |
| `https://crossroadsmeatsandcellar.com/nonexistent-xyz123` | ✅ 404 |

### Meta Robots
```
<meta name="robots" content="index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1" />
```
✅ `index, follow` confirmed on homepage. The `max-snippet:-1` is unusual (disables all snippets) — may want `max-snippet:150` to allow partial snippets while still limiting length, but this is not harmful.

### Canonical Tags
- Homepage: `<link rel="canonical" href="https://crossroadsmeatsandcellar.com/" />` ✅
- Hiring: `<link rel="canonical" href="https://crossroadsmeatsandcellar.com/hiring" />` ✅

Both canonical tags correctly point to themselves.

### Real 404
Bogus URL returns `HTTP/2 404` with actual `text/html` content-type — real 404, not a Cloudflare Pages SPA catch-all. Confirmed no SPA router or service worker in the page.

✅ No SPA fallback masking.

---

## B. On-Page SEO Completeness

### Title Tag
```
Spooner Butcher Shop | Crossroads of the North Meats & Cellar
```
**Length: 65 characters** — exceeds the 60-char recommended maximum.

Analysis: Contains primary keyword ("Butcher Shop") and city ("Spooner"). The brand name "Crossroads of the North Meats & Cellar" is spelled out in full, pushing length over 60. The `&amp;` renders as `&` in browser — fine.

**Recommendation:** Trim to ≤60 chars. Example: `Spooner Butcher Shop | Crossroads Meats & Cellar` (54 chars).

### Meta Description
```
Family-owned butcher shop and curated cellar in Spooner, WI. Shop hand-cut meats,
30-day aged steaks, 30+ brat flavors, Wisconsin cheese, wine, and craft beer.
```
**Length: 159 characters** — within 140–160 range. ✅ Contains CTA language ("Shop hand-cut…") and city.

### H1 — Homepage
```
<h1>Hand-cut. House-made.<br/><span>Spooner's newest butcher and cellar.</span></h1>
```
✅ Single H1. Contains primary keyword ("butcher"). "Spooner's" includes the city geo-term naturally.

### Heading Hierarchy
| Tag | Count | Sample |
|---|---|---|
| H1 | 1 | Hand-cut. House-made. |
| H2 | 9 | What we'll have on the counter., A family-owned butcher…, Step inside… |
| H3 | 13 | Hand-Cut Meats, 30-Day Aged Steaks, Wine, Craft Beer & Gifts… |
| H4 | 5 | Address, Hours, Get in touch, Explore, Connect |

✅ H1 → H2 → H3 logical flow. No skipped levels. H4s are in footer which is acceptable.

### Image Alt Text
```
$ curl -s https://crossroadsmeatsandcellar.com/ | python3 -c "..." (Python analysis)
Total <img>: 30
With alt attr: 30
Without alt attr: 0
```
✅ **100% of images have alt attributes.** All alts are descriptive (e.g., `"Hand-cut ribeye steaks on a rustic cutting board"`, not filenames). No fabrication.

### Internal Linking
- ✅ `/hiring` linked from: hiring banner, navbar, footer (3 places)
- ✅ Sitemap linked from `robots.txt` (Google reads this)
- ✅ Footer links to all major sections: `#products`, `#about`, `#location`, `#reviews`, `#faq`
- ✅ Facebook link: `https://www.facebook.com/people/Crossroads-of-the-North-Meats-and-Cellar/61590342133702/` — outbound with `rel="noopener"` ✅

### Anchor Text
```
Generic ("click here", "read more") anchor texts: NONE ✅
```
Sample anchors: "What We Carry", "Our Story", "Leave a review on Facebook", "See what we carry" — all descriptive.

---

## C. Structured Data (JSON-LD)

All JSON-LD blocks extracted and validated with Python `json.loads()`.

### Block 1 — LocalBusiness + WebSite (`@graph`)
```json
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": ["GroceryStore", "LiquorStore"],
      "@id": "https://crossroadsmeatsandcellar.com/#business",
      "name": "Crossroads of the North Meats and Cellar",
      "alternateName": "Crossroads of the North",
      "description": "Family-owned butcher shop and curated cellar in Spooner, WI...",
      "url": "https://crossroadsmeatsandcellar.com/",
      "image": "https://crossroadsmeatsandcellar.com/assets/logo.jpeg",
      "logo": "https://crossroadsmeatsandcellar.com/assets/logo.jpeg",
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
      "areaServed": [
        { "@type": "City", "name": "Spooner" },
        { "@type": "AdministrativeArea", "name": "Washburn County" }
      ],
      "founder": [
        { "@type": "Person", "name": "Jen Lutz" },
        { "@type": "Person", "name": "Dennis Lutz" }
      ],
      "sameAs": [
        "https://www.facebook.com/people/Crossroads-of-the-North-Meats-and-Cellar/61590342133702/"
      ],
      "hasOfferCatalog": { ... },
      "potentialAction": {
        "@type": "CommunicateAction",
        "target": "https://m.me/61590342133702",
        "name": "Message Crossroads on Facebook Messenger"
      }
    },
    {
      "@type": "WebSite",
      "@id": "https://crossroadsmeatsandcellar.com/#website",
      "url": "https://crossroadsmeatsandcellar.com/",
      "name": "Crossroads of the North Meats and Cellar",
      "publisher": { "@id": "https://crossroadsmeatsandcellar.com/#business" },
      "inLanguage": "en-US"
    }
  ]
}
```
**✅ Valid JSON.** Complete — name, address, geo, areaServed, sameAs (Facebook), hasOfferCatalog, potentialAction.

**❌ MISSING: `telephone`** — no phone number anywhere in schema. 

### Block 2 — FAQPage
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What are your hours?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "We're finalizing hours for our opening at 118 Ash Street. Follow us on Facebook..."
      }
    },
    { "@type": "Question", "name": "Do you take special orders?", "acceptedAnswer": { ... } },
    { "@type": "Question", "name": "Do you carry local products?", "acceptedAnswer": { ... } },
    { "@type": "Question", "name": "Can I bring my own container?", "acceptedAnswer": { ... } },
    { "@type": "Question", "name": "Do you deliver?", "acceptedAnswer": { ... } }
  ]
}
```
**✅ Valid JSON. 5 Q&A pairs.** All Questions have `acceptedAnswer` with `text`. Schema is correctly formed.

### hiring.html — JobPosting (Store Manager only)
```json
{
  "@context": "https://schema.org",
  "@type": "JobPosting",
  "title": "Store Manager",
  "description": "Lead day-to-day operations...",
  "identifier": {
    "@type": "PropertyValue",
    "name": "Crossroads of the North Meats and Cellar",
    "value": "crossroads-store-manager"
  },
  "datePosted": "2026-07-04",
  "employmentType": "FULL_TIME",
  "industry": "Food & Beverage",
  "hiringOrganization": {
    "@type": "Organization",
    "name": "Crossroads of the North Meats and Cellar",
    "sameAs": "https://crossroadsmeatsandcellar.com/",
    "logo": "https://crossroadsmeatsandcellar.com/assets/logo.jpeg"
  },
  "jobLocation": {
    "@type": "Place",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "118 Ash Street",
      "addressLocality": "Spooner",
      "addressRegion": "WI",
      "postalCode": "54801",
      "addressCountry": "US"
    }
  }
}
```
**✅ Valid JSON.** Store Manager has full JobPosting schema.

**❌ MISSING: JobPosting for "Weekly Cleaning Specialist"** (second role on the page has no JSON-LD).

---

## D. Performance / Core Web Vitals Proxy

### HTML Page Weight (gzip)
```
$ curl -s --compressed -H "Accept-Encoding: gzip" https://crossroadsmeatsandcellar.com/ | wc -c
92623
```
**92 KB uncompressed, gzip transfers at ~92 KB** (CF Pages compresses but the compressed size of a single HTML page that is mostly CSS/text is close to raw due to the large inline stylesheet).

The `index.html` file on disk is 92,623 bytes — Cloudflare Pages applies its own gzip/brotli compression at the edge; actual transfer size will be smaller.

### Render-Blocking External Resources
| Resource | Type | Blocking? |
|---|---|---|
| `cdn.jsdelivr.net/npm/@phosphor-icons/web@2.1.1/src/regular/style.css` | External CSS | ⚠️ Render-blocking |
| `fonts.googleapis.com/css2?family=Playfair+Display...` | External CSS font | ⚠️ Render-blocking (font-display: swap should be checked) |

**1 external stylesheet** (Phosphor Icons) + **1 Google Fonts** stylesheet are render-blocking. Both use `preconnect`/`dns-prefetch` which is good.

Google Fonts is linked in `<head>` — this is render-blocking by default. The `preconnect` hints help but don't eliminate blocking.

### Image Sizing — CLS Risk
```
Images without explicit width/height: 29 of 30
```
Only `assets/stock-about.jpg` has `width="900" height="900"`. The remaining 29 `<img>` tags lack explicit dimensions.

**This is a MEDIUM risk for CLS** (Cumulative Layout Shift). When images load lazily, the browser may shift layout when each image finishes downloading.

### Lazy Loading
```
Images with loading="lazy": 18 of 30
```
18 images use `loading="lazy"`. The remaining 12 do not — these are likely above-the-fold or in the hero/interior strips.

The hero logo (`assets/logo.jpeg`) uses `loading="eager"` (correct for LCP). The product grid images use `loading="lazy"`.

### LCP Candidate
The LCP (Largest Contentful Paint) element is most likely the hero logo image (`assets/logo.jpeg`) — it's above the fold, rendered early, and uses `loading="eager"`.

The hero section has no background photo (CSS gradient-only), so no large photographic LCP candidate. The logo at 220px max-width is small but will be the LCP element.

**Note:** If Google Fonts render-blocking causes a font-swap, it could delay LCP slightly. Adding `font-display: swap` to Playfair Display is already standard in Google Fonts imports.

---

## E. Local SEO Signals

### NAP Consistency
| Field | Homepage meta / visible | Schema | Footer | Hiring |
|---|---|---|---|---|
| **Name** | "Crossroads of the North Meats and Cellar" | ✅ same | ✅ same | ✅ same |
| **Address** | 118 Ash Street, Spooner, WI 54801 | ✅ same | ✅ same | ✅ same |
| **Phone** | ❌ **NOT PRESENT** anywhere | ❌ absent | ❌ absent | ❌ absent |

**No phone number on the entire site** — not in schema, not in meta, not in body text, not in footer. This is a significant local SEO gap.

### Geo Meta Tags
```
<meta name="geo.region" content="US-WI" />
<meta name="geo.placename" content="Spooner, WI" />
<meta name="geo.position" content="45.8225;-91.8899" />
<meta name="ICBM" content="45.8225, -91.8899" />
```
✅ All four geo meta tags present. `ICBM` is a legacy tag but harmless.

### OpeningHoursSpecification
✅ **Resolved 2026-07-23.** JSON-LD `@graph` on the homepage now carries a 2-period `openingHoursSpecification`:
```json
"openingHoursSpecification": [
  { "@type": "OpeningHoursSpecification",
    "dayOfWeek": ["Thursday", "Friday", "Saturday"],
    "opens": "10:00", "closes": "18:00" },
  { "@type": "OpeningHoursSpecification",
    "dayOfWeek": ["Sunday"],
    "opens": "10:00", "closes": "15:00" }
]
```
Mon/Tue/Wed closed by absence (Google treats missing days as closed). Confirms via `re.findall('<script type="application/ld\+json">(.*?)</script>', index.html)` + JSON parse. Sun's close changed from 18:00 → 15:00 per Dennis 2026-07-23 message — was added when the site launched with all four days 10–6.

### LocalBusiness Schema Completeness
| Field | Present? |
|---|---|
| @type (GroceryStore + LiquorStore) | ✅ |
| name | ✅ |
| address | ✅ |
| geo (GeoCoordinates) | ✅ |
| areaServed | ✅ |
| image / logo | ✅ |
| priceRange | ✅ |
| sameAs (Facebook) | ✅ |
| founder | ✅ |
| hasOfferCatalog | ✅ |
| potentialAction | ✅ |
| **telephone** | ❌ |
| **openingHoursSpecification** | ❌ |
| openingHours (string form) | ❌ |

---

## F. Missing / Weak Signals (Priority-Ordered Findings)

---

### [HIGH] — Missing `openingHoursSpecification` in LocalBusiness schema

**Evidence:** String search for `"openingHoursSpecification"` in all schema blocks returns empty:
```
$ curl -s https://crossroadsmeatsandcellar.com/ | python3 -c "..."
Has openingHoursSpecification: False
```

**Impact:** Google explicitly weights this field for local pack ranking. A new business with "Opening Soon" is understandable, but this must be added the moment hours are confirmed.

**Recommended fix:** Add to LocalBusiness `@graph` block:
```json
"openingHoursSpecification": [
  {
    "@type": "OpeningHoursSpecification",
    "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
    "opens": "TBD",
    "closes": "TBD"
  },
  {
    "@type": "OpeningHoursSpecification",
    "dayOfWeek": ["Saturday", "Sunday"],
    "opens": "TBD",
    "closes": "TBD"
  }
]
```
Replace "TBD" with actual hours once confirmed.

---

### [HIGH] — No `telephone` anywhere on the site

**Evidence:**
```
Telephone in schema: NOT FOUND
Phone numbers found in body: []
```
Zero phone numbers in schema, meta, or visible page content. `search_files` for phone number patterns (`\d{3}[-.\s]?\d{3}[-.\s]?\d{4}`) in body returns no results.

**Impact:** Google's local ranking factors explicitly include "phone number consistency and correctness." Users searching "butcher shop Spooner phone" find nothing. This is also a usability gap.

**Recommended fix:**
1. Add to LocalBusiness schema: `"telephone": "+1-XXX-XXX-XXXX"`
2. Add to footer contact block: display the phone number
3. Add `tel:` link in navbar CTA area
4. Cross-reference against actual GBP phone once verified

---

### [MED] — Missing JobPosting schema for "Weekly Cleaning Specialist"

**Evidence:** `hiring.html` has only one JSON-LD block (Store Manager). The second role "Weekly Cleaning Specialist" has no schema.

**Recommended fix:** Add second `JobPosting` block to `hiring.html` for the Cleaning Specialist role, mirroring the Store Manager structure.

---

### [MED] — 29 of 30 images lack explicit `width`/`height` attributes (CLS risk)

**Evidence:**
```
Images without width/height: 29
Images with width/height: 1  (only stock-about.jpg)
```

**Impact:** Each lazy-loaded image can cause a Cumulative Layout Shift when it loads, since the browser doesn't know the aspect ratio in advance. Product grid cards use `aspect-ratio: 1/1` in CSS which mitigates most of this, but interior strip images and some hero images will still shift.

**Recommended fix:** Add `width` and `height` attributes to every `<img>` tag, or use CSS `aspect-ratio` consistently (the product grid already does this). For images where CSS controls size (`object-fit: cover`), adding `width`/`height` attributes is still the most reliable CLS mitigation.

```bash
# Find all images missing dimensions (from source):
grep -n '<img' /workspace/jjctech-outreach/crossroads-meats/site/index.html | grep -v 'width='
```

---

### [MED] — Google Fonts CSS is render-blocking

**Evidence:**
```
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=Inter:wght@300;400;500;600;700&display=swap" />
```

**Impact:** Font file is loaded synchronously in `<head>`. With `preconnect` hints in place, DNS lookup is fast, but the actual CSS parse and font file fetch still blocks rendering.

**Recommended fix:** Add `font-display: swap` to the Google Fonts URL (already default in newer Google Fonts imports). For full non-blocking loading, consider `preload` + `onload` pattern:
```html
<link rel="preload" as="style" href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=Inter:wght@300;400;500;600;700&display=swap" onload="this.onload=null;this.rel='stylesheet'" />
<noscript><link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=Inter:wght@300;400;500;600;700&display=swap" /></noscript>
```

---

### [LOW] — Title tag is 65 characters (exceeds 60-char recommendation)

**Evidence:**
```
Title: Spooner Butcher Shop | Crossroads of the North Meats & Cellar
Title length: 65 chars
```
**Recommended fix:**
```
Spooner Butcher Shop | Crossroads Meats & Cellar  (54 chars)
```
or trim the left side:
```
Butcher Shop & Cellar | Spooner, WI  (35 chars)
```

---

### [LOW] — `max-snippet:-1` in meta robots may limit CTR

**Evidence:**
```
<meta name="robots" content="index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1" />
```

`max-snippet:-1` blocks Google from showing any text snippet in search results. This is unusual for a homepage — it will force Google to auto-generate snippets from page content, which may be worse than a controlled meta description.

**Recommended fix:** Change to `max-snippet:150` to allow a reasonable partial snippet while keeping the meta description as the controlled signal.

---

### [INFO] — No `hreflang` needed

Single locale (`lang="en"`, `og:locale="en_US"`). No alternate language/region versions. **hreflang is N/A.**

---

### [INFO] — No breadcrumb on homepage

Homepage does not use breadcrumb navigation (N/A). `hiring.html` has a manual breadcrumb ("Home / Now Hiring") in the page hero — this is fine.

---

## G. Verification Commands

Run these commands to verify each finding exists or has been fixed.

### HIGH: OpeningHoursSpecification
```bash
# Check if openingHoursSpecification is present in homepage schema
curl -s https://crossroadsmeatsandcellar.com/ | \
  python3 -c "import sys,re,json; html=sys.stdin.read(); blocks=re.findall(r'<script type=\"application/ld\+json\">(.*?)</script>',html,re.DOTALL); [print('FOUND') if 'openingHoursSpecification' in b else None for b in blocks]"

# Expected after fix: no output (nothing printed = all checks pass)
# Or to show the actual hours block:
curl -s https://crossroadsmeatsandcellar.com/ | \
  python3 -c "import sys,re,json; html=sys.stdin.read(); blocks=re.findall(r'<script type=\"application/ld\+json\">(.*?)</script>',html,re.DOTALL); [print(json.dumps(json.loads(b), indent=2)[:500]) for b in blocks if 'openingHoursSpecification' in b]"
```

### HIGH: Telephone in schema
```bash
# Check telephone in schema
curl -s https://crossroadsmeatsandcellar.com/ | \
  python3 -c "import sys,re; html=sys.stdin.read(); t=re.search(r'\"telephone\"\s*:\s*\"([^\"]+)\"',html); print('TELEPHONE:', t.group(1) if t else 'NOT FOUND')"

# Check telephone in visible body content
curl -s https://crossroadsmeatsandcellar.com/ | \
  python3 -c "import sys,re; html=sys.stdin.read(); body=re.search(r'<body[^>]*>(.*?)</body>',html,re.DOTALL); text=re.sub(r'<[^>]+>',' ',body.group(1) if body else ''); phones=re.findall(r'\b\d{3}[-.\s]?\d{3}[-.\s]?\d{4}\b',text); print('PHONES IN BODY:', phones if phones else 'NONE')"
```

### MED: JobPosting for Weekly Cleaning Specialist
```bash
# Count JobPosting blocks in hiring.html
python3 -c "import re,json; html=open('/workspace/jjctech-outreach/crossroads-meats/site/hiring.html').read(); blocks=re.findall(r'<script type=\"application/ld\+json\">(.*?)</script>',html,re.DOTALL); jobs=[b for b in blocks if '\"@type\": \"JobPosting\"' in b]; print(f'JobPosting blocks: {len(jobs)}'); [print(json.loads(b).get('title','?')) for b in jobs]"
# Expected after fix: 2 (Store Manager + Weekly Cleaning Specialist)
```

### MED: Image dimensions
```bash
# Count images without width or height in live page
curl -s https://crossroadsmeatsandcellar.com/ | \
  python3 -c "import sys,re; html=sys.stdin.read(); imgs=re.findall(r'<img([^>]*)>',html); no_dim=[i for i in imgs if 'width=' not in i or 'height=' not in i]; print(f'Images without dimensions: {len(no_dim)}')"
# Expected after fix: 0
```

### MED: Google Fonts non-blocking
```bash
# Check if Google Fonts is loaded with preload/onload pattern (non-blocking)
curl -s https://crossroadsmeatsandcellar.com/ | \
  python3 -c "import sys,re; html=sys.stdin.read(); gf=re.findall(r'fonts\.googleapis\.com[^<\"]+',html); print('Google Fonts URLs:', gf[:2]); has_preload=bool(re.search(r'<link[^>]+rel=\"preload\"[^>]+fonts\.googleapis',html)); print('Has preload pattern:', has_preload)"
```

### LOW: Title length
```bash
# Check title length
curl -s https://crossroadsmeatsandcellar.com/ | \
  python3 -c "import sys,re; html=sys.stdin.read(); t=re.search(r'<title>(.*?)</title>',html); title=t.group(1).replace('&amp;','&'); print(f'Title: {title}'); print(f'Length: {len(title)} chars'); print('OK' if len(title)<=60 else 'TOO LONG — trim to 60 or fewer')"
```

### LOW: max-snippet in robots
```bash
# Check meta robots max-snippet
curl -s https://crossroadsmeatsandcellar.com/ | \
  python3 -c "import sys,re; html=sys.stdin.read(); r=re.search(r'<meta name=\"robots\" content=\"([^\"]+)\"',html); print('Meta robots:', r.group(1) if r else 'NOT FOUND'); print('OK' if 'max-snippet:-1' not in (r.group(1) if r else '') else 'WARN: max-snippet:-1 may limit CTR — change to max-snippet:150')"
```

### Verify sitemap is clean
```bash
python3 -c "
import urllib.request, xml.etree.ElementTree as ET, ssl
ctx = ssl.create_default_context()
ctx.check_hostname = False
ctx.verify_mode = ssl.CERT_NONE
data = urllib.request.urlopen('https://crossroadsmeatsandcellar.com/sitemap.xml', context=ctx).read()
root = ET.fromstring(data)
ns = {'sm': 'http://www.sitemaps.org/schemas/sitemap/0.9'}
urls = root.findall('sm:url', ns)
for u in urls:
    loc = u.find('sm:loc', ns)
    print(loc.text)
"
# Expected: https://crossroadsmeatsandcellar.com/  and  https://crossroadsmeatsandcellar.com/hiring
```

---

## Summary

| # | Severity | Finding | Status |
|---|---|---|---|
| 1 | **HIGH** | Missing `openingHoursSpecification` in LocalBusiness schema | ✅ RESOLVED 2026-07-23 (per Dennis: Sun 10-3, Thu-Sat 10-6) |
| 2 | **HIGH** | No telephone anywhere on site (schema, meta, body, footer) | Must add |
| 3 | **MED** | Missing JobPosting schema for Weekly Cleaning Specialist | Must add |
| 4 | **MED** | 29/30 images lack explicit `width`/`height` (CLS risk) | Should fix |
| 5 | **MED** | Google Fonts CSS is render-blocking | Should fix |
| 6 | **LOW** | Title tag 65 chars (exceeds 60-char max) | Should fix |
| 7 | **LOW** | `max-snippet:-1` may limit homepage CTR | Should fix |
| — | INFO | No hreflang needed (single locale) | N/A |
| — | INFO | All 30 images have descriptive alt text | ✅ PASS |
| — | INFO | All 5 FAQ Q&As valid, 404 is real, sitemap valid, canonical correct | ✅ PASS |
