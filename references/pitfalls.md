# Known pitfalls + validation checklist

写完 HTML 后，按下面顺序自检。每一项都有具体怎么找。

---

## #1 — 模板 instruction 注释残留

模板里有 `⚙️ PART X — 何时用 / 何时跳` 和 `✏️ EDIT` 这类编辑提示。**最终交付的 HTML 里必须全部清除**。

### 怎么找

```bash
grep -E '⚙️ PART|何时用|何时跳|✏️ EDIT' /tmp/output.html
```

无任何匹配 = ✓。

---

## #2 — Placeholder 文字残留

模板里 `[YOUR_TOPIC]` / `[NUMBER_HERE]` / `[STAT_BLOCK_N]` 这类大写下划线占位符**必须全部替换成具体内容**。

### 怎么找

```bash
grep -oE '\[(YOUR_|NUMBER_|STAT_|N\s|[A-Z_]{4,})' /tmp/output.html
```

无任何匹配 = ✓。

---

## #3 — CN_NODES / data-id / DETAIL_DATA 三者 ID 不一致

Constellation Map 节点（`CN_NODES`）、Hero 区/章节里的 badge（`data-id`）、和 drawer 详情字典（`DETAIL_DATA`）三者必须 ID 全部对齐。任何一个 ID 在前两者出现但 DETAIL_DATA 没有 → drawer 点开是空白。

### 怎么找

```javascript
// Node 里跑
const fs = require('fs');
const html = fs.readFileSync('/tmp/output.html','utf8');

// 提取所有 CN_NODES.id
const cnSection = html.match(/CN_NODES\s*=\s*\[([\s\S]*?)\];/);
const cnIds = cnSection ? [...cnSection[1].matchAll(/id:\s*['"]([^'"]+)['"]/g)].map(m=>m[1]) : [];

// 提取所有 data-id (badge / dd-pill)
const dataIds = [...new Set([...html.matchAll(/data-id=['"]([^'"]+)['"]/g)].map(m=>m[1]))];

// 提取所有 DETAIL_DATA key
const detailSection = html.match(/DETAIL_DATA\s*=\s*\{([\s\S]*?)^\};/m);
const detailKeys = detailSection ? [...detailSection[1].matchAll(/^\s*['"]([^'"]+)['"]\s*:\s*\{/gm)].map(m=>m[1]) : [];

const allReferenced = [...new Set([...cnIds, ...dataIds])];
const missing = allReferenced.filter(id => !detailKeys.includes(id));

console.log('CN_NODES:', cnIds.length, '| data-id:', dataIds.length, '| DETAIL_DATA keys:', detailKeys.length);
console.log('Missing in DETAIL_DATA:', missing.length === 0 ? '✓ 无' : missing);
```

**Missing 必须是空数组**。否则补全 `DETAIL_DATA` 里缺的 key（每个 entry 至少 4 个 bullet + 来源）。

---

## #4 — JS 语法错误（中文引号 / 转义）

中文标点和 HTML 实体在 JS 字符串里很容易踩坑。

### 怎么找

```bash
node -e "
const fs = require('fs');
const html = fs.readFileSync('/tmp/output.html','utf8');
const scripts = [...html.matchAll(/<script>([\s\S]*?)<\/script>/g)].map(m => m[1]);
console.log('Script blocks:', scripts.length);
scripts.forEach((s, i) => {
  try { new Function(s); console.log('Block', i+1, '✓ valid'); }
  catch(e) { console.log('Block', i+1, '✗ ERROR:', e.message); }
});
"
```

常见错误：
- `desc: "Stripe 收购 Bridge "Q4 announce""` ← 内层双引号没转义 → 改用单引号或反斜杠
- `desc: "市占率 ~17%"` 后跟 `,` 漏了 → 缺 comma
- 中文括号 `（）` 在 string 里 OK，但作为 JS 语法 `（` 不行

---

## #5 — Constellation Map 节点数 / 内容深度不达标

**节点数 sweet spot：18-28 个**。少于 18 → 显得空；多于 28 → 拥挤难看。

### 怎么找

```javascript
// Node 里
const html = require('fs').readFileSync('/tmp/output.html','utf8');
const cnSection = html.match(/CN_NODES\s*=\s*\[([\s\S]*?)\];/);
const ids = [...cnSection[1].matchAll(/id:\s*['"]([^'"]+)['"]/g)].map(m=>m[1]);
console.log('CN_NODES count:', ids.length, '(target: 18-28)');

// 每个 node 应该有 4 个字段
const nodes = [...cnSection[1].matchAll(/\{[\s\S]*?\}/g)].map(m=>m[0]);
const incomplete = nodes.filter(n =>
  !n.includes('id:') || !n.includes('name:') || !n.includes('type:') || !n.includes('ring:')
);
console.log('Incomplete nodes:', incomplete.length, '(target: 0)');
```

