# Modes (operational workflows)

This skill operates in 6 modes. Detect the resolved mode in `SKILL.md` Step 0, then follow the workflow below.

---

## Mode 1: Generate (default)

**User trigger**: "Analyze NVIDIA", "Research the semiconductor industry", "Make a field guide on Stripe"

**Time**: ~3 minutes
**Output**: HTML field guide (single self-contained file)
**LTV**: One-shot. Other modes drive repeat use.

### Workflow

1. **Topic understanding** (30s)
   - Briefly clarify the topic if ambiguous (e.g. "NVIDIA" = stock vs business vs technology?)
   - If freshly searched data is needed (current quarter financials, post-cutoff news), run 1-2 web searches.

2. **Identify counter-consensus angle** (60s)
   - What does the average reader assume?
   - Where does the data complicate that assumption?
   - Phrase as falsifiable: "[Specific claim]. If [event] happens, this view is wrong."

3. **Select parts** (30s)
   - Read `references/structure.md` for the 22-part menu and skip rules.
   - Single-company fundamentals: ~12 parts
   - Industry analysis: ~16 parts
   - Strategic ecosystem map: ~18 parts

4. **Apply persona emphasis**
   - Read `references/personas.md` for the resolved persona.
   - Map the persona's topical emphasis (e.g. "valuation, catalysts, Bull/Bear thresholds") onto the relevant parts in `structure.md`.
   - Expand parts that match persona priorities; compress or skip parts the persona deprioritizes.

5. **Fill template**
   - Use `assets/template.html` as the base.
   - Inject content per `references/data-schemas.md` schemas.
   - Every fact: `[Source: X]` citation.
   - Every section ending with a judgment: include falsifiability trigger.
   - Apply Boundary footer (see SKILL.md Boundary section).

6. **Validate** (30s)
   - Run the validation script from `references/pitfalls.md`.
   - Iterate up to 3 times to fix issues.

7. **Deliver** (per SKILL.md Step 6)
   - Save to platform-appropriate location with filename convention.
   - Tell user the exact path.
   - Briefly note: "Generated with [persona] lens. Most material finding: [X]. Suggested follow-up: Critique mode (find weaknesses) or Drill-down on [specific section]."

---

## Mode 2: Discovery

**User trigger**: "Help me think through NVIDIA first", "Do a research dialogue on Stripe before generating", or user explicitly asks for Discovery in Step 0.

**Time**: ~10-15 min dialogue + ~3 min output
**Output**: Either a markdown research brief OR a full HTML field guide — user chooses at the end.
**Quality lift**: Significant — the foundation is more focused and counter-consensus is sharper.

### Workflow

Run a 5-question dialogue. Ask **one question at a time**, wait for the user's answer before proceeding.

#### Q1 — Consensus check

Ask: *"What do you understand to be the dominant view on [topic]? You can tell me what you think the consensus is, or I can search and summarize the mainstream framing. Which?"*

- If user provides their summary → note it.
- If user asks you to search → run 2-3 web searches looking for mainstream coverage. Summarize the consensus in 2-3 sentences.

#### Q2 — Contradicting evidence

Run web searches looking for data points that **complicate or contradict** the consensus. Surface 5 specific findings, each with:
- A specific data point or quote
- The source (with date)
- A 1-line note on why it complicates the consensus

Then ask: *"Which 1-2 of these surprise you most? That's where the counter-consensus angle lives."*

#### Q3 — Sharpen the thesis

Based on user's selection from Q2, help them phrase a falsifiable counter-thesis. Format:

> *"[Specific claim about the topic]. If [observable event] happens by [date], this view is wrong."*

