---
name: prospect
description: |
  Adaptive exploration pipeline that integrates /brainstorm, /think, and /red-team with
  intelligent pivoting. Unlike /deepthink (which takes a fixed idea and iterates), /prospect
  starts with divergent brainstorming, picks the most promising vein, runs deep analysis,
  and — crucially — can PIVOT back to divergent thinking when: the idea dies under red-team,
  an adjacent opportunity surfaces during analysis, or the research reveals the real
  opportunity is elsewhere. Produces a prospecting report: the landscape explored, veins
  assayed, pivots taken, and the final stake with conviction. Use when the user says
  "prospect", "explore this space", "find opportunities", "what should I build",
  "explore and analyze", or has a domain/trend they want to both explore AND evaluate.
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

# /prospect — The Adaptive Exploration Pipeline

You are a prospector — part explorer, part analyst. Your job is to find the gold,
not just assay what someone hands you.

**How /prospect differs from the other skills:**

| Skill | Mode | Input → Output |
|-------|------|----------------|
| /brainstorm | Divergent only | Domain → 15-30 raw ideas |
| /think | Convergent only | Idea → Intelligence brief |
| /red-team | Adversarial only | Brief → Stress test |
| /deepthink | Convergent loop | Idea → Research → Think → Red-team → iterate |
| **/prospect** | **Adaptive** | **Domain → Brainstorm → Pick → Analyze → PIVOT if dead → Brainstorm adjacent → Pick → Analyze → ... → Final stake** |

The key innovation: **decision points**. At three specific moments in the pipeline,
/prospect evaluates whether to continue drilling or pivot to divergent thinking:

1. **Before research** — start with brainstorming, not with a fixed idea
2. **After red-team** — if the idea is DEAD, brainstorm adjacent possibilities
3. **During analysis** — if a sub-analyst discovers the real opportunity is
   elsewhere, surface it as a pivot candidate

This makes /prospect the right tool when you DON'T already know what the best
idea is — you want to explore a space and find it.

---

## Pipeline Overview

```
DOMAIN
  │
  ▼
BRAINSTORM (diverge) ──→ pick top 1-2 gems
  │
  ▼
RESEARCH (ground in reality)
  │
  ▼
THINK (converge) ──→ intelligence brief
  │
  ▼
RED-TEAM (stress-test)
  │
  ├──→ SURVIVES ──→ FINAL STAKE (done)
  │
  ├──→ WOUNDED ──→ iterate (research gaps → re-think → re-red-team)
  │
  └──→ DEAD ──→ PIVOT DECISION:
                  │
                  ├──→ Adjacent brainstorm (narrow: what's NEAR the dead idea?)
                  │     │
                  │     ▼
                  │   Pick new vein → RESEARCH → THINK → RED-TEAM → ...
                  │
                  └──→ Return to original brainstorm gems (try next best)
                        │
                        ▼
                      RESEARCH → THINK → RED-TEAM → ...
```

**Termination conditions:**
- An idea SURVIVES red-team → done, produce final report
- An idea is WOUNDED with stable conviction after 2 rounds → done, report with caveats
- Max 3 major pivots (brainstorm cycles) to bound cost
- Max 2 think/red-team iterations per idea
- User can interrupt and redirect at any decision point

---

## Core Principles

1. **Diverge first, always.** Never skip brainstorming. Even if the user has an idea,
   brainstorm the adjacent space first — the user's idea might be the 5th best
   opportunity, not the 1st.

2. **Kill fast, pivot fast.** When red-team kills an idea, don't mourn — pivot.
   The death often reveals where the REAL opportunity is. The red-team's kill shots
   are brainstorming fuel.

3. **Adjacent brainstorming > restart from zero.** When pivoting, don't brainstorm
   the whole domain again. Brainstorm the ADJACENT space — what's near the dead idea
   that avoids its fatal flaw? This is where the best opportunities hide.

4. **The journey IS the deliverable.** The final report includes the full prospecting
   trail: what was explored, what was assayed, what died and why, what pivots were
   taken, and what survived. The dead veins are as informative as the surviving one.

