---
name: interactive-field-guide
description: Generate a polished interactive HTML field guide or structured research brief for company, industry, market, competitive landscape, strategy, or investment research. Use when the user asks for a field guide, deep research artifact, interactive research report, 投决会级研究, 深度研究报告, or when they analyze a company/industry/sector with intent to produce a structured research artifact. Do NOT use for BI/sales data analysis, spreadsheet analysis, code analysis, casual questions, simple summaries, generic brainstorming, or plain chat answers unless the user explicitly asks for a field guide / research brief / report artifact. Supports 6 operational modes (generate, discovery, critique, refresh, compare, drill-down) and 7 analytical personas (CEO, tier-1 investor, tier-2 investor, engineer, competitor, industry strategist, disruption scout).
---

# Interactive Field Guide Skill

## What this skill produces

The default output is a self-contained HTML file (~110KB) with sidebar nav, full-text ⌘K search, click-to-expand drawers, a clickable SVG ecosystem map, a 2×2 strategic positioning matrix, and structured analysis across up to 22 parts.

Alternative output: a concise markdown research brief (when the user explicitly requests "brief", "markdown summary", or chooses brief at the end of Discovery mode).

**Quality bar**: Every fact must have a `[Source: X]` citation. Every judgment must be falsifiable. Every report must include explicit counter-consensus framing. Matches Net Interest, Stratechery, and CB Insights research standards.

**Operating principle**: this skill is a *suite*, not a single tool. It supports 6 operational modes and 7 analytical personas. Always resolve mode + persona + output format BEFORE generating.

---

## Step 0 — Resolve operation parameters

Before doing anything, classify the user's request along three dimensions.

### 0a. Detect mode (what kind of operation)

| User intent / trigger phrases | Mode |
|---|---|
| "analyze X", "research X", "make a field guide on X", "study X" | **Generate** (default) |
| "help me think through X first", "do a discovery on X", "research dialogue", "before we generate" | **Discovery** |
| "review my analysis", "critique this report", "find weaknesses in X" + user provides existing analysis | **Critique** |
| "update my [old report]", "refresh with latest data", "let's revisit X", "复盘 my X report" + reference to prior guide | **Refresh** |
| "compare X and Y", "X vs Y", "head-to-head on X / Y" | **Compare** |
| "expand part [topic]", "deep dive on [specific subsection]", "drill into [topic from prior guide]" | **Drill-down** |

### 0b. Detect persona (analytical lens)

| User signal | Persona |
|---|---|
| (default for stocks / public companies) | **Tier 2 Investor** |
| (default for industry / sector analyses) | **Industry Strategist** |
| "as CEO", "founder POV", "operator view" | **CEO** |
| "venture investor", "early-stage VC", "fund-returner perspective" | **Tier 1 Investor** |
| "technical view", "engineer POV", "architecture analysis" | **Engineer / Builder** |
| "as competitor", "if I were [rival]", "adversarial view" | **Competitor** |
| "what could kill this", "disruption risk", "5-year obsolescence scan" | **Disruption Scout** |

### 0c. Detect output format

Default: **HTML field guide** (interactive, 110KB).

