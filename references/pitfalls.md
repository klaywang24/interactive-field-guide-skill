# Pitfalls

Every prior attempt at filling this template has hit at least one of these. Read this whole file before delivering. The fixes are short; the costs of missing them are: a broken document, an embarrassed re-deliver, or a debugging session in front of the user.

## #1 — Editing-instructions left in the file

The bundled `assets/template.html` ships with a 47-line HTML comment block right after `<body>` titled:

```html
<!--
============================================================================
  📘 INTERACTIVE FIELD GUIDE TEMPLATE · 编辑指南
  ============================================================================
  ...
  ▍如何使用本模板
  1. 整篇文档分两类内容:
     A. 静态文字 (HTML 内) —— 标记为 <!-- ✏️ EDIT_X -->
     B. 交互数据 (JS 内) —— 在文件底部 SCRIPT 标签里的 CONFIG 对象里改
  2. 12 个 Part 的内容类型 + 编辑位置:
     ✏️ Part 1   序言/第一性原理      → HTML 段落
     ...
-->
```

This is instructions for the person filling the template. **It is not content.** It must be removed before delivery.

It also includes:
- Four large `/* ============= ⚙️ NAME — Part X (...) 数据 ============= */` JS comment blocks above each data object
- Inline `<!-- ✏️ EDIT_PART_X: ... -->` and `<!-- ⚙️ ... -->` HTML comments scattered through the body
- `/*JSPART2*/` and `/*JSPART3*/` markers between JS blocks

### How to remove

```bash
# Remove the big HTML comment block
python3 -c '
with open("file.html") as f: c = f.read()
start = c.find("<!--\n=========")
end = c.find("-->\n<div class=\"progress-bar\"")
if start != -1 and end != -1:
    c = c[:start] + c[end + len("-->\n"):]
    with open("file.html","w") as f: f.write(c)
'
```

For the JS comment headers, replace each verbose block with a one-liner:

```javascript
/* CASE_STUDY_DATA · Part 3 thematic groups, three-layer evaluation */
const CASE_STUDY_DATA = { ... };
```

For the inline `<!-- ✏️ EDIT_X: ... -->` comments, just delete them.

### Verification

```bash
grep -nE "INTERACTIVE FIELD GUIDE TEMPLATE|EDIT_HERO|EDIT_PART|EDIT_FOOTER|✏️|⚙️|CONFIG 对象|HTML 段落|HTML 5 张卡|JSPART" file.html
```

Should return zero matches.

## #2 — Unescaped double-quotes inside JS string literals

The data objects (`CASE_STUDY_DATA`, `SECTOR_DATA`, `CN_NODES`, `DETAIL_DATA`) are full of long Chinese / English prose strings wrapped in `"..."`. When the prose contains an inner `"emphasis"` it breaks the string literal and the JS fails to parse.

### What this looks like

```javascript
// BROKEN — unescaped inner quote
desc: "MetaMask、Solflare 这些钱包已经把"加密资产 ↔ 法币"的入口建好了。"
//                                       ^^^^^^^^^^^                      
// JS sees this as: string1 = "MetaMask...已经把"
//                  identifier = 加密资产 ↔ 法币
//                  string2 = "的入口建好了。"
// → SyntaxError: Unexpected identifier '加密资产'
```

### Three ways to fix

**Option A (preferred for Chinese)**: Replace inner `"..."` with Chinese corner brackets `「...」`:

```javascript
desc: "MetaMask、Solflare 这些钱包已经把「加密资产 ↔ 法币」的入口建好了。"
```

**Option B**: Escape with backslashes:

```javascript
desc: "MetaMask、Solflare 这些钱包已经把\"加密资产 ↔ 法币\"的入口建好了。"
```

