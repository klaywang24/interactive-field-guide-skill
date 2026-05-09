# Pitfalls — 写完之前必跑 5 项检查

每次产物都会遇到下面之一，**生成完 HTML 后跑这 5 个验证**。任意一项 fail 就回去修。

---

## #1 — 模板指令注释残留

新模板每个 section 顶部有 `<!-- ⚙️ PART N · 标题 ... -->` 的元注释，里面有 "何时用 / 何时跳 / 内容形状 / 关键组件" 4 行说明。**这些是给写作者看的，必须在最终产物里全部删掉**。

### 怎么找

```bash
grep -nE "⚙️ PART|何时用|何时跳|内容形状|关键组件|✏️ EDIT" /tmp/output.html
# 应该返回 0 行
```

如果还有，跑这个清理：

```bash
python3 -c "
import re
with open('/tmp/output.html') as f: c = f.read()
c = re.sub(r'<!--\s*⚙️ PART.*?-->\n?', '', c, flags=re.DOTALL)
c = re.sub(r'<!--\s*✏️.*?-->\n?', '', c, flags=re.DOTALL)
c = re.sub(r'<!-- 可选 component.*?-->\n?', '', c, flags=re.DOTALL)
with open('/tmp/output.html','w') as f: f.write(c)
"
```

---

## #2 — 占位符没替换

模板里有大量 `[YOUR_TOPIC]` / `[NUMBER_1]` / `[STAT_VALUE_1]` / `[N 大原则]` / `[FIRST_PRINCIPLE_HEADLINE]` 这类占位符。**任何一个留下都很尴尬**。

### 怎么找

```bash
grep -nE '\[(YOUR_|NUMBER_|STAT_|N |[A-Z_]{3,})' /tmp/output.html | grep -v '<!--'
# 应该返回 0 行（HTML 注释里的占位符不算）
```

漏一个 placeholder 就回去填，或如果章节不适用就**整个章节跳过**（不要保留半填的章节）。

---

## #3 — JS 数据对象 vs HTML id/data-id 错配

最严重的 bug：drawer 弹出空白 = `data-id` 在 HTML 里有，但 `DETAIL_DATA` 里没对应 key。

### 怎么找

```javascript
// 在 Node 里跑
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

---

## 完整一键验证脚本

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

EOF
```

6 项全 ✓ 才能交付。
