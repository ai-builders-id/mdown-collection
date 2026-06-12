# Landing Page Brief — Company Profile

> **Use case:** Corporate company profile / "About us" landing for an established firm that wants to read as credible, enduring, and human.
> **One-liner concept:** *An institution with a printing press.* — the page should feel like the front matter of a beautifully set annual report, not a startup splash.

---

## 1. Aesthetic Direction

**Editorial, refined, institutional.** Think *Monocle* magazine meets a century-old trading house. Warm off-white paper, deep ink, a single restrained brass accent. Generous margins. Confident serif headlines set large but never shouting. The differentiator someone remembers: **a running marginal column of small-caps metadata** beside the main text, like a printed ledger — dates, figures, locations — that makes the company feel measured and real.

Avoid: glassmorphism, neon gradients, oversized rounded buttons, stock "team high-fiving" photography. This page earns trust through typographic discipline, not decoration.

**Mood words:** measured · tactile · senior · quietly expensive.

---

## 2. Design Tokens

```css
:root {
  /* surfaces — warm paper */
  --bg:          oklch(0.97 0.012 85);   /* aged paper */
  --surface:     oklch(0.99 0.008 85);   /* card / inset */
  --surface-2:   oklch(0.94 0.015 80);   /* footer / band */

  /* ink */
  --ink:         oklch(0.22 0.02 60);    /* near-black warm */
  --ink-soft:    oklch(0.42 0.02 60);
  --ink-faint:   oklch(0.60 0.015 60);

  /* brass accent — used sparingly */
  --accent:      oklch(0.62 0.10 75);    /* brass / antique gold */
  --accent-2:    oklch(0.45 0.08 70);    /* deep bronze (hover) */
  --accent-ink:  oklch(0.98 0.01 85);

  /* lines */
  --line:        oklch(0.86 0.012 75);
  --line-strong: oklch(0.30 0.02 60);

  --max: 1140px;
  --r-sm: 2px; --r-md: 3px;  /* nearly square — print, not app */
}
```

Texture: a **very subtle paper grain** overlay (SVG `feTurbulence`, opacity ~0.03) over `--bg`. It should be felt, not seen.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display / headlines | **GT Sectra**, *Canela*, or free fallback **Fraunces** (opsz on) | High-contrast serif with character |
| Body | **Newsreader** or **Source Serif 4** | Readable serif, comfortable at 18–19px |
| Marginalia / labels | **GT America Mono** or free **JetBrains Mono**, small-caps | Ledger metadata, eyebrows |

Rules:
- Body at `19px / 1.65`, max line length **66ch** — this is a reading page.
- Headlines use **tight leading** (1.05) and a touch of negative letter-spacing.
- Eyebrows and marginal notes are **uppercase mono, 0.72rem, letter-spacing 0.12em**, color `--ink-faint`.
- Numbers (founded year, revenue, headcount) use **tabular figures**.

```css
--t-display: clamp(2.6rem, 6vw, 5rem);
```

---

## 4. Section Blueprint

### 4.1 Masthead (sticky, slim)
- Left: wordmark in serif, all caps, letter-spaced. Right: thin text nav — `Company · Practice · People · Reports · Contact`.
- A single **hairline rule** under the masthead. No drop shadow. On scroll past hero, background fades from transparent to `--bg` with a 1px bottom border.

### 4.2 Hero — "The statement"
- Left 8 columns: an **eyebrow** (`EST. 1998 — FRANKFURT & SINGAPORE`), then a large serif statement headline:
  > *We build the quiet infrastructure that lets ambitious companies move with certainty.*
- A lead paragraph (2 sentences) below in body serif.
- Right 4 columns: the **marginal ledger** — a vertical list of metadata, each row `LABEL · value` in mono small-caps:
  ```
  FOUNDED       1998
  OFFICES       4 cities
  PEOPLE        210
  MANDATES      640+
  OWNERSHIP     Independent
  ```
- No hero image. The typography *is* the hero.

