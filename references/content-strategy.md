# Content Strategy

The visual template is only as good as what fills it. This file is about sourcing — where to look, what counts as substantive, and how to integrate external voices.

## The substance bar

A field guide is the format you reach for when a markdown report would feel flat. That promise is broken if the content is the same prose-density as a markdown report. The aspiration is: every section should make the reader pause, not skim.

Quick test for a section that's pulling its weight:
- The lede makes a specific, defensible claim (not a description of the topic)
- At least one number, name, or quote in the section is something the reader hadn't already seen elsewhere
- The reader could quote a sentence from the section in a Slack message and it would land

If two of those three fail, the section needs more work or should be cut.

## Independent analysts: the shortlist

For any company / industry / strategy topic, search these before falling back to news aggregators. Each has a distinctive analytical voice:

| Source | Author | Niche | URL |
|---|---|---|---|
| Net Interest | Marc Rubinstein | fintech, banking, payments, insurance | netinterest.co |
| Bits about Money | Patrick McKenzie (patio11) | payments infrastructure, banking ops, fraud | bitsaboutmoney.com |
| Acquired | Gilbert / Rosenthal | multi-hour deep dives on great companies | acquired.fm |
| Business Breakdowns | Joincolossus | single-company analytical episodes | joincolossus.com |
| Stratechery | Ben Thompson | tech business strategy / aggregation theory | stratechery.com |
| The Diff | Byrne Hobart | finance + tech intersection | diff.substack.com |
| Not Boring | Packy McCormick | narrative-driven company analysis | notboring.co |
| Money Stuff | Matt Levine (Bloomberg) | finance / markets daily | bloomberg.com/opinion |
| Doomberg | (anonymous team) | energy, commodities, macro | doomberg.substack.com |
| Money Inside Out | Lillian Li | China tech, fintech | moneyinsideout.substack.com |
| Tom Tunguz | Theory Ventures | SaaS metrics, GTM | tomtunguz.com |

For original primary research:
- SEC filings (sec.gov/edgar) — for any US-public company financial data, cite 8-K / 10-K directly, not aggregator summaries
- Earnings call transcripts (Motley Fool / Seeking Alpha) — CEO-attributed quotes from these are gold
- Treasury Borrowing Advisory Committee reports (home.treasury.gov) — for stablecoin / Treasury-market topics
- McKinsey Global Institute, BCG, Bain reports — for market-size figures
- Federal Reserve research papers (federalreserveboard.gov) — for monetary / payment-system topics

## Search query patterns

Effective queries to find substantive analysis (replace `[topic]` with your subject):

- `"Net Interest" [topic]` → finds Marc Rubinstein's coverage
- `"Bits about Money" [topic]` or `patio11 [topic]` → patio11's coverage
- `Acquired podcast [topic]` → Gilbert/Rosenthal episode if it exists
- `Stratechery [topic]` → Ben Thompson if he's covered it
- `[topic] Substack` → finds the highest-signal Substacks on the topic
- `[topic] earnings call transcript` → for direct CEO quotes
- `[topic] 8-K 10-K SEC` → for primary financial filings

Always prefer dated, named-author sources over undated SEO content.

## The "Outside Voices" pattern (Part 6.5)

This is the highest-leverage section the template can render. When you have 3+ independent-analyst sources, build a Part 6.5 between Part 6 and Part 7 with this structure:

```html
<section class="section" id="part-6-5">
  <div class="section-num">PART 6.5 · 外部视角</div>
  <h2>Four independent analysts read <em>[Subject]</em></h2>
  <p class="lede">[1-2 sentence framing of why these voices matter]</p>

  <div class="callout investor"><span class="callout-label">VOICE 01 · [Author] · [Source] (date)</span>
    <p>[1-3 sentences paraphrasing or short-quoting the argument]</p>
    <p>[1 sentence connecting to your report's thesis]</p>
    <p style="color:var(--muted);font-size:12px;"><strong>来源</strong>: [Article title], [Source], [date]</p>
  </div>

  <div class="callout lesson">[Voice 02 - rotate color: lesson]</div>
  <div class="callout fact">[Voice 03 - rotate: fact]</div>
  <div class="callout data">[Voice 04 - rotate: data]</div>

  <div class="callout fact" style="margin-top:24px;"><span class="callout-label">[Synthesis]</span>
    <p>[The cross-source meta-pattern: what do these four voices, from different angles, agree on? This is where the section earns its place.]</p>
  </div>
</section>
```

