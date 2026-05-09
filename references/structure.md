# Field Guide Structure: The 12 Parts

This template encodes a 12-part editorial structure. Read this before you start filling — the structure is opinionated, and several parts only earn their place when the source material has a specific shape.

## Quick reference table

| Part | Name | Visual form | Skip if… |
|---|---|---|---|
| Hero | Title + 4 stats | 4-block stat grid | (never skip) |
| 1 | Preface · First Principles | Prose + callout | (never skip) |
| 2 | 5-card framework | 5 colored top-stripe cards | You don't have 5 distinct ideas |
| 3 | Tab case studies | Tab switcher + 3-layer panel | You don't have ≥3 comparable cases |
| 4 | Sector paired-bar | 8 tabs × 8-region paired bars | Geographic distribution isn't the story |
| 5 | Constellation Map | Center + 3-ring SVG with click drawers | Ecosystem has fewer than 15 entities |
| 6 | 2×2 strategic matrix | Quadrant grid with click badges | No two-dimensional positioning is core |
| 6.5 | Outside Voices | 4 analyst callouts | Fewer than 3 credible analyst sources cited |
| 7 | Heat-map matrix | Table with high/mid/low pills | No multi-dimensional scoring needed |
| 8 | Topic deep dive | Detailed table + supplementary lists | No single sub-topic deserves long-form |
| 9 | Inflection point | Recent-event narrative + role table | No clear time-bound recent event |
| 10 | Timeline | 5 marker+body cards | Fewer than 4 datable events |
| 11 | Tiered judgments | Three colored tier blocks (do / consider / avoid) | Report isn't decision-oriented |
| 12 | Glossary + sources | Term definitions + categorized URL list | (never skip) |
| Footer | Tagline | Centered prose + ornament | (never skip) |

## Detailed part-by-part guidance

