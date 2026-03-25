# CLAUDE.md — Scinnohub AI Drug Discovery Studio

Project context and decisions for Claude Code sessions.

---

## Project Goals & Positioning

**What it is:**
A marketing/landing page for Scinnohub, an AI-driven drug discovery platform. The site presents the studio's six research modules and directs visitors to the main website.

**Audience:**
Researchers, biotech collaborators, and potential partners looking for an overview of Scinnohub's AI capabilities.

**Positioning:**
- Futuristic, scientific, and credible — not a generic SaaS landing page
- Emphasises AI depth (generative design, agentic AI, ADMET) over marketing fluff
- The "AI+" badge on the Agentic AI card signals the flagship/most advanced module

**Out of scope (intentionally):**
- No user accounts, login, or backend
- No CMS or dynamic data
- No JavaScript frameworks or build pipeline

---

## Tech Stack & Rationale

| Concern | Choice | Rationale |
|---|---|---|
| Markup | HTML5, single `index.html` | Zero build step; open in browser or drop on any static host |
| Styling | CSS3, embedded `<style>` block | No framework overhead; full control; portable with the HTML file |
| Interactivity | Vanilla JS via `scroll-behavior: smooth` | Smooth-scroll only; no JS library needed |
| Fonts | Google Fonts CDN — Syne + Inter | Syne (geometric, futuristic) for headings; Inter (neutral, legible) for body |
| Icons | Unicode emoji | Zero HTTP requests; renders natively everywhere |
| Build tool | None | Single file — open directly or serve with `python3 -m http.server` |

**Why not a framework:**
The site is purely presentational with no state, routing, or dynamic data. React/Vue would add build complexity with zero benefit.

**Why a single file:**
Eliminates broken relative paths, simplifies deployment, and makes the whole site reviewable in one scroll.

---

## Directory Structure

```
agent_web/
├── index.html        # Complete site: markup, all CSS, scroll JS
├── CLAUDE.md         # This file — project context for Claude sessions
├── plan.md           # Original design and implementation plan
├── README.md         # Public-facing docs: features, stack, local setup
└── AgenticAI/
    ├── index.html    # Terminal sub-page: xterm.js UI + WebSocket client
    ├── server.js     # Express + ws + node-pty server (port 3001)
    ├── package.json  # Node.js dependencies
    ├── .nvmrc        # Pins Node.js 18 LTS (required for node-pty on Ubuntu 20.04)
    └── AgenticAI.md  # Technical documentation for the sub-page
```

No `src/`, `dist/`, or `assets/` directories. Everything lives in `index.html` (main site) or `AgenticAI/` (terminal sub-page).

---

## Current Progress

### Completed
- [x] Sticky nav header with blur backdrop and anchor links
- [x] Full-viewport hero with badge, title, subtitle, CTA button
- [x] Projects section — 6 cards in responsive CSS Grid
- [x] About section — two-column layout with stat blocks
- [x] Contact section — external link to scinnohub.com
- [x] Footer with copyright
- [x] Light futuristic theme: `#f5f8ff` base + 28px dot-grid texture
- [x] Typography fixes: safe `line-height` and `letter-spacing` values to prevent glyph clipping
- [x] `overflow: hidden` removed from cards (was clipping content)
- [x] "AI+" badge (deep blue, top-right) on Agentic AI card
- [x] Responsive breakpoints at 768px and 480px
- [x] README.md documenting features, stack, and local setup
- [x] Agentic AI sub-page (`AgenticAI/`) — full PTY terminal in the browser via xterm.js + WebSocket + node-pty
- [x] LAN access: server serves entire `agent_web/` tree; "Try now" link rewrites to `http://<hostname>:3001/AgenticAI/` via inline JS
- [x] Terminal input: click-to-focus + keystroke relay to PTY backend — fully interactive Claude Code session
- [x] Terminal sizing: 25vh with explicit `calc(25vh - 38px)` on container (no flex ambiguity for FitAddon)
- [x] CDN versions fixed: `@xterm/xterm@5.5.0`, `@xterm/addon-fit@0.10.0`, `@xterm/addon-web-links@0.11.0`
- [x] AgenticAI.md — technical documentation for the sub-page architecture

### TODOs
- [ ] Add individual detail pages or modal overlays per project card
- [ ] Add a navigation active-state indicator (highlight current section on scroll)
- [ ] Add a mobile hamburger menu (nav links currently wrap/compress on narrow screens)
- [ ] Animate hero title and cards on page load (CSS keyframe fade-in)
- [ ] Add Open Graph meta tags (`og:title`, `og:description`, `og:image`) for link previews
- [ ] Add a favicon
- [ ] Consider a "Publications / Research" section as content grows
- [ ] AgenticAI: add authentication before any public deployment
- [ ] AgenticAI: HTTPS/WSS support (switch to `https.createServer` + TLS cert)

