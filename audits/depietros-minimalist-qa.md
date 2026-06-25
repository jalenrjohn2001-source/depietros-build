# DePietro's Minimalist Build — Pre-Launch Punchlist

**URL audited:** http://127.0.0.1:4174/
**Build:** `~/Desktop/Jchrona/depietros-skill-test/minimalist-build/`
**Audit date:** June 1, 2026
**Mode:** Internal (this is JJ's own template — fixes go in the source, not just this one build)
**Priority legend:** 🔴 Blocker · 🟠 Major · 🟡 Polish · 🔵 Strategic · 🟢 Opportunities

> **Read this first.** Most of the blockers and majors below are NOT DePietro's-specific bugs. They're baked into the minimalist-ui template you're going to reuse on every venue/small-biz pitch. Fix them at the template level once. Don't patch them per-client.

---

## 🔴 Blockers

### B1. Muted text fails WCAG AA contrast — affects every kicker, every eyebrow, every meta label

**Where:** Every element using `color: var(--muted)` — the nav tagline ("Hand-tossed since 2004"), the hero kicker ("Salisbury, Maryland · est. 2004"), the TONIGHT / ADDRESS labels in the hero meta strip, the "22 years on Milford St" caption, every section kicker, and every category jump-nav link (Pizza · Gourmet · Calzones · ...).

**Why it matters:** `--muted: #787774` against `--canvas: #F7F6F3` hits **4.14:1**. WCAG AA for body-size text requires 4.5:1. Anyone with even mild low-vision struggles. A client running the site through any accessibility scanner gets flagged. And this is JJ's *foundation* color for the minimalist template — every future client inherits the bug until fixed.

**How to fix at the template:** In `styles.css`, change:
```css
--muted: #787774;   /* old, 4.14:1 — fails AA */
```
to:
```css
--muted: #5D5D5B;   /* new, ~5.4:1 — passes AA, still reads as gray */
```
Verify by re-running the contrast detector after the swap. Bonus: also lighten `--hint` if you have hint-tier text anywhere — same audit applies.

**Source of truth:** the minimalist-ui skill's `SKILL.md` literally specifies `#787774` as the muted text color. That spec is wrong. Fix it in your local copy of the skill OR override it in your template defaults.

---

### B2. "Open until 9pm" is hardcoded in the hero meta strip

**Where:** Hero meta dl, line: `<dt>Tonight</dt><dd>Open until 9pm</dd>`.

**Why it matters:** It's currently a Sunday (closed). A visitor sees "Open until 9pm" and shows up at the door. Worse, this stays a lie every time the day rolls over or hours change.

**How to fix:** Tiny JS snippet that reads `Date().getDay()` and renders the right closing time from the hours table. If closed today, show "Closed today · open Tue 11a." Wire this into the template so every restaurant build gets it free.

```js
// hours-strip.js — drop into minimalist template
const hours = {
  0: null, 1: null, 2: '9pm', 3: '9pm', 4: '10pm', 5: '10pm', 6: '10pm'
};
const today = new Date().getDay();
const tonight = hours[today];
document.querySelector('.hero__meta [data-tonight]').textContent =
  tonight ? `Open until ${tonight}` : 'Closed today';
```

Same fix kills the hardcoded "OPEN TODAY" badge on the Thursday row of the visit-section hours table.

---

## 🟠 Major

### M1. No `<meta property="og:image">` — link previews ship as blank rectangles

**Where:** `<head>` of `index.html`.

**Why it matters:** Anyone sharing the URL in iMessage, Slack, Facebook, WhatsApp, or any text app gets an empty preview card. For a restaurant, that's a click killer.

**How to fix:** Add to template `<head>`:
```html
<meta property="og:title" content="DePietro's NY Pizzeria — Salisbury, MD since 2004" />
<meta property="og:description" content="Hand-tossed New York-style pizza on the Eastern Shore." />
<meta property="og:image" content="https://yourdomain.com/og-image.jpg" />
<meta property="og:url" content="https://yourdomain.com/" />
<meta name="twitter:card" content="summary_large_image" />
```
Default OG image: a tightly-cropped 1200×630 hero photo. For DePietro's, the same NY-pizza shot cropped landscape. For the template, document that an OG image is a required client deliverable.

---

### M2. Zero schema markup — no LocalBusiness, no Restaurant, no FAQPage

**Where:** No `<script type="application/ld+json">` exists.

**Why it matters:** Schema is how Google builds the SERP rich-result panel that shows hours, menu, phone, and rating on the right side of search results. Without it, DePietro's is competing for plain blue links. With it, the Google panel is theirs. Same logic for AI overviews (ChatGPT, Claude, Perplexity) — schema is how they "read" the page.

**How to fix:** Bake a LocalBusiness/Restaurant JSON-LD template into the minimalist starter. Required fields per Schema.org/Restaurant: `name`, `address`, `telephone`, `openingHours`, `priceRange`, `servesCuisine`, `geo`. For DePietro's:
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Restaurant",
  "name": "DePietro's NY Pizzeria",
  "address": { "@type": "PostalAddress", "streetAddress": "211 Milford St", "addressLocality": "Salisbury", "addressRegion": "MD", "postalCode": "21804" },
  "telephone": "+1-410-546-5558",
  "servesCuisine": "Italian, American",
  "priceRange": "$",
  "openingHoursSpecification": [
    {"@type": "OpeningHoursSpecification", "dayOfWeek": ["Tuesday","Wednesday"], "opens": "11:00", "closes": "21:00"},
    {"@type": "OpeningHoursSpecification", "dayOfWeek": ["Thursday","Friday","Saturday"], "opens": "11:00", "closes": "22:00"}
  ]
}
</script>
```

---

### M3. No favicon link in `<head>`

**Where:** `<head>` of `index.html`.

**Why it matters:** Browser tabs show a generic globe icon. Bookmark bars look unbranded. Phone home-screen icon (if a customer adds to home) is a generic gray square.

**How to fix:** Use the SVG logo you already scraped. Add:
```html
<link rel="icon" type="image/svg+xml" href="depietros-logo.svg" />
<link rel="apple-touch-icon" href="depietros-touch-icon.png" />
```
For the template, document that favicon assets are a client deliverable.

---

### M4. Mobile touch targets under 44×44px — 9 of 15 interactive elements fail

**Where (on desktop measurement; mobile is the same once viewport-resized):**
- Nav links: Menu (37×40), Visit (29×40), Story (35×40)
- Footer phone link "(410) 546-5558" — 108×20
- Footer address "211 Milford St, Salisbury" — 193×20
- Footer nav: Menu (39×20), Hours (41×20)
- Visit-card "Call (410) 546-5558 ↗" — 166×23

**Why it matters:** Apple HIG and Material both call for 44×44 minimum. Under that, real fingers miss the target. WCAG 2.5.5 also flags it.

**How to fix at the template:** In `styles.css`, add minimum-hit-area rules:
```css
.foot__links a,
.foot__nav a,
.nav__links a,
.visit__quick a {
  min-height: 44px;
  display: inline-flex;
  align-items: center;
}
```
This costs you almost nothing visually and clears the WCAG threshold.

**Note on the mobile viewport test:** The Chrome MCP held the page viewport at 1700px even when I resized the window to 390. So the layout-at-mobile portion of this audit is **incomplete** — please manually open Chrome DevTools device toolbar at iPhone SE (375×812) and confirm the nav doesn't overflow, the hero copy stacks under the photo cleanly, and the menu table is scrollable horizontally rather than crushed. If any of those break, add to the punchlist.

---

### M5. Two photos are placeholders, clearly labeled — but for a pitch send, swap them

**Where:**
- Story section: "friends eating at a wood-topped restaurant table" (Unsplash) — clearly labeled `data-placeholder="true"` with a yellow `Photo placeholder · replace before launch` caption.
- Visit section: Picsum-seeded storefront — same placeholder pattern.

**Why it matters:** The build is honest about the placeholders, which is good. But if JJ sends this to the actual DePietro's owner as a pitch artifact, the owner sees strangers eating at a restaurant that isn't his. That undermines the "we built this with YOUR business in mind" pitch.

**How to fix for pitch:**
- Either request a real interior photo and a real storefront photo from the client BEFORE sending
- Or swap the story photo for a cropped pizza-prep shot (food doesn't have an identity problem)
- Storefront slot: a Google Street View screenshot of the actual address (211 Milford St) is a legitimate stand-in until they send a photo

For the template: build a 1-page "photo intake" form that lists every placeholder slot. Client fills it before kickoff.

---

## 🟡 Polish

### P1. Hero photo is Neapolitan, not New York

**Where:** Hero figure. Photo is the leopard-crust margherita with basil leaves — that's a Neapolitan style.

**Why it matters:** A New York pizzeria's identity is the WIDE foldable slice. The hero photo is currently signaling "wood-fired Italian restaurant," not "NY slice shop." The brand mismatch is subtle but it's there.

**How to fix:** Swap to a wide-slice photo. Verified Unsplash candidates from earlier audit: `1571407970349-bc81e7e96d47` (whole pie + Coke on board, NY-deli energy). Already in your photo-preview folder.

### P2. No `prefers-color-scheme` handling

**Where:** No `@media (prefers-color-scheme: dark) { ... }` block in `styles.css`.

**Why it matters:** ~40% of users have dark-mode enabled at the OS level. They'll see the warm-off-white page anyway. That's fine — minimalist editorial is a *committed* light style — but you should make the commitment explicit by either:
- Adding `color-scheme: light` to the `<html>` element to signal "this is a light-mode-only site, don't try to auto-invert"
- OR build a dark variant (more work, but the editorial mono palette translates cleanly)

For the template, lean toward the `color-scheme: light` declaration as the default.

### P3. Saturated tag pill colors are subtle on the warm-off-white background

**Where:** `.tag--red`, `.tag--blue`, `.tag--green`, `.tag--yellow` pills inside the menu items.

**Why it matters:** They're meant to signal category (HOUSE, LOCAL, GLUTEN-FREE, etc.) but on a warm canvas the pale pink / pale blue read as "is that a tag or a smudge?" Especially on mobile where ambient light washes them out.

**How to fix:** Either bump saturation slightly (chroma +0.02), or rely on the text color for the differentiation and make the background transparent with a 1px colored border instead. The minimalist-ui skill explicitly calls for "highly desaturated, washed-out pastels" — keep them muted but make them work harder by adding a 1px border of the same family.

### P4. `Body` is using `font-family: var(--sans)` which lists Geist first

**Where:** `:root` font stack: `"Geist", "Switzer", "SF Pro Display", "Helvetica Neue", system-ui, sans-serif`.

**Why it matters:** Geist is loaded from Google Fonts at the top of `index.html` — good. But Switzer isn't, and "SF Pro Display" only resolves on Apple devices. On Windows/Android, the fallback chain skips straight to "Helvetica Neue" (which itself doesn't ship on Windows) and then to system. The page will look noticeably different across platforms. Pin Geist as the only intended source and make sure the Google Fonts link is loading the weights you actually use (400, 500, 600, 700).

---

## 🔵 Strategic — for the minimalist template going forward

### S1. Codify the template a11y baseline

The minimalist-ui skill ships with a `--muted` value that fails WCAG AA. That's not on JJ — that's a defect in the skill. Lock these defaults into a small `_minimalist-baseline.css` file you import on every project:

- `--muted` ≥ 5:1 contrast against `--canvas` (use #5D5D5B, not the skill's #787774)
- All `a`, `button`, `[role=button]` get `min-height: 44px` automatically
- `:focus-visible` outline at 2px solid `--ink` on every interactive element
- `color-scheme: light` on `<html>` until/unless dark variant exists
- `prefers-reduced-motion` global guard (you already have this — keep it in the baseline)

### S2. Bake LocalBusiness / Restaurant schema into the template

Every restaurant/venue you build deserves it. Make it a fill-in-the-blank JSON-LD block that you customize per client during build kickoff. Same for FAQPage (build the FAQ section + schema at the same time).

### S3. Make OG image + favicon required client deliverables

Add them to your kickoff intake doc. No OG image, no favicon = no launch. This costs you nothing per project and gives every client a baseline of "looks professional everywhere it's shared."

### S4. Bundle the two-animation vocabulary as a reusable module

You wrote it once, used it on DePietro's. Extract the CSS + JS into a `reveal.js` + `reveal.css` pair you import on every minimalist build. Document the two opt-in classes (`.reveal-img`, `.reveal-h`) and the auto-detection-of-below-the-fold pattern. Future you saves 30 minutes per project.

### S5. The minimalist template doesn't have an online-ordering integration story

Restaurants without online ordering are giving up Friday-night revenue, period. Before you pitch this template to any restaurant, decide your default vendor (Toast vs. ChowNow vs. Slice) and have the integration code ready. Otherwise you're selling a brochure site at restaurant prices.

---

## 🟢 Opportunities — what to sell ALONGSIDE the rebuild

For a restaurant client like DePietro's specifically:

| Opportunity | What it solves | Range |
|---|---|---|
| **Online ordering integration** (Toast / ChowNow / Slice) | Captures order intent at the moment of decision instead of asking customers to call. Restaurants without it leak ~25% of would-be orders on busy nights. | $300–600 setup + $50–100/mo retainer |
| **Google Business Profile audit + photo upload** | The Google panel is more visited than the website itself for most local restaurants. Most have outdated photos, wrong hours, and missing menu links. | $250 one-time |
| **LocalBusiness + Restaurant + FAQPage schema** | Makes the site eligible for the Google rich-result panel and AI overviews. | $400–800 one-time |
| **Photo session — interior + food + storefront** | Kills the placeholder problem permanently. 15-20 real photos = enough for the site + a year of Instagram posts. | $300–500 one-time |
| **Monthly menu / hours refresh retainer** | Restaurants change menus seasonally. Sites that get refreshed by their builder feel cared-for; sites that don't feel abandoned. | $75–150/mo |
| **Email capture + monthly newsletter setup** | "What's on this week's chalkboard" newsletter to the regulars. Builds owned audience for the restaurant. | $400 setup + $75/mo |

---

## Delivery order for the dev (you)

**Session 1 — fix the template (2 hours, fixes propagate to every future minimalist build):**
1. Swap `--muted: #787774` → `#5D5D5B` and re-run contrast scanner [B1]
2. Add `min-height: 44px` to nav/footer/visit-card interactive elements [M4]
3. Add `color-scheme: light` to `<html>` [P2]
4. Pin font stack — drop unverified Switzer / SF Pro Display [P4]

