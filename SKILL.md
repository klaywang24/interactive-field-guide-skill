---
name: interactive-field-guide
description: Build a polished, interactive HTML "field guide" — a multi-section research report with sidebar nav, search (⌘K), glossary (⌘G), dark mode, sticky progress bar, click-to-expand drawers, an SVG constellation map, a 2×2 strategic matrix, paired-bar charts, tab-switched case studies, a 12-part editorial structure, and a warm cream + brick-red editorial aesthetic. Use this skill for ANY long-form research deliverable that should be more substantive than a markdown report and more visually-cohesive than ad-hoc HTML — industry analysis, competitive landscape, ecosystem strategy map, fund thesis, market deep dive, founder's research memo. Trigger on phrases like "field guide", "deep dive on [X]", "industry report", "competitive landscape", "ecosystem map", "interactive research report", "strategy deep-dive", "[Company] ecosystem", "research memo", or any request for a research deliverable that should feel polished and explorable rather than a flat document.
---

# Interactive Field Guide

This skill produces a one-file, self-contained interactive HTML research report based on the bundled `assets/template.html` — a warm cream + brick-red editorial template with all interactive components already wired up (drawer, modal, search, glossary, dark mode, progress bar, constellation SVG, 2×2 matrix, paired-bar renderer, tab switcher).

The skill exists because: (a) producing one of these from scratch is an enormous amount of CSS + JS scaffolding — using the template saves hours and avoids design drift, and (b) early attempts at this kind of report tend to pattern-match in shallow ways: leaving the template's editing-instruction comments in the file, padding parts that don't fit the material, using `[placeholder]` text, breaking JS by leaving unescaped inner quotes inside Chinese strings. This skill encodes the lessons.

## What this is for

A "field guide" in this skill's sense is a long-form research artifact (typically 12–25KB of substantive prose + interactive data) about an ecosystem, industry, company, or strategic question. It's the format you'd reach for when:

- A deck would feel dumbed-down
- A markdown doc would feel flat and unscannable
- A table of links would lack analytical voice
- The reader needs to *explore* the material (click into companies, switch tabs, search), not just read top-to-bottom

Examples of good triggering tasks:

- "Map [Company]'s strategic ecosystem — every partner / investor / acquirer it touched in the last 12 months, organized by theme, with a constellation map"
- "Build me an industry field guide on [vertical] — top players, regional differences, regulatory map, who to watch"
- "Turn this CB Insights screenshot into an interactive research report"
- "Write a fund thesis on [theme] with company evidence and an actionable judgments table"
- "Deep dive on [Company]: financial profile, product strategy, competitive moat, recent M&A"

## Workflow at a glance

1. **Read the references first.** Before touching the template, read `references/structure.md`, `references/content-strategy.md`, and `references/pitfalls.md`. The pitfalls one is non-negotiable — every prior attempt at this format has hit at least one of those gotchas.
2. **Capture the user's content scope** before opening the template. Don't fill placeholders; gather the material that will fill them.
3. **Source from independent analysts** (Net Interest, Bits about Money, Acquired, Stratechery, Business Breakdowns, etc.) for any company/industry topic. Do this proactively, not after the user pushes for it.
4. **Map content to parts.** The template has 12 parts; rarely do all 12 fit. Skip parts that would otherwise be padded.
5. **Copy the template, strip its meta-comments, fill the body, fill the JS data, validate.** The strip and the validate are the steps most often skipped — don't.
6. **Deliver via `present_files`.** Outputs go to `/mnt/user-data/outputs/`.

## Step 1 — Capture content scope before opening the template

Resist the pull of opening the template immediately. The template is the *form*; the research is the *substance*. Open template.html only after you have:

- A working list of entities (companies / people / events) the report will cover
- A central thesis or analytical lens (what does this report *argue*?)
- 3–5 candidate quotes/data-points from independent analysts (see content-strategy.md)
- A rough sense of which parts you'll use and which you'll skip

If the user's prompt is vague ("make me a Mastercard report"), ask one or two scoping questions before searching. If the prompt is specific, go straight to research.

## Step 2 — Source content proactively

For any company/industry/strategy topic, search independent analysts before resorting to news headlines. The shortlist:

