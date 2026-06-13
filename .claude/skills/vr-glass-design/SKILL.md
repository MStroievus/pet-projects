---
name: vr-glass-design
description: Generate premium dark glassmorphism UI designs in the "ART VR gallery" style — frosted glass panels floating over dark gradient backgrounds with deep red/blue color bleed, soft rounded cards, translucent layers, elegant letterspaced typography, sidebar navigation lists, hero media panels, and thumbnail carousels. Use this skill whenever the user asks for a dark/glass/frosted/translucent UI, a VR or gallery-style interface, a "premium dark" landing page or dashboard, a spatial/visionOS-like screen, or says "make it look like the ART VR design" — even if they don't say "glassmorphism" explicitly. Also use it when restyling an existing page toward a dark elegant aesthetic.
---

# VR Glass Design

A design system for generating dark glassmorphism interfaces in the style of a premium VR art gallery: a dimly lit room where translucent glass panels float in front of a wall washed by deep red and blue light. Everything you build with this skill should feel like that — calm, deep, expensive.

## Design philosophy

Three ideas drive every decision:

1. **Light comes from the content.** The background is nearly black; color enters the scene as a soft "bleed" of deep red and blue, as if the artwork on screen is illuminating the room. Never use flat solid backgrounds or bright saturated fills for chrome.
2. **Glass, not cards.** Surfaces are translucent layers (`backdrop-filter: blur`) with hairline light borders — you should faintly see the background glow through every panel. Opaque gray boxes break the illusion.
3. **Quiet typography.** White text at three opacity levels does all the hierarchy work. Uppercase micro-labels with very wide letter-spacing add the "gallery placard" elegance.

## Core tokens

Define these CSS variables on `:root` in every output:

```css
:root {
  /* Background scene */
  --bg-base: #0a0e1a;
  --bleed-red: rgba(132, 28, 52, 0.55);   /* deep wine red */
  --bleed-blue: rgba(28, 58, 110, 0.50);  /* deep steel blue */

  /* Glass surfaces (3 elevation levels) */
  --glass-1: rgba(255, 255, 255, 0.04);   /* large containers */
  --glass-2: rgba(255, 255, 255, 0.07);   /* cards, chips, carousels */
  --glass-3: rgba(255, 255, 255, 0.12);   /* active/hover state */
  --glass-border: rgba(255, 255, 255, 0.10);
  --glass-border-strong: rgba(255, 255, 255, 0.25);

  /* Text — exactly three levels */
  --text-hi: rgba(255, 255, 255, 0.96);
  --text-mid: rgba(255, 255, 255, 0.65);
  --text-low: rgba(255, 255, 255, 0.40);

  /* Geometry */
  --r-panel: 28px;   /* big containers, hero media */
  --r-card: 18px;    /* cards, list items */
  --r-chip: 12px;    /* small chips, thumbnails */

  --blur: 24px;
  --shadow-float: 0 30px 80px rgba(0, 0, 0, 0.55);
}
```

Font stack: `-apple-system, "SF Pro Display", Inter, "Segoe UI", sans-serif`. If loading webfonts, use Inter.

## The background recipe

The scene background is layered radial glows over a near-black base — red bleeding from one corner, blue from the opposite:

```css
body {
  min-height: 100vh;
  background:
    radial-gradient(ellipse 80% 60% at 15% 0%, var(--bleed-red), transparent 60%),
    radial-gradient(ellipse 90% 70% at 90% 95%, var(--bleed-blue), transparent 60%),
    radial-gradient(ellipse 50% 40% at 60% 40%, rgba(60, 30, 60, 0.3), transparent 70%),
    var(--bg-base);
  color: var(--text-hi);
}
```

Vary the corner positions per project, but always: one warm bleed, one cool bleed, near-black base. Add a subtle vignette (`box-shadow: inset 0 0 200px rgba(0,0,0,.5)` on a fixed overlay) if the scene looks flat.

## The glass recipe

Every surface uses this exact pattern — translucent fill, blur, hairline border, and a top inner highlight that sells the "glass edge catching light":

```css
.glass {
  background: var(--glass-2);
  backdrop-filter: blur(var(--blur));
  -webkit-backdrop-filter: blur(var(--blur));
  border: 1px solid var(--glass-border);
  border-radius: var(--r-card);
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.08),  /* top edge highlight */
    0 8px 32px rgba(0, 0, 0, 0.25);
}
```

Rules that keep glass believable:
- Layer at most 3 glass levels deep (page panel → card → chip). Deeper stacks turn muddy.
- Active/selected items get `--glass-3` fill, `--glass-border-strong` border, and slightly larger scale or padding — never an accent color fill.
- Hover transitions: `transition: background .25s ease, border-color .25s ease, transform .25s ease`.

## Typography rules

| Role | Style |
|---|---|
| App name / logo | 700 weight, ~22px, normal tracking, uppercase optional |
| Tagline / micro-label | 300 weight, 12–13px, UPPERCASE, `letter-spacing: 0.35em`, `--text-mid` |
| Item title (artwork, card) | 700 weight, 24–32px, `--text-hi` |
| Meta line | 400 weight, 14–15px, `--text-mid`, items separated by `·` with `margin: 0 .6em` |
| List item primary | 400–500 weight, 17–18px, `--text-hi` |
| List item secondary | 400 weight, 13px, `--text-mid` then `--text-low` for the 3rd line |

The wide-tracked uppercase micro-label is the signature of this style — use it for taglines, section headers, and category labels.

## Optional 3D stage tilt

For hero/marketing presentations, the whole UI panel may float with a slight perspective tilt:

```css
.stage {
  transform: perspective(2000px) rotateX(2deg) rotateY(-3deg);
  box-shadow: var(--shadow-float);
}
```

Use it only for showcase/landing contexts — skip it for functional apps where users read and click a lot.

## Imagery

Artwork/media placeholders should themselves be colorful abstract gradients (fluid ink, smoke, marble) so they feed the color-bleed story. When no real images exist, generate CSS placeholder gradients like:

```css
background: linear-gradient(135deg, #8e2f44 0%, #3b2a4d 35%, #2c5364 70%, #15303c 100%);
```

Give hero media `border-radius: var(--r-panel)`, a hairline `--glass-border` border, and a deep shadow.

## Component recipes

Read [references/components.md](references/components.md) for ready-to-adapt HTML/CSS for the signature components:
- Two-column gallery layout (glass sidebar + hero media area)
- Numbered navigation list with active "expanded card" state
- Hero media panel with centered title + meta line
- Thumbnail carousel (glass pill rail, active ring, chevron button, divider)
- Buttons, chips, and scrollbar styling

Adapt the recipes to the actual content — don't reproduce the ART VR gallery verbatim unless asked. The tokens and recipes above are the style; the components are examples of the style applied.

## Output checklist

Before delivering, verify:
- [ ] Background has both a warm and a cool radial bleed over near-black
- [ ] Every panel uses backdrop-filter blur + hairline border + inner top highlight
- [ ] Text uses only the three opacity levels; at least one wide-tracked uppercase label exists
- [ ] Active states use brighter glass, not accent colors
- [ ] Radii are consistent (panel 28 / card 18 / chip 12)
- [ ] `-webkit-backdrop-filter` included for Safari
