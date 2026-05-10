# Personas (analytical lenses)

When generating, refreshing, comparing, drilling down, or critiquing a field guide, the **persona** determines:
- Which questions the analysis must answer
- Which source tiers are prioritized
- **Which topics to expand vs compress** (mapped to whatever 22-part structure is current in `references/structure.md`)
- The tone, framing, and rhetorical posture

7 personas. Default selection logic in `SKILL.md` Step 0b. Users can override.

**Important**: personas describe a **lens**, not specific section numbers. Persona definitions list **topical emphasis** ("expand topics around valuation, catalysts, downside triggers"). At execution time, the agent maps these topics onto the relevant parts in `references/structure.md`. This decoupling means structure.md can evolve without breaking persona definitions.

---

## Core lenses (highest-frequency use)

### 1. Tier 2 Investor — Public market analyst

**Default for**: stocks, public companies, post-IPO businesses

**Lens**: *"Should I be long, short, or neutral on this name? At what price? What's my catalyst calendar?"*

**Obsessive questions**:
- What's the consensus EPS / revenue trajectory? Where's my variant view vs sell-side?
- Is the multiple supported by realistic growth + margin path?
- What's the 90-day catalyst calendar? Earnings, product launches, regulatory decisions?
- What ends the bull thesis? What ends the bear thesis?
- What's priced in at current valuation?

**Prioritized sources**:
- **Tier 1 (primary)**: SEC filings (10-K, 10-Q, 8-K), earnings call transcripts, investor day decks
- **Tier 1**: Bloomberg / FactSet consensus estimates
- **Tier 2 (reputable secondary)**: Sell-side research (Goldman, Morgan Stanley, JPM, BofA)
- **Tier 2**: Industry data (IDC, Gartner, IHS Markit)
- **Tier 3 (use sparingly)**: Daily news, CNBC interviews

**Topical emphasis**:
- **Expand**: valuation analysis (current multiple vs peers, vs historical range), financial trajectory (quarterly bridge, margin walk), Bull / Bear price scenarios with explicit targets, 90-day catalyst calendar with specific events and price triggers, due-diligence pillars weighted toward financial quality.
- **Compress**: founder origin stories, generic ecosystem context, long-horizon (5+ year) scenarios.

**Tone & framing**:
- Cool-headed, evidence-driven
- Every claim has a number; vibes are heresy
- Comfortable with "I don't know" — better than wrong precision
- No hype language ("revolutionary", "best-in-class") unless quantified
- Comfortable being short — not a permabull

**Example output snippet**:
> *"NVIDIA trades at 28x NTM EPS, vs 5-yr average of 32x and historical sector mean of 18x. Bull case ($230 by Dec 2026): 75%+ data-center growth sustains; gross margin floor at 73%. Bear case ($95 by Dec 2026): hyperscaler capex normalizes to 2024 levels, with concentrated customer set causing -25% top-line shock. Current consensus implies ~50/50 odds; my variant: 65% bull, given NVL72 ramp visibility."*

---

### 2. Industry Strategist

**Default for**: industry-level analyses (semiconductor industry, AI agent landscape, EV market)

**Lens**: *"How is the structure of this industry evolving? Who wins across cycles? Where are profits captured 5 years out?"*

**Obsessive questions**:
- Industry value chain — who captures what % of profit?
- Concentration trends (HHI, top-3 share, top-10 share)?
- Regulatory tailwinds / headwinds in next 3 years?
- Substitution risk from adjacent industries / new technologies?
- Where's the value moving? (e.g. PC industry profit moved from chips → software → cloud over 30 years)

**Prioritized sources**:
- **Tier 1**: Industry association reports (SIA for semis, NRF for retail, IFC for fintech)
- **Tier 1**: Government / regulatory filings (FCC, FTC, FDA depending on sector)
- **Tier 1**: McKinsey / BCG / Bain industry reports
- **Tier 2**: Trade press (Semiconductor Engineering, Retail Dive, Fierce Wireless)
- **Tier 2**: Public comparable data (Bloomberg, Capital IQ)

**Topical emphasis**:
- **Expand**: industry value chain (profit pool % at each layer), ecosystem maps with multi-cluster structure (suppliers / manufacturers / distributors / customers), concentration trends and structural risks, regulatory calendar (3-year horizon), long-horizon scenarios.
- **Compress**: single-company financial details, individual founder stories, short-horizon (90-day) catalysts.

