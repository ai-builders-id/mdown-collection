# Landing Page Brief — Restaurant / Hospitality

> **Use case:** Landing for a restaurant, café, or bar — setting mood, showing the menu, and driving reservations.
> **One-liner concept:** *A letterpress menu under warm light.* — vintage, tactile, appetizing. You can smell the kitchen.

---

## 1. Aesthetic Direction

**Vintage menu, letterpress.** Warm cream paper, ink-brown type, a single deep accent (burgundy or olive), classic typography with ornamental flourishes — the feeling of a well-loved neighborhood institution's printed menu. The memorable detail: a **hero that pairs an atmospheric dining-room photograph with a letterpress-style headline**, and a **menu set like a real printed bill of fare** (dotted leader lines from dish to price) rather than generic cards.

Avoid: cold modern-minimal restaurant sites, tiny unreadable menus in a PDF, generic food-stock-photo grids, neon "ORDER NOW" energy. Hospitality is warmth — the site should feel like the room.

**Mood words:** warm · appetizing · timeless · welcoming.

---

## 2. Design Tokens

```css
:root {
  --bg:          oklch(0.95 0.02 85);     /* warm cream */
  --surface:     oklch(0.97 0.015 85);
  --surface-2:   oklch(0.91 0.025 80);    /* kraft band */

  --ink:         oklch(0.26 0.03 50);     /* warm ink-brown */
  --ink-soft:    oklch(0.44 0.03 50);
  --ink-faint:   oklch(0.60 0.025 55);

  --accent:      oklch(0.42 0.13 25);     /* deep burgundy */
  --accent-2:    oklch(0.50 0.08 130);    /* olive (alt) */
  --accent-ink:  oklch(0.96 0.015 85);

  --line:        oklch(0.82 0.02 75);
  --line-strong: oklch(0.30 0.03 50);

  --max: 1100px;
  --r-sm: 2px; --r-md: 4px;  /* print, not app */
}
```

Atmosphere: **paper grain** over cream backgrounds, **ornamental rule dividers** (a centered fleuron `❧` or a thin double rule), and warm food/room **photography with a slightly golden, nostalgic grade**. Optional faint **ink-bleed texture** on large type to sell the letterpress feel.

---

## 3. Typography

| Role | Font | Notes |
| --- | --- | --- |
| Display | **Canela**, *GT Sectra*, or free **Playfair Display / DM Serif Display** | classic high-contrast serif |
| Menu / body | **Cormorant Garamond** or **EB Garamond** | refined, traditional, readable |
| Labels / hours | letter-spaced caps serif, or a quiet small serif | `0.16em` tracking |
| Optional script | a tasteful script (**Tangerine**) for one or two words | the restaurant name flourish only |

Rules:
- Big serif display for the name and section titles, set with classic generous spacing.
- Menu items in Garamond with **dotted leader lines** to prices (tabular figures).
- Use **ornamental dividers** (fleuron, double rule) between sections — vintage menu language.

---

## 4. Section Blueprint

### 4.1 Top bar
- Centered or split: restaurant name (display, maybe one script word), and on the right a `Reserve` link + phone number. Hours/location as a faint letter-spaced line. Classic, calm.

### 4.2 Hero — the room
- A warm, golden-graded photograph of the dining room or a signature dish, with a letterpress-style headline over a cream panel or directly on a darker corner of the image:
  > *A neighborhood table since 1971.*
- A short welcoming line, the cuisine + neighborhood, and two CTAs: `Reserve a table` (burgundy) + `View the menu ↓`. An ornamental divider beneath.

### 4.3 A word of welcome
- A short, warm paragraph from the chef/owner — the story of the place. Set in Garamond at a comfortable reading size, centered, flanked by fleurons. A signature (script or scanned) adds soul.

### 4.4 The menu (signature)
- Set like a **printed bill of fare**: sections (`To Start · Mains · From the Sea · Dolci`) as serif headers with ornamental rules, each dish as `Name ......... price` with **dotted leader lines**, a faint italic description beneath, and small caps for dietary marks (`V · GF`). Two columns on desktop, like a real menu. Optional tabs for `Dinner / Lunch / Wine`.
- This must look hand-set, not like product cards.

