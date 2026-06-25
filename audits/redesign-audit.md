# DePietro's NY Pizzeria — Redesign Audit

**Site:** https://nypizzeriasalisbury.com/
**Address:** 211 Milford St, Salisbury, MD 21804
**Phone:** (410) 546-5558
**Auditor:** generated via the `redesign-existing-projects` Claude skill
**Date:** May 31, 2026

---

## 0. What this site does right (don't break)

- **Real food photography.** Authentic shots of actual menu items, not stock. Keep.
- **Real testimonials.** Brandon M and Jen H are real local customers. Keep — don't replace with fake personas.
- **Specific story.** "Established 2004 ... we simply love to make authentic New York style pizzas." Specific, local, human.
- **Logo has character.** The NYC-skyline illustration above script "Pizzeria" with "SALISBURY, MD" small-caps is a real custom mark. Keep the mark, just give it more breathing room.
- **Honest hours.** They tell you they close around 9/10pm rather than faking 24/7.

---

## 1. Severity: P0 (do these first, biggest visual impact)

### Typography
- **Body font: Nunito.** Generic Google-Fonts default. Reads as a soft, rounded daycare font. Swap to a body sans with character — **Geist Sans**, **Cabinet Grotesk**, **Söhne**, or **Public Sans**. (Avoid Inter/Roboto — same trap as Nunito.)
- **Display font: Antic Didone, in saturated red, used everywhere.** "Salisbury's Best Pizza!", "What are we known for?", section headers — all the same red script-serif. Reads as 1995 family pizzeria template, not 2026 hand-tossed NY institution. Replace with a deliberate display face that fits the brief: **a strong condensed grotesque** (Bricolage Grotesque, Anton, Big Shoulders) for NY-deli energy, OR **a confident contemporary serif** (Recoleta, Migra, Roslindale) for 22-year-old neighborhood spot energy. **Pick one direction. Stop using the red script as the entire display system.**
- **Title Case On Every Header.** "What Are We Known For?" — switch to sentence case. ("What we're known for.")
- **No weight contrast.** Headings and body are both reading at similar weight. Introduce a clear 700+ heading weight against a 400 body for real hierarchy.

### Color
- **Saturated red display text against light background.** OK as an accent, but it's the entire visual identity. Either commit to a deeper, more refined red (think Big Apple Brand, oxblood-into-tomato) OR demote it to a single accent moment and let typography carry the brand.
- **Yellow zigzag section.** The bright yellow band behind the hours table is a jarring color jump in an otherwise gray-and-white page. Per the skill's rules: "Random dark sections in a light mode page ... looks like a copy-paste accident." Either lose the yellow entirely OR tint it down to a warm cream that matches the rest of the page.

### Layout — the AI-template tells
- **Carousel hero.** Multi-slide auto-rotating hero with arrows + dots. Per the skill: "3-card carousel testimonials with dots ... Replace with masonry, embedded social, or a single rotating quote." Same applies here. Pick ONE decisive hero photo (the meatball sub is the strongest) and commit. Carousels suppress engagement and feel template.
- **3 equal cards, image-on-top + bold title + centered body.** "What are we known for?" — pizza / heroes / wings. This is the most generic AI feature-grid layout. Replace with: (1) a tall left-side feature image + 3 stacked items on the right, or (2) zig-zag alternating left/right, or (3) a single hero dish per scroll-section.
- **Cards: border + soft shadow + white background.** Generic. Remove the shadow, keep the photos full-bleed without container chrome, let spacing carry the grouping.
- **Centered everything.** Headings centered, body text centered under photos. Per skill: "Everything centered and symmetrical ... break symmetry with offset margins, mixed aspect ratios, or left-aligned headers."

### Buttons / CTAs
- **One CTA: red "Order Now" pill linked to `tel:`.** It's the only meaningful action on the page. Add a real online-ordering CTA (Toast, Slice, ChowNow — pick one and integrate). Currently you cannot order from this site without making a phone call.
- **No hover or active state on "Order Now."** Add `transform: scale(0.98)` on `:active` plus a subtle darker red on `:hover`.

---

## 2. Severity: P1 (do these next)

- **No mobile-prominent contact strip.** On a phone, the address and phone number should be one tap away at all times. Currently you have to scroll to the bottom for the map and hours table.
- **Hours table buried below the fold.** This is a restaurant. Hours are P0 info. Move to a sticky strip in the nav or directly under the hero.
- **"\* Closing time is approximate" footnote.** Either commit to the real hours or drop the asterisks. The footnote reads as legal hedging on a friendly local site.
- **Testimonials styled as quote-mark cards.** Generic Yelp-card template. Try (a) a single rotating quote with the customer's name large, (b) embed the actual Google reviews, or (c) drop the testimonials section and let the 4.5★ Google rating speak via a small badge.
- **Map iframe with floating address card.** Awkward floating "211 Milford St" card on top of the map. Either kill the card or kill the map; don't show both. Recommend keeping just an `<a href="https://maps.google.com/...">Get directions ↗</a>` text link in a clean visit section.
- **No menu integration.** The "Menu" nav link goes to a separate text page. Many users will bounce. Either bring 3-5 featured dishes on-page (with photo + name + price) or integrate Toast's embedded ordering widget.
- **No favicon / poor social-share preview.** Check for an Open Graph image — likely missing. Add one so when someone shares the link in iMessage it doesn't show a blank rectangle.

---

## 3. Severity: P2 (polish, do last)

- **Photos are inconsistent crop and exposure.** Some are tight on the food, some have wide table backgrounds with bad lighting. Re-shoot or color-correct as a batch.
- **No "Skip to content" link** for keyboard users.
- **Inline iframe map is heavy on mobile.** Consider a static map image with a "View on Google Maps" link.
- **No structured data.** Add `application/ld+json` for `Restaurant` schema. This is what makes Google show your hours/menu/rating in the SERP. Easy win for SEO.
- **No alt text on hero photos** (worth verifying; if missing, fix).

---

## 4. Fix priority — the order I'd actually pitch

1. Typography swap (1 hour) — kills the dated red-script vibe instantly.
2. Hero: kill the carousel, commit to one photo (30 min).
3. Replace 3-card feature grid with zig-zag layout (2 hours).
4. Add real online ordering integration (variable, depends on vendor — Toast Online Ordering is 1-2 hours of setup).
5. Mobile contact strip + sticky CTA (1 hour).
6. Lose the yellow section, tint hours band to warm cream (15 min).
7. Refine logo spacing + drop the floating-card-over-map (30 min).
8. Add `Restaurant` schema + favicon + OG image (45 min).

**Estimated full redesign: 6-8 hours of focused work, no framework migration needed.**

---

## 5. What I'd quote a prospect

For a job like this, the pitch is:
> "Your site loads fine and the photos are real, which puts you ahead of half the pizza shops in Wicomico County. But the design is reading as 'family restaurant template circa 2010,' and you have no way for someone to order without calling. I can rebuild this in the existing WordPress, swap the typography, kill the carousel, add a real online-ordering integration, and ship it in 5 working days for $X. After it's live, your phone rings less and your Toast orders go up."

Numbers depend on whether they already have Toast / Slice / ChowNow. Ask before quoting.
