# Scinnohub — AI Drug Discovery Studio

A static, single-page website for **Scinnohub**, an AI-driven drug discovery platform. The site presents four core research modules, an about section, and a contact link — all in a single deployable HTML file with no build tools required.

---

## Features

### Hero
Full-viewport landing section with the studio name, a tagline, and a call-to-action button that smooth-scrolls to the projects grid.

### Projects (4 modules)
| Module | Description |
|---|---|
| **AI Drug Design** | End-to-end generative molecule design pipelines using deep generative models |
| **Molecular Docking** | Binding affinity prediction and pose optimization at atomic resolution |
| **Property Prediction** | ADMET and physicochemical property forecasting before synthesis |
| **Structure Optimization** | Lead optimization via multi-objective ML models for potency and selectivity |

Each card features a hover lift effect with a blue glow border.

### About
Two-column layout combining a brief studio introduction with four key statistics (10k+ molecules analyzed, 4 AI modules, 98% prediction accuracy, 3× faster discovery).

### Contact
Centered section with a prominent link to [scinnohub.com](http://www.scinnohub.com/) that opens in a new tab.

### Sticky Navigation
Fixed header with `backdrop-filter: blur` that stays visible on scroll, with anchor links to each section.

---

## Technology Stack

| Concern | Choice |
|---|---|
| Markup | HTML5 — single `index.html` file |
| Styling | CSS3 — embedded `<style>` block with CSS custom properties |
| Interactivity | Vanilla JS — smooth scroll via `scroll-behavior: smooth` |
| Fonts | [Google Fonts](https://fonts.google.com/) — **Syne** (headings) + **Inter** (body) |
| Icons | Unicode emoji — no external icon library needed |
| Build tool | None — open the file directly in a browser |

No npm, no bundler, no framework. The entire site is one file.

---

## File Structure

```
agent_web/
├── index.html   # Complete site — markup, styles, and scripts
├── plan.md      # Design and implementation plan
└── README.md    # This file
```

---

## Running Locally

### Option 1 — Open directly (simplest)

```bash
xdg-open index.html          # Linux
open index.html               # macOS
start index.html              # Windows
```

> **Note:** Google Fonts requires an internet connection. Headings will fall back to the system sans-serif font when offline.

### Option 2 — Local HTTP server (recommended)

Using Python (no install needed on most systems):

```bash
cd agent_web
python3 -m http.server 8080
```

Then open `http://localhost:8080` in your browser.

Alternatively, with Node.js:

```bash
npx serve .
```

---

## Design System

| Token | Value | Usage |
|---|---|---|
| `--color-bg` | `#0a0e1a` | Page background |
| `--color-surface` | `#111827` | Cards and footer |
| `--color-surface2` | `#161f30` | About section |
| `--color-accent` | `#4f8ef7` | Blue — CTAs, borders, links |
| `--color-teal` | `#00d4aa` | Teal — tags, labels, highlights |
| `--color-text` | `#f0f4ff` | Primary body text |
| `--color-muted` | `#7a8ba0` | Subtitles and captions |

Responsive breakpoints: `768px` (tablet) and `480px` (mobile). Cards reflow automatically via CSS Grid `auto-fit`.

---

## Deployment

Because the site is a single static file, it can be deployed to any static host with no configuration:

- **GitHub Pages** — push to a repo and enable Pages on `main`
- **Netlify / Vercel** — drag and drop the `agent_web/` folder
- **Any web server** — copy `index.html` to the server's public directory