### 4.5 The dishes (a taste)
- 3–4 signature dishes as warm photographs with serif captions and a one-line sensory description. Golden grade, generous size — make people hungry.

### 4.6 The space / gallery
- A small warm gallery of the room, the bar, details (hands, steam, candlelight). Vintage grade. Conveys atmosphere over information.

### 4.7 Reservations
- A clear, calm booking block: date/time/party-size, or an embedded reservation widget, with the phone number prominent for those who'd rather call (`Or call us — we'd love to hear from you`). Note walk-in policy.

### 4.8 Visit us
- Address, hours (set as a tidy letter-spaced table), a small map, parking/transit notes. Hospitality details done warmly.

### 4.9 Footer
- Cream/kraft: name, one-line ethos, hours, address, phone, social, a final fleuron, year. Maybe `Est. 1971`.

---

## 5. Motion & Interaction

- **Page load:** hero photo settles (slow scale-down), the letterpress headline fades up with a faint ink-bleed, ornamental divider draws in. Warm, unhurried.
- **Menu tabs:** switching (Dinner/Lunch/Wine) crossfades the list gently; dotted leaders redraw.
- **Hover:** dish photos warm/zoom slightly; reserve button deepens; gallery images lift with a soft shadow.
- **Scroll:** fleuron dividers expand from center; dishes fade up.
- Keep motion gentle and few — vintage warmth, not flashy. Reduced-motion: drop scales and ink-bleed; keep fades.

---

## 6. Responsive Behavior

- **`< md`:** two-column menu collapses to a single column (keep the dotted leaders). Hero text moves below the image if legibility suffers. Gallery → swipeable. A sticky `Reserve` bar appears at the bottom.
- **`md`:** dishes 2-up; menu stays one or two columns by width.
- **`lg+`:** full two-column printed-menu layout, max 1100px, generous margins.
- Ensure the menu is **real HTML text** at every size (never a flat image of a menu) for readability + SEO + accessibility.

---

## 7. Accessibility & Responsiveness Checklist

- [ ] Ink-brown on cream ≥ 7:1 (easy) — verify burgundy links/labels hit ≥ 4.5:1.
- [ ] Menu is semantic text (lists/definition lists), not an image; dietary marks have accessible labels (`V` → "Vegetarian").
- [ ] Dotted leader lines are decorative CSS, not characters that clutter SR reading order.
- [ ] Reservation widget is fully keyboard-accessible with labeled fields and announced errors; phone number is a `tel:` link.
- [ ] Food photos have appetizing, descriptive `alt`; decorative grain/fleurons are `aria-hidden`.
- [ ] Hours/address use semantic markup; map has a text address fallback.
- [ ] One `h1` (restaurant name); menu sections and pages nest correctly. Tap targets ≥ 44px.

---

## 8. Build Notes

- **Stack:** HTML/CSS is plenty; light JS for menu tabs and the reservation widget (or embed OpenTable/Resy/SevenRooms). Keep it fast — restaurant sites are often opened on phones, hungry, on bad connections.
- **Dotted leaders:** classic CSS trick — a flex row with the dish name and price at the ends and a `border-bottom: dotted` or repeating-dot pseudo-element filling the gap.
- **Letterpress feel:** subtle text-shadow/inset on large display type + paper grain overlay; don't overdo it.
- **Photography:** apply one consistent warm/golden grade across all photos so the page feels like one evening.
- **Fonts:** Playfair Display (display) + EB Garamond (menu/body); preload display.
- **Gotcha:** the cardinal sin is a menu as a PDF or image. Set it in real type. Keep the palette to cream + ink + one deep accent, and let warm photography and classic typography do the selling.
- **Assets:** dining-room + dish + detail photos (consistently graded), reservation widget/embed, map embed, optional owner signature, fleuron/ornament SVGs.
