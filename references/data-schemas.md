# Component Data Schemas · 数据形状速查

**核心原则**：JS 数据对象的 key **必须**和 HTML 里 `id` / `data-id` 一一对应。任何 key 错配都会让 drawer 弹空白。

---

## Hero Stats

```html
<div class="hero-stats">
  <div class="stat-block">
    <span class="num">[NUMBER]</span>
    <span class="label">[LABEL]</span>
    <span class="detail">[CONTEXT + Source]</span>
  </div>
  <!-- × 4 个 -->
</div>
```

**深度要求**：4 个数字必须在不同维度（不能 4 个都是估值）。每个 detail 必须有时间 + 来源。

---

## Callout 强调框

```html
<div class="callout fact">  <!-- 或 lesson | investor | data -->
  <span class="callout-label">数据可信度承诺</span>
  <p>[内容]</p>
</div>
```

**4 种 type**：
- `fact` (砖红)：核心事实声明
- `lesson` (sky 蓝)：经验教训
- `investor` (teal)：投资人视角
- `data` (gold)：数据 caveats

---

## Condition Strip (5 张并排卡片) — Part 3

```html
<div class="condition-strip">
  <div class="cc-card">
    <div class="cc-num">①</div>
    <p class="cc-title">[条件标题]</p>
    <p class="cc-desc">[一句解释 + 满足/不满足判断标准]</p>
  </div>
  <!-- × 5 张 -->
</div>
```

**深度要求**：5 张内容不能重叠。每张要有判断标准。

---

## Tab + Panel (4-8 个切换标签) — Part 4 / 5 / 15 / 18

```html
<div class="tab-row">  <!-- 或 sector-grid 给 8 个 -->
  <button class="tab-btn active" data-region="key1">
    <span class="tab-num">①</span>[标签名]
  </button>
  <!-- × N -->
</div>
<div class="tab-panel" id="panel-key1">
  <div class="tab-panel-header">
    <h3 class="tab-panel-title">[标题]</h3>
    <span class="tab-panel-meta">[meta 信息]</span>
  </div>
  <!-- panel 内容 -->
</div>
```

**对应 JS**：`renderReap(key)` / `renderSector(key)` / `renderExit(key)` / `renderGtm(key)` 根据 `data-region` 切换 panel。

---

## REAP_DATA · 锚点案例 (Part 4)

```javascript
const REAP_DATA = {
  region1: {
    name: "[案例名]",
    meta: "[meta 描述]",
    layerA: { status: "go|warn|no", label: "✅ 跑通 | 🟡 部分 | ❌ 锁住", desc: "[详细解释]" },
    layerB: { status: "...", label: "...", desc: "..." },
    layerC: { status: "...", label: "...", desc: "..." },
    verdict: "[综合 verdict — 必须是非平庸结论]",
    players: "[关键玩家列表，含估值]"
  },
  // × 4-5 个 region
};
```

**status 规则**：3 选 1，对应 layer-status CSS class。

---

## SECTOR_DATA · 板块深度 (Part 5)

```javascript
const SECTOR_DATA = {
  payments: {
    name: "① Payments · 支付与结算",
    meta: "[全球收入 + 利润池估算]",
    stats: [
      { num: "$1.9T", label: "Stripe TPV (2025)" },
      { num: "~17%", label: "Stripe 全球份额" },
      { num: "0.4%", label: "典型 net take-rate" }
    ],  // 3 个 stat-block
    painpoint: "<strong>核心痛点</strong> · [一句话痛点描述]",
    subdomains: [
      { tag: "P2P", name: "C 端支付", cos: "Venmo · Cash App · Zelle" },
      // × 5-8 个
    ],
    living: [
      { name: "Stripe", type: ["B2B"], desc: "[一句差异化]", stat: "$159B 🟢" },
      // × 10 个活公司
    ],
    dead: [
      { name: "FTX", type: ["B2C"], desc: "[死因 + 启示]", yr: "2022", cause: "fraud", causeLabel: "欺诈" },
      // × 5 个死公司
    ],
    // 可选: smile (微笑曲线), regions (国家分布)
  },
  // × 8 个 sector
};
```

