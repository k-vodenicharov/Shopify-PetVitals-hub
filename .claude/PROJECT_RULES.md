# PetVitals Hub — Project Rules

## What This Project Is
A single-product DTC Shopify store selling Brook, a ceramic cat water fountain. Target market: EU cat owners, 25–45, who care about their pet's health and are willing to pay for quality over cheap plastic alternatives.

The store must feel like a legitimate, modern pet wellness brand — not a dropshipping template.

## The Single Deciding Question
Before every change, ask:
> **"Does this increase trust and make the brand feel more real?"**

If the answer is no or uncertain, don't make the change.

## Development Standards

### Always
- Use `--pvh-*` CSS tokens for all colour, spacing, and typography values
- Write section JS as IIFEs — `(function() { ... })();`
- Test changes on mobile viewport first (375px width minimum)
- Preserve the `data-pvh-enter` scroll-reveal pattern for new animated elements
- Keep `assets/petvitals-custom.css` as the single CSS source of truth
- Use the Shopify MCP tools for any store data operations (products, orders, GraphQL)

### Never
- Introduce npm, a build pipeline, or any frontend framework
- Hardcode hex values, pixel sizes, or font names outside of `petvitals-custom.css`
- Add `t:` localisation keys to custom PVH sections
- Touch the WebGL shader in `petvitals-home.liquid` unless explicitly instructed
- Write to `petvitals-cleanup-v1.css` — it is intentionally empty
- Use inline styles on HTML elements
- Add popups, overlays, or exit-intent scripts
- Duplicate JS logic across sections without flagging it (ATC JS is already duplicated in home + sticky ATC — do not add a third copy)

## File Ownership Rules
| File | Rule |
|------|------|
| `assets/petvitals-custom.css` | All custom CSS goes here. Never split into a new file without discussion. |
| `sections/petvitals-home.liquid` | Homepage only. Self-contained. Do not extract JS or CSS from it without discussion. |
| `sections/pvh-product-lp.liquid` | Product LP only. No homepage logic here. |
| `sections/pvh-product-sticky-atc.liquid` | Sticky bar only. Mirrors ATC JS from homepage — flag if logic diverges. |
| `petvitals-cleanup-v1.css` | Do not write here. Ever. |

## When Suggesting Changes
- State which file(s) will be affected before showing code
- If a change touches more than one section, list all affected files
- If you're unsure whether a change conflicts with existing JS or CSS, say so explicitly — do not guess
- Prefer surgical edits over rewrites — show the minimal change needed

## Deployment Reminder
There is no build step. Changes go live with:
```bash
shopify theme push
```
Always confirm the target theme ID before pushing to avoid overwriting the wrong theme.