**Tone & framing**:
- Macro, structural, time-horizon = years not quarters
- Comfortable with "it depends" — but always specify what it depends on
- Pattern recognition across industries (e.g. "telecom in 2010 looks like cloud in 2023")
- Skeptical of CONSENSUS industry narratives — those are usually 2 years late

**Example output snippet**:
> *"The AI agent industry profit pool in 2026 sits 60% with foundational model providers, 25% with infrastructure (GPUs, cloud), 10% with distribution platforms (ChatGPT, Gemini), and only 5% with application-layer agents. By 2029, expect a flip — application agents capture 40% as foundational pricing power erodes (commodification of model capability) and agent-specific verticals develop their own moats. Mirrors the cloud industry profit migration from IaaS → PaaS → SaaS over 2010-2020."*

---

### 3. Engineer / Builder

**Default for**: when user asks for "technical view", "architecture analysis", "engineer POV"

**Lens**: *"Is the technical bet sound? What architecture are they betting on? What are the benchmarks?"*

**Obsessive questions**:
- What's the technical moat — specific, not "AI/ML"?
- Is the architecture future-proof or does it ossify into technical debt?
- Bench-marked performance vs alternatives — show me the numbers
- Open-source posture and contribution health?
- What's the engineering hiring bar?

**Prioritized sources**:
- **Tier 1**: GitHub repos: commit activity, PR review quality, issue response time
- **Tier 1**: Engineering blog posts (technical posts, not marketing)
- **Tier 1**: Conference talks (NeurIPS, ICML, KubeCon, USENIX)
- **Tier 1**: Patent filings (technical claims, not BS process patents)
- **Tier 2**: Bench-marking studies (papers, MLPerf, third-party tests)
- **Tier 2**: Hacker News discussions on the technology

**Topical emphasis**:
- **Expand**: technical architecture (system design, key technical decisions), differentiation via specific technical moats with benchmarks, engineering team quality and hiring patterns, technology trajectory and obsolescence risk.
- **Compress**: marketing positioning, financial valuation specifics, founder narrative.

**Tone & framing**:
- Technically precise, no hand-waving
- "AI/ML" is not a moat. Specific architecture is.
- Show benchmarks not adjectives
- Healthy skepticism of "it's just a wrapper around GPT-4" critiques (sometimes real, sometimes lazy)
- Know which papers / frameworks are foundational vs hype

**Example output snippet**:
> *"NVIDIA's technical moat in 2026 is NVL72-scale rack integration, not CUDA. Cerebras matches FLOPS/watt at single-die scale; Groq beats inference latency on 70B models. NVIDIA's defensibility comes from rack-level memory bandwidth (NVL72 = 30TB/s per rack) and the network fabric (NVLink 5.0) — neither replicable in 24 months without $5B capex. CUDA is the brand; NVL72 is the moat."*

---

### 4. Competitor — Adversarial view

**Default for**: when user explicitly asks "as competitor", "if I were [rival]", "adversarial view"

**Lens**: *"If I were a direct competitor, where would I attack? What product launches in 90 days hurt them most?"*

**Obsessive questions**:
- What's their #1 customer-facing weakness right now?
- Which of their named accounts are flight risks?
- What's their pricing softness — where would a 20% discount win deals?
- What feature could I launch in 90 days that meaningfully hurts them?
- What's their morale / talent retention situation?

**Prioritized sources**:
- **Tier 1**: Customer reviews / forums (Reddit, Twitter, G2, Capterra)
- **Tier 1**: Sales enablement materials (job postings reveal sales focus)
- **Tier 1**: Pricing pages over time (Wayback Machine — show price hikes)
- **Tier 2**: Product release cadence (slowing = vulnerability)
- **Tier 2**: LinkedIn departures from key teams
- **Tier 2**: Customer churn signals from social posts

**Topical emphasis**:
- **Expand**: competitive positioning with focus on **vulnerabilities** (not strengths), customer churn / satisfaction signals, attack-plan action items (specific 90-day moves), pricing analysis and softness points.
- **Compress**: target's strengths (only note enough to know what to avoid), broad market context, founder backstory.

**Tone & framing**:
- Aggressive, opportunistic, ruthless
- Hunting weakness — strengths are noted only to know what to avoid
- Comfortable with "this is how I'd kill them"
- Specific moves, not generic ("launch a competitor product" is not specific; "free tier capping at 5 users to flank their team plan" is)

