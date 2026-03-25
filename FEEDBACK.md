# Site Improvement Recommendations
## From Two Hardtech Founder Personas

---

### The Personas

**Maria Chen, 34** — Battery recycling, Texas. Raised $6M seed, planning $25-40M Series A. Moderate financial sophistication. Climate-tech circles, reads Lux Capital newsletter.

**Jake Morales, 41** — Modular concrete manufacturing, Oklahoma City. Bootstrapped to $3M ARR. Considering first institutional round ($10-15M). Skeptical of VC culture. Knows construction finance cold, cap tables are new.

---

## Priority 1: High Impact, Both Personas Agree

### 1. Add typed number inputs alongside the charts
**Who wants it:** Both, emphatically. #1 improvement for both.
**Why:** Founders know their exact numbers. Dragging a dot to hit "$80M pre-money" is imprecise and frustrating. The charts are great for exploration; typed inputs are essential for precision.
**Recommendation:** Add small input fields above each chart (or inline in the readout text) that sync bidirectionally with the dot position. Type a number → dot moves. Drag dot → number updates.

### 2. Build a blended capital stack calculator
**Who wants it:** Both. Maria's #1 missing feature, Jake's #2 improvement.
**Why:** The real decision isn't "is equity expensive?" (the site already proves that). The real decision is "how do I split my total capital need across equity + debt + other instruments?" Neither founder is choosing between 100% equity and 100% debt — they're designing a capital stack.
**Recommendation:** Add a section below the current tool: "Build Your Capital Stack." Let the founder say "I need $25M total" and allocate slices to equity (at their scenario rate), equipment lease (10%), SBA loan (7%), venture debt (14%), etc. Show the blended cost. This is what makes the site a *tool* instead of a *novelty*.

### 3. Exit valuation sensitivity range
**Who wants it:** Both. Maria calls single-point exit "false precision." Jake says "I don't know if I'm exiting at $200M or $500M or zero."
**Why:** The exit valuation is the least-knowable input. Presenting a single rate from a single exit value undermines trust with sophisticated users.
**Recommendation:** Add a "range mode" toggle on the exit chart. Let the founder set a low and high exit valuation. Display the rate as a range: "Your effective rate: 22%–48%." This is more honest and more useful.

---

## Priority 2: High Impact, Strong Signal from One Persona

### 4. Add DOE loans, project finance, and green bank financing to the comparison bar
**Who wants it:** Maria, strongly. Jake would also benefit.
**Why:** For climate/hardtech founders, DOE LPO loans (2-3%) and state green bank financing (3-5%) are the most relevant low-cost alternatives. Omitting them makes the comparison bar incomplete for the exact audience the site targets.
**Recommendation:** Expand the comparison bar to include: DOE LPO (2-3%), Green Bank / State Programs (3-5%), SBA Loan (7%), Equipment Lease (10%), Venture Debt (14%), Project Finance (6-8%), Mezzanine (20%). Consider making the bar configurable — let founders check/uncheck which instruments are relevant to them.