The synthesis callout at the end is critical. Without it, the section is just a quote dump. With it, it becomes the report's analytical climax.

**Color rotation**: `investor` (teal) → `lesson` (sky) → `fact` (red) → `data` (gold). Different left-border colors create rhythm. Don't use the same color for all four.

## Quote integration patterns

### Inline quote
Short attributable quote inside body prose. Use sparingly (1–2 per part).

```html
<p>Marc Rubinstein on stablecoins: "稳定币的采用正在和加密市场周期脱钩——它已经不是投机工具，而是基础设施。" (<em>Net Interest</em>, 2025-05)</p>
```

### Callout quote
Featured quote as its own callout. Use for the strongest single observation in a section.

```html
<div class="callout investor"><span class="callout-label">外部观察 · Acquired podcast (Nov 2023)</span>
  <p>"Visa is a <strong>government-enabled duopoly</strong> with 50%+ net margins on $30B+ revenue."</p>
  <p>The implication for [your subject]: [tie-back sentence].</p>
</div>
```

### Composite paraphrase
Several short quotes from the same source woven into a narrative paragraph. Use when the full argument matters more than any single line.

```html
<p>Patrick McKenzie's framing in <em>Bits about Money</em> is that financial infrastructure is heavily path-dependent, that "we're (oft unknowingly) standing atop decades and centuries of work which came before," and that the right way to build new financial products is "in the smart layer above the dumb substrate" rather than trying to replace the substrate itself.</p>
```

## Citation discipline

Every external source cited in the body should also appear in Part 12's sources block. Conversely, every source in Part 12 should be referenced at least once in the body — uncited sources signal padding.

For Part 12, organize sources into named categories with `<h4>` subheaders:
1. Independent analysts (Newsletter / Podcast)
2. Primary sources (SEC, regulatory bodies, earnings calls)
3. Aggregators (Crunchbase, CB Insights, Yahoo Finance)
4. Topic-specific sources (e.g., "Stablecoin / blockchain events" if relevant)

For each independent analyst, include: author + outlet + 1–3 specific article titles + date + URL. Aggregators can be listed by name only.

## Avoiding shallow patterns

These are common shallow patterns to watch for and reject:

- **Citing 5 news headlines from the past month** instead of 1 deep-dive article from any time. Depth > recency on any topic that isn't actively breaking.
- **Quoting CEO talking points** without context. CEOs say what investors want to hear; the analytical value is in what an outside observer notices that the CEO won't say.
- **"According to recent reports..."** — name the report.
- **"Industry experts believe..."** — name the experts.
- **Numerical claims without an SEC filing or primary-source URL** — for any company financial figure, the source must be the company's own filing, not a third-party article that paraphrases the filing.

## Sourcing for the Part 5 Constellation Map drawer content

Each `DETAIL_DATA[node-X]` entry has 3–5 `bg` bullets and a `relevance` paragraph. The minimum bar:

- One bullet on founders / founding (year, who, where they came from)
- One bullet on size (revenue, valuation, user count, employees — pick the most accurate metric you can verify)
- One bullet on a recent action (last 12 months: funding round, partnership, product launch)
- One bullet on the specific connection to the report's subject (why this entity is on this map)

`relevance` paragraph (2–4 sentences): Don't repeat the bullets. Answer "so what does this mean for the report's reader?" — is this entity a competitor, a partner candidate, a comp for valuation, a cautionary tale?

`src`: Always include a concrete source — "Crunchbase · TechCrunch 2025-Q3" beats "various sources".
