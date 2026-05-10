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

**Outputs are designed for sharing**. Treat every artifact as something the user might forward to a colleague, post publicly, or use in a stakeholder presentation. They must be free of any conversation-participant's personal context (see Critical Rule 7).

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
- `references/pitfalls.md` — known pitfalls + 9-check validation script (includes Template Wiring, Personalization Leakage, Accordion Double-binding)

For modes producing markdown output (Critique, Discovery brief-only), load only:
- `references/content-strategy.md`
- `references/pitfalls.md`

## Step 4 — Execute workflow

Follow the mode's workflow step by step. Apply the persona's lens at each judgment point. **Use `assets/template.html` as the rendering base for HTML outputs — surgically replace pre-populated content; do NOT rebuild from scratch (see Critical Rule 6 below).**

## Step 5 — Validate

For HTML outputs: run the **9-check validation script** from `references/pitfalls.md`. Iterate up to 3 times to fix issues. Only deliver after passing all 9 checks.

The 9 checks cover:
1. Template instruction strip
2. Placeholder strip
3. JS validity
4. ID consistency (CN_NODES / data-id / DETAIL_DATA)
5. Constellation count
6. Sidebar consistency
7. Template wiring integrity (added v3.0.1)
8. **Personalization leakage** (added v3.0.2)
9. **Accordion event handler integrity** (added v3.0.2)

For markdown outputs (Critique, brief): self-review against `content-strategy.md` standards — every claim sourced, every judgment falsifiable, counter-consensus angle present. Also run the personalization leakage check (#8) on markdown outputs.

## Step 6 — Deliver (cross-platform)

Detect the runtime environment and write the output appropriately:

**Filename convention**:
- Generate / Discovery / Drill-down: `<topic-slug>-field-guide.html` (or `.md` for brief)
- Refresh: `<topic-slug>-field-guide-refresh-<YYYY-MM-DD>.html`
- Compare: `<topic-A>-vs-<topic-B>-comparison.html`
- Critique: `<topic-slug>-critique.md`

**Filename must NOT contain personal names** (no `klay-bytedance.html`, no `for-klay-nvidia.html`). Use the analysis subject only.

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

6. **Template integrity is non-negotiable.** ALWAYS use `assets/template.html` as the rendering base. NEVER build HTML from scratch, even when the template's pre-populated content is on a topic unrelated to the current task. The template's CSS variables (`--bg`, `--bg-card`, `--ink`, `--accent`, `--gold`, `--rule`, `--good`, `--bad`, `--bg-2`), toolbar button IDs (`searchBtn`, `glossaryBtn`, `darkBtn`), TOC structure (with `class="toc-link"` on every `<a>` inside `<nav class="toc">`), JS button bindings, modal class conventions (`.show`), dark mode target (`body.dark`), and sidebar nav scaffold are all **load-bearing** — removing or rewiring any of them produces a black-and-white skeleton output without the warm-beige + brick-red palette and without working interactivity.

   The correct approach: open `assets/template.html` and surgically replace the pre-populated content (Mastercard / fintech sample data) with your target topic's content. **Preserve every `<style>` block, every `id="..."` attribute on toolbar buttons, every `class="..."` attribute on TOC links, every CSS variable definition, and the entire JS scaffold.** If the surgical edit feels overwhelming because the template has too much unrelated content, you are still required to do it surgically — rebuilding from scratch is forbidden.

   The 9-check validation script in `pitfalls.md` includes a Template Wiring Integrity check (Check #7) that will catch failures of this rule. If Check #7 fails, you have rebuilt instead of edited — go back and start over from a fresh copy of `assets/template.html`.

7. **Personalization leakage guard.** Outputs are research artifacts intended for sharing across teams, organizations, and the open internet. They must be **free of any conversation-participant's personal context** unless the user explicitly requested otherwise.

   ❌ FORBIDDEN in any HTML or markdown output:
   - Footer text like *"Generated for [name]"*, *"为 [name] 编写"*, *"Built for [name]"*, *"Created for [name]"*
   - Body text that volunteers user-side relevance: *"对 [user's company] 含义"*, *"[user's company] parallels"*, *"implications for your [user's startup]"*
   - Any user-side proper nouns (the user's name, their company, their startup, their employer) in any part of the output
   - Filename containing personal names (e.g. `for-klay-nvidia.html`)

   ✅ The skill DOES have access to the user's profile (name, company, projects) through Claude.ai memory or similar context — but **must not use it in output**. That information is for shaping the conversation and asking clarifying questions, not for embedding in the artifact.

   ✅ The ONLY exception: when the user explicitly requests user-side framing in the prompt itself. Examples that authorize personal context:
   - *"How does this relate to my work at [Company X]?"*
   - *"What are the implications for [my startup]?"*
   - *"I'm a [role] at [company] — analyze this from that POV"*

   Without explicit user direction, all output should be generic and reusable across users. The skill is open-source; outputs may be downloaded, shared, archived publicly.

   **Footer convention**: use generic format only.
   - English: `Generated with Interactive Field Guide · [date] · [persona] lens · [N] parts`
   - 中文: `由 Interactive Field Guide 生成 · [date] · [persona] 视角 · [N] parts`

   Validation: pitfalls.md Check #8 detects common leakage patterns.

8. **Component event handler integrity.** Interactive components (accordion, tabs, modals, drawers) must have **exactly one click handler bound per element**. Double-binding causes click toggle race conditions where the first click opens then immediately closes the component — making it appear non-functional.

   Specifically: `.accordion-header` must have **one** `addEventListener('click', ...)` registration, not two. The same applies to `.tab-button`, `.modal-close`, etc.

   Validation: pitfalls.md Check #9 detects multiple click handlers on the same component class.

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
