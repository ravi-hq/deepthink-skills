---
name: deepthink
description: |
  End-to-end deep research and analysis pipeline. Takes a raw idea or market question,
  conducts deep web research, builds a competitive landscape, runs multi-framework
  intelligence analysis (/think), stress-tests it (/red-team), researches the red team
  findings, re-thinks with adversarial data, re-red-teams, and iterates until divergence
  between think and red-team is low (conviction stabilizes). Then generates a comprehensive
  single-file HTML report with all findings: market landscape, competitive analysis,
  intelligence briefs, red team results, how to win, and how you could lose.
  Use when the user says "/deepthink", "deep think", "deep research", or wants a
  comprehensive research-to-report pipeline on any idea, market, or strategic question.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - WebSearch
  - WebFetch
  - AskUserQuestion
  - Agent
  - SendMessage
  - TeamCreate
  - TeamDelete
  - TaskCreate
  - TaskUpdate
  - TaskList
  - TaskGet
---

# /deepthink — Deep Research & Analysis Pipeline

You are a research orchestrator running an iterative intelligence pipeline. Your job:
take a raw idea, conduct deep market research, analyze it through multiple frameworks,
stress-test the analysis, research the weaknesses found, re-analyze, and iterate until
the analysis stabilizes — then produce a comprehensive HTML report.

The output feels like a week of analyst work compressed into one session: deep market
data, multi-framework intelligence, adversarial stress testing, and a polished
presentation-ready report.

---

## Pipeline Overview

```
IDEA → RESEARCH → THINK → RED-TEAM → RESEARCH gaps → RE-THINK → RE-RED-TEAM → ... → REPORT
                                                                                  ↑
                                                                    (iterate until convergence)
```

**Convergence criteria:** The pipeline stops iterating when:
1. Red team verdict is SURVIVES or stable WOUNDED (same verdict two rounds in a row)
2. Failure probability deltas between rounds are < 5% on all major failure modes
3. No new KILL SHOTS found in the latest red team round
4. Maximum 3 iterations (to bound cost)

---

## Invocation

When invoked with `$ARGUMENTS`:

1. If `$ARGUMENTS` contains a clear idea, market, or question → proceed to Phase 1
2. If `$ARGUMENTS` is empty or too vague, ask ONE question via AskUserQuestion:
   "Describe the idea or market you want to research in 2-3 sentences. What are you
   trying to build, enter, or understand?"
3. Do NOT ask more than one round of questions. Work with what you have.

---

## Phase 1 — Deep Market Research

### Goal
Build a comprehensive understanding of the competitive landscape, market dynamics,
existing players, funding patterns, and adoption signals before any analytical
frameworks run.

### Step 1.1 — Scope the Research

Based on the idea, identify 4-6 research vectors:
- **Direct competitors:** Who is building this or something adjacent?
- **Market size:** TAM/SAM/SOM estimates from credible sources
- **Funding landscape:** Who has raised money in this space? How much? When?
- **Adoption signals:** What evidence exists that the market is real (or not)?
- **Regulatory/structural:** Any barriers, tailwinds, or structural dynamics?
- **Technology readiness:** Is the underlying tech mature enough?

Present the research plan briefly:

```
## Deep Research: [Idea Title — 3-5 words]

**Idea:** [1-2 sentence restatement]

**Research vectors:**
1. [vector] — [what we need to find]
2. [vector] — [what we need to find]
...

Starting deep research across [N] vectors...
```

### Step 1.2 — Execute Research

Spawn research agents in parallel using `Agent` tool with `run_in_background: true`.
Each agent gets one research vector and uses `WebSearch` and `WebFetch` to gather data.

Agent prompt template:
```
You are a market research analyst. Your job is to research ONE specific vector
thoroughly using web searches.

IDEA: [the idea]
YOUR RESEARCH VECTOR: [specific vector]
WHAT TO FIND: [specific questions to answer]

INSTRUCTIONS:
- Run 3-5 web searches with different query formulations
- Look for: specific companies, funding amounts, market size estimates, adoption
  data, technology maturity signals, regulatory developments
- Prefer recent data (2024-2026)
- Include specific numbers, names, and sources
- If you find conflicting data, note both claims with sources

OUTPUT FORMAT:
Return a structured research brief:
## [Vector Title]

### Key Findings
- [Finding 1 — with source/date]
- [Finding 2 — with source/date]
...

### Companies/Players
| Company | What they do | Funding | Stage | Threat Level |
|---|---|---|---|---|

### Market Data
- [Specific numbers with sources]

### Signals
- [Bullish signal — evidence]
- [Bearish signal — evidence]

### Gaps in Knowledge
- [What we couldn't find or verify]
```

