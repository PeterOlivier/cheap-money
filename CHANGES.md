# Cheap Money — V2 Changes Spec

Based on feedback from two hardtech founder personas (Maria Chen, battery recycling TX; Jake Morales, modular concrete manufacturing OKC). Capital stack calculator excluded — handled elsewhere.

---

## Change 1: Typed Number Inputs (Inline Editable Readouts)

**Priority:** Highest — #1 friction point for both personas.

**Problem:** Founders know their exact numbers. Dragging a dot to hit "$80M pre-money" on a sqrt-scaled chart is imprecise and frustrating. Both personas asked for this independently as their top request.

**Implementation:**

Turn the bold values in each panel readout into inline editable fields. The current readouts are:

```
Raising $15M at $70M pre-money → 17.6% dilution
Exit at $2.0B in 3 yrs → investor gets $353M (23.5x)
```

Each bold value (`$15M`, `$70M`, `$2.0B`, `3 yrs`) becomes a clickable/tappable inline input:

- On click, the bold text becomes a small `<input type="text">` field, pre-filled with the current value, auto-selected.
- The founder types a number (plain number, e.g. `80` — the `$` and `M` are implied by context and shown as prefix/suffix around the input).
- On Enter or blur, the value is parsed, clamped to the valid range, the dot on the chart snaps to the new position, and everything recalculates.
- The input field is styled to look like the bold text, just with a subtle underline or bottom border to indicate editability. Minimal visual disruption.

**Styling:**
- Inline `<input>` styled with `font: inherit; font-weight: 600; color: var(--text); border: none; border-bottom: 1.5px dashed var(--border); background: transparent;`
- Width sized to content (use a hidden measuring span or `ch` units based on value length).
- On focus: border-bottom becomes solid `var(--accent)`.
- The `$` prefix and `M`/`B`/`yrs` suffix remain as static text outside the input.

**Bidirectional sync:**
- Type a number → dot moves on chart → all outputs update.
- Drag dot on chart → input value updates → all outputs update.
- URL params update in both cases (already works for drag).

**Parsing rules:**
- Accept plain numbers: `80` → $80M, `2.5` → $2.5B (on exit val chart, if > 999 treat as millions, format as B).
- Accept shorthand: `80m` or `80M` → $80M, `2b` or `2B` → $2,000M.
- Reject garbage gracefully — revert to previous value on invalid input.

**Files changed:** `index.html` — modify the readout innerHTML generation in `drawRoundChart()` and `drawExitChart()`, plus add input event handlers and CSS for the inline inputs.

---

## Change 2: Exit Valuation Range Mode

**Priority:** High — both personas flagged single-point exit as "false precision."

**Problem:** Exit valuation is the least-knowable input. Showing a single rate from a single exit value undermines trust. Maria: "false precision." Jake: "I don't know if I'm exiting at $200M or $500M or zero."

**Implementation:**

Add a toggle beneath the exit chart panel title:

```
THE EXIT          [Point ● | Range ○]
```

**Point mode** (default): Exactly as today. One dot, one exit valuation.

**Range mode:** The single dot becomes two dots on the exit chart, representing a low and high exit valuation (same time-to-exit for both). They share the same X position (time) but have different Y positions (exit val low, exit val high).

- A vertical band/stripe is drawn between the two dots, with a subtle fill matching the heatmap gradient.
- The readout changes from:
  ```
  Exit at $2.0B in 8 yrs → investor gets $353M (23.5x)
  ```
  to:
  ```
  Exit between $300M and $2.0B in 8 yrs → rate: 12%–48%
  ```
- The main rate display shows the range: **12% – 48%** with the low end colored by its rate class and the high end colored by its rate class.
- The sticky bar also shows the range.
- The comparison strip shows a range marker instead of a single "YOU" marker — a horizontal bar spanning from the low to high rate.

**State:** Two new state vars: `exitValLow` and `exitValHigh`. In point mode, only `exitVal` is used (existing behavior). In range mode, `exitValLow` and `exitValHigh` replace it. URL params: `el` and `eh` for range mode, `e` for point mode. If `el` and `eh` are present in URL, auto-enable range mode.

**Interaction:** In range mode, drag either dot independently. They are constrained to the same X (time) value. The lower dot cannot go above the upper dot and vice versa.

**Toggle styling:** Small pill toggle, matching the site's typography. Use `font-size: 0.72rem; text-transform: uppercase; letter-spacing: 0.12em;` to match `.panel-title`.

---

## Change 3: Expanded Comparison Bar

**Priority:** Medium-high — serves the climate/hardtech audience specifically.

**Problem:** The comparison bar is the single most impactful element on the page (both personas said so), but it's missing the most relevant low-cost instruments for hardtech: DOE loans, project finance, green bank financing.

**Implementation:**

Expand the benchmark markers on the comparison strip from:

