# Landing Page Brief — SaaS Product

> **Use case:** Marketing landing for a developer-facing SaaS tool (API, observability, infra, dev tooling).
> **One-liner concept:** *A terminal that learned design.* — the warmth of an old phosphor CRT with the precision of a modern product page.

---

## 1. Aesthetic Direction

**Retro-futuristic, terminal glow.** Deep near-black background, phosphor-green and amber accents, monospace voice, faint scanline and grain. But it is *controlled*: this is not a gimmick CRT skin — it is a clean modern layout that borrows the **confidence and legibility of a great terminal**. The memorable detail: a **live-typing terminal hero** where the product's value prop is "typed" out with a blinking caret, followed by a real, runnable-looking command and output.

Avoid: the purple-gradient-on-white SaaS cliché, floating 3D blobs, generic "feature card with rounded icon" rows that all look identical. Lean into the hacker-warmth instead.

**Mood words:** fast · technical · trustworthy · a little nocturnal.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.16 0.01 160);    /* near-black, faint green cast */
  --surface:     oklch(0.20 0.015 160 / 0.7);
  --surface-2:   oklch(0.24 0.02 160 / 0.8);

  --ink:         oklch(0.92 0.03 150);    /* phosphor white-green */
  --ink-soft:    oklch(0.72 0.04 150);
  --ink-faint:   oklch(0.52 0.04 150);

  --accent:      oklch(0.84 0.18 150);    /* phosphor green */
  --accent-2:    oklch(0.80 0.15 80);     /* amber */
  --accent-ink:  oklch(0.14 0.02 160);

  --line:        oklch(0.34 0.04 160 / 0.6);
  --line-strong: oklch(0.50 0.08 150);

  --ok:   oklch(0.84 0.18 150);
  --warn: oklch(0.82 0.16 80);
  --err:  oklch(0.70 0.20 25);

  --max: 1200px;
  --r-md: 8px;
}
```

Atmosphere: a **subtle scanline overlay** (repeating-linear-gradient, 2px, ~3% opacity), a faint **grain**, and a soft **radial glow** behind the hero terminal (green, blurred, low opacity). Glow should bloom, never blind.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display / headings | **Berkeley Mono**, *PP Fraktion Mono*, or free **JetBrains Mono** (heavy weight) | mono headlines = the whole identity |
| Body | **Commit Mono** or a clean grotesque like **Geist** for longer paragraphs | mix mono structure with readable body |
| Code / terminal | **JetBrains Mono** | actual code blocks |

Rules:
- Headlines are mono, tight, sentence-case (`Ship logs you can actually read.`).
- Body can switch to a humanist sans for paragraphs > 2 lines to stay readable; keep all labels/buttons/stats mono.
- A blinking caret (`▋`) is a recurring motif — in the hero, in section eyebrows.

---

## 4. Section Blueprint

### 4.1 Nav
- Mono wordmark with a small status dot (`● operational`, green). Right: `Docs · Pricing · Changelog · GitHub ★ 4.2k`. A primary outline button: `[ Start free ]` with bracket styling.

### 4.2 Hero — the live terminal
- Left: an **eyebrow** with caret (`~/ observability that doesn't lie ▋`), then a mono headline:
  > *Logs, traces, and metrics — in one query language.*
- Sub-paragraph (sans) + two CTAs: solid green `[ Start free ]` and ghost `[ Read the docs → ]`.
- Right: a **terminal window** chrome (three dots, title `bash — yourapp`). Inside, a typed sequence:
  ```
  $ yourapp query 'errors | last 5m | top status'
  → 1,284 events scanned in 38ms
  502  ████████████  61%
  500  ████          18%
  ...
  ```
  The command types in on load; results stream in line by line.

### 4.3 Logo / trust strip
- "Powering teams at —" then monochrome (phosphor-tinted) logos. Low opacity, hover to full.

