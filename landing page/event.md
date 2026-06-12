# Landing Page Brief — Conference / Event

> **Use case:** Landing for a conference, summit, gala, or festival — selling tickets and building anticipation.
> **One-liner concept:** *A ticket to a grand hall.* — art deco geometry, gold on midnight, the ceremony of a printed invitation made interactive.

---

## 1. Aesthetic Direction

**Art deco, geometric, gold.** Deep midnight navy/black, antique-gold linework, symmetrical geometric ornament (chevrons, sunbursts, stepped frames), tall elegant type. The feeling of a 1930s theatre program reimagined for screen. The memorable detail: a **geometric gold "marquee frame"** around the hero with a sunburst motif, and a **live countdown** rendered in deco numerals that feels like a grand clock.

Avoid: generic event templates with a big photo + speaker grid + Eventbrite button. This should feel *ceremonial* — like you were invited.

**Mood words:** grand · ceremonial · luminous · symmetrical.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.17 0.03 265);    /* midnight navy */
  --surface:     oklch(0.22 0.035 265);
  --surface-2:   oklch(0.13 0.025 265);   /* deeper panel */

  --ink:         oklch(0.94 0.02 90);     /* warm ivory */
  --ink-soft:    oklch(0.78 0.03 90);
  --ink-faint:   oklch(0.60 0.03 90);

  --accent:      oklch(0.80 0.13 85);     /* antique gold */
  --accent-2:    oklch(0.68 0.12 80);     /* deep gold (lines) */
  --accent-ink:  oklch(0.15 0.02 265);

  --line:        oklch(0.68 0.12 80 / 0.5);  /* gold hairlines */
  --line-strong: oklch(0.80 0.13 85);

  --max: 1120px;
  --r-sm: 0; --r-md: 2px;  /* sharp, architectural */
}
```

Atmosphere: **gold geometric ornament** as SVG (sunbursts, chevron borders, stepped corners), a subtle **vignette** darkening the edges, and an optional faint **gold dust** particle shimmer. Gold should read as foil/metal — use a subtle linear-gradient on gold elements (lighter top, deeper bottom) rather than flat fill.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display | **Marcellus**, *Cormorant*, or **Playfair Display** | tall, elegant, high-contrast |
| Deco accents / numerals | **Poiret One** or letter-spaced **Marcellus** caps | for countdown + deco labels |
| Body | **Cormorant Garamond** or a clean serif like **Spectral** | refined, formal |
| Labels | letter-spaced uppercase serif/sans | `0.2em` tracking |

Rules:
- Big display in **tall caps with wide letter-spacing** (`0.08em`) — deco loves airy capitals.
- Use **gold hairline rules with a center diamond** (`—◆—`) as section dividers.
- Numerals (date, countdown, edition number) are a feature — set them large and ornamented.

---

## 4. Section Blueprint

### 4.1 Top ribbon
- A slim gold-hairline-bordered ribbon: edition + city + date centered (`THE SUMMIT · No. VII · MILAN · 14–16 NOV 2026`), small deco ornaments flanking.

### 4.2 Hero — the invitation
- Centered, symmetrical, inside a **gold geometric frame** with a sunburst above the title.
- Eyebrow: `YOU ARE CORDIALLY INVITED TO`. Title: the event name in towering display caps. Sub: the theme line in italic serif.
- Beneath: **the countdown** — `DD : HH : MM : SS` in deco numerals within stepped gold frames, labeled below.
- A single ceremonial CTA: `RESERVE YOUR SEAT` — gold button with a thin inner border (double-line deco style).

### 4.3 The promise / about
- A short, elegant paragraph centered at ~55ch, framed by gold diamond dividers. States what the gathering is and why it matters.

### 4.4 Programme highlights
- Three or four pillars (`KEYNOTES · MASTERCLASSES · THE GALA · NIGHT SESSIONS`), each in a stepped-corner card with a deco icon, title in caps, one line of detail. Symmetrical layout, gold linework.

### 4.5 Speakers — the bill
- Presented like a **theatre playbill**: portraits in arched/stepped gold frames, name in display caps, role in letter-spaced labels. A "& many more" deco flourish closes it. Hover: portrait warms, gold frame glints.

### 4.6 Schedule
- A refined agenda table with gold hairlines: time column (deco numerals), session, room. Tabs for Day I / II / III as deco pill toggles. Keep it legible and grand, not spreadsheet-y.

### 4.7 Venue
- The hall/city presented with one atmospheric image inside a gold frame + an address set as an engraved colophon. A small map link.

### 4.8 Tickets
- Two or three tiers as **deco ticket stubs** (perforated edge, stepped corners): `STANDARD · PATRON · BENEFACTOR`. Patron highlighted with a brighter gold frame and a `◆ MOST CHOSEN ◆` banner. Price in display numerals; perks as a gold-bulleted list.

### 4.9 Final CTA
- A grand centered panel within a full sunburst frame: *"The doors open once a year."* + countdown echo + `RESERVE YOUR SEAT`.

### 4.10 Footer
- Symmetrical: event mark centered, gold divider, social + contact in letter-spaced caps, year in deco numerals, a closing ornament.

---

## 5. Motion & Interaction

- **Page load:** the gold hero frame **draws itself** (SVG stroke-dashoffset animation, lines tracing in), the sunburst rays fan out, then title caps fade up letter-by-letter (small stagger). Ceremonial, ~1.2s.
- **Countdown:** ticks every second; the changing digit does a subtle flip/fade. It's the page's heartbeat.
- **Hover:** gold elements get a soft **glint sweep** (a light band crossing the gold gradient); ticket stubs lift with a hard-ish shadow; speaker frames brighten.
- **Scroll:** diamond dividers expand from center; section frames trace in.
- **Gold dust:** very slow drifting particles behind the hero (optional, low count).
- Reduced-motion: frames appear drawn, countdown still ticks (essential info) but without flip animation, no dust/glint.

---

## 6. Responsive Behavior

- **`< md`:** hero frame simplifies (thinner border, smaller sunburst); countdown wraps to 2×2. Speaker playbill → single column of arched portraits. Ticket stubs stack.
- **`md`:** programme 4→2; speakers 2-up.
- **`lg+`:** full symmetrical deco layout, max 1120px, generous side ornament.
- Keep symmetry at every breakpoint — deco depends on it. Center, don't left-align, on mobile.

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Gold on midnight ≥ 4.5:1 for body text (gold can be low-contrast — verify `--ink-soft` and especially gold-on-navy small text; brighten if needed).
- [ ] Countdown is real, accessible text with an `aria-live` region announcing remaining time sensibly (don't spam SR every second — announce at sane intervals or provide the target date as text).
- [ ] Decorative SVG ornament/frames/dust are `aria-hidden`, `pointer-events: none`.
- [ ] Ticket tiers convey "most chosen" with text, not gold alone.
- [ ] Speaker portraits have name+role `alt`; arched frames are CSS/SVG masks, not baked into the photo's meaning.
- [ ] Schedule table uses proper `<table>` semantics with `<th scope>`.
- [ ] One `h1` (event name); sections nest.
- [ ] Focus rings visible on dark (use bright gold `--line-strong`).

---

## 8. Build Notes

- **Stack:** HTML/CSS + a little JS for the countdown and SVG draw-in. The deco ornament is the main craft cost.
- **Ornament:** build the frames, sunburst, and dividers as **inline SVG** so you can animate `stroke-dashoffset` and recolor with `currentColor`/CSS vars. Reuse a small set of motifs.
- **Gold look:** apply a subtle vertical `linear-gradient` (lighter→deeper gold) on gold fills + a soft inner glow; the glint sweep is a masked moving highlight.
- **Fonts:** Marcellus / Playfair Display (display) + Cormorant Garamond (body); preload display.
- **Gotcha:** deco tips into "wedding template" if the geometry is sloppy. Keep ornament **precise and symmetrical**, gold **disciplined**, and type **tall with real letter-spacing**. Less ornament, perfectly placed, beats more.
- **Assets:** speaker portraits (consistent crop, will be framed), one venue image, event wordmark, SVG deco kit (sunburst, chevron border, diamond divider, stepped corner).
