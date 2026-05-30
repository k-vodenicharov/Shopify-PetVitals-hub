# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

PetVitals Hub is a Shopify theme for a DTC pet wellness brand. The current hero product is **Brook** — a ceramic cat water fountain (€64.99), targeting the EU market. The store is built on a third-party Shopify theme (referenced as "Rebel" in code comments) with substantial custom sections and styles layered on top.

## Shopify Theme Commands

There is no build step — files are Liquid/CSS/JS and are deployed directly to Shopify.

```bash
# Preview theme locally against live store data
shopify theme dev --store=<store-handle>.myshopify.com

# Push all changes to the active theme
shopify theme push

# Push to a specific theme (by ID)
shopify theme push --theme=<theme-id>

# Pull the latest theme from Shopify
shopify theme pull
```

The Shopify MCP tools (`mcp__claude_ai_Shopify__*`) are available and preferred for store data operations (products, collections, orders, GraphQL mutations).

## Architecture

### Directory Structure

| Directory | Purpose |
|-----------|---------|
| `sections/` | Liquid section files (rendered on pages) |
| `blocks/` | Reusable Liquid block components |
| `assets/` | CSS, JS, and SVG files |
| `templates/` | JSON templates that assemble sections per page type |
| `config/` | Theme settings schema and data |
| `locales/` | Translation strings |

### Custom PVH Layer vs. Base Theme

All custom PetVitals work is namespaced with the `pvh-` prefix:

- **CSS**: `assets/petvitals-custom.css` — single source of truth for all custom styles. Loaded with `{{ 'petvitals-custom.css' | asset_url | stylesheet_tag }}` at the top of each custom section. Contains CSS tokens (`--pvh-*`), all component classes, and responsive breakpoints. `petvitals-cleanup-v1.css` is intentionally empty — do not write styles there.
- **Custom sections**: `sections/petvitals-home.liquid`, `sections/pvh-product-lp.liquid`, `sections/pvh-product-sticky-atc.liquid`, `sections/pvh-announcement-bar.liquid`, `sections/pvh-product-video.liquid`

Base theme sections live in `sections/` alongside custom ones but do not use the `pvh-` prefix.

### Design System Tokens

All PVH styles use CSS custom properties defined in `:root` inside `petvitals-custom.css`. Never hardcode color, spacing, or typography values — always use `--pvh-*` tokens.

| Token | Value | Use |
|-------|-------|-----|
| `--pvh-white` | `#FAFAF8` | Page background (warm off-white) |
| `--pvh-black` | `#0D1B12` | Headings, primary text |
| `--pvh-green` | `#2D6A4F` | Primary accent, CTAs |
| `--pvh-green-dark` | `#1B4332` | Dark sections (trust bar, CTA, announcement) |
| `--pvh-green-mid` | `#52B788` | Secondary accent |
| `--pvh-green-pale` | `#EAF4EE` | Icon backgrounds |
| `--pvh-green-tint` | `#F4FAF6` | Alternate section backgrounds |
| `--pvh-muted` | `#6B7B72` | Body text, captions |
| `--pvh-border` | `#E2ECE6` | Dividers, card borders |

### Scroll-enter Animation Pattern

Custom sections use a CSS/JS scroll-reveal pattern. Add `data-pvh-enter` to any element; the IntersectionObserver in the section's `<script>` adds `.is-visible` when it enters the viewport. CSS transitions handle the fade-up. No perpetual animations on any interactive element.

### Product Page Architecture (`templates/product.json`)

The product page assembles four sections in order:
1. `product-information` — base theme section with custom blocks (hook copy, trust badges, buy buttons)
2. `pvh-product-lp` — custom long-form landing page (stats strip, feature deep-dive, reviews, comparison table)
3. `product-recommendations` — base theme cross-sell grid
4. `pvh-product-sticky-atc` — sticky ATC bar that appears after the hero scrolls out of view

### Homepage Architecture (`templates/index.json`)

The homepage uses `sections/petvitals-home.liquid` — a single, fully self-contained section with inline `<script>` and `{% stylesheet %}` that renders the entire page (hero, trust bar, problem/solution, how-it-works, benefits, guarantee, FAQ, CTA, sticky bar).

The homepage section hardcodes the Brook product via `{% assign product = all_products['brook'] %}`.

### AJAX Add-to-Cart Pattern

Custom ATC buttons use the class `pvh-ajax-atc` and a `data-variant="<variant_id>"` attribute. The JS in `petvitals-home.liquid` (and duplicated in `pvh-product-sticky-atc.liquid`) handles the `/cart/add.js` fetch and updates cart count across multiple theme-specific selectors.

## Frontend Libraries (CDN Only — No npm)

All libraries are loaded via CDN. Never suggest npm install or local versions of these.

| Library | Version | Purpose |
|---------|---------|---------|
| GSAP | 3.12.5 | Scroll animations, hover effects |
| ScrollTrigger | 3.12.5 | GSAP plugin, scroll-driven animations |
| Three.js | 0.165.0 | WebGL hero canvas on homepage |
| Google Fonts | — | Fraunces (headings) + Inter (body) |

## JavaScript Patterns

- All section JS wrapped in **IIFEs** — `(function() { ... })();` — no ES modules, no bundler syntax
- Cart operations: `/cart/add.js`, `/cart.js`, `/?sections=` (Sections Rendering API)
- Cross-component communication: `CustomEvent` with names `cart:update`, `pvh:cart:added`
- Scroll animations: `IntersectionObserver` for scroll-enter, `requestAnimationFrame` loop for Three.js/WebGL
- Sticky ATC visibility: `IntersectionObserver` on the hero section

## WebGL Hero (petvitals-home.liquid)

The homepage hero is a WebGL canvas built with Three.js. **Do not modify the shader or Three.js setup unless explicitly instructed.**

- `Three.js WebGLRenderer` + `OrthographicCamera`
- Custom GLSL fragment shader: radial sine-wave displacement + chromatic aberration
- 120 ambient floating particles via `PointsMaterial`
- `uMouse` uniform updated via `mousemove` with lerp smoothing
- Render loop via `requestAnimationFrame`

## Localization

User-facing strings in base theme sections use `t:` keys resolved from `locales/en.default.json`. Custom PVH sections use hardcoded English strings directly in the Liquid — do not introduce `t:` keys into custom sections unless explicitly requested.

## Brand & Copy Rules

See `.claude/BRAND_GUIDELINES.md`, `.claude/PROJECT_RULES.md`, and `.claude/CONVERSION_STANDARDS.md` for full guidance. Key rules:

- **Never use:** "Premium Quality", "Best Product", "Amazing Solution", "Must Have", "Revolutionary", "High Quality"
- Copy must be benefit-driven, emotionally specific, and easy to scan
- No fake urgency, fake scarcity, or spammy tactics
- Every change should answer: *"Does this increase trust and make the brand feel more real?"*
- Fonts: Fraunces (headings), Inter (body) — loaded via Google Fonts

## Never Introduce

The following are intentionally absent from this project. Do not suggest or add them under any circumstances:

- React, Vue, Alpine, or any component framework
- TypeScript
- npm, `package.json`, `node_modules`, or any build tooling (no Webpack, Vite, Rollup, etc.)
- Tailwind, Sass, PostCSS, or any CSS preprocessor/utility framework
- `t:` localization keys in custom PVH sections
- Hardcoded color, spacing, or typography values — always use `--pvh-*` tokens
- Inline styles on elements (use classes and tokens instead)
- Any modification to the WebGL shader unless explicitly requested