每个 node 必须有 `id` / `name` / `type` / `ring` / `angle` 五项。

---

## #6 — Hero stats 没来源

Hero 的 4 个 `<span class="detail">` 里**必须**有具体来源（年份 + 媒体 / 公司 PR）。

### 怎么找

```bash
grep -A 1 'class="num"' /tmp/output.html | grep "detail" | head -5
# 每行应该包含: SEC / 年报 / Bloomberg / CNBC / 官方 / Q1 Q2 Q3 Q4 / 2024-2026
```

如果某个 detail 写的是"约"、"近"、"行业平均"这种含糊词 → 重写带具体来源。

---

## #7 — 章节顺序错乱 / 跳号没说明

**写完检查 sidebar nav 跟实际 section 顺序对得上**。如果跳过了 Part 7（无锚点），sidebar 也要把 Part 7 删掉。

### 怎么找

```bash
# 提取 sidebar 里的 part 编号
grep -oE 'href="#part-(\d+)"' /tmp/output.html | sort -u

# 提取实际 section 的 part 编号
grep -oE 'id="part-(\d+)"' /tmp/output.html | sort -u

# 两者必须完全一致
```

---

## #8 — 内容深度不达标（向 VC / CB Insights 看齐）

### 自检清单

每个 section 写完跑一遍：

| 检查项 | 标准 | 怎么算不达标 |
|---|---|---|
| 有具体数字吗 | 每段至少 1 个 | "市场领先" / "增长强劲" 一个都没数字 |
| 有来源吗 | 每个数字标 [Source: X] | "据估计" / "约" 出现 = 没来源 |
| 有反共识吗 | Hero + Part 1 + Part 17 + Part 21 | 全篇都是常识 |
| 可证伪吗 | 判断章节 | "未来可期" 这种话 |
| 有死亡案例吗 | Part 9 / 12 | 只讲赢家 |

---

## #9 — Drawer 内容太薄

drawer 不只是放公司名 + 一句话——是给读者**深挖一个节点**用的。每个 entry 至少：

- `summary`：1 段（30-80 字）
- `bullets`：3-6 条具体事实（每条带数字或时间）
- `source`：1-3 个来源

**标准对照**：写完 Stripe 那个 entry 的深度 = drawer 的最低标准。

---

## #10 — 视觉风格被破坏

新模板使用暖米色（#F1ECDF）+ 砖红（#A0392F）+ JetBrains Mono 等宽字体。

不要：
- 改 CSS 变量颜色
- 加 emoji 滥用（除了 hero 国旗 / 公司图标）
- 用粗体黑色当强调（用 `<em>` 让它变砖红色）
- 加阴影 / gradient 当装饰（破坏 editorial 调性）

如果用户要求"更现代风格"——也不要在这个模板上改 CSS。建议用户用别的工具，本 skill 的视觉风格固定。

**硬规则**：最终 HTML 必须保留模板骨架——单个 `<style>` block、`.toolbar`、`.toc`、`.hero`、`.section`、`.drawer`、`.modal`、`body` CSS variables。**不要重写成新的黑白文档结构**。骨架损坏的具体检测项见 #11。

---

## #11 — Template wiring / 视觉骨架损坏

**最常见也最致命的失败模式**：模型决定"重头写一份新 HTML 复用 template 的 CSS/JS framework"，结果只复用了概念，没真的把所有 CSS 变量、按钮 ID、TOC 结构复制过去——产物变成黑白裸 HTML，没侧栏、按钮失效、配色全无。

这是 v3.0-beta 跑字节跳动报告时实际发生过的 bug。Codex 拆出 7 个具体 wiring 错误：

1. 文件头尾出现 `<style> <style>` 重复 → CSS 解析不稳定
2. HTML 用 `id="searchBtn"` 但 JS 找 `searchToggle` → 按钮全部失效
3. HTML 用 `id="glossaryBtn"` 但 JS 找 `glossaryToggle`
4. HTML 用 `id="darkBtn"` 但 JS 找 `darkToggle` → 暗色模式坏
5. `.toc` CSS 写 `display:none` + `@media(max-width:1500px){.toc{display:none!important;}}` → 桌面正常屏幕完全看不到目录
6. `<a href="#part-N">` 缺 `class="toc-link"` → scrollspy 高亮坏
7. JS 用了 `var(--bad)` / `var(--good)` / `var(--bg-2)` 但 `:root` 没定义 → Constellation Map 节点退回默认色

