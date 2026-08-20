# Capital Factory Digital Style Guide

Status: Reverse-engineered from capitalfactory.com. Not an official brand manual.
Last updated: August 20, 2026

## Design character

Editorial frontier. The site combines a literary serif with a restrained neutral sans. It feels institutional, ambitious, and current: large quiet fields, fine rules, data-like labels, and a single warm gold signal color.

The system should feel:

- Confident, selective, and serious.
- Human and cultured in storytelling; crisp in utility content.
- High-contrast but warm, not sterile, blue-gray, or "generic SaaS."

## Foundations

### Color

| Token | Value | Role |
| --- | --- | --- |
| Canvas | `#0B0B0D` | Primary page background |
| Surface | `#232327` | Subtle raised dark surface |
| Ink | `#F8F7F1` | Primary text and light rules |
| Ink muted | `#C8C3B8` | Body copy and navigation |
| Ink subtle | `#9C978C` | Labels and secondary metadata |
| Gold | `#F2A900` | Accent, category marker, active emphasis |
| Black | `#000000` | Media controls only |

Use the dark canvas with ivory text by default. Gold is a precise signal for labels, selected states, and small emphasis, not a dominant background or general button color. Avoid gradients and competing accent colors.

### Typography

| Style | Typeface | Size / leading | Weight | Tracking | Use |
| --- | --- | --- | --- | --- | --- |
| Display / H1 | Fraunces | 96 / 96 px | 300 | -0.03em | Hero statement |
| Section display / H2 | Fraunces | 36 / 49.5 px | 300 | normal | Major headings, pull quotes |
| Card headline / H3 | Fraunces | 24 / 32 px | 400 | normal | Feature/program title |
| Editorial item | Fraunces | 16 / 20 px | 400 | normal | Company or portfolio name |
| Body large | Inter | 20 / 32.5 px | 400 | normal | Hero supporting copy |
| Body | Inter | 16 / 24 px | 400 | normal | Descriptions, footer copy |
| Navigation | Inter | 14 / 20 px | 400 | normal | Header and footer links |
| Label | Inter | 12 / 16 px | 400 | 0.18em | Eyebrows, category labels |
| Category title | Inter | 14 / 20 px | 500 | 0.22em | Taxonomy headings |

Use Fraunces for expressive editorial messaging and Inter for interface content. Keep display type light and spacious. All caps belong only to small labels.

### Spacing and geometry

Use an 8px base unit.

- 8px: compact interior gap
- 16px: list gap, label margin
- 24px: small content separation
- 32px: support copy after hero
- 40-48px: headline/content separation
- 64px: medium section rhythm
- 80px: desktop page gutter and section padding
- 96px: major internal section break

Observed desktop gutter: 80px on a 1348px viewport. Corners are square; avoid soft rounded cards or pills. Use 1px rules in muted ivory to organize content.

## Layout system

The site uses a full-bleed dark canvas with generously inset editorial content. Sections read as stacked chapters, not separate panels.

```
Wordmark + primary navigation
──────────────────────────────
Gold eyebrow
Large serif hero statement
Muted sans supporting copy
Proof: companies, press, outcomes
──────────────────────────────
Large statement + metrics/data
──────────────────────────────
Gold label + display heading + split features
──────────────────────────────
Taxonomy / portfolio grid
──────────────────────────────
Closing statement + compact footer
```

### Responsive behavior

- Reduce gutters from 80px toward 40px on tablet and 20-24px on mobile.
- Stack two-column content once line lengths become strained.
- Keep the serif/sans contrast intact; scale hero type with `clamp()` instead of making it dense.
- Keep labels at 12px.
- Collapse navigation to a menu rather than wrapping links.
- Rules span the content column unless dividing intentionally full-bleed media.

```css
:root {
  --gutter: clamp(20px, 5.9vw, 80px);
  --display-hero: clamp(48px, 7.12vw, 96px);
  --display-section: clamp(30px, 2.67vw, 36px);
}
```

## Components

### Header

- White Capital Factory wordmark, approximately 212 x 28px.
- Inter 14/20 navigation in muted ivory.
- No heavy header background, shadow, or filled nav buttons.
- Hover/current state shifts toward ivory or gold, ideally with a subtle underline or rule.

### Eyebrow / metadata label

Use for dates, location, program names, or section identity.

```html
<p class="eyebrow">EST. 2009 · AUSTIN, TEXAS</p>
```

```css
.eyebrow {
  color: #F2A900;
  font: 400 12px/16px Inter, sans-serif;
  letter-spacing: .18em;
  text-transform: uppercase;
}
```

