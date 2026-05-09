# Interactive Field Guide · Claude Skill

> 一个 Claude skill，把任何研究主题打成有侧栏导航、互动节点图、2×2 战略象限的精致 HTML 报告
>
> A Claude skill that turns any research topic into a polished interactive HTML report with sidebar nav, an SVG ecosystem map, a 2×2 strategic matrix, and a 12-part editorial structure.

---

## 它解决什么问题 · What it solves

**中文：**

写深度研究的时候，有三种格式都不太够用：

- **Markdown 文档**——能写但很扁平，读者只能从头读到尾，没法快速 explore
- **PPT / Keynote**——视觉好但牺牲文字密度，分析的细节容易丢
- **临时拼的 HTML**——每次都要重新设计 CSS / 交互，做完一份没法复用

这个 skill 用一个 1340 行的暖米色 + 砖红编辑风模板（已经把 sidebar 导航、⌘K 全文搜索、⌘G 术语表、深色模式、可点击的节点 drawer、SVG 星座图、2×2 矩阵、paired-bar 图、tab 案例切换全部 wire up 好了），让 Claude 把任何研究主题——行业分析、竞争格局、生态战略图、基金 thesis、市场深度报告、创始人 research memo——直接生成一份 **比 markdown 更有信息密度、比临时 HTML 更视觉统一** 的可探索文档。

**English:**

Long-form research falls in a gap between three formats: markdown is flat (read top-to-bottom, no exploration), decks sacrifice text density, and ad-hoc HTML requires rebuilding the design system each time. This skill ships a 1340-line warm-cream editorial template with all interactive components pre-wired (sidebar nav, ⌘K search, ⌘G glossary, dark mode, click-to-expand drawers, SVG constellation map, 2×2 matrix, paired-bar charts, tabbed case studies), so Claude can produce explorable, design-cohesive research artifacts on any topic — industry analysis, competitive landscapes, ecosystem maps, fund theses, market deep-dives, research memos.

---

## 安装 · Install

**1. 下载这个 repo 然后打包成 zip**

    git clone https://github.com/klaywang24/interactive-field-guide-skill.git
    cd interactive-field-guide-skill
    zip -r ../interactive-field-guide.zip . -x ".git/*" -x "README.md" -x "LICENSE"

