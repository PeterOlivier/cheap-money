# Cheap Money — Full Spec

## The One-Liner
A single-page microsite that reframes equity fundraising as a loan — showing founders the effective annual interest rate they're paying when they sell equity.

---

## Why This Works

You need $12M for a production line. You could finance the equipment, take on project debt, or sell equity. Equity feels like the cleanest option — no monthly payments, no covenants, no collateral. But equity has a cost, and for founders building physical businesses, where every dollar buys something real (steel, concrete, tooling, permits), understanding that cost isn't abstract — it's the difference between an efficient capital stack and an expensive one.

This site makes the cost tangible: one number, the effective APR of your equity raise.

---

## Target User

**Primary: Founders building physical businesses who need capital for real stuff.**

- Building hardware, manufacturing, infrastructure, energy, climate tech, food/ag, construction
- They need money to buy steel, concrete, equipment, tooling, factory space, raw materials
- Their capital needs are large and lumpy — a $5M equipment order, a $20M factory buildout
- They understand project finance, debt, and equipment leasing because their industries use it
- They're comparing equity against alternatives: equipment financing (8-12%), SBA loans (6-10%), project debt, revenue-based financing
- They think in terms of cost per unit, margin per widget, payback period — not "blitzscaling"
- They find equity uncomfortable because they come from industries where you finance assets with debt, not ownership
- They're pragmatic, not ideological — they'll take equity if it's the right tool, but they want to know the real cost

**Secondary audiences:**
- Climate tech / hard tech founders comparing VC against project finance
- Investors in physical businesses who want a framework for conversations with founders
- CFOs and finance leads at growth-stage hardware/manufacturing companies
- Advisors and accelerators focused on physical industries (Greentown Labs, IACMI, MFG accelerators)

---

## Design Philosophy

### Tone: Practical, Not Preachy
The site is not anti-VC. It's a tool for people who buy things — equipment, materials, factory space — and want to finance those purchases efficiently. The tone is a sharp operations person who thinks in dollars-per-unit, not a finance professor.

Think: the CFO you wish you had, pulling up a napkin at lunch and saying "here's what that round actually costs per year."

### Voice Examples
- **Yes:** "You need $12M for a production line. Here's what it costs to finance it with equity."
- **Yes:** "Equipment financing: 10%. Your Series A: 47%."
- **Yes:** "Every dollar of equity has a cost. Know it before you spend it."
- **No:** "Founders should carefully consider the cost of capital before raising equity."
- **No:** "VC is a bad deal for most founders."
- **No:** "Equity is the most expensive money you'll ever raise." (too preachy)

Use the language these founders actually use:
- "cost per unit," "margin," "payback period"
- "capex," "equipment financing," "project debt"
- "what does this capital actually cost me?"
- "I could lease this equipment for 10% — why am I selling equity?"
- "bill of materials," "tooling costs," "production run"

Avoid: startup-Twitter language ("blitzscaling," "rocketship"), investor jargon (WACC, CAPM), and any framing that assumes the reader is building software.

### Visual Direction
- **Light theme** — clean, bright, trustworthy. Think engineering spec sheet, not hacker terminal.
- **Warm white / off-white background**, dark text, clear hierarchy
- **One accent color** for the rate number — something industrial: deep blue, or construction orange for high rates
- **The rate number is the hero** — 4-5rem, impossible to miss, color-coded green→amber→orange→red
- **Two interactive x-y charts** as inputs — the draggable dot mechanic is the core UX
- **Heatmaps** on both charts give ambient context without requiring reading
- **No framework** — vanilla HTML/CSS/JS, instant load, zero dependencies
- **Total page weight target: <50KB**
- **Typography:** Clean sans-serif (system stack: -apple-system, Segoe UI, etc.) with monospace for numbers only. Feels like a well-designed engineering tool, not a startup landing page.

### Design Inspirations
- **neal.fun** — one big interactive concept, addictive to play with, highly shareable
- **pudding.cool** — data made visceral through interaction
- **Government engineering spec sheets** — clean, functional, information-dense but legible
- **Stripe's documentation** — light theme, clear typography, numbers that pop
- **Construction estimating software** — practical, no-nonsense, built for people who build things

---

## Site Structure (Single Page, Top to Bottom)

### 1. Hero / Hook
**Large text, centered, above the fold.**

> # You need to build a factory.
> # How much does the money cost?
>
> _Equipment financing charges 10%. A bank loan charges 8%. What does equity charge? More than you think._

No navigation. No logo. Just the question.

### 2. The Rate (Always Visible)
A sticky or prominent display showing:

```
EFFECTIVE ANNUAL INTEREST RATE
        43.7%
More expensive than credit card debt.
```

Color-coded: green (<15%) → yellow (15-30%) → orange (30-50%) → red (50%+)

Below the number, one line of context:
- <0%: "Investors lost money. Equity was free."
- <15%: "Cheaper than venture debt."
- 15-30%: "Comparable to credit card APR."
- 30-50%: "More expensive than any traditional lending."
- 50%+: "The most expensive money you'll ever raise."

And a concrete summary:
> "Investor puts in **$5M**, gets back **$40M** — an **8.0x** return."

### 3. Chart 1 — The Round
**Interactive x-y chart. Drag the dot.**

- **X-axis:** Amount raised ($500K → $50M)
- **Y-axis:** Pre-money valuation ($1M → $200M)
- **Heatmap background:** dilution intensity (darker = more dilution)
- **Contour lines:** iso-dilution curves (10%, 20%, 30%, 40%, 50%)
- **Readout above chart:** "Raising **$5M** at **$20M** pre-money → **20%** dilution"