### 怎么找

下面的检查已并入完整一键验证脚本（Check #7）。单跑：

```javascript
// Node 里
const fs = require('fs');
const html = fs.readFileSync('/tmp/output.html','utf8');

const styleOpen = (html.match(/<style>/g) || []).length;
const styleClose = (html.match(/<\/style>/g) || []).length;
const duplicateStyle = styleOpen !== 1 || styleClose !== 1;

const hasToc = /<nav[^>]+class=["'][^"']*\btoc\b/.test(html);
const tocLinks = [...html.matchAll(/<a[^>]+href=["']#part-\d+["'][^>]*>/g)].map(m => m[0]);
const tocLinksMissingClass = tocLinks.filter(a => !/class=["'][^"']*\btoc-link\b/.test(a));

const hasToolbar = /class=["'][^"']*\btoolbar\b/.test(html);
const ids = id => html.includes(`id="${id}"`) || html.includes(`id='${id}'`);

const buttonPairs = [
  ['searchBtn', 'searchToggle'],
  ['glossaryBtn', 'glossaryToggle'],
  ['darkBtn', 'darkToggle'],
];

const mismatchedButtons = buttonPairs.filter(([btn, toggle]) => {
  const htmlHasBtn = ids(btn);
  const htmlHasToggle = ids(toggle);
  const jsUsesBtn = html.includes(`getElementById('${btn}')`) || html.includes(`getElementById("${btn}")`);
  const jsUsesToggle = html.includes(`getElementById('${toggle}')`) || html.includes(`getElementById("${toggle}")`);
  return (htmlHasBtn && jsUsesToggle) || (htmlHasToggle && jsUsesBtn);
});

const requiredVars = ['--bg', '--bg-card', '--ink', '--accent', '--gold', '--rule'];
const missingVars = requiredVars.filter(v => !html.includes(v + ':'));

const usedCustomVars = [...new Set([...html.matchAll(/var\((--[^),\s]+)/g)].map(m => m[1]))];
const definedVars = [...new Set([...html.matchAll(/(--[a-zA-Z0-9-]+)\s*:/g)].map(m => m[1]))];
const undefinedVars = usedCustomVars.filter(v => !definedVars.includes(v));

const badTocMedia = /@media\s*\(max-width:\s*1500px\)\s*\{[^}]*\.toc\s*\{\s*display\s*:\s*none\s*!important/i.test(html);

const wiringErrors = [];
if (duplicateStyle) wiringErrors.push(`style tags ${styleOpen}/${styleClose}, expected 1/1`);
if (!hasToc) wiringErrors.push('missing nav.toc');
if (tocLinksMissingClass.length > 0) wiringErrors.push(`${tocLinksMissingClass.length} toc links missing class="toc-link"`);
if (!hasToolbar) wiringErrors.push('missing toolbar');
if (mismatchedButtons.length > 0) wiringErrors.push('toolbar id mismatch: ' + mismatchedButtons.map(p => p.join(' vs ')).join(', '));
if (missingVars.length > 0) wiringErrors.push('missing core CSS vars: ' + missingVars.join(', '));
if (undefinedVars.length > 0) wiringErrors.push('undefined CSS vars used: ' + undefinedVars.join(', '));
if (badTocMedia) wiringErrors.push('toc hidden at max-width:1500px; use ~1180px or provide working toggle');

console.log('Template wiring:', wiringErrors.length === 0 ? '✓' : '✗ ' + wiringErrors.join(' | '));
```

### 怎么避免

**根因不在校验脚本，在生成阶段**。SKILL.md Critical Rule 6 已要求：必须基于 `assets/template.html` 做 surgical 替换，不得 rebuild from scratch。

如果发现自己在写"为了适配新 topic，重新组织一下整个 HTML 结构"——**停下，丢掉，重新打开 `assets/template.html` 复制**。模板的 CSS 变量、button ID、TOC `class="toc-link"`、JS bindings 都是 load-bearing，不能重组。

校验脚本是最后一道兜底，不是设计阶段的指导原则。

---

## 完整一键验证脚本（7 项）

写完跑这一段：

