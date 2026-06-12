# Landing Page Brief — Design / Dev Agency

> **Use case:** Landing for a premium design or engineering studio that wants to win high-budget clients.
> **One-liner concept:** *A film, not a flyer.* — cinematic, dark, restrained, expensive. Every frame deliberate.

---

## 1. Aesthetic Direction

**Luxury, cinematic, restrained.** Near-black charcoal, a whisper of warm metal accent, enormous quiet whitespace, slow deliberate motion. The confidence to leave the screen mostly empty. Big editorial type, full-bleed muted imagery treated like film stills. The memorable detail: a **letter-by-letter cinematic hero reveal** over a slowly-zooming dark image, and **horizontal scroll for selected work** that feels like panning across a gallery wall.

Avoid: busy dashboards, rainbow gradients, "we're a fun quirky team!" energy, dense feature grids. Luxury is what you *remove*. Silence and timing signal price.

**Mood words:** cinematic · restrained · expensive · deliberate.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.16 0.005 60);    /* charcoal */
  --surface:     oklch(0.20 0.006 60);
  --surface-2:   oklch(0.12 0.004 60);    /* deepest */

  --ink:         oklch(0.93 0.01 70);     /* warm white */
  --ink-soft:    oklch(0.70 0.01 70);
  --ink-faint:   oklch(0.50 0.01 70);

  --accent:      oklch(0.72 0.06 70);     /* warm champagne metal */
  --accent-2:    oklch(0.60 0.05 65);
  --accent-ink:  oklch(0.14 0.005 60);

  --line:        oklch(0.30 0.006 60);
  --line-strong: oklch(0.45 0.008 60);

  --max: 1440px;
  --r-sm: 2px; --r-md: 4px;  /* barely-there radius */
}
```

Atmosphere: full-bleed **desaturated imagery** with a dark gradient scrim for text legibility; a **fine film grain**; a faint **vignette**. Accent appears rarely — a thin rule, a small label, one underline. The luxury is in the restraint of the palette: essentially monochrome + one metal.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display | **PP Editorial New**, *Canela*, or free **Fraunces (light)** / **Playfair Display** | high-contrast editorial serif, set light |
| Body | **Suisse Int'l**, or free **Inter Tight / Geist** at low weight | quiet, refined sans |
| Labels | letter-spaced uppercase sans, small | `0.18em` tracking, `--ink-faint` |

Rules:
- Display set **large but light-weight** (300–400) — size impresses, weight stays elegant.
- Tight leading on headlines (1.0), wide tracking on small labels.
- Lots of single-line statements with air around them. One idea per screen.

---

## 4. Section Blueprint

### 4.1 Nav (minimal)
- Tiny wordmark left, a single letter-spaced label menu right (`WORK · STUDIO · CONTACT`). No button shouting. Background transparent over the hero, fading to `--bg` on scroll.

### 4.2 Hero — the opening shot
- Full-viewport, a slow **Ken-Burns zoom** on a dark cinematic image (or muted video loop). Over it, a single understated statement revealed letter-by-letter:
  > *We design the things people don't stop using.*
- A small label above (`INDEPENDENT DESIGN STUDIO — EST. 2014`). A quiet scroll cue (`↓` or a thin animated line). No buttons in the hero — let it breathe.

### 4.3 Positioning statement
- A near-empty section: one large editorial line centered with vast margin, one supporting sentence in `--ink-soft`. This is the "establishing shot."

### 4.4 Selected work — horizontal gallery
- **Horizontal-scroll** strip of large project stills, each full-height, with a minimal caption (`PROJECT / CLIENT — DISCIPLINE, YEAR`) that fades in. Panning feels like walking a gallery. A thin progress line tracks position.
- Click a still → it expands (or routes) to a case. Imagery does subtle parallax within each frame.

### 4.5 Capabilities
- Set as a quiet editorial list, not cards: `Brand · Product · Web · Motion · Spatial`. Each with one refined sentence. Hairline dividers. No icons.

### 4.6 Approach / philosophy
- Two or three numbered principles (`I · II · III`), each a large statement + a short paragraph. Roman numerals in champagne. Generous vertical space between.

### 4.7 Clients
- A restrained monochrome logo row at low opacity, perfectly aligned. "Selected partners." Hover lifts opacity slightly.

### 4.8 Recognition (optional)
- A quiet line of awards/press in letter-spaced labels — Awwwards, FWA, etc. — understated, single row.

### 4.9 Contact — the close
- Full-bleed dark, one large line: *"Let's make something worth keeping."* + an accent-underlined email and a single calendar link. Office cities in faint labels. No form clutter — maybe just the email.

### 4.10 Footer
- Spare: wordmark, two cities, a thin rule, minimal links, year. Everything aligned, nothing extra.

---

## 5. Motion & Interaction

- **Page load:** image begins its slow zoom; the hero line reveals **letter-by-letter** (mask-up, ~30ms/char) with the label fading in first. Deliberate, ~1.5s, never rushed.
- **Scroll:** sections **cross-fade and rise slowly** (`--dur-slow`+, `--ease-out`); imagery parallaxes gently. Horizontal work gallery scrolls on vertical input (scroll-jacking done *carefully* with a native fallback) or via a smooth horizontal scroll-snap container.
- **Hover:** captions and underlines fade in; work stills brighten/zoom a touch; cursor optionally becomes a small `VIEW` label over work.
- Everything is **slow and certain**. Nothing bounces, nothing is fast. Timing is the luxury signal.
- Reduced-motion: drop the Ken-Burns zoom, letter reveal (show instantly), and scroll-jacking (fall back to normal scroll); keep gentle fades.

---

## 6. Responsive Behavior

- **`< md`:** horizontal work gallery becomes a **vertical** stack of full-width stills (don't force horizontal scroll on touch unless it's natively smooth). Hero line size drops but keeps its letter reveal. Nav → minimal text "Menu" → full-screen quiet overlay.
- **`md`:** capabilities/principles keep generous spacing; reduce side margins.
- **`lg+`:** full cinematic layout, horizontal gallery, max 1440px with wide margins.
- Maintain the whitespace discipline at every size — don't fill the gaps on mobile.

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Text over imagery sits on a sufficient dark scrim — verify ≥ 4.5:1 against the *lightest* area of each image.
- [ ] Hero video/image has descriptive `alt`/`aria-label`; video loop is muted, `playsinline`, and pausable.
- [ ] Horizontal scroll-jacking has a keyboard + reduced-motion + native-scroll fallback (never trap the user).
- [ ] Grain/vignette layers `aria-hidden`, `pointer-events: none`.
- [ ] Champagne accent used for links also passes contrast; links are underlined, not color-only.
- [ ] Work stills are real links with accessible names (client + project), not bare images.
- [ ] One `h1`; principles/sections nest.
- [ ] Focus rings visible on dark (use champagne or warm-white ring).

---

## 8. Build Notes

- **Stack:** React + Motion (or GSAP) for the letter reveal, parallax, and smooth horizontal gallery. This brief leans on motion craft — budget time for timing.
- **Horizontal gallery:** prefer CSS `scroll-snap-type: x` with smooth momentum; only reach for scroll-jacking if you can do it flawlessly with a fallback.
- **Imagery:** treat all photos consistently — desaturate slightly, add matched grain, unify color temperature so the page feels like one film.
- **Fonts:** Fraunces (light) or Playfair (display) + Inter Tight (body); preload display.
- **Gotcha:** the failure mode is "empty = unfinished." It must read as *intentional* emptiness: perfect alignment, exquisite type, and flawless timing. If a section looks bare, the fix is better typography and spacing, not more content.
- **Assets:** 5–8 high-quality project stills (consistent treatment), one cinematic hero image or short muted video loop, client logos (monochrome SVG), wordmark.