**Session 2 — fix this build (1 hour):**
5. Build the dynamic "Tonight" + "OPEN TODAY" JS module [B2]
6. Add OG image + Twitter card meta tags + 1200×630 image [M1]
7. Add favicon SVG link [M3]
8. Add Restaurant JSON-LD schema [M2]
9. Swap hero photo to a NY-style pizza shot [P1]

**Session 3 — verify on real mobile (30 min):**
10. Open Chrome DevTools device toolbar at iPhone SE (375×812), iPhone Pro (390×844), and mid-Android (412×915). Confirm nothing the auto-audit couldn't reach is broken. [M4 follow-up]
11. Test in Safari iOS for real — the OKLCH support and clip-path animations should work but a real-device sanity check is cheap.

**Total estimated dev time:** 3.5–4 hours, of which ~2 hours pay off on every future client build.

---

## What this audit did NOT cover

- **Mobile viewport (<500px) was not visually tested.** Chrome MCP held the page viewport at 1700px regardless of `resize_window` calls. You need to spot-check this manually in Chrome DevTools device toolbar before launching any minimalist build to a real client.
- **Theme toggle** — there isn't one on this build, by design.
- **Lighthouse / Core Web Vitals** — not run; do a Lighthouse pass after deploying to a real host so the test runs over real network conditions.
- **Sitemap.xml / robots.txt / 404 page** — not applicable to a local static mockup but **required** before deploying any real client site.

---

**— JJ**