```bash
node << 'EOF'
const fs = require('fs');
const html = fs.readFileSync('/tmp/output.html','utf8');

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

// 5. Constellation count
console.log('5. CN_NODES count:', cnIds.length >= 18 && cnIds.length <= 28 ? '✓ ' + cnIds.length : '⚠ ' + cnIds.length + ' (target 18-28)');

// 6. Sidebar consistency
const sidebarParts = [...new Set([...html.matchAll(/href="#part-(\d+)"/g)].map(m=>parseInt(m[1])))];
const actualParts = [...new Set([...html.matchAll(/id="part-(\d+)"/g)].map(m=>parseInt(m[1])))];
const sidebarMissing = actualParts.filter(p => !sidebarParts.includes(p));
const sidebarOrphan = sidebarParts.filter(p => !actualParts.includes(p));
console.log('6. Sidebar match:', (sidebarMissing.length + sidebarOrphan.length) === 0 ? '✓ (' + actualParts.length + ' parts)' : '✗ missing nav: ' + sidebarMissing + ', orphan nav: ' + sidebarOrphan);

// 7. Template wiring integrity (新增 — 防止字节跳动那种黑白裸 HTML)
const styleOpen = (html.match(/<style>/g) || []).length;
const styleClose = (html.match(/<\/style>/g) || []).length;
const duplicateStyle = styleOpen !== 1 || styleClose !== 1;

const hasToc = /<nav[^>]+class=["'][^"']*\btoc\b/.test(html);
const tocLinks = [...html.matchAll(/<a[^>]+href=["']#part-\d+["'][^>]*>/g)].map(m => m[0]);
const tocLinksMissingClass = tocLinks.filter(a => !/class=["'][^"']*\btoc-link\b/.test(a));

const hasToolbar = /class=["'][^"']*\btoolbar\b/.test(html);
const ids = id => html.includes(`id="${id}"`) || html.includes(`id='${id}'`);

const buttonPairs = [
  ['searchBtn', 'searchToggle'],
  ['glossaryBtn', 'glossaryToggle'],
  ['darkBtn', 'darkToggle'],
];

const mismatchedButtons = buttonPairs.filter(([btn, toggle]) => {
  const htmlHasBtn = ids(btn);
  const htmlHasToggle = ids(toggle);
  const jsUsesBtn = html.includes(`getElementById('${btn}')`) || html.includes(`getElementById("${btn}")`);
  const jsUsesToggle = html.includes(`getElementById('${toggle}')`) || html.includes(`getElementById("${toggle}")`);
  return (htmlHasBtn && jsUsesToggle) || (htmlHasToggle && jsUsesBtn);
});

const requiredVars = ['--bg', '--bg-card', '--ink', '--accent', '--gold', '--rule'];
const missingVars = requiredVars.filter(v => !html.includes(v + ':'));

const usedCustomVars = [...new Set([...html.matchAll(/var\((--[^),\s]+)/g)].map(m => m[1]))];
const definedVars = [...new Set([...html.matchAll(/(--[a-zA-Z0-9-]+)\s*:/g)].map(m => m[1]))];
const undefinedVars = usedCustomVars.filter(v => !definedVars.includes(v));

const badTocMedia = /@media\s*\(max-width:\s*1500px\)\s*\{[^}]*\.toc\s*\{\s*display\s*:\s*none\s*!important/i.test(html);

const wiringErrors = [];
if (duplicateStyle) wiringErrors.push(`style tags ${styleOpen}/${styleClose}, expected 1/1`);
if (!hasToc) wiringErrors.push('missing nav.toc');
if (tocLinksMissingClass.length > 0) wiringErrors.push(`${tocLinksMissingClass.length} toc links missing class="toc-link"`);
if (!hasToolbar) wiringErrors.push('missing toolbar');
if (mismatchedButtons.length > 0) wiringErrors.push('toolbar id mismatch: ' + mismatchedButtons.map(p => p.join(' vs ')).join(', '));
if (missingVars.length > 0) wiringErrors.push('missing core CSS vars: ' + missingVars.join(', '));
if (undefinedVars.length > 0) wiringErrors.push('undefined CSS vars used: ' + undefinedVars.join(', '));
if (badTocMedia) wiringErrors.push('toc hidden at max-width:1500px; use ~1180px or provide working toggle');

console.log('7. Template wiring:', wiringErrors.length === 0 ? '✓' : '✗ ' + wiringErrors.join(' | '));

EOF
```

**7 项全 ✓ 才能交付。**

任何一项 ✗ 都要回去改，改完重跑这段脚本。Check #7 (Template wiring) 是 v3.0.1 新增——**这一项失败说明你 rebuild from scratch 了，不是 surgical 替换 `assets/template.html`，必须重新开始**。
