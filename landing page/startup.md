# Landing Page Brief — Startup / Pre-Seed

> **Use case:** Launch landing for an early-stage startup raising attention (and maybe a round). Needs to feel inevitable, modern, and warm — momentum without arrogance.
> **One-liner concept:** *Sunrise on a clean desk.* — soft aurora light, optimistic, a sense that the future already started here.

---

## 1. Aesthetic Direction

**Optimistic, gradient-mesh, soft.** A light, airy page with a **living gradient-mesh background** (peach → rose → periwinkle) that drifts slowly, like dawn. Rounded but not childish. Crisp dark text floats over the soft atmosphere. The memorable detail: the **animated mesh gradient hero** combined with a single floating product mockup that tilts gently — it should feel calm, expensive, and alive.

This is the one brief that may flirt with "modern SaaS soft," so the discipline is: **warm sunrise hues, never the purple-on-white cliché**, real product imagery, and copy that says something specific.

**Mood words:** hopeful · effortless · warm · inevitable.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.98 0.012 60);    /* warm white */
  --surface:     oklch(1 0 0 / 0.7);      /* frosted card */
  --surface-2:   oklch(0.99 0.01 60);

  --ink:         oklch(0.24 0.02 280);    /* deep indigo-ink */
  --ink-soft:    oklch(0.44 0.03 280);
  --ink-faint:   oklch(0.60 0.02 280);

  /* mesh stops */
  --accent:      oklch(0.78 0.13 35);     /* peach */
  --accent-2:    oklch(0.74 0.13 350);    /* rose */
  --mesh-3:      oklch(0.80 0.10 270);    /* periwinkle */
  --mesh-4:      oklch(0.88 0.09 90);     /* soft gold */
  --accent-ink:  oklch(0.20 0.02 280);

  --line:        oklch(0.90 0.01 280);
  --line-strong: oklch(0.80 0.02 280);

  --max: 1200px;
  --r-md: 16px; --r-lg: 28px;  /* soft, generous radius */
}
```

Atmosphere: a **mesh gradient** built from 3–4 large radial blobs (`--accent`, `--accent-2`, `--mesh-3`, `--mesh-4`) at low saturation, blurred heavily, drifting on a 20s+ loop. Add a **faint grain** on top so the gradient never looks like cheap CSS. Frosted-glass cards (`backdrop-filter: blur`) over the mesh.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display / headings | **Cabinet Grotesk**, *PP Mori*, or free **Bricolage Grotesque** | friendly geometric with character |
| Body | **Switzer**, or free **Hanken Grotesk / Manrope** | clean, warm, readable |
| Accent numerals | display font, tabular | for stats |

Rules:
- Hero headline `clamp(2.6rem, 7vw, 5.5rem)`, weight 600, **tight leading 1.0**, with one phrase in a **gradient-text** treatment (clip the mesh hues into the text — used exactly once).
- Body 18px / 1.6, generous paragraph spacing.
- Buttons: medium weight, pill radius, never all-caps.

---

## 4. Section Blueprint

### 4.1 Nav (floating pill)
- A frosted **pill-shaped nav** floating with margin from the top: logo left, links center (`Product · Pricing · About`), `Get early access` button right (solid, accent). Subtle shadow, blur backdrop.

### 4.2 Hero
- Centered or left-aligned: a small pill **badge** (`◍ Now in private beta`), then the headline with one gradient-clipped phrase:
  > *The fastest way to turn customer chats into product decisions.*
- Sub-line (one specific sentence, names the user + outcome). Two CTAs: solid `Get early access` + ghost `See how it works ↓`.
- Below or beside: a **floating product mockup** (app screenshot) on a frosted card that tilts ±3° on cursor (parallax), with a soft drop shadow and a tiny floating UI detail breaking its frame.

### 4.3 Social proof (gentle)
- "Backed by operators from —" + a soft row of logos, or "Loved by 1,200+ early users." One believable number with a tiny avatar stack.

### 4.4 Problem → promise
- Two-part: a quiet statement of the pain ("Feedback lives in 6 tools and dies in none"), then the promise ("One inbox. Auto-themed. Always current."). Big type, lots of air.

### 4.5 Features (frosted cards, varied)
- 3 feature cards on frosted glass, but **vary their size** (one wide, two narrow) to avoid the uniform-grid look. Each: a small custom line-icon, a short title, one sentence, and a micro product detail (a real mini-screenshot or animated chip).

### 4.6 How it works
- Three soft steps with connecting curved dotted lines: `Connect → Cluster → Decide`. Each step a frosted card with an illustration.

### 4.7 Metrics / momentum
- A band with 3–4 stats in display numerals over the mesh: `1,200 users · 38 teams · 4.9★ · 3 weeks to PMF`. Honest, specific.

### 4.8 Testimonial
- One large quote on a frosted card, founder photo + name + company. Quote marks in gradient. Keep it singular and strong, not a wall of three.

### 4.9 Final CTA
- A big frosted panel centered over the brightest part of the mesh: *"Be early. It pays."* + email capture (single input + button inline) + "No spam. One launch email." in faint text.

### 4.10 Footer
- Light, airy: logo, short tagline, columns of links, social icons, and a final line *"Made by a small team in [city] with too much coffee and a clear plan."*

---

## 5. Motion & Interaction

- **Page load:** mesh is already drifting; badge → headline (with gradient phrase wiping in its color) → sub → CTAs → mockup rise and settle, staggered 80ms, `--ease-out`. The mockup does a gentle final settle (`--ease-spring`, tiny).
- **Mesh:** continuous slow drift of the blob positions (transform translate on a long loop). Never fast — it's ambient.
- **Mockup:** parallax tilt toward cursor (max 3°), floating detail bobs subtly.
- **Buttons:** pill buttons lift + brighten on hover; email input glows its border on focus.
- **Scroll:** frosted cards fade-and-rise on entry; the curved connector lines draw in.
- Reduced-motion: freeze the mesh at a pleasant frame, drop parallax and bob, keep simple fades.

---

## 6. Responsive Behavior

- **`< md`:** hero stacks (headline → mockup full width below). Floating nav pill becomes a compact bar with a menu toggle → frosted sheet menu. Varied feature cards stack to a single column (the "wide" one stays wide).
- **`md`:** feature layout 3→2 (one full-width + one row of two). Metrics 4→2.
- **`lg+`:** full asymmetric feature grid, side-by-side hero, max 1200px.
- Mesh scales to viewport; reduce blob count on small screens for perf.

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Dark ink on the lightest mesh areas ≥ 4.5:1 — **test against the brightest blob**, not just the base bg. Add a subtle white scrim behind text over busy mesh areas if needed.
- [ ] Gradient-clipped text has a solid-color fallback and is real text (not an image) for SR.
- [ ] Frosted cards keep ≥ 4.5:1 text contrast even where `backdrop-filter` is unsupported (provide a solid fallback bg).
- [ ] Mesh + grain layers are `aria-hidden`, `pointer-events: none`, frozen under reduced-motion.
- [ ] Email capture: real `<label>`, `type="email"`, inline validation announced, success state not color-only.
- [ ] Parallax mockup has a descriptive `alt`.
- [ ] One `h1`; sections nest cleanly.
- [ ] Focus rings visible over the soft bg (use `--ink` ring, not a pale one).

---

## 8. Build Notes

- **Stack:** React + Motion (mesh drift + parallax) or vanilla with CSS `@keyframes` for the mesh and a tiny JS tilt. Keep JS light.
- **Mesh:** 3–4 absolutely-positioned radial-gradient divs, heavily blurred (`filter: blur(80px)`), animated via `transform` on long loops. Or a single `<canvas>` mesh if you want richer motion — but CSS is enough and cheaper.
- **Grain:** SVG `feTurbulence` overlay at ~4% opacity — essential to avoid the "flat CSS gradient" look.
- **Fonts:** Bricolage Grotesque (display) + Hanken Grotesk (body); preload display.
- **Gotcha:** the failure mode is "generic soft SaaS." Defenses: warm sunrise palette (not purple), grain over the gradient, *specific* copy with real numbers, and varied (not uniform) feature cards.
- **Assets:** product screenshot(s) for the mockup, founder/customer photo, investor/customer logos.