Gold is for primary editorial labels; muted ivory works for tertiary labels such as "Portfolio by vehicle."

### Hero

- Begin with a gold eyebrow.
- Use a short, two- or three-line Fraunces headline.
- Follow it with one 20px Inter paragraph in muted ivory.
- Support the claim with real evidence: portfolio names, metrics, press, or outcomes.

### Links

Links are text-first rather than button-shaped.

```css
.link {
  color: #C8C3B8;
  text-decoration: none;
  text-underline-offset: .22em;
}
.link:hover,
.link:focus-visible {
  color: #F8F7F1;
  text-decoration: underline;
}
```

Use Inter for utility links and Fraunces for editorial/company links. A trailing arrow (→) signals forward action.

### Program card / split feature

Desktop features work as two equal propositions, typically separated by a fine rule.

- Gold uppercase eyebrow
- Fraunces 24/32 headline
- Inter 16/24 muted description with trailing arrow

The content carries the component: no rounded card shell, badge, or oversized CTA.

### Metric block

Metrics should resemble reference data, not dashboard widgets:

- Large number
- Compact uppercase label
- Fine dividers
- Spacious composition
- Neutral palette, with gold only for category or active emphasis

### Taxonomy / portfolio list

Use a multi-column desktop grid.

- Category: Inter 14px, uppercase, muted, letter-spaced.
- Items: Fraunces 16/20, ivory.

Lists should feel catalog-like and editorial, not like product cards.

### Press/news row

Pair a compact publication mark (observed around 24 x 24px) with:

- Uppercase company name
- Linked story title
- Minimal horizontal structure

It should read as proof of momentum, without visual clutter.

### Footer

Close with a Fraunces 36px statement. Follow with restrained Inter 14px links and compact copyright text. Keep it on the same dark canvas; do not introduce a contrasting footer slab.

## Imagery, marks, and motion

### Imagery

Favor proof-bearing imagery: founders, machines, facilities, launches, industrial materials, research, and real-world outcomes.

Use large editorial crops and documentary-quality photography. Avoid stock-photo smiles, abstract SaaS illustrations, decorative 3D blobs, and synthetic gradients.

### Brand marks

Use the white wordmark on the dark canvas. Gold can be used for high-attention moments, sparingly. Preserve clear space at least equal to the logo cap height. Do not recolor, outline, distort, or place the mark on low-contrast imagery.

### Motion

Motion should be editorial and subtle:

- Opacity/position reveals
- Fine-rule expansion
- Gentle content stream or marquee
- 160-240ms for UI feedback
- 400-700ms for section-entry transitions

Always respect `prefers-reduced-motion`.

## Accessibility

- Maintain WCAG AA contrast minimum.
- Use a visible ivory or gold 2px keyboard focus outline with offset.
- Preserve semantic heading order.
- Give meaningful images concise alt text; decorative images get empty alt text.
- Never use gold as the sole status indicator. Pair it with text, underline, or iconography.

## Implementation starter

```css
:root {
  --canvas: #0b0b0d;
  --surface: #232327;
  --ink: #f8f7f1;
  --ink-muted: #c8c3b8;
  --ink-subtle: #9c978c;
  --gold: #f2a900;
  --gutter: clamp(20px, 5.9vw, 80px);
}
body {
  margin: 0;
  background: var(--canvas);
  color: var(--ink);
  font-family: Inter, sans-serif;
}
.section {
  padding: 80px var(--gutter);
}
.display {
  font: 300 clamp(48px, 7.12vw, 96px)/1 Fraunces, serif;
  letter-spacing: -.03em;
}
.section-title {
  font: 300 clamp(30px, 2.67vw, 36px)/1.375 Fraunces, serif;
}
.body-large {
  color: var(--ink-muted);
  font: 400 20px/1.625 Inter, sans-serif;
}
.rule {
  border: 0;
  border-top: 1px solid color-mix(in srgb, var(--ink) 25%, transparent);
}
```

## Do and do not

| Do | Do not |
| --- | --- |
| Make one strong editorial statement at a time. | Fill every region with cards, badges, or CTAs. |
| Use gold as a small, high-signal accent. | Turn gold into the dominant background or universal button color. |
| Pair warm serif storytelling with neutral sans utility text. | Use the serif for dense interface text or navigation. |
| Build hierarchy through scale, whitespace, and rules. | Rely on shadows, gradients, rounded containers, or oversized icons. |
| Show concrete proof: companies, press, outcomes, metrics. | Use vague innovation claims without evidence. |