或者直接到 [Releases 页面](https://github.com/klaywang24/interactive-field-guide-skill/releases) 下载现成的 zip（如果有发布的话）。

**2. 在 Claude.ai 里启用 skills**

- 进入 **Settings → Capabilities**，确保 **Code execution and file creation** 是打开的
- 进入 **Customize → Skills**
- 点 **+** 按钮 → **Create skill** → **Upload a skill**
- 选择刚才的 zip 文件
- 在 skill 列表里把它的 toggle 打开

详细官方文档：<https://support.claude.com/en/articles/12512180>

---

## 怎么触发这个 skill · How to trigger

装好之后，在和 Claude 对话时说类似下面的话，skill 会自动 trigger：

**中文示例：**

- "帮我做一份 [行业] 的 field guide"
- "深度分析 [公司]：怎么赚钱、谁是竞争对手、最近的战略动作"
- "做一份 [主题] 的可交互研究报告"
- "Map 出 [公司] 过去 12 个月的战略生态——所有合作方、投资、收购"
- "[赛道] 的竞争格局分析"
- "[主题] 的基金 thesis，要带公司证据和可执行的判断表"

**English examples:**

- "Make me a field guide on the [industry] ecosystem"
- "Deep dive on [Company] — how do they make money, who are the competitors, what's the recent strategy"
- "Build an interactive research report on [topic]"
- "Map [Company]'s strategic ecosystem — every partner, investor, and acquirer in the last 12 months"
- "Competitive landscape of [vertical]"
- "Fund thesis on [theme] with company evidence and an actionable judgments table"

---

## 内部结构 · What's inside

| 文件 / File | 作用 / Purpose |
|---|---|
| `SKILL.md` | 主入口。完整工作流 + 反模式清单 + 验证脚本 / Main entry: workflow, anti-patterns, validation scripts |
| `assets/template.html` | 1340 行的暖米色 + 砖红色编辑风模板，所有交互组件已 wire up / 1340-line warm-cream + brick-red template, all interactive components pre-wired |
| `references/structure.md` | 12-part 编辑结构详解：每个 part 何时用、何时跳 / 12-part editorial structure: when to use each part, when to skip |
| `references/data-schemas.md` | 4 个 JS 数据对象的字段说明 / Schemas for the four JS data objects |
| `references/content-strategy.md` | 内容来源指南（Net Interest / Acquired / Bits about Money 等独立分析师） / Sourcing playbook for independent analysts |
| `references/pitfalls.md` | 容易踩的 10 个坑 + 验证脚本 / 10 hard-won gotchas with validation scripts |

---

## 12-Part 编辑结构 · The 12-part structure

每份生成的 field guide 最多包含 12 个 part，但根据材料决定哪些用、哪些跳：

Each generated guide can contain up to 12 parts; which to use depends on the material:

| Part | 内容 / Content | 何时跳过 / Skip when… |
|---|---|---|
| Hero | 主标题 + 4 个核心数据 | 永不跳过 / Never |
| 1 | 序言 · 第一性原理 / Preface + first principles | 永不跳过 / Never |
| 2 | 5 张关键框架卡 / 5-card framework | 没有 5 个独立的 idea / Fewer than 5 distinct ideas |
| 3 | Tab 案例研究 / Tab case studies | 缺乏可对比案例 / No comparable cases |
| 4 | Paired-bar 板块图 / Sector paired-bar | 地理分布不是重点 / Geography isn't the story |
| 5 | Constellation Map（SVG 节点图）/ Constellation map | 实体少于 15 个 / Fewer than 15 entities |
| 6 | 2×2 战略象限 / 2×2 strategic matrix | 没有清晰的二维定位 / No clear positioning |
| 7 | 热力表 / Heat-map matrix | 不需要多维评分 / No multi-dim scoring |
| 8 | 主题深度 / Topic deep dive | 没有需要长篇的子主题 / Nothing deserves long-form |
| 9 | 近期 inflection 事件 / Inflection point | 没有时效性事件 / No time-bound event |
| 10 | 时间线（5 张卡）/ Timeline | 少于 4 个可日期化事件 / Fewer than 4 dated events |
| 11 | 三档判断（推荐/可选/不要）/ Tiered judgments | 不是决策导向报告 / Not decision-oriented |
| 12 | 术语表 + 数据来源 / Glossary + sources | 永不跳过 / Never |

详见 [`references/structure.md`](references/structure.md)

---

## 设计原则 · Design principles

**主动找独立分析师内容**——遇到公司 / 行业 / 战略类主题，先搜 Net Interest（Marc Rubinstein）、Bits about Money（Patrick McKenzie）、Acquired（Gilbert/Rosenthal）、Stratechery（Ben Thompson）、Business Breakdowns 这种深度分析，再考虑新闻聚合站。一篇 Net Interest 的深度分析顶五条 Reuters 头条。

**不够材料的 part 直接跳，不要凑**——例如 Constellation Map 节点少于 15 个就不要硬加，视觉会很空。一份 7-part 紧凑的报告永远好过 12-part 但有 3 个 part 是凑的。

**用真材料，不用占位文字**——任何 `[placeholder]` 都不能进交付版本。如果某个 section 没有材料，删掉它而不是留占位。

> Source from independent analysts proactively (Net Interest, Bits about Money, Acquired, Stratechery, Business Breakdowns) before falling back to news aggregators. Skip parts that don't fit the material rather than padding. Never ship `[placeholder]` text — if a section has no material, remove the section.

---

## 一个用过这个 skill 的产物示例 · An example output

这个 skill 第一次成型时，是为了把 CB Insights 的 *Mastercard Strategy Map* 截图（60+ 家 Mastercard 合作 / 投资 / 收购的公司）打成一份 130KB 的可交互 HTML，从中给 [Nexar](https://www.nexar.app)（一个 creator-payout decision layer 创业公司）提取可执行的竞争情报。最终产物包括 12 个 part、24 个 Constellation Map 节点（每个可点击查看公司画像）、4 位独立分析师（Marc Rubinstein / Patrick McKenzie / Acquired / Alex Rampell）的引用整合、加上一张 5 件事的"稳定币 inflection point"时间线。

> The skill was originally built to turn a CB Insights *Mastercard Strategy Map* screenshot (60+ Mastercard partner / investee / acquisition companies) into a 130KB interactive HTML report extracting actionable competitive intelligence for [Nexar](https://www.nexar.app), a creator-payout decision layer startup.

---

## License

[Apache 2.0](LICENSE) — 免费用、改、二次分发，只要保留 attribution。

[Apache 2.0](LICENSE) — free to use, modify, redistribute with attribution.

---

## 相关资源 · Related

- Anthropic 官方 skills 仓库 / Anthropic's official skills repo: <https://github.com/anthropics/skills>
- Claude.ai skills 文档 / Claude.ai skills docs: <https://support.claude.com/en/articles/12512180>
- 怎么写自定义 skill / How to create custom skills: <https://support.claude.com/en/articles/12512198>
