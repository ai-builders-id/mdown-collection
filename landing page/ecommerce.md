# Landing Page Brief — Product / Store

> **Use case:** A focused product or small store landing — a single hero product or a tight collection (skincare, coffee, ceramics, a gadget).
> **One-liner concept:** *Sunlight on a clay shelf.* — organic, natural, tactile. You can almost feel the material through the screen.

---

## 1. Aesthetic Direction

**Organic, natural, tactile.** Warm earth tones — clay, sand, terracotta, sage — soft natural light, real material textures, gentle imperfection. Rounded-but-grounded shapes, generous product photography with real shadows. The memorable detail: a **hero product that rotates slowly / responds to drag** with photographic lighting, set on a textured clay surface, so it feels like an object you could pick up.

Avoid: harsh tech-store white voids, neon "SALE" badges, cramped grids of tiny products, aggressive countdown-timer pressure. This sells through *desire and calm*, not urgency.

**Mood words:** warm · tactile · honest · grounded.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.94 0.02 70);     /* warm sand */
  --surface:     oklch(0.97 0.015 75);    /* lighter clay */
  --surface-2:   oklch(0.89 0.03 65);     /* terracotta-tinged band */

  --ink:         oklch(0.28 0.03 50);     /* deep umber */
  --ink-soft:    oklch(0.46 0.03 50);
  --ink-faint:   oklch(0.62 0.025 55);

  --accent:      oklch(0.58 0.11 40);     /* terracotta */
  --accent-2:    oklch(0.62 0.07 145);    /* sage */
  --accent-ink:  oklch(0.97 0.015 75);

  --line:        oklch(0.84 0.02 65);
  --line-strong: oklch(0.40 0.03 50);

  --max: 1240px;
  --r-md: 14px; --r-lg: 24px;  /* soft, hand-thrown */
}
```

Atmosphere: subtle **paper/canvas texture** on backgrounds, **soft natural drop shadows** under products (warm, diffuse — like window light), and organic **blob/arch shapes** in accent tones behind product shots. Nothing crisp-digital; everything slightly warm and soft-edged.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display | **Reckless**, *PP Right Serif*, or free **Fraunces (soft optical)** | warm serif with personality |
| Body | **Schibsted Grotesk**, or free **Hanken Grotesk / Karla** | clean humanist sans |
| Labels / price | grotesk, letter-spaced caps for labels; serif for price | tactile contrast |

Rules:
- Display serif for the product name and big statements — warm, a little rounded.
- Body sans for clarity. Prices in serif feel more "crafted" than mono here.
- Labels (`NEW`, `SMALL BATCH`) in letter-spaced caps, terracotta or sage, never red-on-yellow.

---

## 4. Section Blueprint

### 4.1 Nav
- Slim, warm: wordmark (serif) left, `Shop · Story · Journal` center, a small **cart** with count + a `Shop now` text-link right. Background `--bg`, hairline under on scroll.

### 4.2 Hero — the object
- Split or centered: left, a serif headline + one warm sentence + price + `Add to bag` (terracotta button) and `Free shipping over $X` in faint text. Right (or center), the **hero product** on a clay surface with a real soft shadow, set inside a soft arch shape, slowly rotating or drag-to-spin. A small material label floats nearby (`Stoneware · 320ml · Glazed by hand`).
- Sub-cue: small ingredient/material icons or a one-line provenance.

### 4.3 Trust / values strip
- A warm band with 3–4 honest values: `Small batch · Plastic-free · 60-day returns · Made in [place]`. Simple line icons, no loud badges.

### 4.4 Story / why it's different
- A two-column editorial: a lifestyle photo (warm, natural light) beside a short paragraph about the making. Include a pull-quote in serif. This builds desire through narrative.

### 4.5 The details (features as materials)
- 3 blocks, each pairing a **macro close-up photo** with a short benefit (`The glaze` / `The weight` / `The ritual`). Tactile, sensory copy — describe how it *feels*, not specs alone.

### 4.6 Collection / variants
- If a small range: a soft grid of products on clay pedestals, each with name, price (serif), and a color/variant swatch row (real material swatches). Hover gently lifts the product and deepens its shadow.

### 4.7 Reviews
- A few warm testimonials with small round customer photos and a serif star rating, on `--surface` cards with soft shadows. Real, specific quotes ("It's the first mug I actually reach for").

### 4.8 Journal teaser (optional)
- Two editorial cards linking to "how to care for it" / "the makers" — content that reinforces craft.

### 4.9 Final CTA
- A warm full-width terracotta-tinted panel: the product, a serif line *"Made to be used every day."*, price, and `Add to bag`. Calm, not pushy.

### 4.10 Footer
- Earthy: wordmark, a short brand line, link columns, newsletter signup (`Slow emails, real ones.`), social, payment icons, year.

---

## 5. Motion & Interaction

- **Page load:** headline + product rise gently; the product's shadow grows in as it "lands." Soft, warm, ~80ms stagger, `--ease-out`. The product begins a slow idle rotation.
- **Product:** slow auto-rotate + **drag-to-spin** (release to ease back to idle); shadow shifts with rotation to sell the lighting.
- **Hover:** products lift with deepening soft shadows; swatches enlarge slightly; buttons warm/darken.
- **Add to bag:** the button does a satisfying fill + a small `Added ✓`; the cart count bumps with a tiny spring; optionally the product image "flies" toward the cart.
- **Scroll:** macro photos parallax slightly; arch shapes drift slowly.
- Reduced-motion: stop auto-rotate (keep drag), drop fly-to-cart and drift; keep gentle fades and the cart-count update.

---

## 6. Responsive Behavior

- **`< md`:** hero stacks (product on top in its arch, copy + price + button below). Collection grid → single column or 2-up. Detail blocks stack (photo over text). Sticky **add-to-bag bar** appears at the bottom on scroll for conversion.
- **`md`:** collection 2-up; detail blocks alternate sides.
- **`lg+`:** full split hero, multi-column collection, max 1240px.
- Drag-to-spin works with touch; ensure it doesn't fight vertical page scroll (lock to horizontal drag intent).

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Warm-on-warm text still ≥ 4.5:1 — terracotta text on sand is the trap; verify and darken.
- [ ] Product spinner has a static `alt` image + the value/material stated in text (don't lock info in the 3D view).
- [ ] `Add to bag` announces success via `aria-live`, not color/icon alone; cart count is readable text.
- [ ] Variant swatches have accessible names ("Sage", "Clay") and a selected state beyond color (ring + label).
- [ ] Drag-to-spin has a keyboard alternative or is purely enhancement; it never traps focus or scroll.
- [ ] Texture/blob/arch layers `aria-hidden`, `pointer-events: none`.
- [ ] Prices and reviews use semantic markup; star rating has a text equivalent ("4.8 out of 5").
- [ ] One `h1` (product/brand); sections nest. Tap targets ≥ 44px.

---

## 8. Build Notes

- **Stack:** React or vanilla. Product spin can be a **sprite sequence** (pre-rendered frames swapped on drag — cheap, photographic, no WebGL needed) or a `<model-viewer>`/Three.js view if you have a 3D asset. Sprite sequence is recommended for fidelity + simplicity.
- **Shadows:** warm, diffuse, multi-layer (`filter: blur` on a tinted ellipse under the product) that shifts with rotation frame.
- **Texture:** subtle canvas/paper PNG or SVG noise at low opacity over `--bg`; arches via CSS `border-radius` tricks or SVG masks.
- **Fonts:** Fraunces (display) + Hanken Grotesk (body); preload display.
- **Gotcha:** avoid sterile e-commerce coldness *and* avoid clutter. Let products breathe, keep one warm accent + one sage support, and write **sensory** copy. No fake urgency timers — calm sells craft.
- **Assets:** product spin frames (24–36 frames) or 3D model, 3–4 macro detail photos, 1–2 lifestyle photos, variant swatch images, customer avatars, payment icons.
