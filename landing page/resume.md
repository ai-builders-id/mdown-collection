# Landing Page Brief — Personal Resume / CV

> **Use case:** A single-page personal resume / portfolio landing for an individual (engineer, designer, PM) who wants to be taken seriously and remembered.
> **One-liner concept:** *A specimen sheet for a person.* — structured like a Swiss typographic poster, precise as a datasheet, warm enough to feel like a person made it.

---

## 1. Aesthetic Direction

**Swiss-grid, monospace, precise.** International Typographic Style: a visible grid, flush-left ragged-right text, generous whitespace, one accent color used like a highlighter. Monospace for structure (labels, dates, coordinates), a clean grotesque for names and headings. The memorable detail: a **visible baseline grid and column rules** that the content snaps to — the page looks engineered, like the person.

Avoid: timeline graphics with glowing dots, skill "percentage bars," a circular avatar with a gradient ring, emoji section headers. Those read as template. Precision reads as competence.

**Mood words:** exact · legible · confident · engineered.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.985 0 0);      /* near-white */
  --surface:     oklch(1 0 0);
  --surface-2:   oklch(0.96 0 0);       /* alternating row */

  --ink:         oklch(0.18 0 0);       /* near-black */
  --ink-soft:    oklch(0.40 0 0);
  --ink-faint:   oklch(0.62 0 0);

  --accent:      oklch(0.65 0.22 28);   /* signal red-orange */
  --accent-2:    oklch(0.55 0.22 28);
  --accent-ink:  oklch(0.99 0 0);

  --line:        oklch(0.88 0 0);
  --line-strong: oklch(0.18 0 0);

  --max: 1080px;
  --r-sm: 0; --r-md: 0;  /* zero radius — Swiss */
}
```

Optional dark variant (toggle): invert surfaces (`--bg` → `oklch(0.16 0 0)`), keep the same signal accent. Offer it; default to light.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Name / display | **Neue Haas Grotesk**, *Suisse Int'l*, or free **Archivo / Inter Tight** | Tight, neutral grotesque |
| Headings | same family, **medium weight**, uppercase for section labels | |
| Body | same grotesque, regular | 16–17px |
| Structure / meta | **Spline Mono** or **IBM Plex Mono** | dates, labels, coordinates, IDs |

Rules:
- Section labels: **mono, uppercase, 0.75rem, letter-spacing 0.1em**, prefixed with a numeral (`01 — EXPERIENCE`).
- The **name** is the only large type: `clamp(3rem, 9vw, 6.5rem)`, weight 600, set flush-left against the grid.
- Everything aligns to a **4px baseline grid**; dates sit in a fixed-width mono column so they form a clean vertical edge.

---

## 4. Section Blueprint

### 4.1 Header / identity block
- Top-left: **Name** (display). Directly under it, a mono one-liner role: `SENIOR FRONTEND ENGINEER · BERLIN · UTC+1`.
- Top-right: a compact **contact register** in mono — email, github, site — each on its own line, right-aligned to the grid edge.
- A `--line-strong` rule across the full width closes the header.

### 4.2 Summary — "the thesis"
- One tight paragraph, max 3 sentences, flush-left at ~60ch. No buzzwords; state what you build and the outcome.
- To the left margin, a mono label: `00 — SUMMARY`.

### 4.3 Experience (the spine of the page)
- A two-column grid per entry:
  - **Left fixed column (mono):** dates `2021—NOW`, then company + location stacked.
  - **Right column:** role title (grotesque medium), then 2–3 bullet lines of **impact, not duties** — each leads with a verb and a number.
- Entries separated by hairlines; alternating rows may use `--surface-2`.
- The accent color **highlights one metric per entry** (e.g. the `−38% p95 latency`), like a marker stroke.

### 4.4 Selected projects
- A 2×2 grid of project cards. Each: mono index (`P-01`), project name, one-line description, stack tags as mono chips, and a `↗` link. Borders only — no shadows, zero radius.

### 4.5 Skills — as a register, not bars
- A dense definition list in three columns: `LANGUAGES`, `SYSTEMS`, `TOOLS`. Values are mono comma-separated lists. No proficiency meters — listing it implies competence; bars imply doubt.

### 4.6 Education & speaking (compact)
- A small two-column ledger: institution/event on the left (mono dates), detail on the right. Keep it to a few lines.

### 4.7 Footer / call to action
- A single strong line: *"Open to senior IC and lead roles."* + an accent-underlined email and a "Download PDF" mono button (outline, square).
- Repeat the contact register. Set the print date in mono.

---

## 5. Motion & Interaction

- **Page load:** the **column grid rules draw in** first (vertical lines scale from top, 400ms), then content fades up in source order, staggered 50ms. Quiet and mechanical.
- **Hover:** the highlighted metric's marker stroke wipes in; project cards shift their border to `--line-strong` and the `↗` nudges up-right.
- **Theme toggle:** light ↔ dark crossfades in 200ms; respects saved preference.
- **Cursor (optional, desktop):** a small mono coordinate readout follows the cursor showing `x,y` — a nerdy, on-brand flourish. Keep it subtle; drop it under `prefers-reduced-motion`.

---

## 6. Responsive Behavior

- **`< md`:** the fixed mono date column moves **above** each experience entry as a single line; the page becomes a clean single column. Contact register stacks under the name.
- **`md`:** projects 2→1 or 2 columns depending on width; skills 3→2 columns.
- **`lg+`:** full two-column experience grid with the vertical mono date edge; max 1080px.
- The visible baseline grid is **desktop-only** decoration; hide it under `md` to avoid clutter.

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Signal accent on white hits ≥ 4.5:1 for text use; if used only as a highlight behind text, ensure the text-on-accent contrast passes too.
- [ ] "Download PDF" is a real link to a real file with `download` + accessible label.
- [ ] Dark-mode toggle persists (localStorage) and has an accessible name + pressed state.
- [ ] Cursor coordinate readout is `aria-hidden` and removed for reduced-motion / touch.
- [ ] Experience entries use a semantic structure (`<article>` per role, `<time datetime>` for dates).
- [ ] One `h1` = the name; sections are `h2`, roles `h3`.
- [ ] Grid rules are decorative (`aria-hidden`), never interfere with reading order.
- [ ] Tap targets (links in the contact register) ≥ 44px on touch.

---

## 8. Build Notes

- **Stack:** plain HTML/CSS; tiny JS for theme toggle + optional cursor readout. No framework needed.
- **Print:** ship a `@media print` stylesheet so `Ctrl+P` produces a clean one-page PDF (hide nav/toggle/cursor, force light, tighten margins). This doubles as the "Download PDF."
- **Fonts:** self-host Archivo + IBM Plex Mono (variable). Preload the display weight.
- **Grid system:** CSS `grid` with named columns; expose the baseline grid via a repeating linear-gradient background on a decorative layer, toggled by a `.show-grid` class.
- **Gotcha:** the temptation is to "decorate." Don't. Every added shape weakens the precision claim. The accent color should appear **fewer than five times** on the whole page.
- **Assets:** none required — this brief is intentionally image-light. An optional small monochrome headshot can sit top-right at 1:1, zero radius.