- **Net Interest** (Marc Rubinstein) — netinterest.co — fintech, banking, payments, insurance
- **Bits about Money** (Patrick McKenzie / patio11) — bitsaboutmoney.com — payments infrastructure, banking, fraud
- **Acquired** (Gilbert / Rosenthal) — acquired.fm — multi-hour deep dives on great companies
- **Business Breakdowns** (Joincolossus) — joincolossus.com — single-company analytical episodes
- **Stratechery** (Ben Thompson) — stratechery.com — tech business strategy
- **The Diff** (Byrne Hobart) — diff.substack.com — finance + tech intersection
- **Not Boring** (Packy McCormick) — notboring.co — narrative-driven company analysis
- **Matt Levine's Money Stuff** — Bloomberg — finance/markets daily

For each independent source you cite, include in the report: source name + author + article title + date + URL. The "外部观察家" / "Outside Voices" callout pattern (see `references/structure.md` Part 6.5) is one of the highest-signal sections this template can render — use it whenever you have 3+ analyst sources.

For company-level financial data, always go to the SEC 8-K / 10-K direct, not aggregator sites. For market-size forecasts, prefer original-source reports (TBAC, Standard Chartered, Citi GPS, McKinsey Global Institute, BCG) over secondhand summaries.

## Step 3 — Map content to parts

The template has 12 parts. Read `references/structure.md` for the detailed mapping. The headline rules:

- **Always keep**: Hero, Part 1 (preface), Part 12 (sources). These anchor the document.
- **Keep if material warrants**: Part 5 (Constellation Map — for ecosystems with 15+ entities), Part 6 (2×2 — for strategic positioning), Part 9 (Inflection — for time-bound recent events), Part 11 (tiered judgments — for decision-oriented reports).
- **Skip without guilt**: Part 2 (5-card framework), Part 3 (case tabs), Part 4 (sector paired-bar), Part 7 (heat-map), Part 8 (deep-dive), Part 10 (timeline). Each of these requires substantial material to fill well; padding is worse than skipping.

If you skip a part, also remove its TOC link, its `<section>` block, and any associated JS data. The renderer functions (renderCase, renderSector, renderConstellation) tolerate empty data objects — they won't error if you skip — but the empty UI is ugly.

## Step 4 — Strip the template's editing-instructions

This is the single most-missed step. The template ships with a large HTML comment block right after `<body>` titled "📘 INTERACTIVE FIELD GUIDE TEMPLATE · 编辑指南" plus four JS comment blocks beginning with `/* ============ ⚙️ <NAME> — Part X ... */`. These are instructions for whoever is filling the template — they are not content.