Use `model: "sonnet"` for research agents. Name them: `research-competitors`,
`research-market-size`, `research-funding`, etc.

### Step 1.3 — Compile Research Document

After all research agents return, compile findings into a single research document.
Write to `thoughts/deepthink/YYYY-MM-DD-<slug>-research.md`:

```markdown
---
date: <ISO 8601>
idea: "<idea title>"
research_vectors: [list]
key_players: [list of top 5-10 companies]
estimated_tam: "<number>"
---

# Market Research: [Idea Title]

## Executive Summary
[3-5 sentences synthesizing the research — what the landscape looks like, where the
opportunity is, and what the biggest unknowns are]

## Competitive Landscape
[Compiled from competitor research — table of players, what they do, funding, stage]

## Market Size
[TAM/SAM/SOM with sources and methodology notes]

## Funding & Investment Signals
[Who's investing, how much, what stage, what it implies]

## Adoption & Demand Signals
[Evidence the market is real — or not]

## Technology Readiness
[Is the underlying tech mature enough? What's the gap?]

## Regulatory & Structural Dynamics
[Barriers, tailwinds, structural factors]

## Key Unknowns
[What we couldn't find — these become research targets for later rounds]
```

---

## Phase 2 — First Intelligence Brief (/think)

Run the /think skill on the idea, providing the research as context.

### Execution

Use the `Skill` tool to invoke `/think` with the following argument structure:

Actually — do NOT invoke /think via the Skill tool. Instead, **execute the /think
workflow directly** inline, following the full /think SKILL.md protocol:

1. **Read** the think skill from `.claude/skills/think/SKILL.md`
2. **Triage:** Select 4-7 frameworks. Include the research document as context for
   every sub-analyst agent.
3. **Spawn agents** per the /think protocol (run_in_background, model: sonnet)
4. **Collect results**, surface contradictions, synthesize
5. **Write** the think brief to `thoughts/deepthink/YYYY-MM-DD-<slug>-think-r1.md`
   (note: r1 = round 1)

The key difference from a standalone /think: every sub-analyst agent receives the
full research document as additional context, so their analysis is grounded in
real market data rather than general knowledge.

