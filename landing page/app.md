# Landing Page Brief — Mobile App

> **Use case:** Landing for a consumer mobile app (habit tracker, social, fitness, finance-for-humans) driving App Store / Play installs.
> **One-liner concept:** *A toy you want to hold.* — soft, tactile, candy-bright, with a phone that feels squeezable.

---

## 1. Aesthetic Direction

**Playful, toy-like, candy.** Bright but tasteful pastels with one punchy primary, chunky rounded shapes, soft "claymorphic" depth, springy motion. Everything looks like it has a satisfying give to it. The memorable detail: a **floating, gently bobbing phone mockup with live-looking app UI** that reacts (tilts, the screen content animates) — plus **squishy buttons** that compress on press. It should make people smile and want to tap.

Avoid: corporate flatness, tiny dense text, three identical screenshots in a row, an enterprise tone. This is friendly and confident, not cutesy-overload.

**Mood words:** friendly · bouncy · tactile · delightful.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.97 0.02 250);    /* soft sky-cream */
  --surface:     oklch(1 0 0);
  --surface-2:   oklch(0.95 0.03 250);

  --ink:         oklch(0.28 0.04 265);    /* friendly deep blue-grey */
  --ink-soft:    oklch(0.46 0.04 265);
  --ink-faint:   oklch(0.64 0.03 265);

  --accent:      oklch(0.70 0.19 25);     /* coral pop */
  --accent-2:    oklch(0.78 0.15 160);    /* mint */
  --accent-3:    oklch(0.80 0.14 90);     /* sunny yellow */
  --accent-ink:  oklch(0.99 0.01 25);

  --line:        oklch(0.90 0.02 265);
  --line-strong: oklch(0.80 0.03 265);

  --max: 1200px;
  --r-md: 20px; --r-lg: 32px; --r-pill: 999px;  /* very round */
}
```

Depth: **claymorphic soft shadows** — a light inner highlight + a soft colored drop shadow tinted toward the element's hue (e.g. coral button casts a faint coral shadow). Backgrounds get **floating blob shapes** in pastel accents, slowly drifting, plus optional tiny doodle stickers (sparkles, hearts) placed sparingly.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display / headings | **Gantari**, *Uncut Sans*, or free **Plus Jakarta Sans (bold)** / **Quicksand** | rounded, friendly, confident |
| Body | **Plus Jakarta Sans** or **Nunito** | warm, legible |
| Accents / numbers | display bold, maybe a hand-lettered word as SVG | one playful highlight word |

Rules:
- Headlines big and **rounded-bold**, friendly leading (1.05).
- Allow **one word per headline in a colored, slightly-rotated highlight pill** or hand-drawn underline.
- Body 17–18px, generous line-height (1.6), short paragraphs.
- Buttons: pill, bold, chunky padding, never thin.

---

## 4. Section Blueprint

### 4.1 Nav
- Rounded floating bar: bubbly logo left, a couple of links, and a chunky `Get the app` pill (coral). Maybe a tiny app-icon avatar.

### 4.2 Hero
- Left: a small badge (`★ 4.9 on the App Store`), a big friendly headline with one highlighted word:
  > *Build habits that actually **stick**.*
- Sub-line (warm, specific). Two **store buttons** (App Store + Google Play badges) and/or a `Start free` pill.
- Right: the **floating phone mockup** — bobbing gently, screen showing live-looking app UI (a streak counter ticking, a checkbox that bounces when "completed"). Pastel blobs drift behind it; a couple of UI cards float just outside the phone frame for depth.

### 4.3 Social proof
- A row of cheerful stats with emoji-free icons: `2M+ habits built · 4.9★ · 180 countries`. Big rounded numerals in accent colors.

### 4.4 Features — playful cards
- 3–4 chunky cards on soft surfaces, **each a different pastel**, with a bold custom illustration/icon, a short title, and one friendly line. Vary sizes slightly; let one card overlap a background blob. Each card tilts a touch on hover (springy).

### 4.5 "See it in action"
- A horizontal set of **angled phone screens** (not a flat row) showing real screens, with little labels pointing to features (hand-drawn arrows). Could be a draggable/scrollable carousel with snap.

### 4.6 How it works
- Three bouncy steps with big numerals in circles: `1 Pick a habit · 2 Tap to track · 3 Watch the streak grow`. Each with a mini illustration.

### 4.7 Testimonials
- Speech-bubble cards (rounded, with a tail) in alternating pastels, each with an avatar, name, and a short, real-sounding quote. Slight rotation for a scrapbook feel.

### 4.8 Big CTA
- A full rounded panel in accent gradient (coral→sunny), white friendly headline *"Your future self says thanks."*, store buttons, and a floating phone peeking from the bottom edge.

### 4.9 Footer
- Soft, rounded: logo, a friendly tagline, link columns, store badges again, social icons as little rounded chips, and a warm sign-off line.

---

## 5. Motion & Interaction

- **Page load:** badge → headline (highlight pill pops in with `--ease-spring`) → sub → buttons → phone floats up and starts bobbing. Staggered ~80ms, springy.
- **Phone:** continuous gentle bob + tilt-toward-cursor; the on-screen UI loops a tiny delightful animation (checkbox bounce, streak +1, confetti burst on a loop with a long pause).
- **Buttons:** **squish on press** (scale 0.96 + shadow compress), spring back on release. Hover lifts + brightens.
- **Cards:** spring tilt on hover; blobs drift slowly behind everything.
- **Scroll:** elements pop up with a little overshoot (`--ease-spring`), staggered.
- Reduced-motion: stop bobbing/blob drift/confetti; keep simple fades; buttons still give a subtle press feedback (no bounce).

---

## 6. Responsive Behavior

- **`< md`:** hero stacks (headline → phone below, centered). Store buttons go full-width stacked. Angled phone gallery becomes a swipeable snap-carousel. Feature cards single column (keep the varied pastels).
- **`md`:** features 4→2; testimonials 2-up.
- **`lg+`:** side-by-side hero, varied feature grid, max 1200px.
- Phone mockup scales down gracefully; reduce floating side-cards on small screens to avoid clutter.

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Despite pastels: text contrast ≥ 4.5:1. Pastel-on-pastel is the trap — **darken ink or the card** until it passes; don't ship pale-grey-on-cream.
- [ ] Highlight pills/underlines are decorative; the word stays real text.
- [ ] Store badges are real links with accessible names ("Download on the App Store").
- [ ] Phone screen animations are decorative (`aria-hidden`); the value is also stated in text.
- [ ] Squishy buttons keep a visible focus ring and ≥ 44px tap target.
- [ ] Confetti/bob/blobs all stop under `prefers-reduced-motion`.
- [ ] Carousel is keyboard-navigable with visible focus and not the only way to reach info.
- [ ] One `h1`; friendly headings still nest correctly.

---

## 8. Build Notes

- **Stack:** React + Motion shines here (spring physics for squish/bob/tilt). Vanilla works with CSS spring-ish easings + small JS.
- **Claymorphism:** combine a soft drop shadow, a subtle inner top highlight (`inset 0 2px 0 rgba(255,255,255,.6)`), and a tinted colored shadow per element.
- **Phone UI:** build the on-screen content as real HTML/CSS inside a phone frame so it can animate crisply (don't bake it into a static PNG).
- **Fonts:** Plus Jakarta Sans (display+body) or Quicksand for extra roundness; preload bold.
- **Gotcha:** "playful" must not become "unreadable" or "childish-for-adults." Keep one punchy accent + two soft supports, real contrast, and copy that's warm but smart. Restraint on stickers — a few, not a craft drawer.
- **Assets:** app icon, 4–6 real app screenshots, store badges (official SVGs), avatar photos for testimonials, a small set of doodle SVGs (sparkle, arrow, heart).