---

## Key Design Decisions

### Light theme over dark
Switched from the original dark navy theme to a light `#f5f8ff` (cool-tinted off-white) base. Reason: better readability in lit environments, and a cleaner scientific aesthetic that differentiates from the sea of dark biotech sites. The dot-grid texture (`radial-gradient` at 9% opacity, 28px pitch) adds the blueprint/schematic feel without visual noise.

### Color palette
- **Accent blue `#2563eb`:** More vibrant and legible on white than the original `#4f8ef7`. Used for CTAs, borders, and links.
- **Cyan `#0891b2`:** Secondary accent for tags and labels — warm enough to contrast with the blue without clashing.
- **Deep navy `#1e3a8a`:** Reserved exclusively for the "AI+" badge to signal premium/flagship status.
- **Text `#0f172a`:** Near-black slate — WCAG AA compliant against all background surfaces.

### Typography rules (hard-won fixes)
- `line-height` on display headings must be `≥ 1.15` — Syne at 4rem with `line-height: 1.1` clips descenders.
- `letter-spacing` capped at `-0.01em` for headings — more negative causes glyph overlap in Syne Bold.
- Nav logo uses plain ASCII `o` in "Scinno" — a prior Cyrillic `о` caused inconsistent rendering.
- All `font-size` values use `clamp()` for fluid scaling without JS or breakpoint-specific overrides.

### Card grid
`repeat(auto-fit, minmax(240px, 1fr))` with no explicit column count. Cards reflow naturally at any viewport width without media queries. `overflow: hidden` was deliberately removed from cards after it clipped content on shorter viewports.

### "AI+" badge placement
Positioned absolutely (`top: 1rem; right: 1rem`) inside the card's `position: relative` container. Deep navy fill (`#1e3a8a`) distinguishes it from the electric blue accent used elsewhere. Applied only to the Agentic AI card to avoid badge inflation across all cards.

### About section dot-grid suppression
The about section uses `background-image: none` to override the body's dot-grid. This creates a clean visual pause between the dot-grid sections, improving scanability and preventing the grid from competing with the stat blocks.

### External link safety
The contact button and footer link both carry `target="_blank" rel="noopener noreferrer"` — prevents tab-napping and is the correct pattern for all external links.

---

## AgenticAI Sub-page

### Architecture
Browser (xterm.js) → WebSocket (`ws://host:3001/ws`) → Node.js Express (server.js) → node-pty → `claude` binary

### Running the terminal server
```bash
cd AgenticAI
source ~/.nvm/nvm.sh && nvm use 18   # Node 18 LTS required
npm start                             # → http://localhost:3001
```

Main site served at `/`, terminal sub-page at `/AgenticAI/`, WebSocket at `/ws`.

### Node.js version constraint
**Must use Node 18 LTS.** Node 24's V8 headers use C++20 `concept`/`requires` syntax that g++ 9 (Ubuntu 20.04) cannot compile, causing node-pty build failure. `.nvmrc` pins the project to `18`.

### xterm.js CDN versions (verified working)
| Package | Version | CDN path |
|---|---|---|
| `@xterm/xterm` | `5.5.0` | `.../lib/xterm.js` + `.../css/xterm.css` |
| `@xterm/addon-fit` | `0.10.0` | `.../lib/addon-fit.js` |
| `@xterm/addon-web-links` | `0.11.0` | `.../lib/addon-web-links.js` |

**Do not use `@xterm/xterm@5.3.0`** — this version does not exist on jsDelivr and causes `Terminal is not defined` at runtime.

### Terminal focus & input (critical decisions)
- `term.open(container)` must be called inside `requestAnimationFrame` so the container has real pixel dimensions; calling it earlier produces a 0×0 canvas.
- Focus uses both `term.focus()` AND `term.textarea.focus({ preventScroll: true })` — targeting xterm's internal capture element directly is more reliable than `term.focus()` alone.
- `#terminal-container` must NOT have `overflow: hidden` — it clips xterm's internal textarea and breaks pointer events.
- `.terminal-glass` uses `overflow: hidden` (not `overflow: clip`) for border-radius clipping.
- Terminal height: explicit `height: calc(25vh - 38px)` on `#terminal-container` (total glass 25vh minus 38px chrome bar) — flex-based sizing is unreliable for FitAddon measurement.

### LAN access
The "Try now" link on the Agentic AI card is rewritten at runtime:
```javascript
link.href = 'http://' + window.location.hostname + ':3001/AgenticAI/';
```
This ensures the correct machine IP is used regardless of which server/port delivers the main page.
