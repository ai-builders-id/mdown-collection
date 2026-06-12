# Landing Page Brief — Nonprofit / Cause

> **Use case:** Landing for a nonprofit, NGO, or social cause — building trust, telling a human story, and driving donations or sign-ups.
> **One-liner concept:** *A window, not a billboard.* — humanist, hopeful, photography-led. Real people, real light, honest numbers.

---

## 1. Aesthetic Direction

**Humanist, hopeful, photographic.** Open and bright, led by genuine documentary-style photography of the people served. A clear, warm sky-and-earth palette, plenty of breathing room, and typography that gets out of the way of faces. The memorable detail: a **full-bleed human photograph as the hero with a single honest sentence**, and a **transparent impact ledger** ("here's exactly where $50 goes") that builds trust through clarity, not guilt.

Avoid: dark guilt-trip imagery, aggressive flashing "DONATE NOW" modals, stock smiling-volunteers cliché, vague claims with no numbers. Hope and transparency convert better than shame.

**Mood words:** human · hopeful · transparent · grounded.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.98 0.012 230);   /* bright sky-white */
  --surface:     oklch(1 0 0);
  --surface-2:   oklch(0.95 0.02 230);    /* soft sky band */

  --ink:         oklch(0.24 0.02 250);    /* deep slate */
  --ink-soft:    oklch(0.44 0.03 250);
  --ink-faint:   oklch(0.60 0.02 250);

  --accent:      oklch(0.62 0.15 230);    /* hopeful sky blue */
  --accent-2:    oklch(0.70 0.15 50);     /* warm earth amber (CTA) */
  --accent-ink:  oklch(0.99 0.01 230);

  --line:        oklch(0.90 0.015 250);
  --line-strong: oklch(0.40 0.03 250);

  --max: 1200px;
  --r-md: 12px; --r-lg: 20px;
}
```

Atmosphere: photography carries the visual weight; keep UI clean and bright. Use **warm amber for the donate action** (action = warmth) and **sky blue for trust/links**. Soft shadows, rounded-but-serious corners. A subtle light gradient behind sections, never heavy.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display | **Tiempos**, *GT Super*, or free **Fraunces / Source Serif 4** | warm, trustworthy serif |
| Body | **Inter Tight**, or free **Hanken Grotesk / Public Sans** | clear, neutral, accessible |
| Numbers / impact | display serif, tabular | for the impact figures |

Rules:
- Headlines in a warm serif — feels human and institutional, not corporate.
- Body in a highly legible sans at 18px / 1.65 (accessibility matters most here).
- Impact numbers set **large in serif with tabular figures** — they are the proof.

---

## 4. Section Blueprint

### 4.1 Nav
- Clean: logo left, `Our work · Impact · Stories · About` center, a warm `Donate` button (amber) right. Simple, sticky, hairline on scroll.

### 4.2 Hero — a face and a sentence
- Full-bleed documentary photograph (real person, natural light, dignity — never pity) with a soft gradient scrim for legibility. Over it: one honest serif sentence:
  > *Clean water shouldn't depend on where you were born.*
- A short sub-line and two CTAs: `Donate` (amber, solid) + `See our impact ↓` (ghost). A small trust line: `92% of every dollar reaches the field.`

### 4.3 The problem, stated plainly
- A short, specific paragraph with one big honest statistic (`771 million people lack clean water`). No exaggeration — cite the source in faint text. Clarity builds trust.

### 4.4 What we do
- 3 clear pillars (`Build wells · Train communities · Maintain forever`), each with a small line icon, a photo, and one sentence. Emphasize the *forever* — sustainability, not one-off charity.

### 4.5 The impact ledger (signature)
- A **transparent breakdown**: "Here's where your $50 goes" as a clean horizontal bar / itemized list (`$38 materials · $7 local labor · $5 maintenance fund`). Honesty as a feature. Include a link to financials.

### 4.6 A story (one, told well)
- A single named person's story with a photo and a short first-person quote — concrete, specific, hopeful ("Before, I walked 3 hours. Now my daughter is in school."). One story beats ten statistics.

### 4.7 Proof / impact numbers
- A bright band: `1,240 wells · 2.1M people · 19 regions · 100% maintained`. Big serif numerals, source-backed.

### 4.8 Donation block
- A clear, low-friction donate card: preset amounts (pills), a custom field, monthly/once toggle, and a reassurance line (`Cancel anytime · Tax-deductible · Secure`). Show what each amount *does* (`$50 = one family, one year`).

### 4.9 Trust & transparency
- Logos of charity-rating bodies, a link to annual reports, partner orgs. Quiet, credible.

### 4.10 Footer
- Warm, organized: logo, mission line, link columns, newsletter, social, registration/charity number, year. Include contact + a "we're a registered nonprofit" line.

---

## 5. Motion & Interaction

- **Page load:** hero photo settles (gentle scale-down from 1.04), the sentence and CTAs fade up, staggered 80ms. Calm and warm — never flashy.
- **Impact numbers:** count up once on scroll into view (respect reduced-motion → show final).
- **Impact ledger bar:** segments grow in sequence as it enters view.
- **Hover:** donate button warms/lifts; story photo gently brightens.
- **Donate presets:** selecting an amount updates the "what it does" line with a soft transition.
- Reduced-motion: no count-up animation, no photo scale; keep simple fades. Never use motion to create urgency.

---

## 6. Responsive Behavior

- **`< md`:** hero text bottom-aligned over the photo; CTAs stack full-width. Pillars and impact ledger stack to one column. A **sticky "Donate" bar** at the bottom keeps the action reachable.
- **`md`:** pillars 3→2; donation presets wrap to 2 rows.
- **`lg+`:** full layout, side-by-side story, max 1200px.
- Photography must crop gracefully — provide art-directed crops for the hero (`<picture>`).

---

## 7. Accessibility & Responsiveness Checklist

- [ ] **Accessibility is mission-critical here** — body contrast ≥ 7:1 where possible; everything keyboard-navigable.
- [ ] Hero text over photo sits on a sufficient scrim (≥ 4.5:1 against the lightest area).
- [ ] All photos have respectful, descriptive `alt`; never decorative-only for people's images.
- [ ] Donate form: real labels, clear errors announced via `aria-live`, secure-payment indicator is text + icon.
- [ ] Statistics cite sources; numbers have text equivalents (count-up never hides the value).
- [ ] Amber CTA on white passes 4.5:1 for its text; links underlined, not color-only.
- [ ] One `h1`; sections nest. Tap targets ≥ 44px; sticky donate bar doesn't obscure content.
- [ ] No motion implies urgency/pressure; no auto-popping donation modals.

---

## 8. Build Notes

- **Stack:** accessible HTML/CSS first; light JS for count-up, ledger animation, and the donate widget. Lean on a real, PCI-compliant payment provider (Stripe, Donorbox) — don't hand-roll payments.
- **Photography:** the page lives or dies on real, dignified imagery. Use art-directed `<picture>` crops; optimize heavily (AVIF/WebP, lazy-load below the fold, explicit dimensions).
- **Impact ledger:** simple fl/grid bar with labeled segments; make it readable as a list for SR.
- **Fonts:** Fraunces (display) + Public Sans (body, very accessible); preload display.
- **Gotcha:** avoid both guilt and vagueness. Lead with hope, back every claim with a number and a source, and make the donate path frictionless. Transparency *is* the design.
- **Assets:** 4–6 documentary photos (with consent + dignity), partner/rating logos, one detailed impact breakdown, annual report link.