### 4.4 Features — as terminal "commands"
- Instead of identical icon cards, each feature is framed as a **command + result**:
  - `> instant search` → "Query 30 days of logs in under 50ms."
  - `> no sampling` → "Keep 100% of events. Pay for what you keep, not what you guess."
  - `> alerts as code` → "Define alerts in version-controlled YAML."
- Layout: alternating left/right, each with a small inline terminal snippet. This breaks the boring uniform-grid problem.

### 4.5 How it works — 3 steps
- `01 install` → `02 instrument` → `03 query`. Mono numerals, a real one-line install command per step in a copyable code box (`npm i @yourapp/sdk`), with a copy button that flashes `copied ✓` in amber.

### 4.6 Metrics band
- Four glowing stats: `38ms median query` · `100% retention` · `99.99% uptime` · `12B events/day`. Stats in big mono, subtle green glow.

### 4.7 Pricing
- Three plans as terminal cards (`free`, `pro`, `scale`). Recommended plan has a green border-glow and a `# recommended` comment line. Feature lists use `+`/`−` mono markers, never checkmark icons alone.

### 4.8 Final CTA
- A wide terminal block: `$ curl -sSL yourapp.sh | sh` with copy button, headline *"You're 30 seconds from your first query."*, and the primary button.

### 4.9 Footer
- Dense mono sitemap, status page link with live dot, `made for people who read logs at 2am`. Year mono.

---

## 5. Motion & Interaction

- **Page load:** terminal hero types the command (≈40ms/char), caret blinks, then output lines stream in staggered 120ms with a faint glow pulse. Headline fades up behind it. One orchestrated moment.
- **Scroll:** feature snippets reveal their "output" line as they enter (typing or fade).
- **Copy buttons:** click → label swaps to `copied ✓` amber for 1.2s.
- **Hover:** buttons get a brighter phosphor glow + 1px translate; logos go full opacity.
- **Caret** blinks at 1s steady everywhere it appears.
- All glow/scanline motion disabled under `prefers-reduced-motion`; the typed hero shows its final state instantly.

---

## 6. Responsive Behavior

- **`< md`:** hero stacks — headline first, terminal below at full width (shorter output). Alternating features become single column, snippet under text.
- **`md`:** pricing 3→1 stacked or 2-up; metrics band 4→2.
- **`lg+`:** side-by-side hero, alternating feature rows, max 1200px.
- Nav collapses to a mono `[ menu ]` toggle → full-screen terminal-style overlay menu.

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Phosphor green text on near-black ≥ 4.5:1 (verify `--ink-soft` and amber especially).
- [ ] Scanline/grain/glow layers are `aria-hidden`, `pointer-events: none`, and removed under reduced-motion.
- [ ] Typed hero has the full text present in the DOM (for SR + no-JS); JS only animates its reveal.
- [ ] Copy buttons announce success (`aria-live="polite"` region), not just a color change.
- [ ] Status dots pair color with text (`● operational`), never color alone.
- [ ] Code blocks are real `<pre><code>`, selectable, with language labels.
- [ ] One `h1`; feature/pricing headings nest correctly.
- [ ] Focus rings are visible against the dark bg (use the amber accent for focus).

---

## 8. Build Notes

- **Stack:** React + Motion (or vanilla + a tiny typing util). The typed terminal is the only real JS dependency; keep it dependency-light.
- **Glow:** layered `radial-gradient` + `filter: blur()` on a pseudo-element behind the terminal; animate opacity, not blur, for perf.
- **Scanlines:** `repeating-linear-gradient(transparent 0 2px, rgba(0,0,0,.06) 2px 3px)` fixed overlay.
- **Fonts:** JetBrains Mono (variable) + Geist; preload the heavy mono weight.
- **Gotcha:** the CRT theme can tip into kitsch. Keep glow opacity low, spacing modern, and copy genuinely useful. If it looks like a Halloween hacker movie, dial the green back 20%.
- **Assets:** wordmark, customer logos (SVG), favicon as a green caret.
