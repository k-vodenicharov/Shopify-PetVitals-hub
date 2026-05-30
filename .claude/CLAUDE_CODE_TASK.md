# CLAUDE_CODE_TASK.md — PetVitals Hub Pre-Launch Audit

> Hero product: **Brook — Ceramic Cat Fountain**
> Store: `petvitalshub-com.myshopify.com` (EUR / Bulgaria)
> Status of this file: tasks marked `[VERIFIED]` were confirmed against live Shopify data.
> Tasks marked `[SELF-AUDIT]` could not be checked remotely — **Claude Code must run them against the local repo and report findings before fixing.**

---

## SECTION A — Shopify data fixes `[VERIFIED]`
> These are Shopify Admin actions, not code. Confirm intent before changing live data.

- [ ] **A1. Resolve the price conflict (CRITICAL).** Spec says €64.99; live variants are €89.99 / €94.99 / €99.99 / €109.99. Decide the correct price, then either update variants in Admin or confirm the €64.99 figure was wrong. **Do not launch until reconciled.**
- [ ] **A2. Set Brook to `DRAFT`** until the full audit signs off (currently `ACTIVE` and buyable).
- [ ] **A3. Replace placeholder inventory** (total 53,375 across 8 variants). Set real tracked quantities or disable inventory tracking — do not ship on import defaults.
- [ ] **A4. Fix image alt text.** Featured image alt is empty; others are hash strings. Write descriptive, keyword-relevant alt text for all 5 images.
- [ ] **A5. Substantiate claims.** Keep a source on file for "up to 99.9% of waterborne bacteria" and any "drinks X% more water" headline before running ads.

---

## SECTION B — Verify project files exist `[SELF-AUDIT]`
- [ ] **B1.** Confirm these exist and are readable, report any missing:
  - `.claude/CLAUDE.md`
  - `.claude/PROJECT_RULES.md`
  - `.claude/CONVERSION_STANDARDS.md`
  - `.claude/BRAND_GUIDELINES.md`
  - `sections/petvitals-home.liquid`
  - `sections/pvh-product-lp.liquid`
  - `assets/petvitals-custom.css`

---

## SECTION C — Technical violations `[SELF-AUDIT]`
Run each command from repo root. Any non-empty output = a blocker to fix.

- [ ] **C1. Inline styles** in the two sections:
  ```bash
  grep -nE 'style="' sections/petvitals-home.liquid sections/pvh-product-lp.liquid
  ```
- [ ] **C2. Hardcoded hex colors** (must live in `assets/petvitals-custom.css` only):
  ```bash
  grep -rnE '#[0-9a-fA-F]{3,6}\b' sections/petvitals-home.liquid sections/pvh-product-lp.liquid
  ```
- [ ] **C3. npm dependencies / external bundlers** sneaking into theme files:
  ```bash
  grep -rnE 'require\(|import .* from|node_modules|package\.json|cdn\.jsdelivr|unpkg\.com' sections/ assets/
  ```
- [ ] **C4. JavaScript outside a closed IIFE.** Confirm every `<script>` block wraps its logic in `(function(){ ... })();`. Flag any loose top-level JS or global leakage:
  ```bash
  grep -nE '<script' sections/petvitals-home.liquid sections/pvh-product-lp.liquid
  ```
  Then open each script block and confirm IIFE closure.

---

## SECTION D — Banned copy `[SELF-AUDIT]`
> Note: the live product description is CLEAN. These checks cover the **templates only**, which were not auditable remotely.

- [ ] **D1.** Case-insensitive scan for forbidden buzzwords:
  ```bash
  grep -rniE 'premium quality|best product|amazing solution|must.?have|revolutionary|high quality' sections/petvitals-home.liquid sections/pvh-product-lp.liquid
  ```

---

## SECTION E — Scammy / dark patterns `[SELF-AUDIT]`
Any match here = hard blocker per project rules.

- [ ] **E1. Countdown timers:**
  ```bash
  grep -rniE 'countdown|setInterval|deadline|timer|expires|ends in' sections/ assets/
  ```
- [ ] **E2. Fake stock / scarcity:**
  ```bash
  grep -rniE 'only .* left|stock left|selling fast|low stock|almost gone|spots? remaining' sections/ assets/
  ```
- [ ] **E3. Exit-intent popups:**
  ```bash
  grep -rniE 'exit.?intent|mouseleave|beforeunload|popup|modal.*exit' sections/ assets/
  ```

---

## SECTION F — Conversion & mobile requirements `[SELF-AUDIT]`
- [ ] **F1. Benefit-driven headings.** Open both sections; confirm primary `<h1>`/`<h2>` state an outcome (e.g. "Your cat drinks more"), not a feature or product name. List every heading that fails.
- [ ] **F2. CTA height ≥ 48px.** Inspect the add-to-cart / primary CTA rule in `assets/petvitals-custom.css`; confirm `min-height: 48px` (or equivalent) at mobile breakpoint.
- [ ] **F3. Minimum 16px body font on mobile.** Confirm no `font-size` below `16px` (or `1rem`) is applied to body/CTA text at mobile widths:
  ```bash
  grep -rnE 'font-size:\s*(1[0-5]px|0?\.[0-9]+rem|[0-9]px)' assets/petvitals-custom.css
  ```

---

## SECTION G — Sign-off
- [ ] All `[VERIFIED]` Shopify fixes applied.
- [ ] All `[SELF-AUDIT]` greps return clean (or fixes committed).
- [ ] Brook flipped back to `ACTIVE` only after A1–A5 are closed.

_Generated as part of the PetVitals Hub Pre-Launch Compliance & Conversion Audit. Sections C–F were not auditable remotely and must be confirmed locally before relying on them._
