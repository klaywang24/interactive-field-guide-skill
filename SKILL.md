---
name: interactive-field-guide
description: 'Generate a polished interactive HTML research report with sidebar nav, ⌘K search, an SVG ecosystem map, a 2×2 strategic matrix, click-to-expand drawers, and a 22-part structure. USE AGGRESSIVELY for any research about a company, industry, sector, market, ecosystem, competitor, business model, or investment thesis. English triggers — "deep dive on [X]", "research [Company]", "analyze [Company/industry]", "how does [Company] make money", "is [stock] a good buy", "competitive landscape", "[sector] analysis", "[Company] business model", "[Company] strategy", "[Company] vs [Company]". Chinese triggers — 分析/研究/看一下/了解一下 [公司], [公司] 怎么样, [公司] 商业模式/战略/护城河/基本面, [行业/板块/赛道] 分析/格局/龙头/玩家, 做一份 [X] 的研究报告/行业报告/投资分析, [股票] 值不值得买, [赛道] 投资逻辑. Use whenever user wants substantive research — single companies, whole industries, stock picks, sector landscapes, M&A deep dives. Default to this skill for research unless user explicitly wants plain text or markdown.'
---

# Interactive Field Guide · 22-Part Edition

This skill produces a one-file, self-contained interactive HTML research report based on `assets/template.html` — a warm cream + brick-red editorial template with all interactive components pre-wired (drawer, modal, search, glossary, dark mode, progress bar, constellation SVG, 2×2 matrix, accordion, 8 paired-bar smile chart, GTM tabs, exit tabs, DD pillar grid).

The skill exists because (a) producing one of these from scratch is hours of CSS + JS scaffolding — using the template saves time and avoids design drift, and (b) early attempts pattern-match shallowly: leaving editing-comments in the file, padding parts that don't fit, using `[placeholder]` text, breaking JS by leaving unescaped inner quotes inside Chinese strings. This skill encodes those lessons into a strict workflow.

## Quality bar — VC / CB Insights / 顶级二级 fund 水准

This is not a "blog post with charts." This is a **investment-grade research artifact**. The standard:

- **Every fact has a source citation** — [Source: SEC 10-K Q4 FY2026 / Bloomberg / 公司 PR / 年报 p.42]. No "据估计" / "约" / "near".
- **Every judgment is falsifiable** — not "growing fast" but "YoY +65% to $215.9B"; not "leading" but "31.4% market share, +12pp in 3 years".
- **Every report has 3+ counter-consensus claims** — directly challenge mainstream framing. "NVIDIA's true moat is CUDA, not chip design" beats "NVIDIA leads AI."
- **Every judgment section has falsifiable signals** — "Tier 1 if Q2 FY27 guidance > $50B; downgrade Tier 2 if < $42B."

If a section can't meet this bar, **skip it**. Skipped sections are not failures — padded sections are.

## Workflow

1. **Read references first.** Before touching the template, read `references/structure.md` (22-part menu + skip rules), `references/content-strategy.md` (depth standard + 11 source tiers), `references/pitfalls.md` (validation scripts), `references/data-schemas.md` (component data shapes). These take 90 seconds and prevent 30 minutes of debugging.

2. **Decide which parts to use** before opening template. The 22-part menu has 10 ✅ core / 5 ⭐ important / 5 🟡 optional / 2 ⚠️ rare-use parts. Most reports use 10-15 parts. Only "complete industry handbook" tasks use 18-22.

3. **Source content from independent analysts** before pulling from news headlines. Net Interest, Bits about Money, Acquired, Stratechery, Business Breakdowns, The Diff. Direct SEC filings beat aggregator sites.

4. **Copy `assets/template.html` to working dir, fill the body + JS data, strip meta-comments, validate.** The strip step and validate step are the most-skipped — don't.

5. **Run all 6 validation scripts from pitfalls.md.** All must pass before delivery.

6. **Deliver via `present_files`.** Output to `/mnt/user-data/outputs/`. Brief 2-4 sentence summary, no long postamble.

## Step 1 — Decide scope before opening template

Resist opening the template immediately. Open it only after you have:

- A working list of entities (companies / events / regions) the report will cover (target: 18-28 for Constellation Map)
- A central thesis or counter-consensus argument (Hero h1 should be falsifiable)
- 3-5 candidate quotes/data points from independent analysts
- A rough mapping of which 10-15 parts apply

If user prompt is vague ("分析 Mastercard"), ask 1-2 scoping questions. If specific, go to research.

## Step 2 — Source proactively (don't wait for user push)

For company/industry topics, search these before resorting to news:

- **Net Interest** (Marc Rubinstein) — netinterest.co — fintech, banking, payments, insurance
- **Bits about Money** (patio11) — bitsaboutmoney.com — payments infrastructure, banking
- **Acquired** (Gilbert / Rosenthal) — acquired.fm — multi-hour deep dives
- **Business Breakdowns** (Joincolossus) — single-company analytical episodes
- **Stratechery** (Ben Thompson) — stratechery.com — tech business strategy
- **The Diff** (Byrne Hobart) — diff.substack.com — finance + tech
- **Not Boring** (Packy McCormick) — notboring.co
- **Money Stuff** (Matt Levine) — Bloomberg — finance/markets

For company financials: **always go direct to SEC EDGAR (10-K / 8-K) or HKEX disclosures**, not aggregator sites. For market sizing: prefer original-source reports (TBAC, Standard Chartered, Citi GPS, McKinsey Global Institute, BCG) over secondhand summaries.

When citing sources, in the report show: source name + author + date + URL. Strong reports have visible sources at every paragraph.

## Step 3 — Map content to the 22 parts

The full menu is in `references/structure.md`. Quick map:

| Always use (✅ core, 10) | Strongly recommended (⭐, 5) | Use if relevant (🟡, 5) | Rare (⚠️, 2) |
|---|---|---|---|
| 1, 3, 6, 7, 8, 11, 17, 20, 21, 22 | 5, 13, 14, 15, 21 | 2, 4, 9, 10, 18, 19 | 12, 16 |

For different topic types:
- **Single-company basics** (NVIDIA, Stripe): ~12 parts (1, 2, 3, 6, 7, 8, 11, 13, 14, 17, 20, 21, 22)
- **Sector/industry**: ~14 parts (1, 2, 3, 5, 6, 7, 8, 9, 11, 13, 17, 20, 21, 22)
- **Startup / VC topic**: ~15 parts (1, 3, 4, 7, 8, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22)
- **Complete industry handbook**: 18-22 parts (almost all)

Skip without guilt. Don't pad. The reader prefers a tight 12-part report over a 22-part report with 8 thin sections.

## Step 4 — Strip meta-comments

The template ships with `<!-- ⚙️ PART N · ... -->` comment blocks at the top of each section explaining "何时用 / 何时跳 / 内容形状 / 关键组件." These are instructions for whoever fills the template — they are NOT content.

**Always remove before delivery.** Validation:

```bash
grep -nE "⚙️ PART|何时用|何时跳|内容形状|关键组件|✏️ EDIT" output.html
# Should return 0 matches
```

If matches found, run:

```bash
python3 -c "
import re
with open('output.html') as f: c = f.read()
c = re.sub(r'<!--\s*⚙️ PART.*?-->\n?', '', c, flags=re.DOTALL)
c = re.sub(r'<!--\s*✏️.*?-->\n?', '', c, flags=re.DOTALL)
c = re.sub(r'<!-- 可选 component.*?-->\n?', '', c, flags=re.DOTALL)
with open('output.html','w') as f: f.write(c)
"
```

## Step 5 — Fill the HTML body

Template has placeholders `[YOUR_TOPIC]`, `[NUMBER_1]`, `[FIRST_PRINCIPLE_HEADLINE]` etc. Replace every one with real content. Then validate:

```bash
grep -nE '\[(YOUR_|NUMBER_|STAT_|N\s|[A-Z_]{4,})' output.html | grep -v '<!--'
# Should return 0 (HTML comment placeholders don't count)
```