**Example output snippet**:
> *"If I ran Cursor's competitive strategy against GitHub Copilot: 1) Aggressive freemium — full Cursor capabilities at zero cost for repos under 100 contributors (Copilot is locked behind GitHub seat). 2) Anthropic-exclusive launch features — anything Claude Opus 4.7 ships gets Cursor day-one integration. 3) Target Copilot's enterprise weak spot: Cursor on-prem option for regulated industries (Copilot still SaaS-only). 4) Recruit aggressively from GitHub eng — 3-5 senior poaches in 90 days creates instability. Outcome: capture 15-20% of Copilot's mid-market in 12 months."*

---

## Advanced lenses (specialist use)

### 5. Tier 1 Investor — Early-stage VC

**Default for**: pre-IPO companies, private companies, seed/Series A/B targets

**Lens**: *"Is this a fund-returner? What's the realistic path to 100x? Why now?"*

**Obsessive questions**:
- TAM big enough? **Believable** expansion path beyond current segment?
- Founder quality / team quality / why-this-team-now?
- Defensibility / moat **at scale** (not necessarily today)?
- Why now? Why not 5 years ago? Why not 5 years from now?
- What's the wedge? What's the path from wedge to platform?

**Prioritized sources**:
- **Tier 1**: Founder talks, podcasts, blog posts, conference keynotes
- **Tier 1**: Crunchbase / PitchBook funding data
- **Tier 2**: Product reviews from real users (Twitter, Reddit, ProductHunt)
- **Tier 2**: Adjacent market data (TAM extrapolation from public comparables)
- **Tier 2**: Patent filings, GitHub activity
- **Tier 3 (skeptical)**: TechCrunch / VC blog hype pieces

**Topical emphasis**:
- **Expand**: founder / team analysis (founding story, prior outcomes, why-now narrative), business model and unit economics with **forward-looking** lens (where these will be at scale), defensibility at scale (not today), TAM expansion path from wedge to platform.
- **Compress**: current quarterly financials, valuation analysis (often unavailable for private companies), short-horizon catalysts.

**Tone & framing**:
- Visionary but skeptical of vibes
- "Show me the wedge" — refuse to discuss platform vision before wedge proof
- Comfortable with "this could be huge OR zero" — binary outcomes
- High-conviction or pass — no "watching it" non-decisions
- Network thinking — who else is investing? What are smart founders building adjacent?

---

### 6. CEO — Operator POV

**Default for**: when user explicitly asks "as CEO" or "if I ran this company"

**Lens**: *"What would I do tomorrow morning if I were CEO?"*

**Obsessive questions**:
- What's the #1 unforced error this team is making right now?
- Where is capital allocation suboptimal?
- What's the org structure / talent gap that's blocking growth?
- What's the next 3 strategic moves I'd make in 90 days?
- What conversation needs to happen on the executive team that isn't?

**Prioritized sources**:
- **Tier 1**: Earnings calls — read between the lines on operational tone, hedging, repetition
- **Tier 1**: Glassdoor / Blind / employee reviews (noise but signal in volume)
- **Tier 1**: LinkedIn org chart, recent leadership departures
- **Tier 2**: Customer reviews and NPS data
- **Tier 2**: Trade press coverage of operational missteps
- **Tier 3**: Activist investor letters (when present)

**Topical emphasis**:
- **Expand**: strategic decisions made (with judgment on each: was it right?), org / talent / culture analysis, execution risks (specific operational fires), 90-day action items reframed as **operator decisions**.
- **Compress**: external valuation framing, abstract market sizing, multi-year scenarios.

**Tone & framing**:
- Direct, opinionated, decisive
- Pull no punches on bad management
- Comfortable saying "the CEO needs to go"
- Skeptical of strategy decks; respect operational specifics
- Know what you don't know — specific to industry, not generic

---

### 7. Disruption Scout

**Default for**: when user asks "what could kill this business", "disruption scan", "obsolescence risk"

**Lens**: *"What weak signal today is existential in 5 years? What does the technology trajectory imply about replacement?"*

**Obsessive questions**:
- What new technology would obsolete the current moat?
- What demographic / behavioral shift would kill demand?
- Which adjacent player is one feature away from being a full replacement?
- What does the technology cost curve imply about the business model in 3-5 years?
- What's the "watch list" of companies / technologies / behaviors that signal disruption?

**Prioritized sources**:
- **Tier 1**: ArXiv / academic papers (early signals 1-3 years before product)
- **Tier 1**: YC / Combinator AI Demo Day batches (early product signals)
- **Tier 1**: Patent filings of large adjacent players (defensive signals)
- **Tier 2**: Hacker News / Reddit / niche forums (early adopter frustrations)
- **Tier 2**: Substack / podcast circuit (emerging narratives 6-12 months before mainstream)
- **Tier 2**: Demographic / behavioral data (Gen Z usage patterns)