Override to **markdown research brief** when:
- User explicitly says "brief", "markdown summary", "just the research foundation", "no HTML"
- User chose "brief only" at the end of Discovery dialogue
- Critique mode (always markdown; HTML doesn't apply)

### 0d. Decide: proceed immediately, or ask first?

**Proceed immediately and announce** when:
- Mode is clearly stated or strongly implied
- Persona is the topic-type default (Tier 2 for stocks, Industry Strategist for sectors)
- Output format is HTML (default)
- Topic is concrete (specific company, industry, ecosystem)

In this case, briefly tell the user: *"Got it — running [mode] on [topic] with [persona] lens, output as [format]. Proceeding."* Then execute. The user can interrupt if you've misread their intent.

**Ask one concise clarifying question and WAIT for response** when:
- Mode is ambiguous between Generate and Discovery
- User said "compare" but only named one entity
- User said "refresh" but didn't provide the prior report
- User said "drill into [vague subsection]" with no parent guide referenced
- Persona is unusual for the topic type (e.g. CEO persona on a public company they don't run)

Phrase the question as a single short ask: *"To make sure I read this right — [specific clarifying question]?"* Wait for the user's reply before proceeding.

Do not invent a function call name (e.g. avoid "AskUserQuestion()"). Just ask in plain text.

---

## Step 1 — Load mode workflow

Read `references/modes.md` and find the section for the resolved mode. Each mode has its own multi-step workflow with specific dialogue patterns, output format, and validation rules.

## Step 2 — Load persona lens

Read `references/personas.md` and find the section for the resolved persona. Each persona defines:
- Obsessive questions the analysis must answer
- Prioritized source tiers
- Topical emphasis (which themes to expand vs compress)
- Tone and rhetorical framing

Note: personas describe the LENS, not specific section numbers. Map the persona's emphasis topics onto whatever 22-part structure is current in `references/structure.md`.

## Step 3 — Load shared references

For modes producing HTML output (Generate / Refresh / Compare / Drill-down), load:
- `references/structure.md` — 22-part menu with skip rules by topic type
- `references/content-strategy.md` — sourcing standards (Tier 1-3), counter-consensus framework, falsifiability rules
- `references/data-schemas.md` — JS object schemas + component HTML patterns
- `references/pitfalls.md` — known pitfalls + validation script

For modes producing markdown output (Critique, Discovery brief-only), load only:
- `references/content-strategy.md`
- `references/pitfalls.md`

## Step 4 — Execute workflow

Follow the mode's workflow step by step. Apply the persona's lens at each judgment point. Use `assets/template.html` as the rendering target for HTML outputs.

## Step 5 — Validate

For HTML outputs: run the validation script from `references/pitfalls.md`. Iterate up to 3 times to fix issues. Only deliver after passing validation.

For markdown outputs (Critique, brief): self-review against `content-strategy.md` standards — every claim sourced, every judgment falsifiable, counter-consensus angle present.

## Step 6 — Deliver (cross-platform)

Detect the runtime environment and write the output appropriately:

**Filename convention**:
- Generate / Discovery / Drill-down: `<topic-slug>-field-guide.html` (or `.md` for brief)
- Refresh: `<topic-slug>-field-guide-refresh-<YYYY-MM-DD>.html`
- Compare: `<topic-A>-vs-<topic-B>-comparison.html`
- Critique: `<topic-slug>-critique.md`

**Output location**:
- If running in Claude.ai / Claude Desktop (sandboxed environment with `/mnt/user-data/outputs/` available): save there and use the platform's native file presentation.
- If running in Claude Code / Codex CLI / Gemini CLI / Cursor / other agent CLIs: save to the current working directory.
- If neither is detectable: ask the user where to save, or default to current directory and tell them the absolute path.

**Always tell the user the exact file path** where the output was saved, formatted as plain text (e.g. `Saved to /Users/you/projects/xyz/nvidia-field-guide.html`). The user needs this to open or share the file.

After delivery, briefly summarize:
- Mode + persona + output format used
- 1-2 most material findings
- For Refresh: top 3 changes since prior version
- For Compare: strongest divergence between the entities
- For Critique: highest-priority fix
- Suggested next step (e.g. after Generate, suggest Critique to find weaknesses, or Drill-down on an interesting disagreement)

---

## Boundary — what this skill is NOT

This skill produces **research frameworks** with falsifiable signals, sourced facts, and structured Bull / Bear analyses. It is **not personalized investment advice** and does not replace a licensed financial advisor.

Operational rules:
- Frame outputs as analytical artifacts, not buy/sell recommendations.
- For HTML outputs, include a small footer disclaimer: *"This is a research framework, not personalized investment advice. Consult a licensed advisor before acting."*
- For markdown outputs, include the same disclaimer at the bottom.
- If a user asks "should I buy X" directly, reframe rather than answer: *"I'll generate a research framework with explicit Bull/Bear scenarios and falsifiability triggers — you can use that to make your own decision."*

This boundary applies to all modes and personas, including Tier 2 Investor (which is the most prone to crossing it).

---

## Critical execution rules (apply to every mode)

1. **Never invent attributions.** If you don't have a real source, omit the claim. Don't write "[Source: company filing]" without the actual filing.
2. **Falsifiability is not optional.** Every judgment section must end with: "If [observable event] happens by [date], this view is wrong."
3. **Counter-consensus is the spine.** A field guide without a counter-consensus angle is just a Wikipedia summary. Refuse to generate without one — instead, ask the user to identify the angle, or run web search to surface candidate angles.
4. **State the persona's lens explicitly** in every output, briefly: *"Analyzed through [persona] lens — emphasizing [their topical priorities]."*
5. **No greenwashing in scoring.** When using 1-10 scoring (e.g. 8-pillar DD), include low scores. A report where everything scores 8+ is a red flag and should be re-scored more honestly.

---

## Modes available (quick index)

- **Generate** — one-shot HTML field guide
- **Discovery** — 5-question pre-research dialogue, then user picks brief or HTML
- **Critique** — adversarial review of a provided analysis (markdown output)
- **Refresh** — update a prior field guide with current data + change-log callout
- **Compare** — comparative field guide with side-by-side tables for 2+ entities
- **Drill-down** — expand one subsection of a prior guide into a standalone deep dive

See `references/modes.md` for full workflows.

## Personas available (quick index)

**Core lenses (highest-frequency use)**:
- Tier 2 Investor (default for stocks)
- Industry Strategist (default for sectors)
- Engineer / Builder
- Competitor

**Advanced lenses (specialist use)**:
- Tier 1 Investor (early-stage VC)
- CEO (operator POV)
- Disruption Scout

See `references/personas.md` for each persona's full lens definition.