Agent prompt addition (prepend to each analyst's prompt):
```
MARKET RESEARCH CONTEXT:
[Include the full compiled research document from Phase 1]

Use this research as ground truth. Reference specific companies, numbers, and
findings in your analysis. Do not make claims that contradict the research unless
you have a specific reason to disagree (and state that reason).
```

---

## Phase 3 — First Red Team (/red-team)

Run the /red-team protocol on the Phase 2 think brief.

### Execution

Execute the /red-team workflow directly inline, following the full /red-team
SKILL.md protocol:

1. **Read** the red-team skill from `.claude/skills/red-team/SKILL.md`
2. **Extract** the target from the Phase 2 think brief
3. **Select 5-7 prosecutors**, always including Feynman, Kahneman, Tetlock, Munger
4. **Spawn prosecutor agents** with the full think brief AND research document
5. **Compile** kill sheet with ratings
6. **Write** to `thoughts/deepthink/YYYY-MM-DD-<slug>-redteam-r1.md`

Record the verdict, kill shots, wounds, and revised failure probabilities.

---

## Phase 4 — Gap Research

The red team found weaknesses. Some are speculative, some may have real evidence
behind them. Research the gaps.

### Step 4.1 — Identify Research Gaps

From the red team report, extract:
- Kill shots that hinge on unverified claims (can we verify or refute them?)
- Wounds that reference competitors or market dynamics we haven't researched
- Historical analogues the red team cited (are they accurate?)
- Base rates the red team claimed (can we find the actual data?)

### Step 4.2 — Targeted Research

Spawn 2-4 research agents focused specifically on the gaps the red team identified.
These are targeted, not broad — each agent investigates one specific claim or gap.

Agent prompt template:
```
You are a fact-checker and research analyst. The red team made a specific claim
that needs verification.

THE CLAIM: [specific red team attack or assertion]
CONTEXT: [relevant section of the think brief and red team report]

YOUR JOB: Search for evidence that either supports or refutes this claim.
- Run 2-3 targeted web searches
- Look for: specific data, historical examples, counterexamples
- Be honest about what you find — don't try to confirm or deny, just report

OUTPUT:
## Claim: [restate the claim]
**Verdict:** CONFIRMED / REFUTED / MIXED / INSUFFICIENT DATA
**Evidence:**
- [Finding 1 — source]
- [Finding 2 — source]
**Implication:** [What this means for the original analysis]
```

### Step 4.3 — Update Research Document

Append gap research findings to the research document:
```markdown
## Gap Research (Round 1)
[Findings from targeted research on red team claims]
```

---

## Phase 5 — Re-Think (Round 2)

Run /think again, but now with THREE sources of context:
1. The original research (Phase 1)
2. The red team findings (Phase 3)
3. The gap research (Phase 4)

### Execution

Execute the /think workflow again with enriched context:

1. Triage: May select different frameworks this round based on what the red team found
2. Every sub-analyst receives: original research + red team report + gap research
3. Analysts should explicitly address the kill shots and wounds from the red team
4. Write to `thoughts/deepthink/YYYY-MM-DD-<slug>-think-r2.md`

Agent prompt addition:
```
RED TEAM FINDINGS (Round 1):
[Include the full red team report]

GAP RESEARCH:
[Include gap research findings]

IMPORTANT: The red team found these specific weaknesses in the previous analysis.
Your job is NOT to ignore them or defend against them — it's to produce the most
accurate analysis possible given ALL available information, including the adversarial
findings. If the red team was right about a weakness, incorporate that reality.
If you have evidence they were wrong, state it with the evidence.
```

---

## Phase 6 — Re-Red-Team (Round 2)

Run /red-team on the Round 2 think brief.

### Execution

Same protocol as Phase 3, but prosecutors receive:
- The Round 2 think brief (primary target)
- The Round 1 red team report (so they know what was already found)
- The gap research (so they know what was verified/refuted)

Write to `thoughts/deepthink/YYYY-MM-DD-<slug>-redteam-r2.md`

### Convergence Check

After the Round 2 red team completes, check convergence:

```
CONVERGENCE CHECK:
- Round 1 verdict: [SURVIVES/WOUNDED/DEAD]
- Round 2 verdict: [SURVIVES/WOUNDED/DEAD]
- New kill shots in Round 2: [count]
- Max failure probability delta: [X]%
- Verdict stability: [STABLE/UNSTABLE]
```

**If converged** (same verdict, no new kill shots, deltas < 5%): proceed to Phase 7
**If not converged AND iterations < 3**: go back to Phase 4 (Gap Research) for another round
**If max iterations reached**: proceed to Phase 7 with a note about remaining instability

Present the convergence status to the user:
```
## Convergence Status

**Round 1:** [verdict] | **Round 2:** [verdict]
**New kill shots:** [count]
**Max probability delta:** [X]%
**Status:** [CONVERGED — proceeding to report / ITERATING — round 3 / MAX ITERATIONS — proceeding with caveats]
```

---

## Phase 7 — Generate HTML Report

This is the final deliverable. Generate a comprehensive, single-file HTML report
that presents ALL findings from the pipeline.

### Report Structure

Write to `thoughts/deepthink/YYYY-MM-DD-<slug>-report.html`

The report has these sections:

1. **Hero Section** — Title, one-line recommendation, conviction badge, date
2. **Executive Summary** — 3-5 paragraphs synthesizing everything
3. **The Idea** — What we analyzed, restated clearly
4. **Market Landscape** — Competitive landscape table, market size, funding data,
   key players (from Phase 1 research)
5. **Intelligence Brief** — The final think brief (from the last round), including:
   - Frameworks applied
   - Core Argument
   - Key Insight
   - Load-bearing conditions
   - Failure modes with probabilities
6. **Red Team Findings** — The final red team results:
   - Kill shots (if any survived)
   - Wounds
   - Compounding attacks
   - Survival verdict
   - Revised failure probabilities table
7. **Convergence Analysis** — How the analysis evolved across rounds:
   - What changed between rounds
   - What stabilized
   - Remaining uncertainties
8. **How We Win** — The specific path to winning this market:
   - First move
   - Defensibility strategy (which Powers to build)
   - Timeline
   - Key milestones
9. **How We Lose** — The specific ways this fails:
   - Top failure modes (with probabilities)
   - Competitor responses that kill us
   - Market dynamics that work against us
   - The "nightmare scenario"
10. **What to Validate First** — Prioritized list of assumptions to test
11. **Appendix: Research Sources** — All sources cited across the research phases

### HTML Design System

Use this design system (matches existing reports in this project):

```css
:root {
  --bg: #0a0a0f;
  --surface: #12121a;
  --surface2: #1a1a26;
  --border: #2a2a3a;
  --text: #e8e8f0;
  --text-dim: #888899;
  --text-muted: #555566;
  --accent: #7c6ff7;
  --accent-glow: rgba(124,111,247,0.15);
  --green: #4ade80;
  --green-dim: rgba(74,222,128,0.12);
  --amber: #fbbf24;
  --amber-dim: rgba(251,191,36,0.12);
  --red: #f87171;
  --red-dim: rgba(248,113,113,0.12);
  --blue: #60a5fa;
  --blue-dim: rgba(96,165,250,0.12);
  --cyan: #22d3ee;
  --cyan-dim: rgba(34,211,238,0.12);
}
```

**Design principles:**
- Dark mode, professional, presentation-ready
- Hero section with gradient background (purple/cyan for think reports, red/amber for red team)
- Use the deepthink accent: gradient from purple (#7c6ff7) to cyan (#22d3ee) — represents the blend of analytical thinking and adversarial testing
- Conviction badges: GREEN for HIGH, AMBER for MEDIUM, RED for LOW
- Verdict badges: GREEN for SURVIVES, AMBER for WOUNDED, RED for DEAD
- Section-alternating backgrounds (--bg and --surface)
- Framework cards in a responsive grid
- Competitive landscape as a styled table
- Failure probabilities in a comparison table (original vs. red team vs. final)
- Typography: system font stack, 15px body, 1.6 line-height
- Max width: 960px centered
- All styles embedded (single-file report)
- Print-friendly
- Responsive layout
- Semantic HTML

**Key visual components:**

Hero:
- Eyebrow: "DEEP THINK INTELLIGENCE REPORT"
- Title: The idea/market name with accent-colored emphasis
- Subtitle: The one-line recommendation
- Badge row: conviction level + iterations completed + frameworks used count

Market Landscape Section:
- Competitive grid or table with player cards
- Funding totals visualization
- Market size callout box

Intelligence Section:
- Framework selection cards (showing which were used and why)
- Core argument in a highlighted blockquote
- Key insight in an accent-bordered callout
- Conditions and failure modes as structured lists

Red Team Section:
- Kill shots in red-bordered cards
- Wounds in amber-bordered cards
- Bruises as a compact list
- Failure probability comparison table with color-coded deltas
- Verdict banner

Convergence Section:
- Round-by-round comparison showing evolution
- Probability convergence visualization (simple table showing how estimates changed)

How We Win / How We Lose:
- Two-column layout where possible
- Win path as a numbered timeline
- Lose scenarios as severity-rated cards

### HTML Generation

Generate the full HTML inline. Do NOT use a template file or external tool.
The HTML should be complete, valid, and render beautifully when opened in a browser.

The report should be substantial — typically 800-1500 lines of HTML including
embedded CSS. This is the final deliverable and should feel polished.

---

## Final Presentation

After writing all files, present a summary to the user:

```
## Deep Think Complete: [Idea Title]

**Iterations:** [N] rounds of think/red-team
**Convergence:** [CONVERGED / MAX ITERATIONS]
**Final verdict:** [SURVIVES / WOUNDED / DEAD]
**Final conviction:** [LOW / MEDIUM / HIGH]

---

**Core Argument:** [3-5 sentences]

**Key Insight:** [1-2 sentences — the non-obvious finding]

**How We Win:** [1-2 sentences — the path]

**How We Lose:** [1-2 sentences — the biggest risk]

---

**Files generated:**
- Research: `thoughts/deepthink/YYYY-MM-DD-<slug>-research.md`
- Think R1: `thoughts/deepthink/YYYY-MM-DD-<slug>-think-r1.md`
- Red Team R1: `thoughts/deepthink/YYYY-MM-DD-<slug>-redteam-r1.md`
- [Gap Research, Think R2, Red Team R2 if applicable]
- **Report: `thoughts/deepthink/YYYY-MM-DD-<slug>-report.html`**

Open the HTML report in your browser for the full analysis.
```

---

## Quality Standards

**Research must be real.** Every market claim, competitor mention, and funding number
should come from actual web searches, not general knowledge. Cite sources.

**Numeric probabilities everywhere.** No "likely" or "possible" — specific percentages
with stated assumptions. This applies to the think briefs, red team reports, AND the
final HTML report.

**The convergence loop is the differentiator.** The value of /deepthink over just
running /think then /red-team is that findings from each round inform the next.
The final analysis should be materially better than what a single pass would produce.

**The HTML report must be beautiful.** This is the deliverable the user will share.
It should look like it came from a top-tier strategy consultancy, not a markdown
renderer. Invest in the CSS and structure.

**How We Win and How We Lose must be specific.** Not generic strategy advice — specific
moves, specific competitors, specific timelines, specific failure modes tied to the
research and analysis conducted.

**Take positions.** The report should have a clear recommendation with conviction level.
The user wants to know what to do, not a balanced overview of possibilities.

---

## Cost & Timing Notes

- This skill spawns many agents across multiple phases. It's the most expensive
  skill in the toolkit — use it for ideas that warrant serious analysis.
- Typical run: 15-30 minutes depending on iteration count
- Agent model: sonnet for all sub-agents (research, think analysts, red team prosecutors)
- Expected output: 5-8 markdown files + 1 HTML report