```
7% SBA Loan | 10% Equip. Lease | 14% Venture Debt | 20% Mezzanine | 50%+
```

to:

```
3% DOE / Green Bank | 7% SBA Loan | 10% Equip. Lease | 14% Venture Debt | 20% Mezzanine | 50%+
```

**Details:**
- Add a new leftmost benchmark: `3%` labeled `DOE / Green Bank`. This represents DOE Loan Programs Office (2-3%) and state green bank financing (3-5%).
- The strip gradient and scale need to accommodate the lower end. Currently the strip maps ~5% to ~55%+. Expand the left end to start at ~2%.
- Each benchmark label gets a small info tooltip on hover/tap: a single sentence explaining what the instrument is and who qualifies. Implemented as a `title` attribute or a minimal CSS tooltip.

**Tooltip text:**
- DOE / Green Bank: "DOE Loan Programs Office and state green banks. 2-5% for energy, manufacturing, and infrastructure projects."
- SBA Loan: "SBA 7(a) and 504 loans. 6-10% for small businesses with collateral."
- Equip. Lease: "Equipment financing and leasing. 8-12% for asset-backed purchases."
- Venture Debt: "Venture debt from specialty lenders. 12-16%, typically requires an equity round first."
- Mezzanine: "Mezzanine debt. 18-22%, subordinated to senior debt, often with warrants."

**Files changed:** `index.html` — modify the `.strip-benchmarks` HTML, adjust the strip canvas drawing function to accommodate the wider range, add tooltip CSS.

---

## Change 4: Scenario Save & Compare

**Priority:** Medium-high — transforms a one-visit novelty into a recurring tool.

**Problem:** Founders are in multi-month negotiation processes. They want to compare "Offer A at $30M pre" vs "Offer B at $35M pre" side by side. Currently the tool is a one-shot gut check with no reason to return.

**Implementation:**

Add a "Scenarios" section below the comparison strip and above the flipside section.

**UI:**

```
┌──────────────────────────────────────────────────────┐
│  SAVED SCENARIOS                          [+ Save]   │
│                                                      │
│  (empty state: "Save your current scenario to        │
│   compare it against other offers.")                 │
│                                                      │
│  ┌─────────────────────────────────────────────────┐ │
│  │ Scenario A (editable name)          [Load] [×]  │ │
│  │ $12M at $30M pre → exit $300M / 8yr → 28% APR  │ │
│  └─────────────────────────────────────────────────┘ │
│  ┌─────────────────────────────────────────────────┐ │
│  │ Scenario B (editable name)          [Load] [×]  │ │
│  │ $10M at $35M pre → exit $300M / 8yr → 22% APR  │ │
│  └─────────────────────────────────────────────────┘ │
│                                                      │
└──────────────────────────────────────────────────────┘
```

**Behavior:**
- **[+ Save]** button: saves the current state (amount, valuation, exitTime, exitVal, and optionally exitValLow/exitValHigh if in range mode) to `localStorage` with a default name like "Scenario A", "Scenario B", etc.
- Max 5 saved scenarios.
- Each saved scenario shows a one-line summary: `$12M at $30M pre → exit $300M / 8yr → 28% APR`. The rate is color-coded.
- **[Load]** restores that scenario's values to the active charts.
- **[×]** deletes the scenario.
- Scenario name is editable inline (click to edit, same pattern as the number inputs).
- Scenarios persist in `localStorage` under key `cheapmoney_scenarios`.

**Storage format:**
```json
[
  { "name": "Series A — Offer A", "a": 12, "v": 30, "t": 8, "e": 300 },
  { "name": "Series A — Offer B", "a": 10, "v": 35, "t": 8, "e": 300 }
]
```

**Styling:** Same card style as `.panel` — white bg, 1px border, rounded corners. Each scenario row is a flex row with name, summary, and action buttons. Keep it minimal.

---

## Change 5: Shareable Result Cards

**Priority:** Medium — drives virality. Both personas would screenshot; Maria specifically wants a tweetable card.

**Problem:** The site's growth depends on sharing. Currently founders screenshot the page manually. A designed, shareable card with their specific numbers would spread on Twitter/X and founder Slacks.

**Implementation:**

Add a "Share" button in the rate display section (below the rate comparison text):

```
[Share Your Result]
```

On click, generate a canvas-based card image (1200×628px, Twitter/X optimal) containing:
- The effective rate in large type, color-coded.
- One-line summary: "Raising $12M at $30M pre. Exit at $300M in 8 years."
- The comparison context: "Equipment lease: 10%. This equity: 28%."
- URL: `cheap-money.vercel.app`
- Subtle branding: "cheapmoney.vercel.app" in small type at bottom.

**Card design:**
- White background, matching the site's aesthetic.
- Rate number centered, large, color-coded.
- Clean typography matching the site's system font stack.
- No decorative elements — just the data.

