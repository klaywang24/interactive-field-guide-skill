# Interactive Field Guide Skill

> A research operating system for AI agents. Encodes the discipline — modes, personas, sourcing tiers, counter-consensus, falsifiability — as a reusable framework. Ships with a polished default template; bring your own. **Opinionated by default, extensible by design.**

[![Agent Skills](https://img.shields.io/badge/Agent_Skills-open_standard-blue)](https://agentskills.io)
[![Release](https://img.shields.io/github/v/release/klaywang24/interactive-field-guide-skill?include_prereleases)](https://github.com/klaywang24/interactive-field-guide-skill/releases)
[![License](https://img.shields.io/badge/license-Apache_2.0-green)](LICENSE)
[![Stars](https://img.shields.io/github/stars/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/stargazers)
[![Downloads](https://img.shields.io/github/downloads/klaywang24/interactive-field-guide-skill/total)](https://github.com/klaywang24/interactive-field-guide-skill/releases)
[![Last Commit](https://img.shields.io/github/last-commit/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/commits)
[![中文](https://img.shields.io/badge/中文-README-red)](README.zh-CN.md)

**Quick links**: [Architecture](#three-layer-architecture) · [Bring Your Own Template](#bring-your-own-template) · [Visual Showcase](#visual-showcase-default-template) · [Start Here](#start-here--pick-your-path) · [60-Second First Run](#60-second-first-run) · [Modes](#how-to-use-the-modes) · [Personas](#personas) · [Roadmap](#roadmap)

---

## What this is

This is **not just an HTML generator**. It's a research operating system for AI agents — a reusable framework for turning long research conversations into rigorous, interactive research artifacts.

| Mode | When to use it |
|---|---|
| **Generate** | First-pass research on a new topic |
| **Discovery** | 5-question dialogue → user picks brief or HTML |
| **Critique** | "I wrote an analysis. Find the weaknesses." |
| **Refresh** ⭐ | "Update my Q1 NVIDIA report with Q2 data" |
| **Compare** | "NVIDIA vs AMD with side-by-side tables" |
| **Drill-down** | "Expand customer concentration into its own deep dive" |

**Personas — Core lenses:**

| Persona | Lens it applies |
|---|---|
| **Tier 2 Investor** (default for stocks) | "Long, short, or neutral? At what price?" |
| **Industry Strategist** (default for sectors) | "Where is profit migrating across cycles?" |
| **Engineer** | "Is the technical bet sound? Show benchmarks." |
| **Competitor** | "If I were a rival, where would I attack?" |

**Personas — Advanced lenses:**

| Persona | Lens it applies |
|---|---|
| **Tier 1 Investor** | "Is this a fund-returner? Why now?" |
| **CEO** | "What would I do tomorrow morning if I ran this?" |
| **Disruption Scout** | "What kills this business in 5 years?" |

Same topic, different persona = different artifact.

---

## Three-layer architecture

This skill is built in three concentric layers. Each layer is independently replaceable.

```
┌──────────────────────────────────────────────────────┐
│  Layer 3 — Presentation Template (the shell)         │
│  HTML / CSS / JS / interactions / visual identity    │
│  ← REPLACEABLE: bring your own template              │
├──────────────────────────────────────────────────────┤
│  Layer 2 — Content Schema (the data)                 │
│  Hero stats · sections · CN_NODES · DETAIL_DATA      │
│  · glossary · 22-part menu                           │
│  ← STABLE INTERFACE: same shape across templates     │
├──────────────────────────────────────────────────────┤
│  Layer 1 — Research Workflow (the core IP)           │
│  6 modes · 7 personas · sourcing tiers · counter-    │
│  consensus framework · falsifiability rules          │
│  ← THE FRAMEWORK: this is what makes it useful       │
└──────────────────────────────────────────────────────┘
```

**Layer 1 is the IP** — the research discipline. It's the same regardless of how you render it.
**Layer 2 is structured data** — what facts, sources, and judgments get produced.
**Layer 3 is replaceable** — visual, interactive, branding. Bring your own.

The default template (`assets/template.html`) is one expression of Layer 3. The skill is **not limited to it**.

---

## Bring your own template

The default template is **opinionated**: warm-beige + brick-red, 22-part editorial structure, sidebar nav, ⌘K search, click-to-expand drawers, SVG ecosystem map. It's polished. It looks great. **It's also one of many possible expressions.**

For your fund, your startup, your industry, your team — you can fork the template and adapt:

- **Visual identity** (different palette, fonts, density)
- **Section structure** (don't want 22 parts? use 8)
- **Interaction model** (drawer instead of accordion? tabs instead of sections?)
- **Charts and maps** (different ecosystem visualization)
- **Sidebar / navigation style** (top tabs? collapsible?)
- **Output format** (HTML / markdown / Notion / Slack message / PPT)

### Recommended pattern

1. Start with `assets/template.html` as reference for what slots exist.
2. Replace the visual + interaction shell with your own.
3. Keep stable slots for content: `title`, `hero`, `sections`, `ecosystem_nodes`, `drawer_data`, `glossary`.
4. (Optional) Add your own validation checks for your template's wiring.
5. Save it as your team's reusable template — store it anywhere, just tell the AI where.

### Use your custom template

```
"Generate a NVIDIA field guide using my template at /path/to/my-template.html"
```

The skill will fill **your template's slots** with research-discipline content (Layer 1 + Layer 2), preserving your visual + interaction choices (Layer 3).

### When (not) to use the default

✅ **Use default**: prototype, demo, blog post, anything where editorial polish matters and your audience expects "VC research-style" formatting.

❌ **Use your own**: branded fund deliverables, internal team reports, integrations with Notion/Slack/email, anything where your visual identity matters more than the default's.

The skill is **opinionated by default, extensible by design**. We provide the discipline; you decide the shell.

---

## Visual showcase (default template)

These are real outputs of the **default template** — not the skill's only output style.

![Industry constellation map](assets/screenshots/fintech-constellation-map.png)
*Industry Strategist lens on a fintech sector — interactive SVG ecosystem map. Click any node to jump to the relevant section.*

![Sector tabs navigation](assets/screenshots/fintech-sector-tabs.png)
*Tabbed section structure for multi-segment industry analyses — switch between subsectors without leaving the page.*

![Industry profit-pool analysis](assets/screenshots/global-racket.png)
*Industry-level profit-pool analysis — value-chain economics with explicit margin distribution by participant type.*

These take ~3 minutes to generate and look like this. **Your template can look completely different.**

---

## Start Here — pick your path

| I use... | Do this | Best for |
|---|---|---|
| **Claude.ai** | [Download v3.0.2 zip](https://github.com/klaywang24/interactive-field-guide-skill/releases) → upload as a Skill in Settings | Non-technical users |
| **Claude Code** | `git clone` into `~/.claude/skills/` | Power users on Anthropic stack |
| **Codex CLI** (OpenAI) | `git clone` into `~/.codex/skills/` | OpenAI ecosystem |
| **Gemini CLI** (Google) | `gemini skills install <repo-url>` | Google ecosystem |
| **Cursor** | `git clone` into project's `.cursor/skills/` | In-editor research |

Detailed install commands are in [Install](#install). Once installed, jump to [60-Second First Run](#60-second-first-run).

---

## 60-Second First Run

After install, paste this:

> *"Analyze NVIDIA fundamentals as a Tier 2 investor."*

You'll get an interactive HTML field guide with sourced facts, a counter-consensus thesis, and explicit Bull/Bear scenarios. Open in browser to navigate.

Then try one of these to feel the suite:

> *"Critique that report — find weaknesses."*  
> *"Drill into the customer concentration disagreement."*  
> *"Refresh this report with the latest data."*  
> *"Compare NVIDIA and AMD side by side."*

Each command activates a different mode. Together they make this a research lifecycle tool, not a one-shot generator.

---

## Output formats

Default: **interactive HTML field guide** (~110KB) — sidebar nav, ⌘K full-text search, click-to-expand drawers, clickable SVG ecosystem map, 2×2 strategic positioning matrix, structured analysis across up to 22 parts.

Alternative: **markdown research brief** (1–2 page memo) — counter-thesis, sourced facts, steel-manned Bull/Bear, what-to-watch list. Available when you ask for "brief" / "markdown summary" or pick "brief only" at the end of Discovery mode.

Both formats follow the same quality bar: every fact has a `[Source: X]` citation, every judgment is falsifiable, every artifact has explicit counter-consensus framing.

**Custom formats**: bring your own template (HTML / markdown / JSON for Notion / etc.) — see [Bring Your Own Template](#bring-your-own-template).

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

5 questions (consensus → contradictions → thesis → facts → steel-manned Bull/Bear), then choose: brief or HTML.

### Critique — adversarial review

```
"Review my NVIDIA analysis. Find the weaknesses."
"Critique this report — find unsourced claims and weak counter-consensus."
```

Provide your existing analysis. Output: **markdown critique** (no HTML) with strongest claims, weakest claims, what's missing, specific fixes.

### Refresh — update with latest data ⭐

```
"Refresh my NVIDIA Q1 report with the latest data"
"Update my Stripe analysis from January"
"复盘 my AI agent landscape report — what's changed?"
```

Provide the prior field guide. The skill refreshes every number, flags material changes, re-evaluates your counter-thesis, adds a "What changed since [date]" callout.

### Compare — side by side

```
"Compare NVIDIA and AMD"
"Stripe vs Adyen — which is the better long?"
```

Comparative field guide with hero comparison table (10–15 dimensions), side-by-side tables in each section, explicit scoring rubric.

### Drill-down — expand a subsection

```
"From my NVIDIA report, drill into customer concentration"
"Take the 'AI factory operations' thesis and make it standalone"
```

Treat one subsection as the entire topic of a NEW field guide — full 22-part treatment.

---

## Why this skill is a framework, not a single tool

Most research tools are one-shot generators. Real research isn't:

1. Look at a topic from multiple angles (= **personas**)
2. Update the analysis as the world changes (= **Refresh** ⭐)
3. Compare options before deciding (= **Compare**)
4. Stress-test your own conclusions (= **Critique**)
5. Drill into the cruxes (= **Drill-down**)
6. Coach yourself through the thinking (= **Discovery**)

Garry Tan's [gstack](https://github.com/garrytan/gstack) made this point for engineering — a virtual team beats a single assistant. This skill applies the same principle to research.

**One topic. Six modes. Seven personas. Multiple output formats. Repeated use over time.**

---

## Compatibility

This skill follows the open [Agent Skills standard](https://agentskills.io). Works in any compatible agent — but **output quality depends on the model**.

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

This skill produces **research frameworks** with falsifiable signals, sourced facts, and structured Bull / Bear analyses. It is **not personalized investment advice** and does not replace a licensed financial advisor.

Outputs are framed as analytical artifacts. Every output includes a footer disclaimer.

If you need personalized financial advice, consult a licensed advisor.

---

## Install

### 1. Claude.ai (recommended for non-developers)

1. Download the latest zip from [releases](https://github.com/klaywang24/interactive-field-guide-skill/releases)
2. Claude.ai → **Settings** → **Capabilities** → enable **Code execution and file creation**
3. **Customize** → **Skills** → **+** → **Upload a skill** → select zip → toggle on

### 2. Claude Code

```bash
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.claude/skills/interactive-field-guide
```

### 3. Codex CLI (OpenAI)

```bash
codex --enable skills

git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.codex/skills/interactive-field-guide
```

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

**A. Use Agent mode**, not Ask. Top of chat panel → select **Agent**.

**B. Web search must be on.** Settings → **Features** → **Agent** → enable **Web search**.

**C. Allow terminal commands.** Settings → **Features** → **Agent** → enable **Allow terminal commands**.

**D. Pick a strong model.** Settings → **Models** → Claude Sonnet 4.6 / Opus 4.7 / GPT-5 / Gemini 2.5 Pro.

---

## Output quality by model

| Model | Quality | Notes |
|---|---|---|
| Claude Opus 4.7 | ⭐⭐⭐⭐⭐ | Reference implementation |
| GPT-5 / GPT-5-Codex | ⭐⭐⭐⭐ | Strong reasoning |
| Gemini 2.5 Pro | ⭐⭐⭐⭐ | Excellent long context |
| Claude Sonnet 4.6 | ⭐⭐⭐⭐ | Faster + cheaper |
| Gemini 2.5 Flash / GPT-4o-mini | ⭐⭐⭐ | OK for first drafts |

If output feels shallow, switch to a stronger model first.

---

## What's inside

```
interactive-field-guide-skill/
├── SKILL.md                       # Lightweight router (Layer 1 dispatch)
├── assets/
│   ├── template.html              # Default presentation template (Layer 3)
│   └── screenshots/               # README visual showcase
└── references/
    ├── modes.md                   # Layer 1: 6 operational mode workflows
    ├── personas.md                # Layer 1: 7 analytical persona lenses
    ├── structure.md               # Layer 2: 22-part menu with skip rules
    ├── data-schemas.md            # Layer 2: JS object schemas + slots
    ├── content-strategy.md        # Layer 1: source tiers, falsifiability
    └── pitfalls.md                # Validation checks for default template
```

The split between Layers 1, 2, 3 lives in this folder structure. Replace `assets/template.html` (Layer 3) without touching `references/` (Layer 1 + 2) — the framework still works.

---

## Example gallery

| Example | Mode | Persona | Demonstrates |
|---|---|---|---|
| Fintech industry constellation | Generate | Industry Strategist | SVG ecosystem map with clickable nodes |
| Fintech sector navigation | Generate | Industry Strategist | Tabbed section structure for multi-segment topics |
| Global industry profit-pool | Generate | Industry Strategist | Profit-pool mapping across industry value chain |
| NVIDIA fundamentals | Generate | Tier 2 Investor | Counter-consensus thesis + Bull/Bear with falsifiability |

More examples will land in a companion [`awesome-field-guides`](https://github.com/klaywang24) repo (Q2 2026), including community-contributed templates.

---

## Roadmap

**Q2 2026** (in development):
- Companion `awesome-field-guides` repo with example artifacts
- Example community templates (minimalist white, Notion-style, 中文公众号)
- True 2-column mirrored layout for Compare mode
- `scripts/validate-field-guide.js` and `scripts/smoke-test-field-guide.js` (Playwright) as standalone files

**Q3 2026** (planned):
- "Watch list" mode — passive tracking of 5–10 entities
- `agents/openai.yaml` for richer Codex / OpenAI ecosystem display
- skills.sh marketplace submission
- Claude Code Plugin Marketplace registration

**Q4 2026** (exploring):
- Voice-mode Discovery
- Collaborative critique (multiple agents debate the same analysis)
- Bilingual auto-output toggle

---

## Troubleshooting

**Skill doesn't trigger.** Trigger is intentionally narrow — only fires for company / industry / market / competitive / strategy / investment research artifacts. Force it: *"generate a field guide on X"*.

**Mode not detected correctly.** Be explicit: *"Use Discovery mode for X"* or *"Run Critique on this"*.

**Persona feels wrong.** Specify: *"...as a Tier 1 investor"* or *"...with an Engineer lens"*.

**Output too shallow.** Likely a model issue. Switch to Claude Opus 4.7 / GPT-5 / Gemini 2.5 Pro and re-run.

**Refresh can't find prior data.** Make sure you uploaded or pasted the prior field guide. The skill has no cross-session memory unless you provide it.

**Compare looks single-column.** v3.0.x uses comparison tables, not mirrored 2-column layout. Full mirrored layout is on the Q2 roadmap.

**Default template visual broken.** Re-run with: *"Re-run the field guide and run all 9 validation checks."* If it persists, the model rebuilt instead of editing — see SKILL.md Critical Rule 6.

---

## Why "field guide" and not "report"

A report is read once. A field guide is something you keep open, search, click around, return to.

But more importantly: a field guide is for **navigation**, not display. The 22-part structure, the falsifiable judgments, the sourced facts — these aren't formatting. They're the **research discipline** that makes the artifact trustworthy.

The skill encodes that discipline (Layer 1). The HTML is one shell (Layer 3). **The discipline is the product.**

---

## License

Apache 2.0. Use, fork, modify freely. PRs welcome — especially platform-specific compatibility fixes, new persona contributions, and community templates.

## Author

Built and maintained by [@klaywang24](https://github.com/klaywang24).

This is open infrastructure: research workflows, content schemas, and a default template. If it's useful to your fund / startup / team, fork it and make it yours.

If you build a custom template (or a new persona), submit a PR — community templates land in `templates/community/`.