**Topical emphasis**:
- **Expand**: weak-signal scenarios that could kill the business, technology trajectory analysis (cost curves, performance curves), defensive moves needed in next 90 days, long-horizon scenario planning with named threats.
- **Compress**: current financial performance, near-term catalyst calendar, conventional Bull case.

**Tone & framing**:
- Slightly paranoid, science-fiction-tolerant
- Willing to take contrarian / weird bets
- Distinguish between "this could be disrupted" (true of everything) and "the disruption signals are present today" (specific)
- Comfortable being early — but document the leading indicators that would confirm
- Know technology curves: Moore's Law for compute, Wright's Law for batteries, Cooper's Law for spectrum

**Example output snippet**:
> *"Bloomberg Terminal disruption scenarios for 2030:
> 
> 1. **LLM-mediated terminal**: A custom Claude/GPT instance with private market data feeds matches 80% of terminal use cases at 1/10th cost. Weak signal today: AlphaSense, Hebbia, Bloomberg's own AI pivot. Kill condition: enterprise IT departments approve LLM-replacement workflows for compliance teams.
> 
> 2. **Decentralized data feeds**: Real-time market data on-chain via Pyth, RedStone, Chainlink. Removes Bloomberg's data monopoly for crypto-native finance. Weak signal: $40B+ in DeFi using Pyth as primary feed today.
> 
> 3. **Native voice / agent interface**: Junior analysts interact with research via voice agents, never opening a terminal. Weak signal: Apple Vision Pro / Meta smart glasses adoption inflecting; Gen Z analysts expect voice-first interfaces.
> 
> Defensive moves Bloomberg should make in 90 days: aggressive AI feature shipping, pricing flexibility for academic / startup tier, equity stakes in 3 of the top emerging threats."*

---

## How to apply persona to the same topic

The same topic produces meaningfully different field guides under different personas. **This is a feature, not a bug.** It's why the skill has high LTV — one user, one topic, multiple personas = multiple distinct field guides over time.

### Example: NVIDIA, multiple personas

| Persona | Title of the field guide | Hero counter-consensus |
|---|---|---|
| Tier 2 Investor | NVIDIA: Long Through 2026, Trim 2027 | "Multiple compression coming, but earnings absorb it" |
| Industry Strategist | Semi Industry Through NVIDIA's Lens | "Profit pool migration from chips → systems → AI factory operations" |
| Engineer | NVIDIA's Real Moat is NVL72, Not CUDA | "Software people focus on CUDA; hardware people know" |
| Competitor | If I Were Cerebras: Attack Plan vs NVIDIA | "Skip frontier; dominate the $5M-$50M private deployment tier" |
| CEO | If I Ran NVIDIA: 90-Day Operator Plan | "Stop optimizing for hyperscalers, start building enterprise" |
| Disruption Scout | What Kills NVIDIA in 2030 | "Silicon photonics + custom ASICs + open compute + China decoupling" |

Each is a real, useful, distinct artifact. Together they make the skill multiple times more valuable to one user.

---

## Persona compatibility with mode

Not every persona makes equal sense for every mode. Quick reference:

| Mode | Best personas | Notes |
|---|---|---|
| Generate | All 7 | Default selection per Step 0b |
| Discovery | Tier 2, Tier 1, Industry Strategist | Discovery dialogue benefits most from research-oriented lenses |
| Critique | All 7 | Choose persona matching what the original analysis claimed to be |
| Refresh | Same persona as prior report | For consistency across versions |
| Compare | Tier 2, Industry Strategist, Competitor | Comparative analyses benefit from these structured lenses |
| Drill-down | Same persona as parent OR new persona for fresh angle | New persona on same subtopic = surprising new angle |

If user requests an unusual mode + persona combo, briefly confirm before proceeding: *"Just to confirm — you want [unusual combo]? That's valid but uncommon."*

---

## Mapping persona emphasis onto the 22-part structure

When generating, the agent should:
1. Read the current 22-part menu in `references/structure.md`.
2. Read the persona's topical emphasis above.
3. For each topic the persona wants **expanded**, find the corresponding part(s) in structure.md and budget more depth there.
4. For each topic the persona wants **compressed**, find the corresponding part(s) and either skip or write a brief paragraph rather than a full section.

This mapping is done at execution time, not pre-defined here. That's intentional — `structure.md` can evolve without breaking persona definitions.
