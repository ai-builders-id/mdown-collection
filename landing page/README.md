# Landing Page Briefs

A collection of **production-grade landing-page design briefs** in Markdown. Each file is a self-contained spec: a committed aesthetic direction, type system, color tokens, section-by-section structure with real copy, motion notes, responsive rules, and an accessibility checklist.

These are not generic wireframes. Every brief picks **one bold aesthetic** and executes it precisely — no two share a font, palette, or layout philosophy. Hand any file to a developer (or to Claude with `/frontend-design`) and it should compile into a distinctive page.

---

## How To Use

1. Pick the brief closest to your use case from the index below.
2. Read the **Aesthetic Direction** first — it sets the tone for every later decision.
3. Copy the **Design Tokens** block into your stylesheet; they are real CSS custom properties.
4. Build top-to-bottom following the **Section Blueprint**; the copy is drop-in but meant to be edited.
5. Cross-check the **Accessibility & Responsiveness** checklist before shipping.

Shared conventions (token naming, spacing scale, motion timing, breakpoints) live in [`_DESIGN_TOKENS.md`](./_DESIGN_TOKENS.md). Read it once; every brief assumes it.

---

## Index

| File | Use case | Aesthetic | Theme |
| --- | --- | --- | --- |
| [`compro.md`](./compro.md) | Company profile / corporate | Editorial, refined, institutional | Light, warm paper |
| [`resume.md`](./resume.md) | Personal resume / CV | Swiss-grid, monospace, precise | Light, ink-on-white |
| [`saas.md`](./saas.md) | SaaS product | Retro-futuristic, terminal glow | Dark, phosphor |
| [`portfolio.md`](./portfolio.md) | Creative portfolio | Brutalist, raw, oversized type | High-contrast mono |
| [`startup.md`](./startup.md) | Startup / pre-seed | Optimistic, gradient-mesh, soft | Light, aurora |
| [`event.md`](./event.md) | Conference / event | Art deco, geometric, gold | Dark, midnight |
| [`app.md`](./app.md) | Mobile app | Playful, toy-like, candy | Light, bubblegum |
| [`agency.md`](./agency.md) | Design / dev agency | Luxury, cinematic, restrained | Dark, charcoal |
| [`ecommerce.md`](./ecommerce.md) | Product / store | Organic, natural, tactile | Light, clay |
| [`coming-soon.md`](./coming-soon.md) | Pre-launch / waitlist | Minimal, atmospheric, single-focus | Dark, void |
| [`nonprofit.md`](./nonprofit.md) | Nonprofit / cause | Humanist, hopeful, photographic | Light, sky |
| [`restaurant.md`](./restaurant.md) | Restaurant / hospitality | Vintage menu, letterpress | Warm, cream |

---

## Anatomy Of A Brief

Every file follows the same eight-part structure so you can navigate any of them blind:

1. **Aesthetic Direction** — the one-sentence concept and the feeling to chase.
2. **Design Tokens** — CSS variables for color, type, spacing, radius, shadow.
3. **Typography** — font pairing, scale, and usage rules.
4. **Section Blueprint** — every section, in order, with copy and layout intent.
5. **Motion & Interaction** — page-load choreography and micro-interactions.
6. **Responsive Behavior** — how the layout reflows across breakpoints.
7. **Accessibility & Responsiveness Checklist** — must-pass items.
8. **Build Notes** — framework hints, asset list, and gotchas.

---

## Principles (shared across all briefs)

- **Commit to one direction.** Refined minimalism and loud maximalism both work; timidity does not.
- **Distinctive typography.** No Arial, no Roboto, no default Inter-on-white. Pair a characterful display face with a clean body face.
- **Atmosphere over flat fills.** Gradient meshes, grain, geometric texture, layered shadow — backgrounds should have depth.
- **One hero moment.** A single, well-orchestrated page-load reveal beats a dozen scattered micro-animations.
- **Real copy.** Placeholder copy is written to be believable and editable, never `lorem ipsum`.
- **Accessible by default.** Contrast, focus states, reduced-motion, and semantic structure are part of the design, not a patch.