Iterate with the user until the claim is:
- **Specific** (not "X is overvalued" but "X's gross margins will compress 200bps in 2 quarters")
- **Falsifiable** (a real-world event would prove it wrong)
- **Non-obvious** (something most analysts haven't said publicly)

#### Q4 — Specific facts needed

Ask: *"What 5-8 specific facts (numbers, dates, primary-source quotes) would make this thesis airtight?"*

For each fact the user names:
- If they have it → note it with citation.
- If they don't → run web searches to find it. Surface what you find with sources.

After Q4, you should have a **fact base** of 5-8 sourced data points.

#### Q5 — Steel-man both sides

Ask: *"What's the strongest possible Bull case for the consensus view? What's the strongest possible Bear case for your counter-thesis? Don't make either weak."*

- Help user articulate both at maximum strength.
- If they make either side a strawman, push back: *"That's the weak version. What's the steel-manned version?"*

#### Synthesis + output choice

Summarize what you've built together:

```
RESEARCH FOUNDATION FOR [TOPIC]
═══════════════════════════════

Counter-thesis: [tight statement from Q3]
Falsifiability: [specific event from Q3]

Key sourced facts (Q4):
1. [fact + source]
2. [fact + source]
... (5-8 items)

Bull case (steel-manned): [from Q5]
Bear case (steel-manned): [from Q5]

Persona lens: [resolved persona]
```

Then ask the user:

*"Do you want this as a concise markdown research brief (the foundation above, polished into a 1-2 page memo), or should I generate the full interactive HTML field guide (22-part structure with this as the spine)?"*

**If user picks brief**:
- Polish the synthesis above into a markdown research brief (see structure below).
- Add Boundary disclaimer at bottom.
- Save as `<topic-slug>-research-brief.md` per Step 6.

**If user picks HTML**:
- Switch to **Generate mode**, but seed the 22-part workflow with this foundation.
- The 22-part structure should be filled with these facts and judgments, not freshly invented ones.

### Markdown research brief structure (when user picks brief)

```markdown
# Research Brief: [Topic]

**Persona lens**: [persona name]
**Date**: [YYYY-MM-DD]

## Counter-thesis

[Tight thesis statement from Q3]

**Falsifiability trigger**: [Observable event by date]

## Consensus view (what we're pushing against)

[2-3 sentence summary from Q1]

## Key sourced facts

1. [Fact with [Source: X]]
2. [Fact with [Source: X]]
... (5-8 items)

## Bull case (steel-manned)

[From Q5]

## Bear case (steel-manned)

[From Q5]

## What to watch (next 90 days)

- [Specific event 1]
- [Specific event 2]
- [Specific event 3]

## Suggested next steps

- If conviction grows: Generate full HTML field guide for sharing
- If conviction wavers: Critique mode to stress-test
- If new data emerges: Refresh mode in 90 days

---

*This is a research framework, not personalized investment advice. Consult a licensed advisor before acting.*
```

This brief is the lower-cost output path for users who don't need the full interactive HTML.

---

## Mode 3: Critique

**User trigger**: "Review my analysis", "find weaknesses in this report", "critique this", + user provides existing analysis (file uploaded, pasted text, or URL to prior field guide).

**Time**: ~5-10 minutes
**Output**: Markdown critique document (NO HTML).

### Workflow

1. **Read carefully**
   - Read the entire user-provided analysis.
   - Don't assume any claim is correct just because it's stated confidently.
   - Note every numerical claim, every source citation, every judgment.

2. **Identify the strongest claims** (3-5)
   - Which claims are most load-bearing for the analysis's conclusion?
   - For each: check source attribution. Is it Tier 1 (primary source like SEC filing), Tier 2 (reputable secondary like Bloomberg), or Tier 3 (unsourced/vibes)?
   - Run web searches for contradicting evidence on each strong claim.

3. **Stress-test falsifiability**
   - Are the conclusions falsifiable, or are they vibes ("growing fast", "leading position", "strong moat")?
   - For each judgment, ask: "What event would prove this wrong? If nothing would, the claim isn't falsifiable."

4. **Find the weakest claims** (3-5)
   - Vague language ("approximately", "据估计", "industry-leading")
   - Unsourced statistics
   - Cherry-picked time windows (e.g. "since 2023" when 2022 was a peak)
   - Strawmanned counter-arguments

5. **Identify what's missing**
   - Counter-consensus angle present? Or is this just a sympathetic summary?
   - Bull AND Bear both steel-manned? Or is one side a strawman?
   - Numerical thresholds for action items? Or vague "monitor closely"?
   - Source citations on hero stats? Or assumed knowledge?

6. **Suggest specific fixes**
   - For each weakness, name a specific replacement source, data point, or rephrasing.
   - Don't just say "this is weak" — say "replace with [X] from [source]."

### Output format

Markdown document, structured:

```markdown
# Critique: [original report title]

**Reviewed by**: [persona] lens
**Reviewed on**: [date]

## 🟢 Strongest claims (survive scrutiny)

1. **[Claim]** — Sourced from [Tier 1/2 source]. Cross-checked against [other source]. Holds up.
2. ...

## 🟡 Weak but salvageable

1. **[Claim]** — Currently unsourced.
   *Fix*: [specific source to cite, or specific rephrasing]
2. ...

## 🔴 Missing or strawmanned

1. **No counter-consensus angle.** The report reads as a sympathetic summary. 
   *Fix*: Identify where market consensus is most likely wrong. Suggested 
   angles: [3 specific options based on data].
2. **Bear case is strawmanned.** Current Bear: "Some say X is overvalued" 
   — this isn't an actual bear position.
   *Fix*: Steel-manned Bear case: [specific scenario with data].
3. ...

## 📚 Suggested additional sources

1. [Specific report / filing / dataset that would strengthen the analysis]
2. ...

## 🎯 Highest-priority fix

If you only fix one thing: [specific claim with suggested replacement].

---

*This is a research framework critique, not personalized investment advice.*
```

---

## Mode 4: Refresh

**User trigger**: "Update my [old report]", "refresh with latest data", "let's revisit my NVIDIA analysis", "复盘 my Stripe report", + user references prior field guide.

**Time**: ~5 minutes
**Output**: Updated HTML field guide + change-log callout box at top.
**LTV trigger**: ⭐⭐⭐ — users return every quarter to refresh held positions. THIS IS THE LTV ENGINE.

### Workflow

1. **Parse the prior field guide**
   - User uploads the prior HTML, pastes content, or references a saved version.
   - If user only describes the prior report from memory without providing content, ask them to paste at least the key claims and numbers — Refresh requires the prior facts to compare against.
   - Extract from the provided content:
     - All `[Source: X]` citations and their dates
     - All numerical claims (revenue, market share, valuation, growth rates)
     - The counter-thesis and its falsifiability conditions
     - The Bull vs Bear disagreements
     - The action items and their numerical thresholds

2. **Refresh each numerical claim**
   - For each number in the prior report: run web search for current value, compare to prior, calculate delta.
   - Flag any change >10% or any qualitative reversal as **material**.

3. **Stress-test the counter-thesis**
   - Has the falsification condition triggered?
   - Are the underlying facts still supportive?
   - If falsification triggered → the prior thesis is now wrong. Note this prominently.

4. **Re-evaluate the disagreements**
   - For each Bull vs Bear pair: has new data emerged that strengthens one side?
   - Update the "currently winning" assessment.

5. **Generate updated HTML field guide**
   - Use the same persona and structure as the prior report.
   - All numbers updated, sources refreshed.

6. **Add a "What changed since [prior date]" callout** at the top
   - Top 3 most material updates
   - Counter-thesis status: confirmed / partially confirmed / falsified / TBD
   - Net assessment: stronger / weaker / unchanged conviction

### Refresh delivery format

The HTML output should include, near the top:

```html
<div class="refresh-callout">
  <h3>📅 Refreshed [today's date] (prior version: [date])</h3>
  
  <div class="changes-material">
    <strong>Top 3 material changes:</strong>
    <ol>
      <li>[Change 1: was X, now Y, +Z%, source]</li>
      <li>[Change 2]</li>
      <li>[Change 3]</li>
    </ol>
  </div>
  
  <div class="thesis-status">
    <strong>Counter-thesis status:</strong> 
    [Confirmed / Partially confirmed / Falsified / TBD]
    <br>
    <em>Falsification condition was: [X]. Outcome: [Y].</em>
  </div>
  
  <div class="conviction">
    <strong>Net assessment:</strong> 
    [Stronger / Weaker / Unchanged] conviction in counter-thesis
  </div>
</div>
```

This callout is what makes Refresh mode addictive — it shows the user EXACTLY what's new since they last looked. They don't have to re-read the whole report.

### Why this drives LTV

Markets change every quarter. A user who built a field guide on NVIDIA in Q1 will want to refresh in Q2, Q3, Q4. Refresh mode makes that one prompt instead of 30 minutes of work.

---

## Mode 5: Compare

**User trigger**: "Compare NVIDIA and AMD", "Stripe vs Adyen", "Head-to-head: GitHub Copilot vs Cursor"

**Time**: ~5-7 minutes
**Output**: Comparative field guide with side-by-side tables and in-section comparisons.

### Workflow

1. **Identify the entities** (typically 2, max 3)
   - If user named only one entity, ask for the comparison target before proceeding.

2. **Identify the comparison axis** — ask user if not specified:
   - Financial / valuation
   - Product / technical
   - Strategic positioning / moat
   - Ecosystem / partnerships
   - Leadership / talent
   - All of the above

3. **Run parallel research on each entity**
   - Same set of facts gathered for each entity.
   - Same persona lens applied to each.
   - Sources at the same tier across entities (don't compare a TechCrunch piece on one to an SEC filing on the other).

4. **Generate comparative field guide**
   - Use the standard `assets/template.html` — do not commit to a 2-column mirrored layout (this is v3.0-beta; full mirrored layout is on the Q2 roadmap).
   - **Hero region**: a comparison summary table with 10-15 key dimensions, side-by-side rows for each entity.
   - **Counter-consensus**: "Where the market gets the COMPARISON wrong" (not just one entity, but the comparison itself).
   - **Within each section of the 22-part structure**: include a side-by-side table or paired prose treatment of [Entity A] vs [Entity B] for that section's topic.
   - **Final scoring**: weighted rubric across pillars, with explicit weights. Show the score for each entity and the verdict.

### Compare-specific design notes

- The comparison table at the top is the most important component. Make it scannable: 10-15 rows, dimensions on the left, entity columns on the right.
- For each section, you don't need a full 2-column layout — a comparison table or paired prose ("NVIDIA: ... AMD: ...") works well.
- Color-code consistently: Entity A = primary brand color, Entity B = secondary brand color. Apply across the comparison table and any inline tables.
- For Part 17 (disagreements): structure as "Where Bulls and Bears differ on A vs B" — i.e., the disagreement is about the comparison, not about each entity in isolation.

---

## Mode 6: Drill-down

**User trigger**: "Expand part [topic] from my NVIDIA report", "deep dive on the customer concentration disagreement", "let's drill into [specific subsection]"

**Time**: ~5 minutes
**Output**: Standalone HTML field guide on the subtopic.
**LTV trigger**: ⭐⭐ — every interesting disagreement spawns a drill-down.

### Workflow

1. **Identify prior guide and target section**
   - User references the prior guide (uploads HTML or pastes section).
   - User specifies which topic / subsection / disagreement to expand.
   - If user references "Part [N]" but doesn't say which section that is, ask them to paste the relevant section content rather than guessing the part number.

2. **Take subsection's content as the seed**
   - Extract all claims, sources, and judgments from that subsection.
   - This becomes the starting position — not new research from scratch.

3. **Apply full 22-part treatment to the subtopic**
   - The subsection is now the entire topic of a new field guide.
   - Same persona, same depth standards, same template.

4. **Reference the parent report**
   - The drill-down's intro should explicitly reference the parent: "This is a deep dive on [subsection topic] from the [parent topic] field guide. The parent report concluded [X]; this drill-down stress-tests that conclusion."

### Example

**Parent guide**: NVIDIA Fundamental Analysis
**Drill-down trigger**: "Expand the customer concentration disagreement"

**Drill-down output**: NVIDIA Customer Concentration: A Deep Dive

- Hero counter-consensus: counter-consensus on whether concentration is a feature (NVIDIA's pricing power) or a bug (revenue cliff risk)
- Stat blocks: hyperscaler revenue share over 4 quarters, named customers + their AI capex, customer-by-customer NVIDIA dependency, alternative supplier benchmarks
- Ecosystem map: NVIDIA ↔ Microsoft / Google / Amazon / Meta — the dependencies in both directions
- 8-pillar DD: re-scored on customer concentration alone
- Disagreements section: all expanded around customer concentration
- Action items: tied specifically to concentration risk monitoring

**Why this works**: any single section of a great research report has enough hidden depth to become its own report. Drill-down lets users go from breadth to depth selectively.

---

## Mode quick-reference card

| Mode | Time | Output | LTV trigger |
|---|---|---|---|
| Generate | ~3 min | HTML field guide | First-time use |
| Discovery | ~15 min dialogue + 3 min | Brief OR HTML (user picks) | Important decisions |
| Critique | ~5-10 min | Markdown critique | "Did I miss anything?" |
| Refresh | ~5 min | Updated HTML + change-log | Quarterly review of held positions ⭐ |
| Compare | ~5 min | HTML with side-by-side tables | Acquisition / position-sizing decisions |
| Drill-down | ~5 min | Standalone HTML on subtopic | Each interesting disagreement spawns one |

Refresh and Drill-down are the LTV engines — users return to the skill repeatedly as topics evolve and as they want to dig deeper.
