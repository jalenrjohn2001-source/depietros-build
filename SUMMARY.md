# DePietro's NY Pizzeria — Six-Skill Test Summary

Live test of the 6 ranked design skills against a real prospect site
(https://nypizzeriasalisbury.com/, a 22-year-old family pizzeria in
Salisbury, MD). Goal: prove out what each skill actually does and how
they'd be used in a real client engagement.

---

## What we tested

| # | Skill | Mode | Output type | Status |
|---|---|---|---|---|
| 1 | `impeccable` | craft | Working landing page | ✅ Built (Havana 13 test, see `~/Desktop/Jchrona/havana13-impeccable-test/`) |
| 2 | `redesign-existing-projects` | audit | Markdown punch list | ✅ See `audits/redesign-audit.md` |
| 3 | `minimalist-ui` | craft | Working landing page | ✅ See `minimalist-build/` |
| 4 | `brandkit` | image gen | Brand identity boards | ⚠️ Image-gen skill — see notes below |
| 5 | `design-motion-principles` | audit | Markdown punch list | ✅ See `audits/motion-audit.md` |
| 6 | `full-output-enforcement` | utility | (modifies other skills) | ✅ Active throughout — see notes below |

---

## 1. `impeccable` — already tested on Havana 13

- **Output:** `~/Desktop/Jchrona/havana13-impeccable-test/`
- **Verdict:** Premium output, demanding setup. Worth the friction for new client builds.
- See that folder's screenshots and the prior chat for the full walkthrough.

## 2. `redesign-existing-projects` — actually useful for client pitches

- **Output:** `audits/redesign-audit.md` (~250 lines, real punch list)
- **What it produced:** 9 P0 issues, 7 P1 issues, 5 P2 issues, fix priority order, suggested pitch language.
- **What's strong:**
  - Catches the carousel, the cliché 3-card grid, and the dated red script display font
  - Identifies what NOT to break (real testimonials, real photos, custom logo)
  - Estimates effort (6-8 hours total redesign) and gives you a quote-able pitch
- **What you can do with this output:**
  - Drop it into a Google Doc, change "audit" to "Discovery report," send it as a $500 pre-engagement deliverable
  - Use the P0/P1 split as the basis for a fixed-price quote
  - Or skip the pitch and just hand it to a contractor to implement
- **Verdict:** This is the highest-ROI skill for your business as it exists today.

## 3. `minimalist-ui` — clean editorial look, very different from impeccable

- **Output:** `minimalist-build/` — open `http://127.0.0.1:4174/` while the server runs
- **Aesthetic vs. impeccable's Havana 13:**
  - Same brief class (small restaurant landing) totally different voice
  - **Impeccable's Havana 13:** committed terracotta, big sans display headline with brand-color italic accent, dark oxblood event section
  - **Minimalist DePietro's:** warm off-white, italic editorial serif (Instrument Serif) running the show, ZERO accent color except pastel pill tags, asymmetric bento grid
- **What's strong:**
  - The "22 YEARS ON MILFORD ST" cap as an inset tab on the hero photo is a real editorial move
  - Mono kickers + serif headlines + pastel tag pills = the "Linear / Stripe blog" aesthetic
  - Bento grid is asymmetric (1 large + 2 medium + 2 medium + 1 wide) — not the cliché 3-equal-cards
- **Where it's weaker than impeccable:**
  - The skill is more prescriptive (specific hex colors, specific border-radius) — less room to deviate
  - Pastel tag colors can feel saccharine if overused
  - No commitment to a brand color means the page can read as "stock editorial template" rather than "DePietro's specifically"
- **Verdict:** Default skill for venues, professional services, anything where quiet confidence beats personality.

## 4. `brandkit` — needs native image generation (we don't have it)

- **What it does:** Generates a premium brand-kit board (3×3 grid, 4:3 or 16:10) with:
  - Logo concept
  - Color system
  - Typography spec
  - UI mockups as brand applications
  - Print/identity touchpoints
