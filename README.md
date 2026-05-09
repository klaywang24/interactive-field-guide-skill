# Interactive Field Guide · Claude Skill

> 中文版 · [README.zh-CN.md](README.zh-CN.md)

A Claude skill that turns any research topic — a company, an industry, a sector, a strategic question — into a polished, interactive HTML field guide with sidebar navigation, an SVG ecosystem map, click-to-expand drawers, accordions, paired-bar charts, and a **22-part editorial structure**.

Built to the depth standard of VC investment memos, top-tier secondary-fund research, and CB Insights industry reports.

---

## What it solves

Long-form research falls into a gap between three formats:

- **Markdown** — flat, no exploration, hard to skim
- **PPT / Keynote** — visual but sacrifices text density
- **Ad-hoc HTML** — rebuild the design system every time

This skill ships a 1,500-line warm-cream + brick-red template with all interactive components pre-wired (sidebar nav, ⌘K search, ⌘G glossary, dark mode, click-to-expand drawers, SVG constellation map, 2×2 matrix, accordion, paired-bar charts, tabbed case studies, 8-pillar DD grid). Claude fills the body with substantive content and validates the output against six checks.

---

## Quality bar

Not "blog post with charts" — a research artifact at investment-memo level:

- ✅ **Every fact has a source citation** — `[Source: SEC 10-K Q4 FY2026 / Bloomberg / Company PR]`. No "approximately" / "据估计".
- ✅ **Every judgment is falsifiable** — not "growing fast" but `YoY +65% to $215.9B`; not "leading" but `31.4% market share, +12pp in 3 years`.
- ✅ **Every report has 3+ counter-consensus claims** — directly challenge mainstream framing. "NVIDIA's true moat is CUDA + bundled networking, not chip raw FLOPS" beats "NVIDIA leads AI."
- ✅ **Every judgment section gives falsifiable signals** — `Tier 1 if Q2 FY27 guidance > $50B; downgrade Tier 2 if < $42B`.

---

## Install

### Option A — Clone & zip

    git clone https://github.com/klaywang24/interactive-field-guide-skill.git
    cd interactive-field-guide-skill
    zip -r ../interactive-field-guide.zip . -x ".git/*" -x "README*.md" -x "LICENSE"

### Option B — Releases page

Download the latest zip from the [Releases page](https://github.com/klaywang24/interactive-field-guide-skill/releases) (when published).

### Enable in Claude.ai

1. Settings → Capabilities → enable **Code execution and file creation**
2. Customize → Skills → **+** → **Create skill** → **Upload a skill**
3. Select the zip
4. Toggle the skill on

Detailed official docs: [support.claude.com/en/articles/12512180](https://support.claude.com/en/articles/12512180)

---

## How to trigger

Once installed, ask Claude something like:

| If you want | Try saying |
|---|---|
| Industry overview | "Build a field guide on [vertical]" |
| Company deep dive | "Deep dive on [Company] — how do they make money, who are the competitors, what's the recent strategy" |
| Strategic ecosystem map | "Map [Company]'s strategic ecosystem — every partner, investor, and acquirer in the last 12 months" |
| Stock analysis | "Is [stock] a good buy?" / "Fundamentals on [Company]" |
| Sector landscape | "Competitive landscape of [vertical]" |
| Fund thesis | "Fund thesis on [theme] with company evidence and an actionable judgments table" |

Chinese triggers also work — `分析下英伟达基本面` / `研究一下小红书的商业模式` / `半导体板块龙头都有哪些` / `做一份新能源车的行业报告`.

---

## What's inside

| File | Purpose |
|---|---|
| `SKILL.md` | Main entry — workflow, anti-patterns, validation scripts |
| `assets/template.html` | 1,500-line warm-cream + brick-red template, all interactive components pre-wired |
| `references/structure.md` | 22-part editorial structure: when to use each part, when to skip |
| `references/data-schemas.md` | Data shapes for the 6 JS data objects + every component's HTML pattern |
| `references/content-strategy.md` | VC / CB Insights depth standard; 11 source tiers; counter-consensus framework |
| `references/pitfalls.md` | 10 hard-won gotchas with copy-paste validation scripts |

---

## The 22-part structure

Each generated guide picks 10–22 parts based on the topic. **Skip rules are strict** — padding parts is worse than skipping.

| # | Part | Tier | Skip when… |
|---|---|---|---|
| 1 | First principles / preface | ✅ Core | Almost never |
| 2 | Data credibility | 🟡 Optional | Single authoritative source |
| 3 | Core conditions / framework | ✅ Core | Pure financials |
| 4 | Anchor case study | 🟡 Optional | No single representative case |
| 5 | Sector deep-dives | ⭐ Important | Single-company analysis |
| 6 | Cross-vertical comparison | ✅ Core | Single dimension |
| 7 | Constellation Map (SVG) | ✅ Core | Almost never |
| 8 | 2×2 archetype matrix | ✅ Core | Players don't split into 4 |
| 9 | Death vs. success 2×2 | 🟡 Optional | New / hyper-hyped vertical |
| 10 | Talent / resource heatmap | 🟡 Optional | Single-company |
| 11 | Trends timeline | ✅ Core | Pure forecast |
| 12 | Death-cause donut | ⚠ Rare | Insufficient sample |
| 13 | Pain-point / principles accordion | ⭐ Important | Pure financials |
| 14 | 8-pillar evaluation framework | ⭐ Important | Education-toned topic |
| 15 | Exit / M&A strategy | 🟡 Optional | Public-company fundamentals |
| 16 | Cold-start paths | ⚠ Rare | Mature company analysis |
| 17 | Fundamental disagreements | ✅ Core | Strong consensus topic |
| 18 | GTM strategy library | 🟡 Optional | Pure research/financials |
| 19 | Validation method grid | 🟡 Optional | Outcome-focused topic |
| 20 | Frontier topic / key variable | ✅ Core | Static historical only |
| 21 | Tiered judgment + 90-day actions | ⭐ Important | Almost never |
| 22 | Glossary + sources | ✅ Core | Never |

Typical part counts:

- **Single-company fundamentals** (NVIDIA-style): ~12 parts
- **Strategic ecosystem map** (Stripe-style): ~11 parts
- **Industry / sector**: ~14 parts
- **Complete industry handbook** (fintech-style): 18–22 parts

See [references/structure.md](references/structure.md) for the full menu and skip rules.

---

## Design principles

1. **Independent analysts first** — for any company / industry topic, search Net Interest, Bits about Money, Acquired, Stratechery, Business Breakdowns, The Diff, Not Boring, Money Stuff before resorting to news headlines. One Net Interest piece beats five Reuters headlines.
2. **Direct sources over aggregators** — SEC 10-K / 8-K direct, original IR pages, monetary-authority filings beat secondhand summaries.
3. **Falsifiable judgments only** — every conclusion includes "if X happens, my view is wrong".
4. **Skip don't pad** — 12 tight parts beat 22 padded parts.

See [references/content-strategy.md](references/content-strategy.md) for the full sourcing playbook.

---

## Compatibility

| Tool | Works? |
|---|---|
| claude.ai (web / desktop / mobile) | ✅ |
| Claude Code (CLI) | ✅ |
| Claude API (developer) | ✅ |
| Codex / ChatGPT | ❌ Not Anthropic |
| Cursor / Copilot / other | ❌ |

Skills are an Anthropic-specific format. Requires a paid Claude account.

---

## License

Apache-2.0 — see [LICENSE](LICENSE).