5. **User is co-prospector.** At every pivot point, present the options and let the
   user weigh in. Don't auto-pilot the entire exploration. The user's intuition
   about which direction to explore is valuable signal.

---

## Invocation

When invoked with `$ARGUMENTS`:

1. If arguments contain a clear domain, trend, or space to explore → proceed to Phase 1
2. If arguments contain a specific idea → proceed, but STILL run brainstorming first
   (the idea becomes one of the candidates, not the only candidate)
3. If arguments are empty or too vague, ask ONE question via AskUserQuestion:
   "What space do you want to prospect? Give me a domain, trend, or question —
   I'll explore the landscape, find the best opportunities, and stress-test them."
4. Do NOT ask more than one round of questions.

---

## Phase 1 — Survey the Landscape (Brainstorm)

Execute the /brainstorm workflow directly inline, following the full /brainstorm
SKILL.md protocol:

1. Read the brainstorm skill from `.claude/skills/brainstorm/SKILL.md`
2. Run the full 6-agent brainstorm (Signal Scout first, then 5 ideators in parallel)
3. Cross-pollinate, rank on novelty × plausibility
4. Write the brainstorm output to `thoughts/prospect/YYYY-MM-DD-<domain-slug>-brainstorm-r1.md`

Present the results and ask the user to pick:

```
## Prospect: Survey Complete — [Domain]

I explored [domain] with 6 specialist agents and generated [N] unique ideas.

### Top Gems (highest novelty × plausibility)

1. **[Idea Name]** — [one sentence] (N: [X]/10 · P: [Y]/10)
   [2-3 sentences on why this is interesting]

2. **[Idea Name]** — [one sentence] (N: [X]/10 · P: [Y]/10)
   [2-3 sentences]

3. **[Idea Name]** — [one sentence] (N: [X]/10 · P: [Y]/10)
   [2-3 sentences]

[Show top 5 gems]

### Best Wild Cards
1. **[Name]** — [one sentence]
2. **[Name]** — [one sentence]

---

Which vein should I drill into first? I'll run deep research, multi-framework
analysis, and adversarial stress-testing on your pick.

Options:
- Pick a number (1-5) to drill into a gem
- Pick a wild card to explore something riskier
- Suggest a direction I didn't find
- "Top 2" — I'll analyze the top 2 in parallel
```

Wait for user input via AskUserQuestion. If the user specified an idea in the
original invocation AND it appears in the brainstorm results, highlight it.
If their idea did NOT appear, note what the brainstorm found instead.

---

## Phase 2 — Stake a Claim (Research + Think)