**Always remove them.** They survive into the final HTML if you forget (HTML comments aren't visible to the reader but are visible in source view, and the user will notice).

Specifically:
- The HTML comment block between `<body>` and `<div class="progress-bar">` — delete all of it.
- The four JS comment headers (above CASE_STUDY_DATA, SECTOR_DATA, CN_NODES, DETAIL_DATA) — replace with one-line descriptions, e.g. `/* CASE_STUDY_DATA · five thematic groups, three-layer evaluation */`.
- The `/*JSPART2*/` and `/*JSPART3*/` markers — delete.
- All `<!-- ✏️ EDIT_X: ... -->` and `<!-- ⚙️ ... -->` HTML comments scattered in the body — delete.

A quick check after stripping:

```bash
grep -nE "INTERACTIVE FIELD GUIDE TEMPLATE|EDIT_HERO|EDIT_PART|EDIT_FOOTER|✏️|⚙️|CONFIG 对象|HTML 段落|HTML 5 张卡|JSPART" "$file"
```

Should return zero matches.

## Step 5 — Fill the HTML body

The template's body has placeholder text in `[brackets]` and `[Chinese square brackets]`. Replace every one with real content. After filling, scan for any leftover placeholders:

```bash
grep -nE '\[(YOUR|Industry|highlight|关键概念|玩家|事实|来源|相关性|副标|描述|条件|地区|对象|案例|关键|简短|简介|简短副标)' "$file"
```

Should return zero matches before delivery.

## Step 6 — Fill the JS data objects

Four JS objects drive the interactive sections:

| Object | Drives | Schema |
|---|---|---|
| `CASE_STUDY_DATA` | Part 3 (tab case studies) | `references/data-schemas.md` |
| `SECTOR_DATA` | Part 4 (paired-bar charts) | `references/data-schemas.md` |
| `CN_NODES` | Part 5 (constellation node positions + types) | `references/data-schemas.md` |
| `DETAIL_DATA` | Drawer content for Part 5 nodes AND Part 6 archetype badges | `references/data-schemas.md` |

Read `references/data-schemas.md` before filling these. Two especially common mistakes:

1. **Key mismatch.** Every `id` in `CN_NODES` must have a matching key in `DETAIL_DATA`. Every `data-id` on a Part 6 archetype badge must also have a matching key. Validate after filling:

   ```bash
   node -e '
   const html = require("fs").readFileSync("file.html","utf8");
   const nodes = [...new Set([...html.matchAll(/{ id: "([^"]+)"/g)].map(m=>m[1]))];
   const badges = [...new Set([...html.matchAll(/data-id="([^"]+)"/g)].map(m=>m[1]))];
   const keys = [...new Set([...html.matchAll(/^\s*"([^"]+)":\s*\{/gm)].map(m=>m[1]))];
   console.log("nodes missing detail:", nodes.filter(x => !keys.includes(x)));
   console.log("badges missing detail:", badges.filter(x => !keys.includes(x)));
   '
   ```

2. **Unescaped inner quotes** — see Step 7. This is the #1 cause of broken JS in this template.

## Step 7 — Validate JS syntax

The template's data objects are large JS string-literal heavy. Strings in CN/EN context that contain inner double-quotes break the JS parser. Always validate after filling:

```bash
node -e '
const fs = require("fs");
const html = fs.readFileSync("file.html","utf8");
const m = html.match(/<script>([\s\S]*?)<\/script>/);
try { new Function(m[1]); console.log("✓ JS valid"); }
catch(e) { console.log("✗", e.message); process.exit(1); }
'
```

If it fails with `Unexpected identifier '<some Chinese phrase>'`, you have an unescaped inner quote. Fix per `references/pitfalls.md` — preferred fix is replace inner `"…"` with Chinese corner brackets `「…」` for Chinese context, or escape with `\"` for English.

## Step 8 — Deliver

Place the final HTML at `/mnt/user-data/outputs/<descriptive-name>.html` and call `present_files`. Don't add a long postamble — the document speaks for itself. A brief 2–4 sentence summary of what was produced, plus one or two specific follow-up offers, is enough.

## Anti-patterns to avoid

These came up repeatedly when this format was being built without the skill:

- **Padding parts to fill all 12.** If you have content for 7 parts, ship 7 parts. The reader prefers a tight 7-part report over a 12-part report with three filler sections.
- **Generic bullet lists where the template wants prose.** The lede, callouts, and tier conclusions are written-prose surfaces. Don't insert markdown-bullet-list reflexes there.
- **Leaving the template's "this is what each part does" comments in the file.** Strip them. (See Step 4.)
- **Citing only news aggregators.** Independent analysts are higher signal — a Net Interest piece beats five Reuters headlines. (See Step 2.)
- **Using `[placeholder]` text under any circumstances.** Even a draft pass should use real content. If you don't have material for a section, skip the section.
- **Adding the constellation SVG with only 5–6 nodes.** It looks empty. The map is designed for 18–28 nodes. Fewer than that, skip Part 5.
- **`[Chinese full-width brackets]` left from the template.** These hide from English-only `grep`s. Always grep for both `[a-zA-Z]` placeholders AND `[\u4e00-\u9fa5]` placeholders before delivering.

## Files in this skill

- `assets/template.html` — the 1340-line warm-cream + brick-red Field Guide template, all CSS + JS components wired up. Copy this to start.
- `references/structure.md` — what each of the 12 parts is for, when to keep/skip, content patterns that work for each.
- `references/data-schemas.md` — exact JS object schemas for CASE_STUDY_DATA, SECTOR_DATA, CN_NODES, DETAIL_DATA, with annotated examples.
- `references/content-strategy.md` — how to source substantive material; the independent-analyst shortlist; quote integration patterns; the "Outside Voices" callout pattern.
- `references/pitfalls.md` — the gotchas that have broken every prior attempt: meta-comment stripping, JS string escaping in Chinese context, key/badge ID mismatches, mobile-responsiveness traps.
