# DePietro's NY Pizzeria — Motion / Interaction Audit

**Site:** https://nypizzeriasalisbury.com/
**Auditor:** generated via the `design-motion-principles` Claude skill (Audit mode)
**Framework:** Emil Kowalski + Jakub Krehel + Jhey Tompkins lenses
**Project type:** Marketing / restaurant landing page → Jakub primary, Jhey secondary, Emil selective

---

## Detected motion on the page

- **15 elements with CSS transitions** (mostly default hover states on links/buttons)
- **1 CSS animation** (likely the carousel auto-rotate)
- **Carousel** at hero — auto-advancing slides with arrow controls and dots
- **Smooth scroll** enabled via `scroll-behavior: smooth` on `<html>`
- **No prefers-reduced-motion handling detected.** This is a hard accessibility failure.

---

## Findings (the three lenses)

### Emil Kowalski — "Should this animate at all?"

| Element | Verdict | Why |
|---|---|---|
| Hero carousel auto-rotate | **Cut.** | Auto-rotating sliders are an attention-stealer on a page where the user is trying to read your story. Per Emil: high-frequency UI surface, no purposeful payoff. The fact that one slide is "Unbeatable Heros and Subs" but another might be pizza means the visitor sees a random food before they see what makes you THIS restaurant. |
| "Order Now" button | **Animate, fast.** | Currently no `:active` state. Add `transform: scale(0.98)` on press, 100ms. This is the only motion on the page that has to feel physical — it's the one click that matters. |
| Nav links | **Already OK.** | Default underline-on-hover is fine. Don't over-design this. |
| Scroll | **Default is fine, drop the smooth-scroll polyfill.** | `scroll-behavior: smooth` adds inertia the user didn't ask for. Most browsers handle this natively now and many users prefer the snap. |

### Jakub Krehel — "Is this subtle and polished enough for production?"

| Element | Verdict |
|---|---|
| Card hover on the "What are we known for?" grid | **Missing — add subtle elevation.** Cards have no hover state. Add `box-shadow: 0 2px 8px rgba(0,0,0,0.04)` on hover, 200ms cubic-bezier(0.16, 1, 0.3, 1). |
| Hero text overlay over photo | **Currently static — could fade in on load.** Subtle `opacity 0 → 1` over 600ms after image loads. NOT a translateY entrance — too dramatic for a restaurant page. |
| Photo entrance animations | **None — and that's actually fine here.** Resist the urge to add scroll-reveals on every photo. They're product shots, not story panels. |

### Jhey Tompkins — "What could this become?"

For a neighborhood pizzeria, the answer is: **don't over-think this**. The Jhey lens is for portfolios and creative sites. The two places playful motion *could* help:

1. **Logo on page-load.** A 400ms draw-in of the NYC-skyline illustration could earn a smile without being precious. Keep it under 500ms, never repeat on subsequent navigations.
2. **Hours table micro-interaction.** When you scroll the hours into view, the "OPEN" / "CLOSED" indicator for *today* could subtly pulse once. Small reward for the visitor checking if they can come tonight.

Both are optional and easy to overdo. If in doubt, ship without them.

---

## Hard fails — must fix

### 1. No `prefers-reduced-motion` handling
The carousel and the smooth scroll both run regardless of OS-level motion settings. This is an accessibility WCAG 2.1 violation. Add:
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.001ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.001ms !important;
    scroll-behavior: auto !important;
  }
}
```

### 2. Carousel auto-advance with no pause-on-hover
Standard carousel anti-pattern. If the visitor is reading "Unbeatable Heros" and the slide changes mid-sentence, that's the carousel actively making the page worse. **Recommendation: kill the carousel entirely, per the Emil lens above.** If you must keep it, at minimum: pause on hover, pause on focus, slow the duration to 8+ seconds per slide.

### 3. Missing `:focus-visible` styles on interactive elements
Quick check via DevTools shows nav links and the Order Now button rely on browser defaults. For keyboard users on Safari this means no visible focus ring. Add explicit focus-visible outlines.

---

## Frequency Gate check

The Order Now CTA is the highest-frequency interaction on this page. Per Emil's principle: "Keyboard-initiated — never animate." A user pressing Tab + Enter to fire the call should NOT trigger a 300ms animation; it should fire instantly. Current state: no animation at all, which is correct. Keep it that way.

---

## What to ship

In priority order:

1. **Add reduced-motion CSS guard** (5 minutes, hard fix)
2. **Kill the auto-rotating carousel** (30 minutes — replace with one decisive hero photo)
3. **Add subtle `:active` scale on Order Now** (5 minutes)
4. **Add visible focus rings on nav + CTAs** (10 minutes)
5. **Add card hover elevation on the feature grid** (10 minutes — but only if you keep the grid; better to redesign per the redesign audit)

**Total motion polish: under an hour. The carousel kill is the single highest-impact change.**

---

## What NOT to ship

- Scroll-driven reveal animations on every section. This is a 6-section restaurant landing; the page is short. Adding stagger reveals everywhere makes a small page feel busier than it is and risks the "Reveal-never-fires-page-blank" failure mode.
- Parallax on the hero photo. It's a meatball sub. The parallax adds nothing.
- Lottie hover animations on the menu cards. Stay quiet.
- Any animation that runs on `scroll` event listener. Use `IntersectionObserver` if you reveal anything at all.