- **What it CAN'T do here:** Claude Code in your terminal doesn't have native image generation (no `image_gen` tool like Codex has). Same applies to `imagegen-frontend-web`, `imagegen-frontend-mobile`, `gpt-taste`, `image-to-code`, `stitch-design-taste` (the image-gen half of it).
- **Workarounds for DePietro's brand kit:**
  1. **ChatGPT Images / DALL-E 3** — paste the brandkit SKILL.md as the system prompt, then ask for the De Pietro's brand board. Saves the prompt template that the skill provides.
  2. **Midjourney** — use the skill's "Reference Style DNA" section as your prompt structure ("dark charcoal outer canvas, clean grid-based presentation boards, sparse typography...")
  3. **Codex** (OpenAI's coding agent) — has native `image_gen`. The skill is explicitly designed to work there.
- **What a brandkit output WOULD include for De Pietro's** (hypothetical):
  - Logo concept refined: kill the gradient script "Pizzeria," restate the NYC skyline as a simpler geometric mark, single weight wordmark in sans
  - Color system: deep tomato red + warm off-white + charcoal + dough beige
  - Typography spec: one display serif (Roslindale Display Condensed?) + Geist Sans body
  - Mockups: storefront window vinyl, take-out box stamp, pizza box top print, hat embroidery, business card
  - Print: menu card, hours card, delivery zone map
- **Verdict:** Real value, but requires a different tool to actually generate output. Save the SKILL.md as a prompt template.

## 5. `design-motion-principles` audit — produces a real report

- **Output:** `audits/motion-audit.md` (~130 lines, three-designer framework)
- **What it produced:**
  - Detected the existing carousel + 15 transitions + 1 animation
  - Caught the missing `prefers-reduced-motion` (WCAG fail — hard fix)
  - Applied Emil/Jakub/Jhey lenses with weighting (restaurant landing → Jakub primary)
  - Recommended killing the carousel (Emil's "should this animate at all?")
  - Total motion polish: <1 hour of work, biggest impact = carousel kill
- **What's strong:**
  - The per-element verdict table is immediately actionable
  - The "Frequency Gate" framework (rare / occasional / frequent / keyboard) is a real mental tool you can apply forever
  - Calls out what NOT to ship (no parallax, no scroll-reveals everywhere)
- **Verdict:** Pair this with `redesign-existing-projects` on every audit. They produce different findings and complement each other.

## 6. `full-output-enforcement` — utility, no standalone output

- **What it does:** When active, Claude will not output truncated patterns like `// ...rest of code`, `// continue pattern`, or "for brevity I'll skip the rest." It forces complete, runnable code.
- **How you'd use it:**
  - On any "build this whole thing" request where you don't want to manually prompt "continue" 5 times
  - On migrations or refactors where missing sections are bugs
  - Trigger phrases: "complete", "full output", "exhaustive", "no shortcuts"
- **Demo (what default Claude vs. enforced Claude looks like):**
  - **Default:** `function App() { /* main component */ return ( /* ... existing UI ... */ ) }` — useless
  - **Enforced:** full function body, every JSX node, every prop, runnable as-is
- **Banned patterns it blocks:** `// ...`, `// TODO`, `// implement here`, `// similar to above`, `for brevity`, "I'll leave that as an exercise"
- **Verdict:** Quietly active in the background. Probably already saved you a "...what does the rest of the function look like" prompt cycle today.

---

## Side-by-side: impeccable vs. minimalist-ui on the same brief

If you took the De Pietro's brief and ran it through BOTH skills (which we effectively did — Havana 13 with impeccable on Tuesday, DePietro's with minimalist today), here's the comparison:

| Dimension | impeccable | minimalist-ui |
|---|---|---|
| Brand color use | Committed strategy — terracotta = 40% of surface | Zero brand color — pastel pill tags only |
| Display type | Bricolage Grotesque (warm geometric sans) | Instrument Serif italic (editorial) |
| Hero composition | Big bold sans + italic accent + photo on right, full-bleed warmth | Editorial serif headline + photo on right with inset year-tab |
| Section transitions | Dark oxblood "events" section as brand moment | Same warm off-white throughout, depth from typography only |
| Voice | Confident, opinionated, specific | Quiet, restrained, considered |
| Best for | Restaurants with personality, agencies, branded experiences | Venues, professional services, editorial, "established" businesses |

**Honest take:** The minimalist build of DePietro's feels MORE PREMIUM than the impeccable build of Havana 13, but Havana 13 feels MORE OWNED — like it could only be that one restaurant. Pick based on whether the client wants to be seen as "considered and timeless" or "specific and themselves."

---

## What I'd actually pitch to DePietro's

1. **Audit deliverable** ($500 pre-engagement): the two audit MDs in `audits/`, edited into a clean PDF with the existing-site screenshots and the proposed-site screenshots from the minimalist build
2. **Site rebuild quote** ($2,400 - $3,800): WordPress redesign on their existing host, applying the minimalist-ui aesthetic (it fits a 22-year-old neighborhood spot better than impeccable's louder voice), implementing the redesign audit's P0/P1 fixes, integrating Toast Online Ordering ($Y/month vendor fee)
3. **Optional ongoing** ($150/month): monthly menu update on Toast, quarterly content refresh, basic SEO monitoring

Lead-gen play: send them the `redesign-audit.md` cold (cleaned up, branded), no ask. If they reply, you've already done the discovery.
