# Interactive Field Guide Skill

> A research-discipline framework for AI agents. Generate polished interactive HTML field guides or structured research briefs on any company, industry, market, or strategy topic — sidebar nav, ⌘K search, SVG ecosystem map, 22-part editorial structure. **6 modes × 7 personas. VC / CB Insights depth.**

[![Agent Skills](https://img.shields.io/badge/Agent_Skills-open_standard-blue)](https://agentskills.io)
[![Release](https://img.shields.io/github/v/release/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/releases)
[![License](https://img.shields.io/badge/license-Apache_2.0-green)](LICENSE)
[![Stars](https://img.shields.io/github/stars/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/stargazers)
[![Downloads](https://img.shields.io/github/downloads/klaywang24/interactive-field-guide-skill/total)](https://github.com/klaywang24/interactive-field-guide-skill/releases)
[![Last Commit](https://img.shields.io/github/last-commit/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/commits)
[![中文](https://img.shields.io/badge/中文-README-red)](README.zh-CN.md)

---

## What's new in v3

This is **not just an HTML generator**. v3 turns the skill into a *research suite* — six operational modes you call as needed, seven analytical personas that change the lens entirely, two output formats (HTML field guide or markdown research brief).

You're meant to use it many times for one topic, not once.

| Mode | When to use it |
|---|---|
| **Generate** | First-pass research on a new topic |
| **Discovery** | Important decisions — 5-question dialogue produces meaningfully sharper output, then you choose brief or HTML |
| **Critique** | "I wrote an analysis. Find the weaknesses." |
| **Refresh** ⭐ | "Update my Q1 NVIDIA report with Q2 data" — quarterly LTV driver |
| **Compare** | "NVIDIA vs AMD with side-by-side tables" |
| **Drill-down** | "Expand the customer concentration disagreement into its own deep dive" |

### Personas

**Core lenses (highest-frequency use):**

| Persona | Lens it applies |
|---|---|
| **Tier 2 Investor** (default for stocks) | "Long, short, or neutral? At what price?" |
| **Industry Strategist** (default for sectors) | "Where is profit migrating across cycles?" |
| **Engineer** | "Is the technical bet sound? Show benchmarks." |
| **Competitor** | "If I were a rival, where would I attack?" |

**Advanced lenses (specialist use):**

| Persona | Lens it applies |
|---|---|
| **Tier 1 Investor** | "Is this a fund-returner? Why now?" |
| **CEO** | "What would I do tomorrow morning if I ran this?" |
| **Disruption Scout** | "What kills this business in 5 years?" |

Same topic, different persona = different field guide. NVIDIA viewed as a Tier 2 Investor focuses on Q2 earnings and customer concentration. NVIDIA viewed as a Disruption Scout focuses on Cerebras / Groq / silicon photonics threats. Both are useful. Both are sharp.

---

## Output formats

Default: **interactive HTML field guide** (~110KB, sidebar nav, ⌘K search, click-to-expand drawers, SVG ecosystem map, 2×2 strategic positioning matrix, structured analysis across up to 22 parts).

Alternative: **markdown research brief** (1-2 page memo with counter-thesis, sourced facts, steel-manned Bull / Bear, what-to-watch list). Available when:
- User explicitly asks for "brief" or "markdown summary"
- User chooses "brief only" at the end of Discovery mode

Both formats follow the same quality bar: every fact has a `[Source: X]` citation, every judgment is falsifiable, every artifact has explicit counter-consensus framing.

---

## Why this skill is built as a suite, not a single tool

Most research tools are one-shot generators. You ask once, get one artifact, and you're done. Single-use.

Real research isn't like that. Real research is:
1. Look at a topic from multiple angles (= **personas**)
2. Update the analysis as the world changes (= **Refresh** ⭐)
3. Compare options before deciding (= **Compare**)
4. Stress-test your own conclusions (= **Critique**)
5. Drill into the cruxes (= **Drill-down**)
6. Coach yourself through the thinking (= **Discovery**)

Garry Tan's [gstack](https://github.com/garrytan/gstack) made this point for engineering — a virtual team (CEO / Engineer / Reviewer / QA / Release) is more powerful than a single assistant. This skill applies the same principle to research.

**One topic. Six modes. Seven personas. Repeated use over time.** That's the design.

### Where the LTV comes from

The LTV engines are **Refresh** and **Drill-down**:

- **Refresh**: markets change every quarter. A user who built a NVIDIA field guide in Q1 returns in Q2 to refresh — Refresh mode parses the prior guide, updates every number, flags material changes, and tells them whether their counter-thesis is still alive. One prompt instead of 30 minutes of work.
- **Drill-down**: every interesting disagreement in a parent guide spawns a standalone deep-dive on that subtopic. One field guide on NVIDIA can spawn 3-5 drill-downs (customer concentration, NVL72 architecture, China exposure, etc.).

Combined with the persona matrix (same topic × 7 personas), one user with one topic can produce a dozen distinct field guides over time. That's the recurring value.

---

## Compatibility

This skill follows the open [Agent Skills standard](https://agentskills.io) (released by Anthropic on December 18, 2025). It works in any compatible agent — but **output quality depends on the underlying model**.

| Agent | Install path | Status |
|---|---|---|
| **Claude.ai** | Settings UI (zip upload) | ✅ Reference implementation |
| **Claude Code** | `~/.claude/skills/` | ✅ Reference implementation |
| **Codex CLI** (OpenAI) | `~/.codex/skills/` | 🟡 Format compatible, output untested |
| **Gemini CLI** (Google) | `~/.gemini/skills/` | 🟡 Format compatible, output untested |
| **Cursor** | `.cursor/skills/` (project only) | 🟡 Format compatible, output untested |

> **🟡 means**: the SKILL.md format loads correctly, but I have not personally validated rendered HTML quality on these platforms. The skill assumes strong reasoning + long context + web search — best on Claude Opus / GPT-5 / Gemini 2.5 Pro.

---

## Boundary — what this skill is NOT

This skill produces **research frameworks** with falsifiable signals, sourced facts, and structured Bull / Bear analyses. It is **not personalized investment advice** and does not replace a licensed financial advisor.

Outputs are framed as analytical artifacts, not buy/sell recommendations. Every HTML and markdown output includes a footer disclaimer to that effect.

If you need personalized financial advice, consult a licensed advisor.

---

## Install

### 1. Claude.ai (recommended for non-developers)

1. Download the latest zip: [releases page](https://github.com/klaywang24/interactive-field-guide-skill/releases)
2. In Claude.ai → **Settings** → **Capabilities** → enable **Code execution and file creation**
3. **Customize** → **Skills** → **+** → **Upload a skill** → select the zip → toggle on
4. Try it: *"Run a fundamental analysis on NVIDIA"*

### 2. Claude Code

```bash
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.claude/skills/interactive-field-guide
```

Restart Claude Code. Skill is auto-discovered by description.

### 3. Codex CLI (OpenAI)

```bash
codex --enable skills

git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.codex/skills/interactive-field-guide
```

Restart Codex. Verify with `/skills` inside a session.

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

## How to use the modes

### Generate — one-shot research

```
"Analyze NVIDIA fundamentals"
"Research the semiconductor industry"
"Map Stripe's strategic ecosystem"
```

The skill detects mode + persona automatically. You can override:

```
"Analyze NVIDIA fundamentals as a Tier 1 investor"
"Research the semiconductor industry from a CEO perspective"
"Map Stripe's strategic ecosystem with a competitor lens"
```

### Discovery — sharper output through dialogue

```
"Help me think through NVIDIA before generating a field guide"
"Do a research dialogue on the AI agent landscape"
```

The skill walks through 5 questions (consensus → contradictions → thesis → facts → steel-manned Bull/Bear), THEN asks: **"Do you want this as a concise markdown research brief, or should I generate the full interactive HTML field guide?"**

You pick. Brief is faster and lighter; HTML is the full artifact.

**When to use Discovery**: high-stakes decisions. Position-sizing. Internal memos that will be circulated. Or just when you want to think out loud before generating.

### Critique — adversarial review

```
"Review my NVIDIA analysis. Find the weaknesses."
"Critique this report — find unsourced claims and weak counter-consensus."
```

Provide your existing analysis (paste content, upload file). The skill produces a **markdown critique** (no HTML) — strongest claims, weakest claims, what's missing, specific fixes.

**When to use**: before sharing an analysis with stakeholders. Pressure-testing your own work.

### Refresh — update with latest data ⭐

```
"Refresh my NVIDIA Q1 report with the latest data"
"Update my Stripe analysis from January"
"复盘 my AI agent landscape report — what's changed?"
```

Provide the prior field guide (upload the HTML or paste content). The skill:
- Refreshes every number against current sources
- Flags material changes (>10% shifts, qualitative reversals)
- Re-evaluates your counter-thesis (still alive? falsified? confirmed?)
- Adds a "What changed since [date]" callout box at the top

**This is the LTV mode.** Markets change every quarter. Use Refresh quarterly on every position you hold.

### Compare — side by side

```
"Compare NVIDIA and AMD"
"Stripe vs Adyen — which is the better long?"
"Cursor vs GitHub Copilot — head-to-head technical analysis"
```

The skill produces a comparative field guide with **a hero comparison table** (10-15 dimensions), **side-by-side tables in each section**, and an explicit scoring rubric. (Full 2-column mirrored layout is on the v3.x roadmap; current version uses standard template + comparison tables, which works well in practice.)

### Drill-down — expand a subsection

```
"From my NVIDIA report, drill into customer concentration"
"Take the 'AI factory operations' thesis from my semis report and make it standalone"
"Expand the disruption-by-photonics scenario into its own field guide"
```

Reference the parent field guide and the specific section. The skill treats that subsection as the entire topic of a NEW field guide — full 22-part treatment.

---

## Configuration notes

The skill needs four capabilities from the underlying agent: **(1) web search** for current facts, **(2) file write** for the HTML output, **(3) Python or Node** for the validation script, **(4) long context** (~50K tokens) for templates + references.

Claude.ai / Claude Code have all four enabled by default. Codex / Gemini / Cursor sometimes don't.

### Gemini CLI configuration

Gemini CLI has long context (1M+ tokens) so the template fits easily. Verify:

**A. Web search must be enabled.** In a session: `/tools list` — look for `google_search`. If absent, install the search extension or enable in `~/.gemini/settings.json`.

**B. File write permissions.** When writing HTML output, Gemini will prompt. To skip:

```jsonc
// ~/.gemini/settings.json
{
  "tools": {
    "auto_approve": ["write_file"]
  }
}
```

**C. Workspace trust** (only if installing as workspace skill at `.gemini/skills/`). Run `/trust`, or install at user-scope `~/.gemini/skills/` instead.

### Cursor configuration

**A. Use Agent mode, not Ask.** Top of chat panel → select **Agent**. Skills only fire in Agent mode.

**B. Web search must be on.** Settings → **Features** → **Agent** → enable **Web search**.

**C. Allow terminal commands.** Settings → **Features** → **Agent** → enable **Allow terminal commands**.

**D. Pick a strong model.** Settings → **Models** → set to Claude Sonnet 4.6 / Opus 4.7 / GPT-5 / Gemini 2.5 Pro.

**E. Output location.** HTML writes to project root by default (the skill auto-detects platform). Open via file tree → right-click → **Reveal in Finder/Explorer**.

---

## Output quality by model

| Model | Expected quality | Notes |
|---|---|---|
| Claude Opus 4.7 | ⭐⭐⭐⭐⭐ | Reference implementation; designed and tested on Opus |
| GPT-5 / GPT-5-Codex | ⭐⭐⭐⭐ | Strong reasoning, slightly different style |
| Gemini 2.5 Pro | ⭐⭐⭐⭐ | Excellent long context |
| Claude Sonnet 4.6 | ⭐⭐⭐⭐ | Faster + cheaper; counter-consensus depth slightly less |
| Gemini 2.5 Flash / GPT-4o-mini | ⭐⭐⭐ | Format works, depth reduced; OK for first drafts |

If output feels shallow, **switch to a stronger model first** before assuming the skill is the problem.

---

## What's inside

```
interactive-field-guide-skill/
├── SKILL.md                       # Lightweight router: mode + persona + format dispatch
├── assets/
│   └── template.html              # 1,500-line HTML template
└── references/
    ├── modes.md                   # 6 operational mode workflows
    ├── personas.md                # 7 analytical persona lenses (Core + Advanced)
    ├── structure.md               # 22-part menu with skip rules
    ├── data-schemas.md            # JS object schemas + component patterns
    ├── content-strategy.md        # Source tiers, counter-consensus, falsifiability
    └── pitfalls.md                # Known gotchas + validation checks
```

**Architecture**: SKILL.md is a router. It detects mode + persona + output format and dispatches to the appropriate workflow in `references/modes.md` and lens in `references/personas.md`. Personas describe topical emphasis (lens), not specific section numbers — the agent maps them onto whatever 22-part structure is current in `structure.md`.

The 22-part editorial structure covers: hero counter-consensus, stat blocks, ecosystem maps, strategic matrices, business model, unit economics, growth attribution, multi-pillar due diligence, competitive moat, fundamental disagreements (Bull vs Bear with falsifiable triggers), 90-day action items with numerical thresholds, and more.

---

## Try it

After install, try these to feel each mode:

**Generate** (one-shot):
- *"Analyze NVIDIA fundamentals"*
- *"Map the AI agent landscape"*

**Discovery** (sharpen first, then choose brief or HTML):
- *"Help me think through Stripe's strategic position before generating"*

**Critique** (adversarial review of your own):
- *"Here's my analysis of [topic]. Find the weaknesses."*

**Refresh** (update prior):
- *"Refresh my NVIDIA Q1 report with current data"*

**Compare** (head-to-head):
- *"Compare NVIDIA and AMD as a Tier 2 investor"*

**Drill-down** (expand subsection):
- *"Drill into customer concentration from my NVIDIA report"*

**Persona override** (any mode):
- *"...with an Engineer lens"*
- *"...as a Disruption Scout"*
- *"...from a CEO POV"*

**Format override**:
- *"...output as a markdown brief, not HTML"*

---

## Roadmap

**Q2 2026** (in development):
- True 2-column mirrored layout for Compare mode (currently uses comparison tables)
- `scripts/validate-field-guide.js` and `scripts/validate-compare.js` as standalone files (currently inline in `pitfalls.md`)
- Mode-specific validation checks in `pitfalls.md`

**Q3 2026** (planned):
- "Watch list" mode — track 5-10 entities passively, auto-generate alerts on material changes
- `agents/openai.yaml` for richer Codex / OpenAI ecosystem display
- skills.sh marketplace submission
- Claude Code Plugin Marketplace registration

**Q4 2026** (exploring):
- Voice-mode Discovery (instead of typed dialogue)
- Collaborative critique (multiple agents debate the same analysis)
- Bilingual auto-output (Chinese / English toggle)

---

## Troubleshooting

**Skill doesn't trigger.** The trigger is intentionally narrow — the skill activates on requests for company / industry / market / competitive / strategy / investment research artifacts. Casual questions, BI queries, and simple summaries won't trigger it. If you want to force it, say "generate a field guide on X" or "research artifact on X."

**Mode not detected correctly.** Be explicit: "Use Discovery mode for X" or "Run Critique on this".

**Persona feels wrong.** Specify: "...as a Tier 1 investor" or "...with an Engineer lens".

**HTML looks broken / missing sections.** Re-run with: *"Re-run the field guide and run the validation script."*

**Web search returns nothing on Gemini CLI / Cursor.** See [configuration notes](#configuration-notes) — search is off by default in some setups.

**Output too shallow.** Likely a model issue. Switch to Claude Opus 4.7 / GPT-5 / Gemini 2.5 Pro and re-run.

**Refresh mode can't find prior data.** Make sure you uploaded or pasted the prior field guide. The skill doesn't have memory across sessions unless you provide the prior content.

**Compare mode looks like single-column.** v3.0-beta uses comparison tables, not mirrored 2-column layout. Full mirrored layout is on the Q2 roadmap.

---

## Why "field guide" and not "report"

A report is read once. A field guide is something you keep open, search, click around, return to. The output is designed for the second behavior.

But more importantly: a field guide is for **navigation**, not display. The 22-part structure, the falsifiable judgments, the sourced facts — these aren't formatting. They're the **research discipline** that makes the artifact trustworthy.

The skill's design encodes that discipline. The HTML is the artifact. **The discipline is the product.**

---

## License

Apache 2.0. Use, fork, modify freely. PRs welcome — especially platform-specific compatibility fixes, new persona contributions, and mode-specific validation scripts.

## Author

Built by [@klaywang24](https://github.com/klaywang24). Founder of [Nexar](https://nexar.io) — creator-payout decision layer for US DTC brands.

If this skill is useful, a ⭐ on the repo helps others find it. If it's *very* useful, share a screenshot of the field guide it produced — that's the highest compliment.
