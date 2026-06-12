# Shared Design Tokens & Conventions

Every brief in this folder assumes the conventions below. Read this once; the individual files only override the values, never the structure.

---

## 1. Token Naming

All briefs expose color, type, and layout as CSS custom properties on `:root`. Names are consistent so a developer can switch themes by swapping a single block.

```css
:root {
  /* surfaces */
  --bg:            /* page background */;
  --surface:       /* card / panel background */;
  --surface-2:     /* raised / hover surface */;

  /* text */
  --ink:           /* primary text */;
  --ink-soft:      /* secondary text */;
  --ink-faint:     /* tertiary / captions */;

  /* brand */
  --accent:        /* primary accent */;
  --accent-2:      /* secondary accent / gradient stop */;
  --accent-ink:    /* text that sits on --accent */;

  /* lines */
  --line:          /* default border */;
  --line-strong:   /* emphasized border */;

  /* feedback */
  --ok:            /* success */;
  --warn:          /* warning */;
  --err:           /* error */;
}
```

Prefer `oklch()` for color so lightness and chroma stay perceptually even when you shift hue. Fall back to hex only when a brand color is fixed.

---

## 2. Spacing Scale

A single 4px-based scale. Use the step name in comments; use the value in code.

| Step | Value | Typical use |
| --- | --- | --- |
| `2xs` | 4px | icon gap, tight inline |
| `xs` | 8px | label-to-control |
| `sm` | 12px | inside small components |
| `md` | 16px | default gap |
| `lg` | 24px | between related blocks |
| `xl` | 40px | between subsections |
| `2xl` | 64px | between sections (mobile) |
| `3xl` | 96px | between sections (desktop) |
| `4xl` | 144px | hero breathing room |

```css
:root {
  --s-2xs: 4px;  --s-xs: 8px;   --s-sm: 12px; --s-md: 16px;
  --s-lg: 24px;  --s-xl: 40px;  --s-2xl: 64px;
  --s-3xl: 96px; --s-4xl: 144px;
}
```

---

## 3. Type Scale

A modular scale (ratio ≈ 1.25, "major third"). Each brief picks its own fonts but keeps these step names.

| Step | rem | Use |
| --- | --- | --- |
| `--t-xs` | 0.75 | captions, legal |
| `--t-sm` | 0.875 | secondary body |
| `--t-base` | 1.0 | body |
| `--t-md` | 1.25 | lead paragraph |
| `--t-lg` | 1.563 | h4 / card title |
| `--t-xl` | 1.953 | h3 |
| `--t-2xl` | 2.441 | h2 |
| `--t-3xl` | 3.052 | h1 (mobile hero) |
| `--t-4xl` | 4.5–7.0 | display hero (desktop, clamp it) |

Always size the hero with `clamp()` so it scales fluidly:

```css
--t-display: clamp(2.5rem, 8vw, 7rem);
```

---

## 4. Layout Grid

- **Max content width:** 1200px (text-heavy) or 1440px (visual-heavy). Set per brief.
- **Columns:** 12-col grid on desktop, 4-col on mobile.
- **Gutter:** `--s-lg` (24px) desktop, `--s-md` (16px) mobile.
- **Section rhythm:** vertical padding of `--s-3xl` desktop, `--s-2xl` mobile.

```css
.container { width: min(100% - 2 * var(--s-lg), var(--max, 1200px)); margin-inline: auto; }
.grid      { display: grid; grid-template-columns: repeat(12, 1fr); gap: var(--s-lg); }
```

---

## 5. Motion Timing

Reuse a small set of named easings and durations. Consistency reads as craft.

```css
:root {
  --ease-out:   cubic-bezier(0.16, 1, 0.3, 1);   /* entrances */
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1); /* moves */
  --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1); /* playful pop */

  --dur-fast: 150ms;
  --dur-mid:  300ms;
  --dur-slow: 600ms;
  --dur-hero: 900ms;
}
```

**Page-load choreography:** stagger reveals with `animation-delay` in 60–90ms increments. One orchestrated entrance beats scattered effects.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

## 6. Breakpoints

| Name | Min width | Target |
| --- | --- | --- |
| `sm` | 480px | large phones |
| `md` | 768px | tablets |
| `lg` | 1024px | laptops |
| `xl` | 1280px | desktops |
| `2xl` | 1536px | wide displays |

Design mobile-first. Each brief notes how the hero and nav reflow at `md` and `lg`.

---

## 7. Radius & Shadow

```css
:root {
  --r-sm: 6px; --r-md: 12px; --r-lg: 20px; --r-pill: 999px;

  --shadow-sm: 0 1px 2px rgb(0 0 0 / 0.06);
  --shadow-md: 0 8px 24px -8px rgb(0 0 0 / 0.18);
  --shadow-lg: 0 24px 60px -20px rgb(0 0 0 / 0.30);
}
```

Sharp/brutalist briefs override radius to `0` and use offset hard shadows. Soft/playful briefs raise radius and blur. Don't mix philosophies inside one page.

---

## 8. Accessibility Baseline (applies to every brief)

- Body text contrast ≥ **4.5:1**; large text ≥ **3:1**.
- Every interactive element has a **visible focus ring** (never `outline: none` without a replacement).
- All images have `alt`; decorative images use `alt=""`.
- Color is never the **only** signal (pair with icon, label, or shape).
- Honor `prefers-reduced-motion` (block above).
- Tap targets ≥ **44×44px**.
- Logical heading order: one `h1`, no skipped levels.
- Forms: every input has a `<label>`; errors are announced, not just colored.

---

## 9. Performance Baseline

- Preload the display font; `font-display: swap` on body fonts.
- Hero image: responsive `srcset`, modern format (AVIF/WebP), explicit `width`/`height` to avoid layout shift.
- Defer non-critical JS; CSS-only animation wherever possible.
- Target: LCP < 2.5s, CLS < 0.1, no blocking third-party scripts above the fold.
