# Landing Page Brief — Creative Portfolio

> **Use case:** Portfolio landing for a designer, photographer, art director, or studio-of-one who wants to look fearless and current.
> **One-liner concept:** *A poster that scrolls.* — type so large it becomes the image, layout that deliberately breaks the grid.

---

## 1. Aesthetic Direction

**Brutalist, raw, oversized type.** High-contrast black/white with one violent accent. Massive display type that runs to the edges and sometimes clips. Visible structure: exposed grid lines, raw `<hr>`s, file-name captions, "view source" honesty. Asymmetry and overlap on purpose. The memorable detail: a **giant kinetic name/word that reacts to the cursor** (skew, split, or color-invert on hover) — the page feels alive and a little dangerous.

Avoid: tasteful centered hero + 3 neat cards, soft shadows, pastel anything, "Hi, I'm ___ and I love coffee." Brutalism is confident, not cute.

**Mood words:** loud · honest · current · grid-breaking.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.97 0 0);        /* paper white */
  --surface:     oklch(0.97 0 0);
  --surface-2:   oklch(0.14 0 0);        /* inverted blocks */

  --ink:         oklch(0.12 0 0);        /* black */
  --ink-soft:    oklch(0.32 0 0);
  --ink-faint:   oklch(0.55 0 0);

  --accent:      oklch(0.72 0.24 145);   /* acid green — or swap hue per artist */
  --accent-2:    oklch(0.65 0.27 25);    /* alt: hazard orange */
  --accent-ink:  oklch(0.10 0 0);

  --line:        oklch(0.12 0 0);        /* lines are BLACK and thick */
  --line-strong: oklch(0.12 0 0);

  --max: 1600px;   /* near-full-bleed */
  --r-sm: 0; --r-md: 0;  /* zero radius, always */
}
```

No gradients, no blur, no soft shadow. Shadows, if any, are **hard offset** (`6px 6px 0 var(--ink)`). Borders are **2–3px solid black**. The accent is used like spray paint — once or twice, big.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display | **Druk**, *PP Neue Machina*, or free **Anton / Archivo Black / Bebas Neue** | ultra-bold, condensed, screaming |
| Body | **Helvetica Now**, or free **Archivo / Space Mono** for captions | neutral grotesque counterweight |
| Captions / meta | **Space Mono** | file names, indices, footnotes |

Rules:
- Display type is **huge**: `clamp(4rem, 18vw, 16rem)`, line-height **0.85**, can clip at the viewport edge.
- Mix **uppercase condensed** display with tiny mono captions — extreme scale contrast is the point.
- Let words break and wrap intentionally. Hyphenate. Overlap a caption onto an image.

---

## 4. Section Blueprint

### 4.1 Top bar (raw)
- A black 2px rule across the top. Left: mono name + role (`MAYA OKONKWO — ART DIRECTOR / LAGOS`). Right: mono `[ WORK ] [ INFO ] [ CONTACT ]`. No logo graphic — the name *is* the brand.

### 4.2 Hero — the kinetic word
- One enormous word or short phrase filling the viewport: **`SELECTED / WORK`** or the person's name, set in display caps, line-height 0.85, possibly clipping the right edge.
- On cursor move, the word **skews / splits / inverts** subtly. Behind/around it, mono metadata: `2019—2026 · 40+ PROJECTS · AVAILABLE Q3`.
- A small downward mono cue: `scroll ↓`.

### 4.3 Work index (the core)
- A **list**, not a card grid. Each project is a full-width row: large index (`01`), big project title (display), client + year + discipline in mono, and a thumbnail that **reveals on hover** (image slides in from the side, or replaces the row background). Rows separated by thick black rules.
- Hovering a row can invert it to `--surface-2` (black) with accent type. This is the signature interaction.

### 4.4 Featured case (break the grid)
- One project blown up: oversized number `↳ 01`, an image that bleeds past the container, an overlapping caption block rotated slightly or offset. Asymmetric — image on a 7-col span, text overlapping into the margin.

### 4.5 Capabilities / services
- A run-on **marquee** or a dense mono list: `ART DIRECTION — BRANDING — MOTION — TYPE — EDITORIAL —` repeating, scrolling horizontally. Loud and rhythmic.

### 4.6 About / info
- Big display statement, then a tight mono bio column. Include a **raw fact list**: clients, awards, press — set as a monospace table with black rules, like a printout.

### 4.7 Contact — the slam
- A full-bleed inverted block (`--surface-2`): one giant line, *`LET'S MAKE / SOMETHING LOUD`*, with the email as an accent-underlined link sized almost as large. Social links in mono beneath.

### 4.8 Footer
- Minimal, honest: `© 2026 · BUILT WITHOUT A TEMPLATE · LAGOS ↔ ANYWHERE`. Mono. A last thick rule.

---

## 5. Motion & Interaction

- **Page load:** the hero word **slams in** — scales from 1.1→1 with a hard cut, mono metadata flickers in after. Fast, abrupt, confident (no gentle fades here).
- **Cursor:** hero word reacts (skew/split/invert). Optional **custom cursor** — a small accent square or a mono coordinate.
- **Work rows:** hover reveals thumbnail + inverts row; the index number jumps to accent.
- **Marquee:** continuous horizontal scroll, pauses on hover.
- **Scroll:** elements snap in with hard, short transitions (`--dur-fast`), not slow fades. Brutalism is jumpy on purpose — but keep it under control.
- Reduced-motion: kill marquee auto-scroll, cursor reactivity, and slams; reveal content statically.

---

## 6. Responsive Behavior

- **`< md`:** hero word shrinks but still clips intentionally; work index stays a list (it already works as one column). Thumbnails show inline below each title instead of on hover (no hover on touch). Marquee keeps scrolling but slower.
- **`md`:** featured case asymmetry relaxes slightly so text stays readable.
- **`lg+`:** full grid-breaking, overlaps, near-full-bleed 1600px max.
- Custom cursor and hover reveals are **desktop/pointer-only** (`@media (hover: hover)`).

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Despite the chaos: body + caption contrast ≥ 4.5:1; accent-on-black and accent-on-white both verified.
- [ ] Work index is a real semantic `<ul>`/list of links; each project links somewhere and has accessible text (not image-only).
- [ ] Hover-reveal thumbnails have a non-hover fallback on touch and for keyboard focus.
- [ ] Custom cursor never hides the real cursor without a focus-visible fallback; disabled for reduced-motion.
- [ ] Marquee has `aria-hidden` if decorative, or is paused/stoppable if it carries meaning.
- [ ] Clipped display type never clips *essential* words out of view on small screens.
- [ ] One `h1` (hero), project titles `h2`/`h3`.
- [ ] Focus states are bold and obvious (a thick black outline fits the aesthetic — embrace it).

---

## 8. Build Notes

- **Stack:** React + Motion, or vanilla + a little JS for cursor reactivity and marquee. CSS handles most of it.
- **Kinetic word:** transform on `mousemove` mapped to a small skew/translate range; throttle with `requestAnimationFrame`.
- **Hover-invert rows:** swap CSS classes; preload thumbnails so reveal is instant.
- **Fonts:** Anton or Archivo Black (display) + Space Mono (captions). Display font must load fast — preload it; the whole page hangs on that giant word.
- **Gotcha:** brutalism is *designed* roughness, not actual carelessness. Keep the grid intentional, contrast real, and the accent disciplined. One accent color, used 2–3 times, hits harder than five.
- **Assets:** 6–12 project thumbnails (consistent crop), optional case images that bleed, no logo file needed.