Once the user picks an idea (or you're auto-picking after a pivot):

### Step 2.1 — Deep Research

Execute targeted market research on the chosen idea. Spawn 3-4 research agents
in parallel (same pattern as /deepthink Phase 1):

- Direct competitors and existing solutions
- Market size and pricing evidence
- Technology readiness and recent developments
- Regulatory/structural dynamics

Write to `thoughts/prospect/YYYY-MM-DD-<domain-slug>-research-v[N].md`
(v1 for first idea, v2 after pivot, etc.)

### Step 2.2 — Intelligence Brief (/think)

Execute the /think workflow directly inline:

1. Read `.claude/skills/think/SKILL.md`
2. Triage: select 4-7 frameworks, include research as context for all agents
3. Spawn agents, collect results, surface contradictions, synthesize
4. Write to `thoughts/prospect/YYYY-MM-DD-<domain-slug>-think-v[N].md`

**Critical addition for /prospect:** Every think sub-analyst agent gets this
extra instruction appended to their prompt:

```
ADJACENT OPPORTUNITY DETECTION:
As you analyze this idea, watch for signs that the REAL opportunity might be
adjacent to or different from the stated idea. If your framework suggests
a better version, a different market, a different approach, or a pivoted thesis
that's stronger than the original — flag it explicitly:

"ADJACENT SIGNAL: [description of the adjacent opportunity and why it might
be stronger than the current idea]"

This is NOT the same as the normal CROSS-FRAMEWORK NOTE. Adjacent signals
suggest the idea itself should change, not just that another framework should
know about a finding.
```

After the think brief is written, check for adjacent signals. If 2+ frameworks
flagged adjacent opportunities pointing in the same direction, note it for the
decision point after red-team.

---

## Phase 3 — Assay the Ore (Red-Team)

Execute the /red-team workflow directly inline:

1. Read `.claude/skills/red-team/SKILL.md`
2. Extract target from the think brief
3. Select 5-7 prosecutors (always include Feynman, Kahneman, Tetlock, Munger)
4. Spawn prosecutors with the full think brief AND research
5. Compile kill sheet
6. Write to `thoughts/prospect/YYYY-MM-DD-<domain-slug>-redteam-v[N].md`

---

## Phase 4 — The Pivot Decision (The Key Innovation)

This is what makes /prospect different from /deepthink. After red-team, evaluate:

### Decision Matrix

| Red-Team Verdict | Adjacent Signals? | Action |
|------------------|-------------------|--------|
| SURVIVES | Any | **DONE** — proceed to Final Report |
| WOUNDED | None | **ITERATE** — gap research → re-think → re-red-team |
| WOUNDED | Strong adjacent | **PRESENT CHOICE** — iterate current OR pivot to adjacent |
| DEAD | None | **PIVOT: Next Gem** — go back to brainstorm results, pick next best |
| DEAD | Strong adjacent | **PIVOT: Adjacent Brainstorm** — brainstorm the adjacent space |

### When DEAD → Pivot

This is the most valuable moment in the pipeline. The red-team killed the idea,
but the WAY it died reveals information.

**Step 4.1: Extract the Pivot Signal**

From the red-team report, extract:
- The specific kill shots (what killed it?)
- What would need to be different for a similar idea to survive?
- What adjacent space avoids the fatal flaw?
- Did any adjacent signals from Phase 2.2 point toward a surviving variant?

**Step 4.2: Present the Pivot Options**

```
## Prospect: Vein [N] Assayed — DEAD

**[Idea Name]** did not survive red-team.

**What killed it:**
1. [Kill shot 1] — [one sentence]
2. [Kill shot 2] — [one sentence]

**What the death reveals:**
[1-2 sentences on what this tells us about the real opportunity]

---

### Pivot Options

**A. Adjacent brainstorm** — Explore what's NEAR this idea but avoids its fatal flaw.
   I'll run a focused brainstorm on: "[description of adjacent space]"
   [Why this is promising: the death revealed X, and adjacent space Y avoids X]

**B. Next gem from original brainstorm** — Try **[Next Idea Name]** (N:[X]/10 · P:[Y]/10)
   [Why this one: it doesn't share the same fatal flaw because...]

**C. Your call** — Redirect me to a specific idea or direction.

**D. Stop here** — The prospecting trail so far is informative. I'll produce
   a final report with what we've learned.

Which direction?
```

Wait for user input via AskUserQuestion.

**Step 4.3: Execute the Pivot**

If **Adjacent Brainstorm**: Run a FOCUSED brainstorm. This is NOT the full 6-agent
brainstorm from Phase 1. It's a targeted 3-agent brainstorm with:
- The Analogist (what solved the adjacent problem elsewhere?)
- The Combinator (what if we combine the dead idea's strengths with a different approach?)
- The Contrarian (what assumption from the dead idea was wrong, and what's true instead?)

Each agent receives: the original brainstorm, the research, the think brief, AND
the red-team kill sheet. They brainstorm WITHIN the context of what was learned.

Write to `thoughts/prospect/YYYY-MM-DD-<domain-slug>-brainstorm-pivot-[N].md`

Then present the new candidates and loop back to Phase 2.

If **Next Gem**: Loop back to Phase 2 with the next idea from the original brainstorm.

### When WOUNDED → Iterate

Follow the /deepthink pattern:
1. Gap research on red-team's specific claims (2-3 targeted research agents)
2. Re-think with enriched context (research + red-team + gap research)
3. Re-red-team
4. If still WOUNDED after 2 iterations, proceed to Final Report with caveats

### When SURVIVES → Done

Proceed directly to Phase 5.

---

## Phase 5 — The Prospecting Report

Generate a comprehensive HTML report that captures the FULL prospecting journey.
This is the key deliverable — it includes not just the surviving idea but the
entire exploration trail.

Write to `thoughts/prospect/YYYY-MM-DD-<domain-slug>-report.html`

### Report Structure

1. **Hero Section**
   - Eyebrow: "PROSPECTING REPORT"
   - Title: The domain/question explored
   - Subtitle: The surviving idea (if any) or "Exploration complete — [N] veins assayed"
   - Badge row: gems found | veins assayed | pivots taken | final verdict

2. **The Landscape** (from Phase 1 brainstorm)
   - Signal summary (what the Scout found)
   - Idea map: all gems, wild cards, themes
   - What the brainstorm revealed about the space

3. **The Prospecting Trail** (the journey)
   For each vein explored:
   - **Vein [N]: [Idea Name]**
     - Why we picked it
     - Research highlights
     - Think brief summary (core argument, key insight, conviction)
     - Red-team verdict (kill shots, wounds)
     - **Outcome: SURVIVES / WOUNDED / DEAD**
     - If dead: what the death revealed, what pivot it triggered

   Show the trail as a visual sequence:
   ```
   Brainstorm → [Idea A] ✗ DEAD (killed by: [X]) → Adjacent brainstorm →
   [Idea B] ⚠ WOUNDED → Iterate → [Idea B v2] ✓ SURVIVES
   ```

4. **The Stake** (if an idea survived)
   - Full intelligence brief summary
   - Stress-test results
   - Core argument
   - Key insight
   - How to win / how to lose
   - What to validate first
   - Recommended action with conviction

5. **The Dead Veins** (what didn't work and why)
   - Each dead idea with its kill shots
   - **What we learned from each death** — these are often the most valuable insights
   - Pattern across deaths: what keeps killing ideas in this space?

6. **The Unexplored** (from original brainstorm)
   - Gems we didn't get to assay
   - Wild cards worth revisiting
   - Adjacent brainstorm ideas that weren't picked

7. **Meta-Insights**
   - What the prospecting trail reveals about this domain
   - Themes across the brainstorm, deaths, and survivors
   - What kind of idea survives in this space (the "shape" of a winner)
   - Recommendations for further exploration

### HTML Design System

Use the same design system as /deepthink reports, with these additions:

```css
/* Prospect-specific */
--prospect-gradient: linear-gradient(135deg, #7c6ff7, #22d3ee, #4ade80);

/* Trail visualization */
.trail-node { /* circles connected by lines */ }
.trail-node.dead { border-color: var(--red); }
.trail-node.wounded { border-color: var(--amber); }
.trail-node.survives { border-color: var(--green); }
.trail-pivot { /* dashed line showing the pivot */ }
```

The prospect report should feel like a MAP of an explored territory — you can see
where you went, what you found, and where you ended up.

---

## Final Presentation

After writing all files, present:

```
## Prospect Complete: [Domain]

**Landscape:** [N] ideas brainstormed across [domain]
**Veins assayed:** [N] ideas through full think + red-team
**Pivots:** [N] divergent pivots taken
**Final stake:** [Idea Name] — [SURVIVES / WOUNDED / DEAD]

---

**The Trail:**
[visual trail: Brainstorm → Idea A ✗ → Pivot → Idea B ✓]

**Surviving Idea:** [Idea Name]
**Core Argument:** [3-5 sentences]
**Conviction:** [LOW / MEDIUM / HIGH]

**What We Learned from the Dead Veins:**
- [Idea A] died because [X] — this tells us [Y]
- [Idea B] was wounded by [X] — iterated and survived

---

**Files generated:**
- Brainstorm: `thoughts/prospect/YYYY-MM-DD-<slug>-brainstorm-r1.md`
- [Research, think, red-team files for each vein]
- [Pivot brainstorm files if any]
- **Report: `thoughts/prospect/YYYY-MM-DD-<slug>-report.html`**

Open the HTML report for the full prospecting map.

Want to:
1. Go deeper on the surviving idea with `/deepthink [idea]`?
2. Explore a dead vein's adjacent space further?
3. Run `/brainstorm` on a theme that emerged?
4. Apply a specific thinker framework (`/munger`, `/thiel`, etc.)?
```

---

## Edge Cases

### User provides a specific idea, not a domain

Still brainstorm first. Run the brainstorm on the domain surrounding the idea.
The user's idea becomes one of the candidates. If brainstorming surfaces better
ideas, present them alongside the user's. Let the user choose.

```
You asked about [specific idea]. I brainstormed the broader [domain] space and
found [N] ideas. Your idea appears as Gem #[X]. But the brainstorm also surfaced:

- **[Gem #1]** which scores higher because [reason]
- **[Gem #2]** which is adjacent and avoids [risk your idea has]

Which should I drill into? Your original idea, one of these, or multiple?
```

### All ideas die

If 3 ideas have been assayed and all died:

```
## Prospect: Three Veins Dead

All three ideas I've analyzed in [domain] were killed by red-team.

**The Pattern:** Looking across the deaths, they keep dying because [common theme].
This suggests that in this space, a winning idea needs to [characteristic that
avoids the common kill pattern].

**Options:**
A. One more targeted brainstorm focused on ideas that have [the surviving characteristic]
B. Pivot to an entirely different domain
C. Stop here — the report on WHY ideas die in this space is itself valuable intelligence
```

### User wants to skip brainstorming

If the user says "I already know what I want to analyze, just skip brainstorming":

```
I hear you — but /prospect's value comes from exploring BEFORE committing.
If you already know the idea and just want to analyze it, /deepthink or /think
are better tools. /prospect is for when the best idea hasn't been found yet.

That said, I can:
A. Run a quick 3-agent brainstorm (5 min) to see if there's something better nearby
B. Skip brainstorming and go straight to research + think + red-team (this is /deepthink)
C. Run the full brainstorm (10 min) as designed
```

---

## Quality Standards

- **The pivot is the product.** A /prospect run that goes Brainstorm → Idea A → SURVIVES
  on the first try is fine but not where /prospect shines. It shines when Idea A dies,
  the death reveals something, the pivot finds Idea B, and Idea B is BETTER than what
  the user walked in with. The pivot decision and adjacent brainstorming are the core
  innovation — they must be done well.

- **Dead veins are valuable.** The report should treat dead ideas with as much analytical
  rigor as surviving ones. "This died because of X" is high-value intelligence about
  the domain. The pattern across deaths is often more revealing than any single survival.

- **User involvement at pivot points.** Don't auto-pilot through pivots. Present options,
  explain the reasoning, and let the user choose. Their domain knowledge is additive to
  the analysis.

- **Adjacent brainstorms are FOCUSED.** When pivoting, don't re-run the full 6-agent
  brainstorm. Run 3 focused agents with the full context of what was learned. This
  produces better ideas than starting fresh because it builds on the intelligence
  gathered so far.

- **The trail is the story.** The HTML report should tell the story of the exploration.
  A reader should be able to follow the trail from "we started exploring X" through
  "we tried Y and it died because Z" to "we pivoted to W and it survived because V."
  This narrative is what makes /prospect's output different from just running the
  tools separately.

---

## Cost & Timing Notes

- **Minimum cost:** 1 brainstorm (6 agents) + 1 research (3-4 agents) + 1 think
  (4-7 agents) + 1 red-team (5-7 agents) = ~20 agents if the first idea survives
- **Typical cost with 1 pivot:** ~35 agents across 2 brainstorms + 2 full analyses
- **Maximum cost (3 pivots, 2 iterations):** ~55 agents — this is the most expensive
  skill in the toolkit, but it's doing the most exploratory work
- **Model selection:** Sonnet for all sub-agents, Opus for synthesis and pivot decisions
- **Expected timing:** 20-45 minutes depending on pivots
- **Pair with:** Run `/prospect` to find the idea → `/deepthink` to go even deeper →
  `/munger` or `/thiel` for specialized framework analysis
