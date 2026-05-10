# Interactive Field Guide Skill

> Generate VC / CB Insights-grade research reports with AI agents. 6 operational modes × 7 analytical personas × 2 output formats. Open [Agent Skills](https://agentskills.io) standard.

[![Agent Skills](https://img.shields.io/badge/Agent_Skills-open_standard-blue)](https://agentskills.io)
[![Release](https://img.shields.io/github/v/release/klaywang24/interactive-field-guide-skill?include_prereleases)](https://github.com/klaywang24/interactive-field-guide-skill/releases)
[![License](https://img.shields.io/badge/license-Apache_2.0-green)](LICENSE)
[![Stars](https://img.shields.io/github/stars/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/stargazers)
[![Downloads](https://img.shields.io/github/downloads/klaywang24/interactive-field-guide-skill/total)](https://github.com/klaywang24/interactive-field-guide-skill/releases)
[![Last Commit](https://img.shields.io/github/last-commit/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/commits)
[![中文](https://img.shields.io/badge/中文-README-red)](README.zh-CN.md)

**Quick links**: [Visual Showcase](#visual-showcase) · [Start Here](#start-here--pick-your-path) · [60-Second First Run](#60-second-first-run) · [Modes](#how-to-use-the-modes) · [Personas](#personas) · [Roadmap](#roadmap)

---

## Visual showcase

Three real outputs from this skill — different topics, different visual components, all VC-grade depth.

![Fintech industry constellation map](assets/screenshots/fintech-constellation-map.png)
*Industry Strategist lens on fintech — interactive SVG ecosystem map. Click any node to jump to the relevant section.*

![Fintech sector tabs navigation](assets/screenshots/fintech-sector-tabs.png)
*Tabbed section structure for multi-segment industry analyses — switch between subsectors without leaving the page.*

![Global racket sports industry analysis](assets/screenshots/global-racket.png)
*Industry-level profit-pool analysis on global racket sports — value-chain economics with explicit margin distribution by participant type.*

---

## What's new in v3

This is **not just an HTML generator**. v3 turns the skill into a *research suite* — six operational modes you call as needed, seven analytical personas that change the lens entirely, two output formats (HTML field guide or markdown research brief).

You're meant to use it many times for one topic, not once.

| Mode | When to use it |
|---|---|
| **Generate** | First-pass research on a new topic |
| **Discovery** | Important decisions — 5-question dialogue produces sharper output, then choose brief or HTML |
| **Critique** | "I wrote an analysis. Find the weaknesses." |
| **Refresh** ⭐ | "Update my Q1 NVIDIA report with Q2 data" — quarterly LTV driver |
| **Compare** | "NVIDIA vs AMD with side-by-side tables" |
| **Drill-down** | "Expand the customer concentration disagreement into its own deep dive" |

**Personas — Core lenses (highest-frequency use):**

| Persona | Lens it applies |
|---|---|
| **Tier 2 Investor** (default for stocks) | "Long, short, or neutral? At what price?" |
| **Industry Strategist** (default for sectors) | "Where is profit migrating across cycles?" |
| **Engineer** | "Is the technical bet sound? Show benchmarks." |
| **Competitor** | "If I were a rival, where would I attack?" |

**Personas — Advanced lenses (specialist use):**

| Persona | Lens it applies |
|---|---|
| **Tier 1 Investor** | "Is this a fund-returner? Why now?" |
| **CEO** | "What would I do tomorrow morning if I ran this?" |
| **Disruption Scout** | "What kills this business in 5 years?" |

Same topic, different persona = different field guide.

---

## Start Here — pick your path

| I use... | Do this | Best for |
|---|---|---|
| **Claude.ai** | [Download v3.0-beta zip](https://github.com/klaywang24/interactive-field-guide-skill/releases) → upload as a Skill in Settings | Non-technical users |
| **Claude Code** | `git clone` into `~/.claude/skills/` | Power users on Anthropic stack |
| **Codex CLI** (OpenAI) | `git clone` into `~/.codex/skills/` | OpenAI ecosystem |
| **Gemini CLI** (Google) | `gemini skills install <repo-url>` | Google ecosystem |
| **Cursor** | `git clone` into project's `.cursor/skills/` | In-editor research |

Detailed install commands are in the [Install](#install) section. Once installed, jump to [60-Second First Run](#60-second-first-run).

---

## 60-Second First Run

After install, paste this into your AI agent:

> *"Analyze NVIDIA fundamentals as a Tier 2 investor."*

You'll get an interactive HTML field guide (~110KB) with sourced facts, a counter-consensus thesis, and explicit Bull/Bear scenarios. Open it in a browser to navigate.

Then try one of these to feel the suite:

> *"Critique that report — find weaknesses."*  
> *"Drill into the customer concentration disagreement."*  
> *"Refresh this report with the latest data."*  
> *"Compare NVIDIA and AMD side by side."*

Each command activates a different mode. **Together they make this a research lifecycle tool, not a one-shot generator.**

---

## Output formats

Default: **interactive HTML field guide** (~110KB) — sidebar nav, ⌘K full-text search, click-to-expand drawers, clickable SVG ecosystem map, 2×2 strategic positioning matrix, structured analysis across up to 22 parts.

Alternative: **markdown research brief** (1–2 page memo) — counter-thesis, sourced facts, steel-manned Bull/Bear, what-to-watch list. Available when you ask for "brief" / "markdown summary" or pick "brief only" at the end of Discovery mode.

Both formats follow the same quality bar: every fact has a `[Source: X]` citation, every judgment is falsifiable, every artifact has explicit counter-consensus framing.

---

## How to use the modes

### Generate — one-shot research

```
"Analyze NVIDIA fundamentals"
"Research the semiconductor industry"
"Map Stripe's strategic ecosystem"
```

The skill auto-detects mode + persona. Override with:

```
"Analyze NVIDIA fundamentals as a Tier 1 investor"
"Research the semiconductor industry from a CEO perspective"
```

### Discovery — sharper output through dialogue

```
"Help me think through NVIDIA before generating a field guide"
"Do a research dialogue on the AI agent landscape"
```

The skill walks through 5 questions (consensus → contradictions → thesis → facts → steel-manned Bull/Bear), then asks: **"Do you want this as a concise markdown research brief, or should I generate the full interactive HTML field guide?"**

**When to use Discovery**: high-stakes decisions, position-sizing, internal memos that will be circulated.

### Critique — adversarial review

```
"Review my NVIDIA analysis. Find the weaknesses."
"Critique this report — find unsourced claims and weak counter-consensus."
```

Provide your existing analysis. The skill outputs a **markdown critique** (no HTML) with strongest claims, weakest claims, what's missing, and specific fixes.

### Refresh — update with latest data ⭐

```
"Refresh my NVIDIA Q1 report with the latest data"
"Update my Stripe analysis from January"
"复盘 my AI agent landscape report — what's changed?"
```

Provide the prior field guide. The skill refreshes every number against current sources, flags material changes, re-evaluates your counter-thesis, and adds a "What changed since [date]" callout at the top.

**This is the LTV mode.** Markets change every quarter. Use Refresh quarterly on every position you hold.

### Compare — side by side

```
"Compare NVIDIA and AMD"
"Stripe vs Adyen — which is the better long?"
"Cursor vs GitHub Copilot — head-to-head technical analysis"
```

Produces a comparative field guide with a hero comparison table (10–15 dimensions), side-by-side tables in each section, and an explicit scoring rubric.

### Drill-down — expand a subsection

```
"From my NVIDIA report, drill into customer concentration"
"Take the 'AI factory operations' thesis and make it standalone"
```

The skill treats one subsection of a parent field guide as the entire topic of a new field guide — full 22-part treatment.

---

## Why this skill is a suite, not a single tool

Most research tools are one-shot generators. Real research isn't:

1. Look at a topic from multiple angles (= **personas**)
2. Update the analysis as the world changes (= **Refresh** ⭐)
3. Compare options before deciding (= **Compare**)
4. Stress-test your own conclusions (= **Critique**)
5. Drill into the cruxes (= **Drill-down**)
6. Coach yourself through the thinking (= **Discovery**)

Garry Tan's [gstack](https://github.com/garrytan/gstack) made this point for engineering — a virtual team beats a single assistant. This skill applies the same principle to research.

**One topic. Six modes. Seven personas. Repeated use over time.** That's the design. Combined with the persona matrix, one user × one topic × multiple personas + quarterly refreshes + drill-downs = a dozen distinct field guides over time.

---

## Compatibility

This skill follows the open [Agent Skills standard](https://agentskills.io) (released by Anthropic, December 2025). Works in any compatible agent — but **output quality depends on the model**.

| Agent | Install path | Status |
|---|---|---|
| **Claude.ai** | Settings UI (zip upload) | ✅ Reference implementation |
| **Claude Code** | `~/.claude/skills/` | ✅ Reference implementation |
| **Codex CLI** (OpenAI) | `~/.codex/skills/` | 🟡 Format compatible, output untested |
| **Gemini CLI** (Google) | `~/.gemini/skills/` | 🟡 Format compatible, output untested |
| **Cursor** | `.cursor/skills/` (project only) | 🟡 Format compatible, output untested |

> **🟡** = format loads correctly; rendered HTML quality not personally validated. The skill assumes strong reasoning + long context + web search — best on Claude Opus / GPT-5 / Gemini 2.5 Pro.

---

## Boundary — what this skill is NOT

This skill produces **research frameworks** with falsifiable signals, sourced facts, and structured Bull/Bear analyses. It is **not personalized investment advice** and does not replace a licensed financial advisor.

Outputs are framed as analytical artifacts, not buy/sell recommendations. Every HTML and markdown output includes a footer disclaimer.

---

## Install

### 1. Claude.ai (recommended for non-developers)

1. Download the latest zip from [releases](https://github.com/klaywang24/interactive-field-guide-skill/releases)
2. In Claude.ai → **Settings** → **Capabilities** → enable **Code execution and file creation**
3. **Customize** → **Skills** → **+** → **Upload a skill** → select the zip → toggle on
4. Try it: *"Analyze NVIDIA fundamentals"*

### 2. Claude Code

```bash
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.claude/skills/interactive-field-guide
```

Restart Claude Code — skill is auto-discovered by description.

### 3. Codex CLI (OpenAI)

```bash
codex --enable skills

git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.codex/skills/interactive-field-guide
```

Restart Codex. Verify with `/skills`.

### 4. Gemini CLI (Google) — see [configuration notes ↓](#gemini-cli-configuration)

```bash
gemini skills install https://github.com/klaywang24/interactive-field-guide-skill
```

### 5. Cursor — see [configuration notes ↓](#cursor-configuration)

```bash
mkdir -p .cursor/skills
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  .cursor/skills/interactive-field-guide
```

Reload window: **Cmd/Ctrl+Shift+P** → **Developer: Reload Window**

---

## Configuration notes

The skill needs four capabilities: **(1) web search**, **(2) file write**, **(3) Python or Node**, **(4) long context** (~50K tokens).

Claude.ai / Claude Code have all four enabled by default. Codex / Gemini / Cursor sometimes don't.

### Gemini CLI configuration

**A. Web search must be enabled.** In session: `/tools list` — look for `google_search`.

**B. File write permissions.** To skip per-write prompts:

```jsonc
// ~/.gemini/settings.json
{ "tools": { "auto_approve": ["write_file"] } }
```

**C. Workspace trust** (only if installing at `.gemini/skills/`). Run `/trust`, or install at user-scope `~/.gemini/skills/` instead.

### Cursor configuration

**A. Use Agent mode**, not Ask. Top of chat panel → select **Agent**. Skills only fire in Agent mode.

**B. Web search must be on.** Settings → **Features** → **Agent** → enable **Web search**.

**C. Allow terminal commands.** Settings → **Features** → **Agent** → enable **Allow terminal commands**.

**D. Pick a strong model.** Settings → **Models** → Claude Sonnet 4.6 / Opus 4.7 / GPT-5 / Gemini 2.5 Pro.

**E. Output location.** Skill auto-detects platform and writes HTML to project root.

---

## Output quality by model

| Model | Quality | Notes |
|---|---|---|
| Claude Opus 4.7 | ⭐⭐⭐⭐⭐ | Reference implementation |
| GPT-5 / GPT-5-Codex | ⭐⭐⭐⭐ | Strong reasoning |
| Gemini 2.5 Pro | ⭐⭐⭐⭐ | Excellent long context |
| Claude Sonnet 4.6 | ⭐⭐⭐⭐ | Faster + cheaper |
| Gemini 2.5 Flash / GPT-4o-mini | ⭐⭐⭐ | OK for first drafts |

If output feels shallow, **switch to a stronger model first** before assuming the skill is the problem.

---

## What's inside

```
interactive-field-guide-skill/
├── SKILL.md                       # Lightweight router
├── assets/
│   ├── template.html              # 1,500-line HTML template
│   └── screenshots/               # README visual showcase
└── references/
    ├── modes.md                   # 6 operational mode workflows
    ├── personas.md                # 7 analytical persona lenses
    ├── structure.md               # 22-part menu with skip rules
    ├── data-schemas.md            # JS object schemas + components
    ├── content-strategy.md        # Source tiers, falsifiability rules
    └── pitfalls.md                # Validation checks
```

SKILL.md is a router. It detects mode + persona + format and dispatches to the right workflow. Personas describe topical emphasis (lens), not specific section numbers — the agent maps them onto whatever 22-part structure is current in `structure.md`.

---

## Example gallery

| Example | Mode | Persona | Demonstrates |
|---|---|---|---|
| Fintech Industry Constellation | Generate | Industry Strategist | SVG ecosystem map with clickable nodes |
| Fintech Sector Navigation | Generate | Industry Strategist | Tabbed section structure for multi-segment topics |
| Global Racket Sports | Generate | Industry Strategist | Profit-pool mapping across industry value chain |
| NVIDIA Fundamentals | Generate | Tier 2 Investor | Counter-consensus thesis + Bull/Bear with falsifiability |

More examples will land in a companion [`awesome-field-guides`](https://github.com/klaywang24) repo (Q2 2026).

---

## Roadmap

**Q2 2026** (in development):
- True 2-column mirrored layout for Compare mode
- `scripts/validate-field-guide.js` and `scripts/validate-compare.js` as standalone files
- Mode-specific validation checks in `pitfalls.md`

**Q3 2026** (planned):
- "Watch list" mode — passive tracking of 5–10 entities, auto-alerts on material changes
- `agents/openai.yaml` for richer Codex / OpenAI ecosystem display
- skills.sh marketplace submission
- Claude Code Plugin Marketplace registration
- Companion [`awesome-field-guides`](https://github.com/klaywang24) repo with example artifacts

**Q4 2026** (exploring):
- Voice-mode Discovery
- Collaborative critique (multiple agents debate the same analysis)
- Bilingual auto-output toggle

---

## Troubleshooting

**Skill doesn't trigger.** The trigger is intentionally narrow — only fires for company / industry / market / competitive / strategy / investment research artifacts. Force it with: *"generate a field guide on X"*.

**Mode not detected correctly.** Be explicit: *"Use Discovery mode for X"* or *"Run Critique on this"*.

**Persona feels wrong.** Specify: *"...as a Tier 1 investor"* or *"...with an Engineer lens"*.

**Output too shallow.** Likely a model issue. Switch to Claude Opus 4.7 / GPT-5 / Gemini 2.5 Pro and re-run.

**Refresh can't find prior data.** Make sure you uploaded or pasted the prior field guide. The skill has no cross-session memory unless you provide it.

**Compare looks single-column.** v3.0-beta uses comparison tables, not mirrored 2-column layout. Full mirrored layout is on the Q2 roadmap.

---

## Why "field guide" and not "report"

A report is read once. A field guide is something you keep open, search, click around, return to. The output is designed for the second behavior.

But more importantly: a field guide is for **navigation**, not display. The 22-part structure, the falsifiable judgments, the sourced facts — these aren't formatting. They're the **research discipline** that makes the artifact trustworthy.

The skill encodes that discipline. The HTML is the artifact. **The discipline is the product.**

---

## License

Apache 2.0. Use, fork, modify freely. PRs welcome — especially platform-specific compatibility fixes, new persona contributions, and mode-specific validation scripts.

## Author

Built by [@klaywang24](https://github.com/klaywang24). Founder of [Nexar](https://nexar.io) — creator-payout decision layer for US DTC brands.

If this skill is useful, a ⭐ helps others find it. If it's *very* useful, share a screenshot of the field guide it produced — that's the highest compliment.