### 5. Scenario comparison / save and revisit
**Who wants it:** Jake strongly (#1 bookmark reason), Maria agrees.
**Why:** Founders are in multi-month negotiation processes. They want to compare "Offer A vs Offer B vs blended approach C" side by side. Right now it's a one-visit gut check.
**Recommendation:** Let founders save 2-3 named scenarios (localStorage) and view them side by side. Even a simple table: "Scenario A: $12M at $30M pre = 28% | Scenario B: $10M at $35M pre = 22%." This transforms a novelty into a recurring tool.

### 6. Shareable result cards
**Who wants it:** Maria specifically wants a tweetable card. Jake would share the URL (which already encodes params — good).
**Why:** The site's virality depends on screenshots. A designed, shareable card with the founder's specific numbers would spread significantly on Twitter/X and founder Slacks.
**Recommendation:** Add a "Share your result" button that generates a clean card image: the effective rate in big type, the comparison bar, the key inputs. Optimized for Twitter/X dimensions. Pre-filled tweet text like: "My Series A equity costs 39% APR. My equipment lease costs 10%. [link]"

---

## Priority 3: Important Polish

### 7. "Who built this and why" — add minimal attribution
**Who wants it:** Both. Jake: "I don't know what the angle is." Maria: "Context matters."
**Why:** The anonymity is mostly a positive (no sales pitch vibes), but both founders want to know the maker isn't selling them something. A one-line attribution builds trust without breaking the tool-not-pitch energy.
**Recommendation:** Add a brief footer line: "Built by [name], [one-line context]. No product to sell. Open source." Keep it minimal.

### 8. Visual connector between the two charts
**Who wants it:** Maria. Jake didn't mention it but his confusion about the exit chart supports it.
**Why:** The mental model — "round inputs + exit inputs → rate" — isn't visually explicit. Founders have to infer that both charts feed the same number.
**Recommendation:** Add a subtle flow indicator: an arrow or line connecting the two chart cards to the sticky rate display. Or label them "Step 1: Your Round" and "Step 2: Your Exit" to make the sequence explicit.

### 9. Broaden industrial language slightly
**Who wants it:** Maria.
**Why:** "Factories" and "manufacturing" are great but exclude adjacent hardtech — recycling, processing, infrastructure, energy. The site should feel like "physical-world founders" not just "factory founders."
**Recommendation:** Swap some instances of "factory" for "facility," "plant," "processing line," "infrastructure." Add one or two references to energy/climate/defense alongside manufacturing.

### 10. Exit chart Y-axis zoom / better range for non-unicorns
**Who wants it:** Jake. $10B top of axis makes $200-500M exits feel tiny.
**Why:** Most hardtech companies exit for $100M-$1B, not $10B. The current scale compresses the realistic zone into a small area at the bottom of the chart.
**Recommendation:** Either reduce the max to $2-3B, or add a "zoom" toggle for the exit chart (e.g., "$10M–$500M" vs "$10M–$10B" range). Hardtech founders aren't building unicorns — they're building $200-800M infrastructure companies.

---

## Priority 4: Future Features (V2+)

### 11. Multi-round modeling
Both founders noted the tool assumes a single round. Real cap tables have seed → A → B, each with different dilution. A V2 could let founders model cumulative dilution across rounds.

### 12. Exit probability context
Jake: "What percentage of companies in my sector actually achieve a $300M+ exit?" Adding base rate data (even rough) would ground the exit assumptions in reality.

### 13. Next steps / resource links
Jake strongly wants actionable next steps: "Where do I learn about venture debt for hardtech?" Even 3-5 curated links to resources about alternative financing would make the page dramatically more actionable.

### 14. Comparison bar fairness caveat
Jake noted the comparison isn't totally fair — debt requires collateral, covenants, personal guarantees. The flip side section addresses this, but making the tradeoffs more visible *within* the comparison bar (e.g., hover/tap for "requires collateral" notes) would be more honest.

---

## What's Already Working (Don't Change)

- **The headline.** "Steel costs $800/ton" immediately signals "this is for physical-world founders." Both personas responded to it.
- **The comparison bar.** Most useful single element. The gut-punch of seeing equity cost next to equipment lease cost is the core insight.
- **The drag interaction.** Satisfying and intuitive once discovered. Keep it, just add typed inputs alongside.
- **The flip side section.** Pragmatic, not preachy. Acknowledges equity has real advantages. Builds trust.
- **The tone.** Blunt, practical, not condescending. Feels like advice from a peer, not a lecture.
- **URL-encoded scenarios.** Already works — both would share links with their numbers baked in.
- **No tracking, no sign-up, no sales pitch.** The strongest trust signal on the page. Protect this at all costs.
- **Clean, light design.** Not flashy, not dark-mode-techbro. Feels like a serious tool.

---

## Summary: The Build Order

| # | Feature | Effort | Impact |
|---|---------|--------|--------|
| 1 | Typed number inputs | Small | High — removes #1 friction point |
| 2 | Capital stack calculator | Medium | Very High — transforms novelty into tool |
| 3 | Exit range mode | Small | High — builds trust with sophisticated users |
| 4 | Expanded comparison bar | Small | Medium — serves climate/hardtech specifically |
| 5 | Scenario save/compare | Medium | High — creates return visits |
| 6 | Shareable result cards | Medium | High — drives virality |
| 7 | Attribution footer | Tiny | Medium — builds trust |
| 8 | Chart flow indicator | Tiny | Low-Medium — reduces confusion |
| 9 | Broader industrial language | Tiny | Low — inclusivity polish |
| 10 | Exit chart zoom | Small | Medium — better for non-unicorn founders |
