# Landing Page Brief — Coming Soon / Waitlist

> **Use case:** A pre-launch holding page that captures emails and builds intrigue before a product, drop, or studio reveal.
> **One-liner concept:** *A single light in the dark.* — minimal, atmospheric, one focal point, one action.

---

## 1. Aesthetic Direction

**Minimal, atmospheric, single-focus.** Near-total darkness with one source of light and one piece of type. No nav, no sections to scroll, no clutter — the entire page is the hero. The memorable detail: a **living atmospheric background** (a slow noise/aurora field, a breathing glow, or drifting particles) behind a single line of type and one email field. It should feel like standing in a dark room watching something about to begin.

Avoid: a generic centered "Coming Soon" + clip-art clock + three social icons crammed at the bottom. The whole point is restraint and atmosphere. One idea, executed beautifully.

**Mood words:** quiet · anticipatory · atmospheric · singular.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.13 0.01 270);    /* near-black void, faint cool cast */
  --surface:     oklch(0.18 0.015 270 / 0.5);
  --surface-2:   oklch(0.22 0.02 270 / 0.6);

  --ink:         oklch(0.95 0.01 270);    /* soft white */
  --ink-soft:    oklch(0.70 0.015 270);
  --ink-faint:   oklch(0.48 0.02 270);

  --accent:      oklch(0.78 0.14 200);    /* cool luminous cyan-blue */
  --accent-2:    oklch(0.70 0.16 320);    /* distant magenta (aurora edge) */
  --accent-ink:  oklch(0.12 0.01 270);

  --line:        oklch(0.30 0.02 270 / 0.6);
  --line-strong: oklch(0.55 0.04 270);

  --max: 640px;   /* deliberately narrow — one column of focus */
  --r-pill: 999px;
}
```

Atmosphere is the whole design: a **slow aurora/noise field** (cyan→magenta, very low saturation), a **central breathing glow** behind the type, fine **grain**, and a soft **vignette** pulling focus to center. Light moves slowly, like weather.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display | **PP Neue Montreal**, *Söhne*, or free **Geist / Instrument Sans** | clean, modern, quietly confident |
| Accent line | a light serif (**Fraunces light**) *or* mono (**Geist Mono**) for the small label | one tasteful contrast |
| Body / field | same sans, regular | the email field + microcopy |

Rules:
- One **large, calm headline** — `clamp(2rem, 6vw, 4rem)`, light-to-regular weight, tight leading, perfectly centered.
- A tiny **label** above in letter-spaced caps or mono (`LAUNCHING SOON · 2026`).
- Everything else is small and faint. The hierarchy is: glow → headline → field → faint footer line.

---

## 4. Section Blueprint

*(There is only one screen. No scrolling required — design it to be complete in a single viewport.)*

### 4.1 The atmosphere (full-bleed background)
- A living aurora/noise field fills the viewport, darkest at the edges (vignette), with a soft luminous glow concentrated behind the center. Optional slow-drifting particles or a single faint horizon line.

### 4.2 The mark (top, small)
- A small centered wordmark or monogram, faint, letter-spaced. That's the only branding.

### 4.3 The statement (center)
- A small label: `SOMETHING IS COMING`. Then one headline line:
  > *We're building the quiet place for focused work.*
- One faint supporting sentence, ≤ 12 words. No more.

### 4.4 The single action — waitlist
- One **pill input** with an inline submit (`your@email →`), glowing border on focus. On submit: the field collapses into a calm success state — *"You're on the list. We'll be in touch."* — with a soft glow pulse. Microcopy beneath: `No spam. One email when we open the doors.`
- Optionally show a faint live count: `1,294 already waiting`.

### 4.5 The faint footer (bottom)
- A single faint row: a couple of text links (`Twitter · Instagram`) and `© 2026`. Letter-spaced, low opacity. Nothing competes with the center.

---

## 5. Motion & Interaction

- **Page load:** the atmosphere is already alive. The glow **breathes in**, then the label → headline → supporting line → email field reveal in sequence (fade + 8px rise, staggered 90ms, `--ease-out`). Unhurried, ~1.2s.
- **Background:** continuous slow aurora drift + a central glow that gently pulses (a long, calm breath ~6s). Particles, if used, drift slowly.
- **Field focus:** border + glow intensify; a faint ring of light.
- **Submit success:** field morphs to the success message with a single satisfying glow pulse and a check; count ticks up by one.
- **Cursor (optional):** a very faint light bloom follows the cursor, like a torch in the dark — subtle, desktop-only.
- Reduced-motion: freeze the aurora at a pleasant frame, stop the breathing/particles/cursor bloom; keep a simple fade-in and the success state.

---

## 6. Responsive Behavior

- **`< md`:** everything is already a single centered column — it just narrows. Headline scales via `clamp`. The email pill stays comfortably tappable (≥ 44px). Footer links stack only if cramped.
- **`md`+:** max content width 640px, vertically centered with `min-height: 100svh`. Atmosphere scales to fill; reduce particle count on small screens for battery/perf.
- Use `svh`/`dvh` units so mobile browser chrome doesn't crop the centered layout.

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Soft-white text on the void ≥ 4.5:1 — verify against the *brightest* part of the moving aurora (add a subtle central scrim if the glow ever washes out the type).
- [ ] Aurora/glow/particles/cursor-bloom layers are `aria-hidden`, `pointer-events: none`, and frozen under `prefers-reduced-motion`.
- [ ] Email field has a real `<label>` (visually hidden is fine), `type="email"`, and inline validation that's announced, not color-only.
- [ ] Success state is announced via `aria-live="polite"`, not conveyed by glow alone.
- [ ] Live waitlist count, if shown, is not essential and degrades gracefully if it fails to load.
- [ ] One `h1` (the statement). Footer links have visible focus rings against the dark.
- [ ] Layout uses `svh`/`dvh` and stays centered without clipping on short mobile viewports.

---

## 8. Build Notes

- **Stack:** a single HTML file + CSS + a little JS is ideal. The atmosphere can be pure CSS (animated radial/conic gradients + grain) for zero dependencies, or a lightweight `<canvas>`/shader if you want richer aurora — but CSS-only keeps it fast and is usually enough.
- **Aurora (CSS):** 2–3 large blurred radial gradients (cyan, magenta) drifting via `transform` on long loops + a grain overlay + vignette. The breathing glow is an opacity/scale keyframe on a central radial.
- **Form:** wire to a real provider (e.g. a serverless function or a form service). Show optimistic success, handle errors gracefully with a quiet inline message.
- **Fonts:** Geist + optional Fraunces-light for the label; preload the display weight.
- **Gotcha:** the temptation is to add "just one more thing" (a countdown, social proof, a feature list). Resist — the power is the singularity. If it needs a second screen, it's a different brief. One light, one line, one action.
- **Assets:** none required beyond a wordmark/monogram SVG. Optional: a single subtle texture for grain.