**Share flow:**
1. Click "Share Your Result."
2. Canvas generates the card.
3. Use `navigator.share()` if available (mobile), with the image as a file attachment plus the URL with params.
4. Fallback: `canvas.toBlob()` → create a download link for the image + copy the param-encoded URL to clipboard. Show a small toast: "Image downloaded. Link copied."

**OG meta tags:** Already present in the `<head>`. For dynamic previews (when someone shares a param-encoded URL), the OG tags remain static (we can't do server-side rendering on a static site). The share card image is the dynamic piece — founders share the image directly.

---

## Change 6: Attribution Footer

**Priority:** Low effort, medium trust impact.

**Problem:** Both personas asked "who made this?" Jake: "I don't know what the angle is." Maria: "Context matters." The anonymity is mostly good but a brief line builds trust.

**Implementation:**

Update the footer from:
```
No tracking. No cookies. View source.
```
to:
```
No tracking. No cookies. View source. Built by [Your Name].
```

Keep it to one line. No company name, no sales pitch, no "schedule a call." Just a name (linked to Twitter/X or personal site) so people know a real person made this and there's no hidden agenda.

---

## Change 7: Chart Flow Indicator (Step Labels)

**Priority:** Low effort, reduces confusion.

**Problem:** Maria noted the relationship between the two charts isn't visually explicit. You have to infer they both feed the same rate number.

**Implementation:**

Change the panel titles from:
```
THE ROUND          THE EXIT
```
to:
```
STEP 1 — THE ROUND          STEP 2 — THE EXIT
```

And update the hint text from:
```
Click or drag the dot on each chart to set your scenario.
```
to:
```
Set your raise, then your exit. Drag the dot or click the numbers to edit.
```

This makes the sequence explicit and also hints at the new inline-editable numbers.

---

## Change 8: Broader Industrial Language

**Priority:** Tiny effort, inclusivity polish.

**Problem:** Maria noted the language is slightly too manufacturing-specific. "Factories" excludes recycling, processing, energy infrastructure, defense.

**Implementation — specific text changes:**

1. **Subtitle** — change:
   ```
   You price every input — materials, labor, equipment, land.
   ```
   to:
   ```
   You price every input — materials, labor, equipment, facilities.
   ```
   ("facilities" is broader than "land" — covers factories, plants, processing lines, labs)

2. **Flipside section, first item** — change:
   ```
   If the factory isn't built, you can't pledge it.
   ```
   to:
   ```
   If the facility isn't built, you can't pledge it.
   ```

3. **Flipside section, second item** — change:
   ```
   Debt on a failed factory is brutal.
   ```
   to:
   ```
   Debt on a failed project is brutal.
   ```

4. **Rate context** (in `rateContext()` function) — change:
   ```
   You could lease a fleet of excavators for less.
   ```
   to:
   ```
   You could lease a fleet of equipment for less.
   ```

These are small swaps that broaden the appeal from "factory founders" to "physical-world founders" without losing the industrial feel.

---

## Change 9: Exit Chart Y-Axis Scale Improvement

**Priority:** Small effort, meaningful for non-unicorn founders.

**Problem:** Jake noted that $10B top of axis makes $200-500M exits feel like a tiny area at the bottom. Most hardtech companies exit $100M-$1B.

**Implementation:**

Reduce the exit chart Y-axis max from `$10B` to `$3B`. Update `exit_yRange` from `[10, 10000]` to `[10, 3000]`.

This makes the $100M-$1B zone (where most hardtech exits happen) occupy roughly the middle third of the chart instead of a cramped bottom sliver. Founders building toward $200-800M outcomes will have much better precision when placing their dot.

If a founder wants to model a $5B+ exit, the typed input (Change 1) lets them enter any number — the dot will pin to the top of the chart but the math still works.

---

## Implementation Order

All changes are to `index.html` (single file, no dependencies).

| Order | Change | Depends On | Est. Lines Changed |
|-------|--------|------------|-------------------|
| 1 | Change 8: Broader language | None | ~5 lines, text only |
| 2 | Change 7: Step labels + hint text | None | ~4 lines, text only |
| 3 | Change 6: Attribution footer | None | ~1 line |
| 4 | Change 9: Exit Y-axis max | None | ~1 line (`exit_yRange`) |
| 5 | Change 3: Expanded comparison bar | None | ~20 lines |
| 6 | Change 1: Typed number inputs | None | ~80 lines (CSS + JS + HTML gen) |
| 7 | Change 2: Exit range mode | Change 1 | ~100 lines |
| 8 | Change 4: Scenario save/compare | Change 1 | ~80 lines |
| 9 | Change 5: Shareable result cards | Changes 1-3 | ~60 lines |

Changes 1-5 (quick text/scale changes) can be done in a single pass. Changes 6-9 are progressively more complex and should be built and tested sequentially.