**type 可选**：`B2B` | `B2C`
**cause 可选**：`fraud` | `macro` | `model` | `ops`（对应 cause-tag CSS class）

---

## CN_NODES · Constellation Map 节点 (Part 7)

```javascript
const CN_NODES = [
  {
    id: "stripe",            // ← 必须和 DETAIL_DATA key + drawer data-id 一致
    name: "Stripe",          // 节点显示名
    sub: "$159B · 支付",      // 节点下方小字
    type: "global",          // 决定颜色 — 见下
    ring: 1,                 // 1=最内圈, 3=最外圈
    angle: 30                // 0-360 度，决定节点位置
  },
  // 18-28 个节点（sweet spot）
];
```

**type 可选**：
- `local` 浅蓝（小本土玩家）
- `global` 深蓝（全球玩家）
- `crypto` 灰蓝（加密原生）
- `local-tier1` 深蓝粗边（本地龙头）
- `exit` 退出案例
- `center` 中心节点（用 gradient）

**ring 分布建议**：
- ring 1（内圈）：3-5 个最核心
- ring 2（中圈）：8-12 个二级
- ring 3（外圈）：5-10 个边缘

**angle 分布**：均匀分布同圈节点，避开重叠。

---

## DETAIL_DATA · Drawer 详情 (Part 7 / 8 / 14 共用)

```javascript
const DETAIL_DATA = {
  stripe: {              // ← 和 CN_NODES.id / archetype-badge.data-id / dd-pill.data-id 对应
    tag: "GLOBAL · #1",
    name: "Stripe",
    meta: "$159B · 2026/02 tender",
    tags: [
      { label: "全球", type: "global" },  // type: local|global|crypto|exit
      { label: "支付", type: "" }
    ],
    summary: "[1 段总结 — 这家公司的核心 thesis]",
    bullets: [
      "[关键事实 1，带数字 + 来源]",
      "[关键事实 2]",
      // × 3-6 条
    ],
    source: "CNBC / Stripe PR / 2025 年报"
  },
  // 一个 entry 对应一个 id
};
```

**关键：完整性验证脚本（pitfalls.md 里的）会扫所有 `id` / `data-id` 是不是都在 DETAIL_DATA 里。漏一个 = drawer 弹空白。**

---

## 2×2 Matrix (Part 8 / 9)

```html
<div class="matrix-2x2">
  <div class="matrix-axis-y">[Y 轴标签]</div>
  <div class="matrix-axis-x">[X 轴标签]</div>
  <div class="matrix-axis-top">[右上 +Y +X 标签]</div>
  
  <div class="matrix-quad q1">  <!-- q1 右上 / q2 左上 / q3 左下 / q4 右下 -->
    <div class="matrix-quad-label">[象限名]</div>
    <h4 class="matrix-quad-title">[一句象限主张]</h4>
    <p>[1-2 句解释]</p>
    <span class="archetype-badge" data-id="key1">[公司/原型 1]</span>
    <span class="archetype-badge" data-id="key2">[公司/原型 2]</span>
    <!-- × 3-5 个 badge -->
  </div>
  <!-- × 4 quadrant -->
</div>
```

**badge.data-id 必须在 DETAIL_DATA 里有对应 entry。**

---

## Accordion (Part 13 / 16)

```html
<div class="accordion-list">
  <div class="accordion-item">  <!-- Part 16 用 path-item 多一个 class -->
    <div class="accordion-header">
      <div class="accordion-num">①</div>
      <div class="accordion-title-block">
        <h4 class="accordion-title">[原则 / 路径标题]</h4>
        <span class="accordion-meta">[一句副标题]</span>
      </div>
      <button class="accordion-toggle">+</button>
    </div>
    <div class="accordion-body">
      <div class="accordion-content">
        <p class="accordion-summary">[展开内容 — 强调用 <strong> 和 <em>]</em></p>
      </div>
    </div>
  </div>
  <!-- × 5-10 项 -->
</div>
```

