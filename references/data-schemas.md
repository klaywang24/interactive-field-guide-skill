# JS Data Schemas

Four JS objects in the bundled template's `<script>` tag drive the interactive sections. This file is the schema reference. Read it in full before filling any of these objects.

## Where they live

All four objects are defined inside the single `<script>` tag at the bottom of `assets/template.html`. After packaging, they look like:

```javascript
const CASE_STUDY_DATA = { ... };  // Part 3 tabs
const SECTOR_DATA     = { ... };  // Part 4 paired-bar
const CN_NODES        = [ ... ];  // Part 5 constellation node positions
const DETAIL_DATA     = { ... };  // Drawer content for both Part 5 nodes AND Part 6 badges
```

Plus the renderer functions (`renderCase`, `renderSector`, `renderConstellation`, `openDrawer`) — don't modify these unless you understand them.

## Object 1: `CASE_STUDY_DATA`

Drives Part 3's tab-switched case panels. Five entries keyed by tab id (`region-a` through `region-e`, matching the HTML `data-region` on each `<button class="tab-btn">`).

```javascript
const CASE_STUDY_DATA = {
  'region-a': {
    name: "Tab title shown at top of panel",
    meta: "Right-aligned subtitle (date / status / count)",
    layerA: {
      status: "go" | "warn" | "no",   // controls icon color
      label: "Short status text shown above layer name",
      desc:  "Body prose for this layer. Supports <strong>, <em>, <span class=\"verify-tag\">已核实</span>"
    },
    layerB: { ... },
    layerC: { ... },
    verdict: "One-sentence summary at panel bottom",
    players: "Comma-separated list of entities, with parenthetical metadata"
  },
  'region-b': { ... },
  // ... up to region-e
};
```

**Layer naming**: The renderer hardcodes the labels "Layer A · 入口层", "Layer B · 引擎层", "Layer C · 应用层" by default. To use different labels, modify the `renderCase` function (lines around `${d.layerA.label}` / `<div class="layer-name">`).

**Status mapping**:
- `go` → green text (success / open opportunity)
- `warn` → yellow/gold text (cautionary / partial)
- `no` → red text (closed / blocked)

The `label` is free text; conventions:
- `"✅ 跑通"` / `"✅ Working"`
- `"🟡 拥挤"` / `"🟡 Crowded"`
- `"❌ 已锁住"` / `"❌ Locked up"`

## Object 2: `SECTOR_DATA`

Drives Part 4's eight-tab paired-bar charts. Eight entries keyed by tab id (`sector-1` through `sector-8`).

```javascript
const SECTOR_DATA = {
  'sector-1': {
    name: "① Sector name shown at panel top",
    meta: "Subtitle: total scale info",
    layers: [
      { name: "USA",    rev: 35, profit: 38, note: "Player names for this region" },
      { name: "Europe", rev: 18, profit: 22, note: "..." },
      // 8 region rows total
    ],
    insight: "Insight paragraph below bars (supports HTML)",
    players: "Representative players list"
  },
  // sector-2 through sector-8
};
```

**The two metrics (`rev` and `profit`)** can be repurposed. The renderer just plots two bars per row and applies a "high/low" color when one bar is significantly larger than the other. So you can rename them mentally:

| Original use | Alternative repurposings |
|---|---|
| Global revenue % vs Profit pool % | Industry analysis — original |
| | Company count % vs Public/unicorn share % — ecosystem mapping |
| | User base % vs Revenue share % — two-sided platform |
| | Adoption % vs Satisfaction score — product comparison |

To rename the bar headers visible in the UI, edit the `renderSector` function (line with `<div class="bar-headers">`) — change `公司数量 %` / `上市/独角兽 %` to whatever pairing you're using.

**SECTOR_MAX**: A constant near `renderSector` defaulting to 65. The renderer normalizes bar widths to `(value / SECTOR_MAX) * 100`. Adjust SECTOR_MAX if your data is on a different scale (e.g., set to 100 if your data is already 0–100 percentages).

**Color logic**:
- `profit > rev × 1.15` → profit bar turns red (high-margin)
- `profit < rev × 0.75 && profit > 0` → rev bar turns red (low-margin)
- `profit ≤ 0` → profit bar turns dark red (loss-making)

## Object 3: `CN_NODES`

Drives Part 5's Constellation Map node positions and types. Array of node objects.

```javascript
const CN_NODES = [
  // Ring 1 — innermost (radius ~125 from center 480,360)
  { id: 'node-stripe',   x: 380, y: 305, r: 13, type: 'local-tier1', name: 'Stripe',   sub: '~$70-90B', cat: 'cat-1' },
  // Ring 2 — middle (radius ~230)
  { id: 'node-gr4vy',    x: 290, y: 245, r: 11, type: 'local',       name: 'Gr4vy',    sub: '$27M · USA', cat: 'cat-1' },
  // Ring 3 — outermost (radius ~320)
  { id: 'node-tempo',    x: 200, y: 230, r: 13, type: 'crypto',      name: 'Tempo ★',  sub: '$5B · Stripe', cat: 'cat-2' },
  // ... etc, target 18-28 nodes total
];
```

