# Scinnohub AI Drug Discovery Studio — Website Plan

## 1. Recommended Tech Stack

**Keep it simple: single-file HTML with embedded CSS and minimal JS.**

| Concern       | Choice                          | Rationale                                          |
|---------------|---------------------------------|----------------------------------------------------|
| Markup        | HTML5 (single `index.html`)     | Zero build step, easy to deploy anywhere           |
| Styling       | CSS3 (embedded `<style>`)       | No framework overhead; full control over design    |
| Interactivity | Vanilla JS (embedded `<script>`)| Smooth-scroll, subtle hover effects only           |
| Fonts         | Google Fonts (Inter + Syne)     | Clean, modern scientific aesthetic; loaded via CDN |
| Icons         | None / Unicode symbols          | No extra requests; keeps it lightweight            |

> No framework (React, Vue, etc.) is needed — the site is purely presentational with no dynamic data.

---

## 2. File Structure

```
agent_web/
├── index.html        # Single deliverable file (markup + styles + scripts)
└── plan.md           # This plan
```

All CSS and JS live inside `index.html` to eliminate file-path issues and enable zero-config deployment (open in browser, upload to any host).

---

## 3. Design Considerations

### Color Palette
| Role             | Color                  | Usage                           |
|------------------|------------------------|---------------------------------|
| Primary bg       | `#0a0e1a` (deep navy)  | Page background                 |
| Surface          | `#111827` (dark card)  | Cards, sections                 |
| Accent 1         | `#4f8ef7` (blue)       | CTAs, headings, borders         |
| Accent 2         | `#00d4aa` (teal-green) | Hover states, icons, highlights |
| Text primary     | `#f0f4ff`              | Body copy                       |
| Text muted       | `#7a8ba0`              | Subtitles, captions             |

Scientific/biotech aesthetic: dark background evokes lab environment; blue+teal mirrors molecular visualization tools.

### Typography
- **Headings**: Syne (geometric, futuristic) — conveys AI/tech innovation
- **Body**: Inter (neutral, highly legible) — clinical precision feel
- Font sizes: `clamp()` for fluid scaling; no fixed px breakpoints needed

### Spacing & Layout
- Max content width: `1100px`, centered with `margin: 0 auto`
- Section vertical padding: `100px 0`
- Cards use CSS Grid (`repeat(auto-fit, minmax(240px, 1fr))`): adapts from 1–4 columns
- Generous whitespace (`gap: 2rem`) between cards for breathing room
- Subtle `border: 1px solid rgba(79,142,247,0.2)` on cards + `backdrop-filter: blur` glass effect

### Micro-interactions
- Card hover: `transform: translateY(-6px)` + glow border (`box-shadow`)
- Header sticky with `backdrop-filter: blur(12px)` + semi-transparent background
- Smooth scroll via `scroll-behavior: smooth` on `<html>`

### Responsiveness
- Mobile-first media query at `768px` collapses nav, stacks sections
- Cards reflow naturally via CSS Grid `auto-fit`

---

## 4. Section Breakdown

### Header / Hero
- Sticky top nav bar: logo text "Scinnohub" left, nav links right (Projects, About, Contact)
- Full-viewport hero below nav:
  - Main title: "AI Drug Discovery Studio"
  - Subtitle: short tagline (e.g. "Accelerating molecular innovation with artificial intelligence")
  - CTA button: "Explore Projects" → smooth scrolls to #projects

### Projects Section
Four cards in a responsive grid:

| Card                    | Icon (Unicode) | Short description                                    |
|-------------------------|----------------|------------------------------------------------------|
| AI Drug Design          | ⚗              | End-to-end generative molecule design pipelines      |
| Molecular Docking       | 🔬             | Binding affinity prediction and pose optimization    |
| Property Prediction     | 📊             | ADMET and physicochemical property forecasting       |
| Structure Optimization  | ⚙              | Lead optimization via multi-objective ML models      |

### About Section
- Two-column layout: left = short paragraph (3–4 sentences); right = decorative stat blocks (e.g. "10k+ Molecules Analyzed", "4 AI Modules")
- Background: slightly lighter surface to visually separate from Projects

### Contact Section
- Centered layout, minimal: heading + one sentence + a prominent button linking to `http://www.scinnohub.com/`
- Button opens in new tab (`target="_blank" rel="noopener"`)

### Footer
- Single line: copyright + link to scinnohub.com

---

## 5. Step-by-Step Implementation Plan

### Step 1 — Skeleton HTML structure
Create `index.html` with semantic sections: `<header>`, `<section id="projects">`, `<section id="about">`, `<section id="contact">`, `<footer>`.

**Verify:** Open in browser → correct section IDs exist; nav links anchor correctly.

### Step 2 — CSS reset + design tokens
Add CSS reset (`box-sizing`, `margin`/`padding: 0`, `font-family`) and custom properties (`--color-bg`, `--color-accent`, etc.).

**Verify:** Page renders with dark background; no default browser margins visible.

### Step 3 — Header & Hero
Build sticky nav + hero with heading, tagline, CTA button.

**Verify:** Nav stays fixed on scroll; "Explore Projects" scrolls smoothly to `#projects`.

### Step 4 — Project Cards
Render 4 cards in CSS Grid with icon, title, description.

**Verify:** Cards display in 4 columns on desktop, collapse to 1–2 on mobile (resize browser).

### Step 5 — Hover & animation polish
Add `transition`, `transform`, and `box-shadow` rules; add glass-morphism effect on cards.

**Verify:** Hovering a card lifts it visually with a glow; no layout shift.

### Step 6 — About Section
Two-column layout with intro text + stat blocks.

**Verify:** Renders correctly; collapses to single column on `<768px`.

### Step 7 — Contact Section
Centered content + external link button to `http://www.scinnohub.com/`.

**Verify:** Button opens scinnohub.com in a new tab; link contains `rel="noopener"`.

### Step 8 — Footer
Single-line footer with copyright.

**Verify:** Sticks to bottom of content flow; no overlap with contact section.

### Step 9 — Responsive QA
Test at 375px (mobile), 768px (tablet), 1280px (desktop) using browser DevTools.

**Verify:** No horizontal scroll; all text readable; cards reflow correctly.

### Step 10 — Final polish
Check: font loading (Google Fonts fallback), color contrast (WCAG AA), no broken links.

**Verify:** Lighthouse accessibility score ≥ 90; all nav anchors resolve.