**JS 行为**：点击 header 切换 `.open` class，body 展开。

---

## DD Grid (Part 14)

```html
<div class="dd-grid">
  <div class="dd-pill" data-id="dd-revenue">  <!-- ← 必须在 DETAIL_DATA 里 -->
    <span class="dd-num">①</span>
    <p class="dd-name">[维度名 — e.g. 收入质量]</p>
    <span class="dd-hint">[评分 hint — e.g. 1-10 RUBRIC]</span>
    <span class="dd-arrow">→</span>
  </div>
  <!-- × 8 个 -->
</div>
```

**点击 dd-pill 弹 drawer，drawer 内容来自 DETAIL_DATA[id]。**

---

## Timeline Cards (Part 11)

```html
<div class="tl-grid">
  <div class="tl-card">
    <div class="tl-marker">
      <span class="tl-yr">2025</span>
      <span class="tl-q">Q3</span>
      <span class="tl-num">事件 #5</span>
    </div>
    <div class="tl-body">
      <h3>[事件标题]</h3>
      <p>[1-2 句解读 — 这件事的含义，不是描述]</p>
      <p style="font-size:11px;color:var(--muted);">[Source: ...]</p>
    </div>
  </div>
  <!-- × 5-10 张 -->
</div>
```

---

## Heat Pill (Part 10)

```html
<span class="heat-pill heat-high">高</span>
<span class="heat-pill heat-mid">中</span>
<span class="heat-pill heat-low">低</span>
```

通常嵌在 `<table>` 的 `<td>` 里。

---

## Tier Block (Part 17 / 21)

```html
<div class="tier-block tier1">  <!-- tier1 砖红 / tier2 金 / tier3 灰 -->
  <span class="tier-label">[档位标签 — e.g. 强买入]</span>
  <h4 class="tier-title">[档位主张]</h4>
  <ol>
    <li>[条件 1]</li>
    <li>[条件 2]</li>
  </ol>
</div>
<!-- × 3 (Part 21) 或 × 5 (Part 17) -->
```

---

## Company Table

```html
<table class="company-table">
  <thead>
    <tr>
      <th class="col-name">公司</th>
      <th class="col-stat">估值</th>
      <th class="col-yr">年份</th>
      <th>差异化</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="col-name">Stripe</td>
      <td class="col-stat">$159B</td>
      <td class="col-yr">2026/02</td>
      <td>商家支付 API · 7 行代码接入</td>
    </tr>
    <tr class="dead-row">  <!-- 死亡公司加这个 class 自动变红 -->
      <td class="col-name">FTX</td>
      <td class="col-stat">$32B → $0</td>
      <td class="col-yr">2022</td>
      <td>欺诈 · SBF 挪用客户资金</td>
    </tr>
  </tbody>
</table>
```

---

## 命名约定

| 元素 | 命名规则 | 例子 |
|---|---|---|
| CN_NODES.id | 小写 + `-` 分隔 | `stripe`, `bridge`, `aspire-sg` |
| Sector key | 单词或缩写 | `payments`, `lending`, `wealth` |
| DETAIL_DATA key | 和 id / data-id 严格一致 | `stripe`, `arch-vertical` |
| dd-pill data-id | `dd-` 前缀 | `dd-revenue`, `dd-moat` |
| archetype-badge data-id | `arch-` 前缀 | `arch-vertical`, `arch-platform` |

混用前缀容易撞 key，最好按上面统一。

---

## 数据完整性自查（写完每个 part 跑一遍）

```javascript
// 验证脚本（写在 console 里跑）
const cnIds = CN_NODES.map(n => n.id);
const badgeIds = [...document.querySelectorAll('[data-id]')].map(el => el.dataset.id);
const detailKeys = Object.keys(DETAIL_DATA);
const allRefs = [...new Set([...cnIds, ...badgeIds])];
const missing = allRefs.filter(id => !detailKeys.includes(id));
console.log('Missing in DETAIL_DATA:', missing);  // 必须是空数组
```

漏一个 → 视觉上 drawer 弹空白 → 用户立刻发现是 bug。