### 4. Chart 2 — The Exit
**Interactive x-y chart. Drag the dot.**

- **X-axis:** Time to exit (1 → 15 years)
- **Y-axis:** Exit valuation ($10M → $2B, log scale)
- **Heatmap background:** effective rate zones (green → red)
- **Contour lines:** iso-rate curves (10%, 15%, 25%, 40%, 60%, 100%)
- **Readout above chart:** "Exit at **$200M** in **7 yrs** → investor gets **$40M** (8.0x)"

### 5. The Comparison Strip
A single horizontal bar putting the rate in context against familiar benchmarks — the ones these founders actually compare against:

```
[SBA loan 7%] [equipment lease 10%] [venture debt 14%] [mezzanine 18%] [▼ YOUR EQUITY: 44%] [hard money 25%]
```

The user's rate is marked on this spectrum. This is the moment — seeing your equity cost plotted next to the equipment financing you could have used instead.

### 6. The Flip Side (Intellectual Honesty)
A short section that acknowledges when equity makes sense. Builds credibility with a pragmatic audience.

> **Equity doesn't require collateral.**
> If your factory isn't built yet, you can't pledge it. Pre-revenue companies often can't qualify for equipment financing or project debt. Equity fills the gap before assets exist.
>
> **Equity absorbs downside risk.**
> If the project fails, there's no loan to repay. Debt on a failed factory is brutal. Equity investors share the loss.
>
> **Some capital needs can't be debt-financed.**
> R&D, hiring, regulatory approvals, working capital during buildout — these aren't assets a lender will underwrite.
>
> **So what's the point?**
> Most physical businesses use a capital stack: some equity, some debt, some equipment financing. Know the effective cost of each piece. Don't use 44% equity money to buy a machine you could lease at 10%.

### 7. The Math (Collapsible)
For the sophisticated user who wants to verify:

```
r = (E / (V + A))^(1/T) − 1

Where:
  A = Amount raised
  V = Pre-money valuation
  E = Exit valuation
  T = Years to exit
  V + A = Post-money valuation
```

One paragraph explaining the derivation. Link to the source code (it's all in the HTML).

### 8. Footer
Minimal. One line:

> Made by [your name]. No tracking. No cookies. View source.

---

## Interaction Design Details

### The Draggable Dot
- **Click anywhere** on a chart to jump the dot there
- **Drag** for continuous adjustment
- **Crosshairs** follow the dot (dashed lines to both axes)
- Values **snap** to sensible increments (amount: $500K steps; valuation: $1M steps; time: 1yr; exit: varies by magnitude)
- **Touch-friendly** — works on mobile with pointer events
- Cursor: crosshair over the chart area

### Heatmaps
- Rendered as canvas pixel grids (60x60 resolution is sufficient)
- Round chart: intensity = dilution percentage (darker red = more dilution)
- Exit chart: intensity = effective rate (green→yellow→orange→red gradient)
- Contour lines drawn on top at key thresholds with faint labels

### Responsiveness
- Two charts side-by-side on desktop (current layout)
- Stacked on mobile (<768px)
- Rate display shrinks but stays prominent
- Charts maintain aspect ratio (~1.3:1)

### Shareability
- The URL should encode state: `?a=5&v=20&t=7&e=200` so people can share their specific scenario
- OG meta tags with a dynamic description: "Raising $5M at $20M pre-money with a $200M exit in 7 years = 43.7% APR"
- The rate display should be screenshot-friendly — high contrast, no clutter around it

---

## Copy Inventory

### Headlines to test:
1. "You need to build a factory. How much does the money cost?"
2. "Equity has no monthly payments. It's still not free."
3. "What's the APR on your Series A?"
4. "You could lease that equipment at 10%. Your equity costs 47%."
5. "The most expensive way to buy a machine."
6. "Know the cost before you pour the concrete."

### Comparison context lines:
- "Cheaper than an SBA loan." (rate < 7%)
- "Equipment financing territory." (7-12%)
- "Venture debt territory." (12-18%)
- "Mezzanine debt territory." (18-30%)
- "More expensive than any debt instrument." (30-50%)
- "You're paying more than a hard money lender charges." (50%+)
- "This would be illegal if it were a loan." (100%+)

---

## Technical Spec

### Stack
- Single `index.html` file
- Vanilla CSS (no preprocessor)
- Vanilla JS (no framework, no build step)
- Canvas API for charts
- Zero external dependencies

### Performance Targets
- Total page weight: <50KB
- Time to interactive: <500ms
- No layout shift
- No web fonts (system monospace stack)

### URL State
- Read/write query params on load and on input change
- `?a=5&v=20&t=7&e=200`
- Use `replaceState` (not `pushState`) to avoid polluting browser history

### Meta Tags
- `og:title`: "Is Equity Cheap Money?"
- `og:description`: Dynamic — "Raising $Xm at $Ym pre → Z% effective APR"
- `og:image`: Static preview image (can be generated once, doesn't need to be dynamic)
- `twitter:card`: summary_large_image

---

## What to Build Next (in order)

1. **Light theme rebuild** — swap dark theme for clean, bright, industrial-feeling design
2. **Hero copy** — rewrite with physical-business framing (factories, equipment, materials)
3. **Comparison strip** — horizontal benchmark bar (SBA loan → equipment lease → venture debt → mezzanine → your equity) with the user's rate marked
4. **The Flip Side section** — when equity makes sense (pre-asset, R&D, downside absorption)
5. **URL state** — encode inputs in query params so scenarios are shareable
6. **Collapsible math section** — for the CFO who wants to verify
7. **OG meta tags** — for link previews when shared
8. **Mobile polish** — test and refine stacked layout