### 4.3 Proof band
- A full-width band in `--surface-2` with **four figures**, separated by thin vertical rules:
  `€4.2B advised` · `27 years` · `18 sectors` · `94% repeat clients`.
- Figures in display serif, labels in mono below.

### 4.4 What we do — "Practice"
- A 3-column set of practice areas. Each: a small numeral (`01 / 02 / 03`), a serif sub-head, two lines of body, and a quiet "Read more →" in brass.
- Dividers are hairlines, not cards. Keep it airy.

### 4.5 The long-form "About"
- Two-column reading layout: main column body serif, **marginal column** holds pull-quotes and dates. This is the signature section — it should read like an essay with printed margins.
- Include one **drop-cap** on the opening paragraph.

### 4.6 People
- A restrained grid of leadership: square duotone portraits (ink + paper), name in serif, role in mono. Hover lifts portrait from duotone to full warmth over `--dur-mid`.

### 4.7 Selected work / clients
- A logo wall rendered as **monochrome ink** at low opacity; on hover each returns to full opacity. Caption: "Trusted by operators who don't talk about it."

### 4.8 Contact / CTA
- Not a loud button. A single line: *"Start a conversation —"* followed by a brass-underlined email link and the two office addresses set as a printed colophon.

### 4.9 Footer / colophon
- Set like the back of a book: company legal name, registration number, offices, a thin rule, then small mono links. Year set in tabular figures.

---

## 5. Motion & Interaction

- **Page load:** hero eyebrow, headline lines, lead, and ledger rows reveal in sequence — each rises 12px and fades in, staggered 70ms, `--ease-out`, total under 1s. The ledger rows count up their numbers once (founded → 1998) if motion is allowed.
- **Scroll:** practice dividers draw in from left to right (`scaleX` 0→1) as they enter.
- **Hover:** brass links get a thin underline that wipes in from the left. Portraits transition duotone → color.
- Keep all motion **slow and certain** — nothing bounces. This is a senior brand.

---

## 6. Responsive Behavior

- **`< md`:** marginal ledger moves **below** the headline as a 2-column mini-grid. Reading margins collapse; pull-quotes become full-width blockquotes with a left brass rule.
- **`md`:** practice grid 3→2 columns; people grid 2 columns.
- **`lg+`:** full marginal layout returns; max width 1140px, centered.
- Masthead nav collapses into a text "Menu" toggle under `md` — a simple full-height overlay, serif links, no hamburger cliché if avoidable (use the word "Menu").

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Body contrast ≥ 7:1 (ink on paper is easy here — verify the brass links hit 4.5:1).
- [ ] Brass is never the only signal — links are also underlined.
- [ ] Duotone portraits have descriptive `alt` (name + role).
- [ ] Drop-cap is decorative CSS (`::first-letter`), not a separate character that breaks reading order.
- [ ] Number count-up respects `prefers-reduced-motion` (show final value instantly).
- [ ] Marginal ledger reads in correct DOM order for screen readers (place after headline in source, position with grid).
- [ ] One `h1` (the hero statement); practice/people use `h2`/`h3`.
- [ ] Paper grain overlay is `aria-hidden` and `pointer-events: none`.

---

## 8. Build Notes

- **Stack:** static HTML/CSS is enough; add a sprinkle of JS only for the number count-up and scroll reveals (IntersectionObserver).
- **Fonts:** self-host Fraunces + Newsreader + JetBrains Mono from a variable-font CDN; preload Fraunces.
- **Grain:** inline SVG `<filter>` with `feTurbulence baseFrequency="0.9"`, applied via a fixed full-screen `div` at opacity 0.03.
- **Gotcha:** resist adding cards/shadows. The credibility comes from rules, margins, and restraint. If it starts looking like a SaaS page, remove a shadow.
- **Assets:** wordmark (SVG), 4–6 leadership portraits (square, will be duotoned via CSS `filter` or SVG), client logos (monochrome SVG).