Any placeholder left = either fill it or remove the entire section (don't leave half-filled).

## Step 6 — Fill the JS data objects

The template's JS has 6 data objects (most start empty with schema comments):

| Object | Drives | Notes |
|---|---|---|
| `REAP_DATA` | Part 4 anchor case (4 regions × 3 layers) | Skip Part 4 → leave object empty `{}` |
| `SECTOR_DATA` | Part 5 sector deep-dives (8 sectors with 10 alive + 5 dead each) | Skip Part 5 → leave empty |
| `CN_NODES` | Part 7 Constellation Map (18-28 nodes) | Almost always populated |
| `DETAIL_DATA` | Part 7 / 8 / 14 drawer content | Must have keys for ALL CN_NODES.id + all data-id values |
| `GTM_DATA` | Part 18 (4 GTM modes) | Skip Part 18 → leave empty |
| `EXIT_DATA` | Part 15 (3 exit paths) | Skip Part 15 → leave empty |

Read `references/data-schemas.md` for exact shapes before filling. Two common errors:

1. **Key mismatch** — drawer pops blank if `data-id` has no matching `DETAIL_DATA` key. Validate (script in pitfalls.md #3).
2. **Unescaped inner quotes** — `desc: "Bridge "Q4" announce"` breaks parser. Use single quotes or backslash-escape.

## Step 7 — Validate before delivery (all 6 must pass)

From `references/pitfalls.md` — copy-paste this whole block, swap `output.html` to your file path:

```bash
node << 'EOF'
const fs = require('fs');
const html = fs.readFileSync('output.html','utf8');

// 1. Template instruction strip
const stripMatches = html.match(/⚙️ PART|何时用|何时跳|✏️ EDIT/g);
console.log('1. Template strip:', stripMatches ? '✗ ' + stripMatches.length + ' matches' : '✓');

// 2. Placeholder strip
const placeholders = html.match(/\[(YOUR_|NUMBER_|STAT_|N\s|[A-Z_]{4,})/g);
console.log('2. Placeholders:', placeholders ? '✗ ' + placeholders.length : '✓');

// 3. JS validity
const scripts = [...html.matchAll(/<script>([\s\S]*?)<\/script>/g)].map(m=>m[1]);
let jsValid = true;
scripts.forEach((s,i) => { try { new Function(s); } catch(e) { jsValid=false; console.log('JS error block', i, e.message); }});
console.log('3. JS valid:', jsValid ? '✓' : '✗');

// 4. ID consistency
const cnSection = html.match(/CN_NODES\s*=\s*\[([\s\S]*?)\];/);
const cnIds = cnSection ? [...cnSection[1].matchAll(/id:\s*['"]([^'"]+)['"]/g)].map(m=>m[1]) : [];
const dataIds = [...new Set([...html.matchAll(/data-id=['"]([^'"]+)['"]/g)].map(m=>m[1]))];
const detailSection = html.match(/DETAIL_DATA\s*=\s*\{([\s\S]*?)^\};/m);
const detailKeys = detailSection ? [...detailSection[1].matchAll(/^\s*['"]([^'"]+)['"]\s*:\s*\{/gm)].map(m=>m[1]) : [];
const missing = [...new Set([...cnIds, ...dataIds])].filter(id => !detailKeys.includes(id));
console.log('4. ID match:', missing.length === 0 ? '✓ (' + cnIds.length + ' nodes + ' + dataIds.length + ' badges → ' + detailKeys.length + ' keys)' : '✗ Missing: ' + missing);

// 5. Constellation count (18-28 sweet spot)
console.log('5. CN_NODES count:', cnIds.length >= 18 && cnIds.length <= 28 ? '✓ ' + cnIds.length : '⚠ ' + cnIds.length + ' (target 18-28)');

// 6. Sidebar consistency
const sidebarParts = [...new Set([...html.matchAll(/href="#part-(\d+)"/g)].map(m=>parseInt(m[1])))];
const actualParts = [...new Set([...html.matchAll(/id="part-(\d+)"/g)].map(m=>parseInt(m[1])))];
const sidebarMissing = actualParts.filter(p => !sidebarParts.includes(p));
const sidebarOrphan = sidebarParts.filter(p => !actualParts.includes(p));
console.log('6. Sidebar match:', (sidebarMissing.length + sidebarOrphan.length) === 0 ? '✓ (' + actualParts.length + ' parts)' : '✗ missing nav: ' + sidebarMissing + ', orphan nav: ' + sidebarOrphan);

EOF
```

All 6 must show ✓ before `present_files`.

## Step 8 — Deliver

Output to `/mnt/user-data/outputs/<descriptive-name>.html`. Call `present_files` with the path.

Brief summary in chat (2-4 sentences max). Don't recap the report — the document speaks for itself. One follow-up offer is fine ("I can also expand Part X if you want more depth").

## Anti-patterns

- **Padding parts to fill all 22.** If you have content for 12 parts, ship 12. The reader prefers tight reports.
- **Generic bullet lists where the template wants prose.** Hero, lede, callouts, tier conclusions are prose surfaces. Don't use markdown-bullet reflexes.
- **Leaving template's `⚙️ PART N` comments in the file.** They survive into HTML if you forget. Strip them. (Step 4.)
- **Citing only news aggregators.** A Net Interest piece beats five Reuters headlines. (Step 2.)
- **Using `[placeholder]` text.** Even draft passes use real content. Skip sections you can't fill substantively.
- **Constellation with 5-6 nodes.** Looks empty. Map is for 18-28 nodes. Below 18, skip Part 7 entirely.
- **Hero with a non-falsifiable claim.** "NVIDIA leads AI" = bad. "NVIDIA's true moat is CUDA + bundled networking, not chip raw FLOPS" = good.
- **Skipping the validation step.** All 6 checks in Step 7 must pass. The 30 seconds it takes prevents 10 minutes of bug-hunting after the user opens it.

## Files in this skill

- `assets/template.html` — 1500-line warm-cream + brick-red template with all 22 sections + components wired up. Copy this to start.
- `references/structure.md` — what each of the 22 parts is for, when to keep/skip, depth standards.
- `references/data-schemas.md` — exact JS object schemas for REAP_DATA / SECTOR_DATA / CN_NODES / DETAIL_DATA / GTM_DATA / EXIT_DATA.
- `references/content-strategy.md` — VC / CB Insights depth standard; 11 source tiers; counter-consensus framework; hero writing rules.
- `references/pitfalls.md` — all 10 known failure modes with copy-paste validation scripts.
