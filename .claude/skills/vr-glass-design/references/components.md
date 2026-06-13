# Component recipes — VR Glass Design

Ready-to-adapt HTML/CSS for the signature components. All snippets assume the `:root` tokens and `.glass` recipe from SKILL.md are present.

## Contents
1. [Two-column gallery layout](#1-two-column-gallery-layout)
2. [Sidebar header (logo + tagline)](#2-sidebar-header)
3. [Numbered navigation list + active card](#3-numbered-navigation-list)
4. [Hero media panel + caption](#4-hero-media-panel)
5. [Thumbnail carousel](#5-thumbnail-carousel)
6. [Buttons & chips](#6-buttons--chips)
7. [Scrollbars & focus states](#7-scrollbars--focus-states)

---

## 1. Two-column gallery layout

A large glass panel containing a narrower sidebar and a wide content area. The outer panel is `--glass-1` (subtlest), children sit on top of it.

```html
<main class="stage panel">
  <aside class="sidebar"><!-- header + nav list --></aside>
  <section class="content"><!-- hero media + caption + carousel --></section>
</main>
```

```css
.panel {
  display: grid;
  grid-template-columns: minmax(260px, 320px) 1fr;
  gap: 48px;
  width: min(1400px, 94vw);
  min-height: 80vh;
  margin: 5vh auto;
  padding: 48px;
  background: var(--glass-1);
  backdrop-filter: blur(var(--blur));
  -webkit-backdrop-filter: blur(var(--blur));
  border: 1px solid var(--glass-border);
  border-radius: calc(var(--r-panel) + 8px);
  box-shadow: inset 0 1px 0 rgba(255,255,255,.06), var(--shadow-float);
}
@media (max-width: 900px) {
  .panel { grid-template-columns: 1fr; min-height: auto; }
}
```

## 2. Sidebar header

Bold app name over a wide-tracked uppercase tagline — the "gallery placard" look.

```html
<header class="brand">
  <h1>ART VR</h1>
  <p>The Virtual Gallery</p>
</header>
```

```css
.brand h1 { font-size: 24px; font-weight: 700; letter-spacing: .02em; }
.brand p {
  margin-top: 10px;
  font-size: 12px; font-weight: 300;
  text-transform: uppercase; letter-spacing: .35em;
  color: var(--text-mid);
}
```

## 3. Numbered navigation list

Inactive items: a small glass chip with the number + plain label. The active item expands into a glass card with a thumbnail and meta lines. Selection is expressed by *more glass and more content*, never by accent color.

```html
<nav class="nav">
  <button class="nav-item">
    <span class="nav-chip">1</span>
    <span class="nav-name">Elena Rostova</span>
  </button>
  <!-- ... -->
  <button class="nav-item active">
    <span class="nav-thumb" style="background:linear-gradient(135deg,#8e2f44,#2c5364)"></span>
    <span class="nav-meta">
      <span class="nav-name">Julian Cross</span>
      <span class="m1">Contemporary</span>
      <span class="m2">Mixed Media</span>
    </span>
  </button>
</nav>
```

```css
.nav { display: flex; flex-direction: column; gap: 28px; margin-top: 56px; }
.nav-item {
  display: flex; align-items: center; gap: 20px;
  background: none; border: 0; padding: 0;
  color: var(--text-hi); font: inherit; text-align: left; cursor: pointer;
}
.nav-chip {
  display: grid; place-items: center;
  width: 52px; height: 52px;
  font-size: 13px; color: var(--text-mid);
  background: var(--glass-2);
  backdrop-filter: blur(var(--blur));
  border: 1px solid var(--glass-border);
  border-radius: var(--r-chip);
  box-shadow: inset 0 1px 0 rgba(255,255,255,.08);
  transition: background .25s, border-color .25s;
}
.nav-item:hover .nav-chip { background: var(--glass-3); }
.nav-name { font-size: 18px; }

/* Active: expanded glass card */
.nav-item.active {
  padding: 14px 18px;
  background: var(--glass-3);
  backdrop-filter: blur(var(--blur));
  border: 1px solid var(--glass-border-strong);
  border-radius: var(--r-card);
  box-shadow: inset 0 1px 0 rgba(255,255,255,.12), 0 8px 32px rgba(0,0,0,.3);
}
.nav-thumb {
  width: 64px; height: 64px; flex-shrink: 0;
  border-radius: var(--r-chip);
  border: 1px solid var(--glass-border);
}
.nav-meta { display: flex; flex-direction: column; gap: 3px; }
.nav-meta .m1 { font-size: 13px; color: var(--text-mid); }
.nav-meta .m2 { font-size: 13px; color: var(--text-low); }
```

## 4. Hero media panel

Large rounded media with a hairline border and deep shadow; caption centered below — bold title, then a `·`-separated meta line.

```html
<figure class="hero">
  <div class="hero-media" style="background:linear-gradient(160deg,#9e3450 0%,#4a2742 30%,#33576b 65%,#16313e 100%)"></div>
  <figcaption>
    <h2>Vibrant Grove</h2>
    <p><span>2024</span><span class="dot">·</span><span>Contemporary</span></p>
  </figcaption>
</figure>
```

```css
.hero-media {
  aspect-ratio: 16 / 10;
  border-radius: var(--r-panel);
  border: 1px solid var(--glass-border);
  box-shadow: 0 24px 60px rgba(0,0,0,.45);
}
.hero figcaption { text-align: center; margin-top: 28px; }
.hero h2 { font-size: 28px; font-weight: 700; }
.hero p { margin-top: 8px; font-size: 15px; color: var(--text-mid); }
.hero .dot { margin: 0 .6em; color: var(--text-low); }
```

For real images use `object-fit: cover` on an `<img>` inside `.hero-media` with `overflow: hidden`.

## 5. Thumbnail carousel

A glass pill rail holding rounded-square thumbnails; the active thumb gets a bright ring and slight scale; a hairline divider separates the chevron button.

```html
<div class="carousel glass">
  <button class="thumb" style="background:linear-gradient(135deg,#c98a3d,#7b4fa0)"></button>
  <button class="thumb" style="background:linear-gradient(135deg,#bcd8d2,#2e6e72)"></button>
  <button class="thumb" style="background:linear-gradient(135deg,#2e4ed7,#d7572e)"></button>
  <button class="thumb active" style="background:linear-gradient(135deg,#9e3450,#33576b)"></button>
  <span class="divider"></span>
  <button class="chevron" aria-label="Next">›</button>
</div>
```

```css
.carousel {
  display: flex; align-items: center; gap: 14px;
  width: max-content; margin: 32px auto 0;
  padding: 14px 18px;
  border-radius: 22px;
}
.thumb {
  width: 64px; height: 64px;
  border: 1px solid var(--glass-border);
  border-radius: 14px;
  cursor: pointer; padding: 0;
  transition: transform .25s, border-color .25s;
}
.thumb:hover { transform: translateY(-2px); }
.thumb.active {
  border: 2px solid var(--glass-border-strong);
  box-shadow: 0 0 0 1px rgba(255,255,255,.15), 0 8px 24px rgba(0,0,0,.35);
  transform: scale(1.06);
}
.divider { width: 1px; height: 40px; background: var(--glass-border); margin: 0 6px; }
.chevron {
  width: 56px; height: 56px;
  display: grid; place-items: center;
  font-size: 22px; color: var(--text-hi);
  background: none; border: 0; border-radius: var(--r-chip);
  cursor: pointer; transition: background .25s;
}
.chevron:hover { background: var(--glass-2); }
```

## 6. Buttons & chips

Primary action — brighter glass, never a solid accent fill:

```css
.btn {
  padding: 14px 28px;
  font: 500 15px/1 inherit; color: var(--text-hi);
  background: var(--glass-3);
  backdrop-filter: blur(var(--blur));
  border: 1px solid var(--glass-border-strong);
  border-radius: 999px;
  box-shadow: inset 0 1px 0 rgba(255,255,255,.15);
  cursor: pointer; transition: background .25s;
}
.btn:hover { background: rgba(255,255,255,.18); }
.btn.ghost { background: var(--glass-1); border-color: var(--glass-border); }
```

Tag chip: same as `.btn` but `padding: 6px 14px; font-size: 12px;` with uppercase + `letter-spacing: .15em`.

## 7. Scrollbars & focus states

```css
::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-thumb { background: var(--glass-3); border-radius: 99px; }
::-webkit-scrollbar-track { background: transparent; }

:focus-visible {
  outline: 1px solid var(--glass-border-strong);
  outline-offset: 3px;
}
```

Keyboard focus uses the same hairline-light language as everything else — visible but quiet.