**Option C**: Use single-quotes for the outer string (only works if you don't have inner single-quotes — JavaScript apostrophes in English text break this):

```javascript
desc: 'MetaMask、Solflare 这些钱包已经把"加密资产 ↔ 法币"的入口建好了。'
```

For Chinese prose, A is by far the cleanest visually.

### Detection

After filling all four data objects, run:

```bash
python3 << 'EOF'
import re
with open('file.html') as f: c = f.read()
m = re.search(r'<script>([\s\S]*?)</script>', c)
js = m.group(1)
script_off = c[:m.start()].count('\n') + 1

for i, line in enumerate(js.split('\n')):
    ln = script_off + i
    # Find positions of unescaped "
    poss = []
    for j, ch in enumerate(line):
        if ch == '"':
            bs = 0; k = j-1
            while k >= 0 and line[k] == '\\':
                bs += 1; k -= 1
            if bs % 2 == 0: poss.append(j)
    # If 4+ unescaped " in a single line, check that text between odd-pair gaps starts with valid JS separator
    if len(poss) >= 4:
        for p in range(1, len(poss)-1, 2):
            between = line[poss[p]+1:poss[p+1]].strip()
            if not between: continue
            if not (between[0] in ',:}{[]+=' or between.startswith(('//','&&','||','?'))):
                print(f"L{ln}: broken string -- between quotes: {between[:60]!r}")
                print(f"  {line[:200]}")
                break
EOF
```

This catches the pattern. Lines from template-literal blocks (with backticks) will produce false positives — ignore those (they're the renderer functions, not data).

### Final validation

```bash
node -e '
const fs = require("fs");
const m = fs.readFileSync("file.html","utf8").match(/<script>([\s\S]*?)<\/script>/);
try { new Function(m[1]); console.log("✓ JS syntax OK"); }
catch(e) { console.log("✗", e.message); process.exit(1); }
'
```

If this fails with `Unexpected identifier '<some text>'`, you have an unescaped inner quote on the line containing that text.

## #3 — DETAIL_DATA / CN_NODES / archetype-badge id mismatch

Drawers open by looking up `DETAIL_DATA[id]`. If a node's `id` in `CN_NODES` or a badge's `data-id` doesn't have a matching key in `DETAIL_DATA`, clicking it does nothing (no error in console either — the renderer silently returns).

### Detection

```bash
node -e '
const fs = require("fs");
const html = fs.readFileSync("file.html","utf8");
const nodes = [...new Set([...html.matchAll(/{ id: "([^"]+)"/g)].map(m=>m[1]))];
const badges = [...new Set([...html.matchAll(/data-id="([^"]+)"/g)].map(m=>m[1]))];
const keys = [...new Set([...html.matchAll(/^\s*"([^"]+)":\s*\{/gm)].map(m=>m[1]))];
console.log("CN_NODES count:", nodes.length);
console.log("Badge count:", badges.length);
console.log("nodes ∉ DETAIL_DATA:", nodes.filter(x => !keys.includes(x)));
console.log("badges ∉ DETAIL_DATA:", badges.filter(x => !keys.includes(x)));
'
```

Both "missing" arrays should be empty.

**Common cause**: Renaming a node id without updating `DETAIL_DATA`, or adding a new badge in HTML and forgetting to add a matching `DETAIL_DATA[case-X]` entry.

## #4 — Placeholder text in `[Chinese full-width brackets]` left behind

The template uses `[bracket placeholders]` in both English and Chinese. English ones (`[YOUR INDUSTRY]`, `[Industry]`, `[关键概念]`) are easy to grep. But the Chinese full-width brackets (`【关键概念】`) and the half-width Chinese-context brackets (`[关键概念]` — same `[` `]` chars but with Chinese inside) are easy to miss.

### Detection

```bash
grep -nE '\[(YOUR|Industry|highlight|关键概念|玩家|事实|来源|相关性|副标|描述|条件|地区|对象|案例|关键|简短|简介|简短副标|地区/对象|玩家 \d|事实 \d)' file.html
```

Should return zero matches.

## #5 — Constellation Map with too few nodes looks empty

The constellation SVG is a 960×720 viewBox with 3 large rings. With fewer than ~15 nodes, there's a lot of empty space and the visual feels broken. Three options:

1. **Get more entities** — expand the scope of the report or include adjacent players
2. **Tighten the rings** — reduce the ring radii in the SVG `<circle>` elements (e.g., 90 / 180 / 260 instead of 125 / 230 / 320) and reduce node coordinates accordingly
3. **Skip Part 5 entirely** — remove the section from HTML and TOC

If you ship Part 5 with 5–10 nodes, the report looks unfinished. Always pick one of these three.

## #6 — Renderer functions break when data is missing

The renderers (`renderCase`, `renderSector`, `renderConstellation`) return early when called with a nonexistent key:

```javascript
function renderCase(key){
  const d = CASE_STUDY_DATA[key]; if(!d) return;
  // ...
}
```

So they don't error. But the initial render call at the bottom of the script:

```javascript
renderCase('region-a');
renderSector('sector-1');
renderConstellation('all');
```

silently does nothing if `'region-a'` isn't a key. The result: the reader clicks a tab and nothing happens.

If you skip Part 3 or Part 4, also remove the corresponding initial render call. If you keep the parts but rename the first tab id, update the initial render call to match.

## #7 — TOC navigation links pointing to skipped sections

The `<nav class="toc">` block has `<a href="#part-N">` links. If you skip a part (e.g., remove Part 4), also remove its TOC link. Leaving dead links produces "anchor not found" jumps that scroll to nowhere.

If you add a part (e.g., insert Part 6.5), remember to add its TOC link.

## #8 — Mobile responsiveness traps

The template's TOC sidebar uses `@media(max-width:1500px){.toc{display:none!important;}}` — it hides on screens narrower than 1500px to prevent overlap with main content. This is correct behavior (don't change it).

But: the `.matrix-2x2` 2×2 quadrant uses `aspect-ratio: 1.45/1`. On portrait/mobile widths it switches to `aspect-ratio: 1/1.45` and the column structure changes. If you put very long text in a quadrant title, it can overflow. Keep `matrix-quad-title` to ≤6 words.

The `.condition-strip` 5-card row collapses to 2 columns at <900px and 1 column at <600px. Cards in this layout shouldn't have `min-height` overrides that break stacking.

## #9 — Center node SVG hardcoded outside CN_NODES

The Constellation center node (Mastercard / Stripe / whatever the report's subject is) is rendered by a hardcoded `<g class="cn-node">` block in the SVG, NOT by the `CN_NODES` array. To change the center label/subtitle, edit the SVG directly:

```html
<g class="cn-node">
  <circle cx="480" cy="360" r="48" class="cn-circle center"/>
  <text x="480" y="354" ...>Mastercard</text>
  <text x="480" y="370" ...>NYSE:MA · ~$520B</text>
</g>
```

Forgetting this leaves the center showing template placeholder text.

## #10 — Center gradient color stuck at template default

The radial gradient at the center uses three `stop-color` values inside `<defs><radialGradient id="centerGradient">`. The default is purple (`#A57AC0` / `#7B4FA0` / `#4A2D6E`). Most reports look better with a brand-matched gradient:

- For payments / fintech: `#FF7F50` / `#EB001B` / `#A0392F` (Mastercard-ish red)
- For tech / AI: a deep blue gradient (`#6FA8DC` / `#3D85C6` / `#073763`)
- For climate / nature: a green gradient (`#93C47D` / `#38761D` / `#274E13`)

Match the gradient to your subject for visual coherence with the rest of the report.

## Final checklist before `present_files`

```bash
# 1. No template instructions remaining
grep -nE "INTERACTIVE FIELD GUIDE TEMPLATE|EDIT_HERO|EDIT_PART|EDIT_FOOTER|✏️|⚙️|CONFIG 对象|HTML 段落|HTML 5 张卡|JSPART" file.html
# Expect: 0 lines

# 2. No unfilled placeholders
grep -nE '\[(YOUR|Industry|highlight|关键概念|玩家|事实|来源|相关性|副标|描述|条件|地区|对象|案例|关键|简短|简介|简短副标)' file.html
# Expect: 0 lines

# 3. JS parses
node -e '
const m = require("fs").readFileSync("file.html","utf8").match(/<script>([\s\S]*?)<\/script>/);
try { new Function(m[1]); console.log("✓"); } catch(e) { console.log("✗", e.message); process.exit(1); }
'
# Expect: ✓

# 4. All node/badge ids have DETAIL_DATA entries
node -e '
const html = require("fs").readFileSync("file.html","utf8");
const ids = [...new Set([...html.matchAll(/{ id: "([^"]+)"/g)].map(m=>m[1]))];
const badges = [...new Set([...html.matchAll(/data-id="([^"]+)"/g)].map(m=>m[1]))];
const keys = [...new Set([...html.matchAll(/^\s*"([^"]+)":\s*\{/gm)].map(m=>m[1]))];
console.log("missing:", [...ids, ...badges].filter(x => !keys.includes(x)));
'
# Expect: missing: []
```

If all four checks pass, you're ready to deliver.
