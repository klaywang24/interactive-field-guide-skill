# Interactive Field Guide Skill

> Turn any research topic into a polished, interactive HTML field guide — sidebar nav, ⌘K search, SVG ecosystem map, 2×2 strategic matrix, 22-part editorial structure. **VC / CB Insights depth standard.**

[![Agent Skills](https://img.shields.io/badge/Agent_Skills-open_standard-blue)](https://agentskills.io)
[![License](https://img.shields.io/badge/license-Apache_2.0-green)](LICENSE)
[![中文](https://img.shields.io/badge/中文-README-red)](README.zh-CN.md)

---

## What it does

You say: **"Analyze NVIDIA fundamentals"** or **"Research the semiconductor industry"** or **"Map Stripe's strategic ecosystem"**

You get: a single self-contained HTML file (~110KB) with sidebar nav, full-text ⌘K search, click-to-expand drawers, a clickable SVG ecosystem map, a 2×2 strategic positioning matrix, and structured analysis across up to 22 parts — every fact sourced, every judgment falsifiable, with **explicit counter-consensus framing** (where the market is wrong and why).

**Quality bar:** matches Net Interest, Stratechery, and CB Insights research standards. Built for the kind of analysis a VC partner reads before an investment committee.

[See a real example →](https://github.com/klaywang24/interactive-field-guide-skill/releases) (NVIDIA fundamental analysis, 119KB HTML)

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

> **🟡 means**: the SKILL.md format loads correctly, but I have not personally validated the rendered HTML quality on these platforms. The skill's design assumes strong reasoning + long context + web search — so quality is best with Claude Opus / GPT-5 / Gemini 2.5 Pro and may be shallower with smaller models.

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
# Enable skills feature flag (one-time)
codex --enable skills

# Install the skill
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.codex/skills/interactive-field-guide
```

Restart Codex. Verify with `/skills` inside a session.

**Trigger options:**
- Implicit: `codex "Analyze NVIDIA fundamentals"`
- Explicit: `codex "$interactive-field-guide Research Stripe"`

### 4. Gemini CLI (Google) — see [configuration notes ↓](#gemini-cli-configuration)

```bash
# One command — Gemini CLI handles the clone for you
gemini skills install https://github.com/klaywang24/interactive-field-guide-skill
```

Or manually:
```bash
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.gemini/skills/interactive-field-guide
```

In a session, verify with `/skills list`. Use `/skills reload` if it doesn't appear without a restart.

### 5. Cursor — see [configuration notes ↓](#cursor-configuration)

⚠️ **Cursor only supports project-level skills** — there is no global `~/.cursor/skills/`. You must install it in each project where you want to use it.

```bash
# Inside your project root
mkdir -p .cursor/skills
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  .cursor/skills/interactive-field-guide
```

Reload window: **Cmd/Ctrl+Shift+P** → **Developer: Reload Window**

Invoke via the slash command menu (`/`) in Cursor agent mode.

---

## Configuration notes

The skill needs four capabilities from the underlying agent: **(1) web search** for current facts, **(2) file write** for the 110KB HTML output, **(3) Python or Node** for the validation script, **(4) long context** (~50K tokens) for the template + references.

Claude.ai / Claude Code have all four enabled by default. Codex / Gemini / Cursor sometimes don't.

### Gemini CLI configuration

Gemini CLI has long context (1M+ tokens) so the template fits easily. The two things to verify:

**A. Web search must be enabled.** Gemini CLI ships with Google Search but it may be off by default in some installs. In a session:

```
/tools list
```

Look for `google_search` or similar. If absent, install the search extension or enable it in your `~/.gemini/settings.json` under `tools`.

**B. File write permissions.** When the skill writes the HTML output, Gemini will prompt for permission. Approve once per session. To skip the prompt:

```jsonc
// ~/.gemini/settings.json
{
  "tools": {
    "auto_approve": ["write_file"]
  }
}
```

**C. Workspace trust** (only if installing as a workspace skill at `.gemini/skills/`). Run `/trust` in your session and restart, or use the user-scope path `~/.gemini/skills/` instead which doesn't require trust.

### Cursor configuration

Cursor's main constraint is **how it surfaces the HTML output**. Cursor is editor-first, not chat-first — generated files appear in the file tree, not as a chat-side artifact.

**A. Use Agent mode, not Ask.** Open the chat panel → top of the panel select **Agent** (not **Ask** or **Edit**). Skills only fire in Agent mode.

**B. Web search must be on.** In Cursor Settings → **Features** → **Agent** → enable **Web search**. Verify in chat by typing `@web` and confirming the option appears.

**C. Allow terminal commands.** The skill's validation script runs Node or Python. In Settings → **Features** → **Agent** → enable **Allow terminal commands** (or set per-command approval).

**D. Pick a strong model.** Settings → **Models** → set the agent model to Claude Sonnet 4.6 / Opus 4.7 / GPT-5 / Gemini 2.5 Pro. Smaller / faster models will produce shallower analysis (the skill's quality bar assumes strong reasoning).

**E. Output location.** The skill writes the HTML to your project root. Open it via Cursor's file tree (right-click → **Reveal in Finder/Explorer**) and double-click to view in your browser.

---

## Output quality by model

Honest expectations, based on the skill's design requirements (sourced facts, falsifiable counter-consensus, multi-source synthesis):

| Model | Expected quality | Notes |
|---|---|---|
| Claude Opus 4.7 | ⭐⭐⭐⭐⭐ | Reference implementation; the skill was designed and tested on Opus |
| GPT-5 / GPT-5-Codex | ⭐⭐⭐⭐ | Strong reasoning, slightly different style; close to reference |
| Gemini 2.5 Pro | ⭐⭐⭐⭐ | Excellent long context; close to reference |
| Claude Sonnet 4.6 | ⭐⭐⭐⭐ | Faster + cheaper; counter-consensus depth slightly less than Opus |
| Gemini 2.5 Flash / GPT-4o-mini | ⭐⭐⭐ | Format works, depth reduced; OK for first drafts |

If the output feels shallow, **switch to a stronger model first** before assuming the skill is the problem.

---

## Try it

After install, try any of these prompts:

- *"Run a fundamental analysis on NVIDIA"*
- *"Research Stripe's strategic ecosystem"*
- *"Build a sector deep-dive on the semiconductor industry"*
- *"Map the AI agent landscape"*
- *"Industry report on Chinese new energy vehicles"*

The skill auto-detects the topic type (single company / industry / ecosystem) and selects a subset of the 22 parts accordingly — single-company fundamentals use ~12 parts; industry analyses use ~16; comparative ecosystem maps use ~18.

---

## What's inside

```
interactive-field-guide-skill/
├── SKILL.md                       # Main instructions, 22-part workflow
├── assets/
│   └── template.html              # 1,500-line HTML template (warm cream + brick red)
└── references/
    ├── structure.md               # 22-part menu with skip rules
    ├── data-schemas.md            # 6 JS object schemas + component patterns
    ├── content-strategy.md        # VC depth standard, source tiers, counter-consensus framework
    └── pitfalls.md                # 10 known gotchas + 6-check Node validation script
```

The 22-part editorial structure covers: hero counter-consensus, 4 stat blocks, ecosystem map, 2×2 strategic matrix, business model, unit economics, growth attribution, 8-pillar due diligence (with 1-10 scoring), competitive moat, 5 fundamental disagreements (Bull vs Bear with falsifiable triggers), 90-day action items with numerical thresholds, and more.

See [`references/structure.md`](references/structure.md) for the full part menu and skip logic by topic type.

---

## Troubleshooting

**Skill doesn't trigger.** Check the description in `SKILL.md` matches your prompt. The skill activates when the agent sees keywords like *analyze, research, deep-dive, fundamental, ecosystem, industry report*.

**HTML looks broken / missing sections.** The validation script catches most issues. Re-run with: *"Re-run the field guide and run the 6-check validation."*

**Web search returns nothing on Gemini CLI / Cursor.** See [configuration notes](#configuration-notes) above — search is off by default in some setups.

**Output too shallow.** Likely a model issue, not a skill issue. Switch to Claude Opus 4.7 / GPT-5 / Gemini 2.5 Pro and re-run.

**Cursor doesn't see the skill.** Make sure you used `.cursor/skills/` in your **project root**, not a global path. Reload window after install.

---

## Why "field guide" and not "report"

A report is read once. A field guide is something you keep open, search, click around, return to. The output is designed for the second behavior — you're meant to live in it for 30 minutes, not skim it for 3.

---

## License

Apache 2.0. Use, fork, modify freely. PRs welcome — especially platform-specific compatibility fixes if you find issues.

## Author

Built by [@klaywang24](https://github.com/klaywang24). Founder of [Nexar](https://nexar.io) — creator-payout decision layer for US DTC brands.

If this skill is useful, a ⭐ on the repo helps others find it.
