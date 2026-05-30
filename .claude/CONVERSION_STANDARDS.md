# PetVitals Hub — Conversion Standards

## Core Principle
Every element earns its place by doing one of three things: building trust, clarifying value, or reducing friction. If it does none of these, remove it.

## Page Hierarchy (What the Customer Must Feel)
1. **Safe** — this is a real brand, not a scam
2. **Understood** — they get my problem
3. **Convinced** — the product solves it specifically
4. **Confident** — I know what I'm buying and what happens after

## Homepage Section Order
The homepage (`petvitals-home.liquid`) renders in this order — do not reorder without reason:
1. Hero — emotional hook, single CTA
2. Trust bar — social proof numbers or press logos
3. Problem/solution — agitate the problem, introduce Brook
4. How it works — 3–4 steps, scannable
5. Feature deep-dive — benefits before specs
6. Reviews — real, specific, attributed
7. Guarantee — 30-day, no conditions
8. FAQ — top 5 objections answered
9. CTA section — closing offer, repeat buy button

## Product Page Section Order
Assembled via `templates/product.json`:
1. `product-information` — hook copy, trust badges, buy buttons (above the fold)
2. `pvh-product-lp` — stats strip → feature deep-dive → reviews → comparison table
3. `product-recommendations` — cross-sell (below main content)
4. `pvh-product-sticky-atc` — always visible after hero scrolls out

## CTA Rules
- Primary button text: action-led, specific. **"Add to Cart"** or **"Get Brook"** — never "Shop Now", "Buy", "Click Here"
- Every above-the-fold section must have exactly one primary CTA
- Sticky ATC bar must always be visible on mobile after scrolling past the hero
- Button colour: always `--pvh-green` background, `--pvh-white` text

## Trust Signals — Required Locations
| Signal | Where |
|--------|-------|
| Star rating + review count | Above the fold on product page |
| 30-day guarantee badge | Near the buy button |
| Free shipping threshold | Announcement bar + near buy button |
| Review cards (named, attributed) | Homepage + product LP |
| Packaging transparency note | Product description (already added) |

## Mobile Standards
- Body text minimum: 16px (1rem)
- CTA buttons minimum height: 48px
- Section padding minimum: 48px vertical on mobile
- No horizontal scroll — ever
- Sticky ATC bar must not cover content or overlap nav

## What to Avoid
| Pattern | Why |
|---------|-----|
| Countdown timers | Fake urgency — destroys trust |
| "Only X left in stock" (if fake) | Fake scarcity |
| Exit-intent popups | Friction, not conversion |
| Auto-playing video with sound | Aggressive, poor mobile UX |
| Walls of text | Nobody reads them |
| More than one primary CTA per section | Splits attention |
| Animations on CTA buttons | Distracting, not tested |

## Copy Formatting Rules
- Use **short paragraphs** — 2–3 sentences max
- Use **bullet points** for features/specs — never prose lists
- Use **bold** to highlight the key phrase in a paragraph, not entire sentences
- Headings should state a **benefit or outcome**, not a category label
  - ✅ "Your cat drinks 40% more water"
  - ❌ "Product Features"