**Field semantics**:
- `id` — unique key. Must have a matching key in `DETAIL_DATA`.
- `x`, `y` — SVG coordinates. ViewBox is `0 0 960 720`, center is `480, 360`. Place nodes on rings using polar coordinates: `x = 480 + r·cos(θ)`, `y = 360 + r·sin(θ)`.
- `r` — circle radius. 10–11 for normal nodes, 13 for emphasized (Tier-1, Exits).
- `type` — controls fill/stroke color:
  - `local` — pale blue fill, navy stroke (default)
  - `local-tier1` — deep navy fill, darker stroke (Tier-1 emphasis)
  - `crypto` — gold-tinted fill (experimental / web3 / new)
  - `exit` — mid-blue fill with thick stroke (confirmed acquisitions; pair with `★` in name)
- `name` — label rendered next to the node. Append `★` for exits/emphasized.
- `sub` — small subtitle below name (one line; valuation, status, country).
- `cat` — filter category. The Part 5 region tabs have `data-cregion` values (`cat-1`, `cat-2`, etc.) that filter nodes by `cat`. Tab `all` shows everything.

**Layout tips**:
- Place 6 evenly-spaced nodes in Ring 1 at angles 30° / 90° / 150° / 210° / 270° / 330° relative to center. The two horizontal positions (3 o'clock and 9 o'clock) work best for the most important Tier-1 entities.
- Avoid placing nodes too close to ring labels (text at y=220, 115, 25). Nodes inside Ring 1 should not be at y < 240.
- Labels render side-aligned (left-side nodes get right-anchored labels, vice versa). For dense clusters, manually shift `x`/`y` by 5–10px to avoid label overlap.

**Common gotcha**: The center node is hardcoded in the SVG `<g class="cn-node">` block, NOT in `CN_NODES`. To change the center label/subtitle, edit the `<text>` elements inside that block directly.

## Object 4: `DETAIL_DATA`

Drives all drawer content. Keyed by either:
- A `node-*` id from `CN_NODES` (Part 5 click-throughs)
- A `case-*` id from a Part 6 archetype-badge `data-id` attribute

```javascript
const DETAIL_DATA = {
  'node-stripe': {
    title: "Stripe",
    sub:   "全球最大私营支付基础设施 · ~$70-90B 估值",
    tags:  [["global", "Tier 1 · 战略核心"]],   // [type, label] pairs
    bg: [
      "<strong>创始人</strong>: ...",
      "<strong>体量</strong>: ...",
      "<strong>近期动作</strong>: ...",
      "<strong>客户</strong>: ..."
    ],
    relevance: "<strong>对你的研究的含义</strong>。可以引用 <em>independent analyst quotes</em>。",
    src: "Source 1 · Source 2 · 来源备注"
  },
  // ... one entry per node-* id and per case-* id
};
```

**Field semantics**:
- `title` — drawer top heading (large)
- `sub` — subtitle below title (small caps style metadata)
- `tags` — array of `[type, label]` tuples. Each tag renders as a colored pill. Types and their colors:
  - `local` (green) — local champion / regional player
  - `global` (red) — Tier-1 / global player / direct competitor
  - `crypto` (gold) — crypto / web3 / experimental
  - `exit` (teal) — confirmed acquisition / exit case
- `bg` — array of "key facts" bullet points (renders as `<ul>` in drawer). Supports HTML. Aim for 3–5 bullets, each 1 sentence with **bold key**: detail format.
- `relevance` — single paragraph (supports HTML) explaining "what this entity means for your research / business / decision". This is the highest-value field — invest more text here than in any other.
- `src` — comma- or `·`-separated list of sources. Renders in monospace footer of drawer.

**Validation requirement**: Every `id` in `CN_NODES` AND every `data-id` on a Part 6 `archetype-badge` element MUST have a matching key in `DETAIL_DATA`. After filling, run:

```bash
node -e '
const fs = require("fs");
const html = fs.readFileSync("file.html","utf8");
const ids = [...new Set([...html.matchAll(/{ id: "([^"]+)"/g)].map(m=>m[1]))];
const badges = [...new Set([...html.matchAll(/data-id="([^"]+)"/g)].map(m=>m[1]))];
const keys = [...new Set([...html.matchAll(/^\s*"([^"]+)":\s*\{/gm)].map(m=>m[1]))];
console.log("nodes missing detail:", ids.filter(x=>!keys.includes(x)));
console.log("badges missing detail:", badges.filter(x=>!keys.includes(x)));
'
```

Both arrays should be empty.

## Tab id naming convention

The HTML tab buttons use `data-region`, `data-sector`, `data-cregion` attributes. These must match keys in the JS data exactly:

| HTML attribute | JS object key | Example |
|---|---|---|
| `data-region="region-a"` | `CASE_STUDY_DATA['region-a']` | Part 3 |
| `data-sector="sector-1"` | `SECTOR_DATA['sector-1']` | Part 4 |
| `data-cregion="cat-1"` | `CN_NODES[i].cat === 'cat-1'` | Part 5 filter |
| `data-id="case-juspay"` | `DETAIL_DATA['case-juspay']` | Part 6 |
| (constellation node click) | `DETAIL_DATA[<node id>]` | Part 5 |

Stick with the kebab-case convention. Use descriptive ids (`node-stripe`, `case-juspay`) instead of generic ones (`node-1`, `case-a-1`) — descriptive ids make the data easier to maintain and the bug surface smaller.