### Hero
4 stat blocks + headline + sub. The 4 stats should mix the **scale of the subject** (revenue, market cap, user count) with the **insight of the report** (e.g., "0 acquisitions in 12 months" if the report's thesis is about partnership-led strategy). If all 4 stats are pure scale numbers, the hero feels like a Wikipedia infobox; if all 4 are insight-y, the reader doesn't know how big the subject is. Mix.

The headline uses `<em>` for italicized accent words (in the brick-red color). One or two `<em>`s land; three+ feels noisy. The headline should make a specific claim, not describe the report.

Bad: "An overview of Mastercard's strategy"
Good: "Mastercard 已经不是 *卡组织* 了 — 它是一台靠 *合作* 而非 *收购* 运转的全球结算操作系统"

### Part 1 — Preface
The lede (italicized prose) sets up the question. The body paragraphs lay out the thesis. The mandatory `callout fact` at the bottom is the data-credibility commitment — list real sources, not "various sources".

Add a `callout investor` between the body and the credibility callout when there's a single high-signal external observation that frames the whole report. Example: an Acquired podcast quote that captures the central tension. Don't force this if the material doesn't suggest it.

### Part 2 — 5-card framework
Each of the 5 cards has a top stripe in a different theme color (sky / gold / accent / teal / moss). The cards should articulate **5 distinct strategic intents** behind the report's subject — not 5 product categories, not 5 business segments. Compress the entity-level detail into the description; let the card title carry the *intent*.

Bad title: "Cybersecurity"
Good title: "非支付边界 / Beyond Payments"

The mandatory `callout fact` at the bottom should crystallize the meta-pattern that ties the 5 cards together. Optionally add a second `callout data` for a key supporting statistic.

### Part 3 — Tab case studies
Five tabs across the top, each clicking to a 3-column "layer" panel + verdict + players list. The 5 tabs should be **mutually exclusive case groups** — geographies, customer segments, strategic theme groups, etc. The 3-layer schema (`layerA / layerB / layerC` with `status`, `label`, `desc`) is opinionated: it forces the analysis into "input layer / engine layer / application layer" or "wedge / engine / platform" thinking. Reuse the schema; rename Layer A/B/C labels in the JS.

Status colors: `go` (green ✅), `warn` (yellow 🟡), `no` (red ❌). The verdict line should be 1 sentence; the players line should be a comma-separated string with brief metadata in parens.

### Part 4 — Sector paired-bar
Eight tabs, each clicking to a paired-bar comparison across 8 regions. The two bars compare **two metrics** — typical pairings:

- "Global revenue %" vs "Profit pool %" (the original use, for industry analysis)
- "Company count" vs "Public/unicorn share" (for ecosystem mapping)
- "User count" vs "Revenue share" (for two-sided platforms)

The renderer auto-colors disproportionate bars red. So the visual delivers an immediate "this region is over/under-indexed" message — pick metric pairings that exploit this.

If geography isn't the right slicing axis, swap "regions" for the actual axis (customer segments, time periods, product categories) and rename the headers in the renderer.

### Part 5 — Constellation Map
The single most striking section when populated well; the most embarrassing when populated poorly. **Don't include it with fewer than 15 entities.** Sweet spot: 18–28 nodes.

The center node is the report's subject (a company, a thesis, a movement). Three rings radiate outward:
- Ring 1 (innermost, ~6 nodes) — Tier-1 strategic core
- Ring 2 (middle, ~8 nodes) — Execution-level partners
- Ring 3 (outermost, ~10 nodes) — Edge / experimental / recent / acquired

Node types: `local`, `local-tier1`, `crypto` (gold tint, useful for "experimental"), `exit` (with star icon, for confirmed acquisitions). Each node clicks open a drawer rendering the matching `DETAIL_DATA[id]` — **the drawer content is what makes this section valuable**. Skimping on drawer content breaks the value proposition.

Node text labels render to the side (left if `n.x < 480`, right otherwise). On dense clusters, manually adjust `n.x / n.y` to avoid label overlap.

The center radial gradient color (`#centerGradient` in SVG defs) is themable — change the three `stop-color` values to match the report's subject. Default is purple; for finance/payments, switch to brick-red gradient.

### Part 6 — 2×2 strategic matrix
Four quadrants Q1 (top-right, "golden") / Q2 (top-left) / Q3 (bottom-left) / Q4 (bottom-right, "local strongest"). Pick axes that cut the entity space cleanly:

- "Maturity" × "Direct relevance to your thesis" (most flexible)
- "Geographic scope" × "Product depth"
- "Capital intensity" × "Network effect strength"

Each quadrant gets 2–4 archetype-badges; clicking a badge opens a drawer (same `DETAIL_DATA` schema as Constellation). Use `archetype-badge featured` (red bg) to mark the 2–3 most important entities; reserve this for entities the reader must know.

The mandatory `callout investor` at the bottom should give 3 selection rules — one per row of insight ("Rule 1: …", "Rule 2: …", "Rule 3: …"). These rules are usually the most actionable lines in the entire document; write them last, after the rest of the report is settled.

### Part 6.5 — Outside Voices (custom add)
Not in the original template. Add it when the report is research-heavy and you've cited 3+ independent analysts. Format: 4 callouts, each labelled with `VOICE 0X · <Source Name> · <Date>`, each containing:
- 1–3 sentences paraphrasing the analyst's argument (with a short quote if attribution-worthy)
- 1 sentence connecting the argument to your report's thesis
- 1 line `<source>` footnote in muted text

Color rotate the callouts: `investor` (teal) / `lesson` (sky) / `fact` (red) / `data` (gold). End with a meta-callout that synthesizes the cross-source pattern ("4 票一致" / "Consensus across sources").

This section punches above its visual weight — analyst-named callouts dramatically increase the reader's confidence in the report.

### Part 7 — Heat-map
A multi-dimensional scoring table (rows × columns) with `heat-pill` spans (`heat-high` / `heat-mid` / `heat-low`). Use when the report wants to **score N entities against M criteria** in a single glance. Common patterns:

- "10 themes × 3 relevance dimensions" (this is the Mastercard report's use)
- "5 competitors × 5 capabilities"
- "8 markets × 4 entry-difficulty factors"

Always end with a `callout fact` that names the highest-scoring rows/columns and gives the reader's instruction.

### Part 8 — Topic deep dive
A detailed table (4+ columns, ≤12 rows) of the most important entities for one specific theme + one or two supplementary list blocks (regulatory points, technical specs, etc.). Use when one of the 10/12 themes deserves long-form treatment beyond what fits in the constellation drawer.

### Part 9 — Inflection point
Used for recent (last 6 months) time-bound events that justify the entire report. Pattern:
1. Lede: when did it happen + why is it an inflection point
2. List of 3–5 contemporaneous events (the cluster that defines the inflection)
3. Optional secondary explainer (TBAC, regulatory committee, etc.)
4. Forecast/prediction table (current state → projected state)
5. Role-attribution table (who's doing what)
6. `callout investor` titled "对 [entity] 的含义" / "Implications for [entity]"

This section is most powerful when supported by independent-analyst sourcing — the inflection narrative is more credible when framed by Net Interest / Acquired / etc., not just news.

### Part 10 — Timeline
Five `tl-card`s (year + quarter + body title + body prose + source line). Each card is a discrete event; the prose explains what happened and what it implies. Don't try to fit 8 events; if you have 8, pick the 5 most strategic. If you have 3, skip Part 10 entirely.

### Part 11 — Tiered judgments
Three colored blocks: tier1 (red, "do this") / tier2 (gold, "consider") / tier3 (gray, "avoid"). 4–5 items per tier. This is the most decision-oriented section — write it as if you're the operator who will use the report tomorrow morning.

End with a `callout fact` titled "一句话总结" / "In one sentence" — distill the entire report to one declarative claim. This usually becomes the document's most-quoted line.

### Part 12 — Glossary + sources
Glossary terms get inline definitions (auto-mirrored to the ⌘G modal). Sources are categorized with `<h4>` subheaders. **Always include**:
1. Independent-analyst sources (Net Interest, Acquired, etc.) — by name + date + URL
2. Primary sources (SEC filings, regulatory reports, earnings calls) — by issuer + date
3. Aggregators (Crunchbase, CB Insights, Yahoo Finance) — by name only
4. Data declaration paragraph — what's directly cited, what's directional estimate, what's analytical framework

### Footer
Centered ornament + 1 line of attribution + 1 italicized closing quote. The closing quote should echo (not repeat) the report's Hero claim.
