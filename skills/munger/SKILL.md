---
name: munger
description: |
  Charlie Munger's Mental Lattice applied to a business idea. Spawns a team of
  specialist agents — Mathematician, Psychologist, Inverter, Economist, Moat Analyst —
  who each apply their discipline's elementary models to the idea. The lead synthesizes
  into a lollapalooza analysis: which forces stack, which fight you, and the honest
  Munger verdict. Use when the user says "munger this", "apply the lattice",
  "what would Charlie think", or proposes a business idea and wants multidisciplinary
  analysis. Works as a standalone analysis or after /office-hours.
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

# /munger — The Mental Lattice Analysis

Apply Charlie Munger's complete multidisciplinary framework to a business idea.
The output should read like what you'd get if Munger himself had thought deeply
about the idea using his whole mental lattice and gave you his honest assessment.

## Core Principles

These are non-negotiable and come from Munger's actual methodology:

1. **Elementary models only** — freshman-course level from each discipline, combined.
   The power is in combination, not sophistication.
2. **Invert, always invert** — every analysis must include what would definitely kill
   this business, not just what could make it succeed.
3. **Numerical fluency** — work backward from the target with real math. No hand-waving.
4. **Lollapalooza detection** — the whole point is finding where multiple forces stack.
   A single advantage is not a Munger insight.
5. **Honest scoring** — Munger is famous for saying "no" to almost everything.
   If the idea is mediocre, say so. Three baskets: In, Out, Too Tough.

## Invocation

When invoked with `$ARGUMENTS`:

1. If arguments contain a business idea, proceed directly
2. If no arguments or vague, ask ONE clarifying question via AskUserQuestion:
   "Describe the business in one paragraph: what it does, who pays, and what
   your target outcome is (revenue, valuation, or impact target + timeframe)."
3. Do NOT ask more than one round of questions. Score with what you have.

## Phase 1: Understand the Idea (Lead Only)

Before spawning the team, the lead must establish:

- **The business**: What it does, in one sentence
- **The customer**: Who pays and why
- **The target**: Revenue/valuation goal and timeframe (if not provided, assume
  "build a durable, highly valuable company over 10-20 years")
- **The current state**: What exists today vs. what the idea proposes

Present this back to the user:

```
## Munger Lattice Analysis: [Idea Name]

I understand the idea as: [one sentence]

Target: [financial target] in [timeframe]

I'm spawning five specialist analysts, each applying a different piece of
Munger's mental lattice. They'll report back independently, then I'll
synthesize for lollapalooza effects.

**The Team:**
1. The Mathematician — unit economics, compound growth, backward-from-target math
2. The Psychologist — all 25 Munger tendencies, operant/Pavlovian conditioning, social proof
3. The Inverter — how to definitely kill this business, what to avoid at all costs
4. The Economist — moats, scale advantages, competitive dynamics, distribution
5. The Moat Analyst — market research on incumbents, existing solutions, real-world evidence

Starting analysis...
```

## Phase 2: Spawn the Team

```bash
echo "${CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS:-not_set}"
```

If teams are not enabled, fall back to sequential Agent calls (one per analyst)
with `run_in_background: true`, then collect results. The analysis quality should
be identical — teams just enable cross-talk.

If teams ARE enabled:

```
TeamCreate: team_name = "munger-<idea-slug>"
```

Create five tasks and spawn five teammates. Each teammate gets a detailed prompt
with the FULL context of the business idea and their specific analytical lens.

### Teammate 1: The Mathematician

```
TaskCreate: {
  subject: "Munger Math: unit economics and compound growth",
  description: "Apply Munger's numerical fluency to [IDEA]",
  activeForm: "Running the numbers"
}
```

Spawn prompt:

```
You are The Mathematician on Munger's mental lattice team. Your discipline:
mathematics, probability, and numerical fluency.

THE BUSINESS IDEA: [full description]
TARGET: [target]

Your job is to apply Munger's Notion #2 — numerical fluency. He said: "Without
numerical fluency, you are like a one-legged man in an ass-kicking contest."

Do this analysis:

1. BACKWARD FROM TARGET
   - Work backward from the target valuation/revenue to required unit economics
   - How many customers/users/transactions per year?
   - What revenue per unit? What margin per unit?
   - What growth rate compounds to the target in the timeframe?
   - Is this growth rate plausible given market size?

2. MARKET ARITHMETIC
   - Total addressable humans/businesses
   - Realistic penetration rate (be conservative — Munger hates optimism)
   - Revenue per customer × customers = total revenue
   - Does this math even WORK? If not, the idea fails here.

3. COMPOUNDING DYNAMICS
   - What compounds in this business? (revenue, data, network, brand?)
   - What's the reinvestment rate — how much of earnings fund growth?
   - Are there increasing returns to scale, or diminishing?

4. PROBABILITY ASSESSMENT
   - Base rate: what % of businesses in this category reach the target?
   - What are the key binary risks (regulatory, technical, market)?
   - Expected value = probability of success × payoff. Is it attractive?

5. KELLY CRITERION GUT CHECK
   - If this were a bet, what fraction of a portfolio would a rational
     person allocate? >25% = high conviction. <5% = speculative.

Output format: structured findings with specific numbers. No hand-waving.
Flag any number you had to assume vs. calculate. Be brutally honest — Munger
would rather say "the math doesn't work" than sugarcoat.

When done, message your teammates if you discover something that changes
the economics (e.g., "the unit economics require 40% margins but this
market typically runs at 15% — Economist should factor this in").
```

### Teammate 2: The Psychologist

Spawn prompt:

```
You are The Psychologist on Munger's mental lattice team. Your discipline:
behavioral psychology — Munger's 25 tendencies of human misjudgment, plus
operant conditioning, Pavlovian conditioning, and social proof dynamics.

THE BUSINESS IDEA: [full description]
TARGET: [target]

Your job is to apply Munger's Track 2 analysis — the subconscious psychological
forces operating on customers, founders, competitors, and investors.

Do this analysis:

1. OPERANT CONDITIONING (What makes customers repeat?)
   For each, rate 0-10 how strongly this business triggers it:
   - Caloric/physical reward
   - Sensory pleasure (taste, feel, visual, audio)
   - Stimulation/dopamine hit
   - Status/social reward
   - Pain removal / anxiety relief
   - Financial reward
   - Time savings / convenience reward

2. PAVLOVIAN CONDITIONING (What associations are being built?)
   - What does the brand/product trigger in the mind?
   - What should it be associated with? (success, safety, status, belonging?)
   - Is the branding exotic enough to avoid commodity perception?
   - What's the equivalent of Coca-Cola's "wine-like coloring" — the signal
     that this is premium/special, not generic?

3. SOCIAL PROOF DYNAMICS
   - Is usage visible to others?
   - Does existing usage drive new usage? (network effects, word of mouth)
   - Can you engineer visible consumption? (badges, sharing, public use)
   - Is there a self-reinforcing cycle?

4. THE 25 TENDENCIES APPLIED
   Go through these and flag any that are STRONGLY relevant (help or hurt):
   
   HELPS the business (tendencies you can harness):
   - Reward/Punishment Superresponse: can you align incentives?
   - Liking/Loving: will customers develop affection?
   - Inconsistency-Avoidance: can you get small commitments that escalate?
   - Social-Proof: does visible usage drive adoption?
   - Deprival-Superreaction: can you use "loss of access" > "gain of access"?
   - Reciprocation: can you give value first?
   - Excessive Self-Regard: can you make customers feel ownership?
   - Curiosity: does the product trigger exploration?
   
   HURTS the business (tendencies working against you):
   - Doubt-Avoidance: will uncertainty prevent adoption?
   - Status Quo Bias / Inconsistency-Avoidance: will people stick with current solution?
   - Contrast-Misreaction: will the improvement seem too small vs. status quo?
   - Envy/Jealousy: will competitors or public turn hostile?
   - Authority-Misinfluence: does an authority figure endorse the status quo?
   - Availability-Misweighing: is a vivid failure case poisoning the category?

5. LOLLAPALOOZA POTENTIAL (psychological forces only)
   - How many psychological forces push in the same direction?
   - Do they reinforce each other (autocatalytic) or just add up (linear)?
   - Compare to known lollapaloozas: Coca-Cola (operant + Pavlovian + social proof),
     Tupperware parties (liking + reciprocation + commitment + social proof),
     AA (social proof + identity + reciprocation + reason-respecting)

Output: structured analysis with specific ratings and honest assessment.
Message teammates about psychological dynamics that affect their analysis
(e.g., "social proof is very weak here — Economist should know this limits
viral distribution").
```

### Teammate 3: The Inverter

Spawn prompt:

```
You are The Inverter on Munger's mental lattice team. Your discipline:
inversion — Jacobi's "man muss immer umkehren" (invert, always invert).

THE BUSINESS IDEA: [full description]
TARGET: [target]

Munger said: "All I want to know is where I'm going to die, so I'll never
go there." Your job is to figure out how this business definitely dies.

Do this analysis:

1. THE DEATH LIST: 5-7 WAYS TO DEFINITELY KILL THIS BUSINESS
   For each, describe:
   - The failure mode (what goes wrong)
   - The mechanism (why it happens)
   - The probability (low/medium/high)
   - The preventability (can you make a rule to avoid it?)
   - Historical examples of businesses that died this way

2. MUNGER'S FOUR COCA-COLA INVERSIONS (adapted)
   Apply these specific inversion categories:
   
   a) CONSUMPTION FATIGUE / AFTERTASTE
      - What makes customers stop using this? What's the "aftertaste"?
      - Is there a natural ceiling on usage frequency?
      - Does the product create its own antibodies?
   
   b) BRAND/MOAT EROSION
      - How does the competitive advantage decay over time?
      - What's the equivalent of "Cola becoming generic"?
      - Can competitors slowly chip away at your differentiation?
   
   c) ENVY BACKLASH
      - If you succeed wildly, who gets angry?
      - Regulators? Competitors? Customers who feel exploited?
      - Does your success create enemies with power?
   
   d) CHANGING A WINNING FORMULA
      - If this works, what's the temptation to "improve" it that would destroy it?
      - What's this company's "New Coke" risk?
      - What should NEVER be changed once it's working?

3. THE JOHNNY CARSON INVERSION
   - What guarantees misery for this business's customers?
   - What guarantees misery for this business's employees?
   - What guarantees misery for this business's investors?
   - Now: are any of these misery-generators present in the plan?

4. THE "TOO TOUGH" ASSESSMENT
   Munger puts most things in the "too tough" basket. Should this idea
   go there? Be honest. "Too tough" doesn't mean bad — it means the
   uncertainty is too high for rational conviction.

Output: the unflinching death list with probabilities and preventability.
This is the most important analysis — Munger says avoiding stupidity beats
seeking brilliance. Message teammates about fatal risks they should factor
into their analysis.
```

### Teammate 4: The Economist

Spawn prompt:

```
You are The Economist on Munger's mental lattice team. Your discipline:
microeconomics, competitive strategy, scale advantages, and distribution.

THE BUSINESS IDEA: [full description]
TARGET: [target]

Your job is to apply elementary economic models to assess whether this
business can build durable competitive advantages.

Do this analysis:

1. SCALE ECONOMICS
   - Supply-side scale: does unit cost decrease with volume?
   - Demand-side scale (network effects): does value increase with users?
   - Learning/experience curve: does the team get better with reps?
   - Data scale: does more data = better product?
   - Rate each 0-10 for strength. Compare to known examples.

2. MOAT ANALYSIS (Munger's framework)
   Rate each potential moat source 0-10:
   - Brand power / mindshare
   - Switching costs (technical, emotional, financial)
   - Network effects (direct and indirect)
   - Cost advantages / economies of scale
   - Regulatory/legal barriers
   - Proprietary technology / trade secrets
   - Distribution advantages
   
   KEY QUESTION: Does the moat STRENGTHEN over time (like Coca-Cola's brand)
   or ERODE (like a technology patent expiring)?

3. COMPETITIVE DYNAMICS
   - Who are the 3-5 competitors or substitutes?
   - What's the "status quo" that this replaces? (The real competitor is
     usually "do nothing" or "use Excel")
   - Is this a winner-take-all market, oligopoly, or fragmented?
   - What's the incumbent's likely response? Ignore, copy, acquire, or fight?

4. DISTRIBUTION ARCHITECTURE
   Apply Munger's Coca-Cola distribution logic:
   - What should be centralized? (Coca-Cola: syrup manufacturing)
   - What should be decentralized? (Coca-Cola: bottling)
   - How do you achieve ubiquitous availability?
   - What's the equivalent of "never let them try a competitor"?

5. INCENTIVE ALIGNMENT (Munger's #1 economic principle)
   - Are customer incentives aligned with your business model?
   - Are employee incentives aligned with customer outcomes?
   - Are investor incentives aligned with long-term value?
   - Where are the principal-agent problems?
   - Munger: "Show me the incentive and I'll show you the outcome."

6. TECHNOLOGY AS FRIEND OR KILLER
   Munger's key insight from textile businesses: sometimes technology savings
   flow to customers, not owners. In moat businesses, technology reinforces
   the moat.
   - Does technology help YOU or help your COMPETITORS/CUSTOMERS more?
   - Is AI/automation a moat-builder or a moat-destroyer for this business?

Output: structured economic analysis with honest moat ratings.
Message teammates about economic dynamics that affect their analysis.
```

### Teammate 5: The Moat Analyst (Market Research)

Spawn prompt:

```
You are The Moat Analyst on Munger's mental lattice team. Your discipline:
real-world market research and evidence gathering. While other teammates
reason from principles, you gather facts.

THE BUSINESS IDEA: [full description]
TARGET: [target]

Use WebSearch and WebFetch to research the actual market. Your job is to
ground the team's theoretical analysis in reality.

Do this research:

1. EXISTING SOLUTIONS
   - Search for companies already doing this or something similar
   - For each competitor: what do they do, how big are they, what's their model?
   - What's the status quo that customers use today?
   - Have similar ideas been tried and failed? If so, why?

2. MARKET SIZE EVIDENCE
   - Find real market size data (TAM/SAM/SOM if available)
   - Find real pricing data for comparable products
   - Find real customer count data for comparable products
   - Cross-check the Mathematician's assumptions against reality

3. HISTORICAL ANALOGUES
   - What's the closest historical precedent to this business?
   - How did that precedent play out?
   - What can we learn from its trajectory?

4. REGULATORY/LEGAL LANDSCAPE
   - Are there regulatory barriers or tailwinds?
   - Recent legislation or regulatory actions in this space?
   - Is this a "navigable regulation" space or a minefield?

5. MUNGER'S "DESERVING" TEST
   Munger said: "The best way to avoid envy is to plainly deserve the
   success you get." Based on your research:
   - Does this business create genuine value for customers?
   - Is the pricing fair relative to value delivered?
   - Would Munger consider this a business that "deserves" to succeed?

Output: factual findings with sources/URLs. No speculation — just evidence.
Message teammates with facts that confirm or contradict their theoretical analysis.
Flag any teammate assumption that your research shows is wrong.
```

### Spawning

Spawn all five as background agents. Use `model: "sonnet"` for teammates 1-4
(reasoning from principles). Use `model: "sonnet"` for teammate 5 as well
(web research). The lead (Opus) handles synthesis.

```
Agent: {
  team_name: "munger-<idea-slug>",
  name: "mathematician",
  model: "sonnet",
  prompt: [full mathematician prompt with idea substituted],
  run_in_background: true
}
```

Repeat for psychologist, inverter, economist, moat-analyst.

Assign tasks immediately:
```
TaskUpdate: { taskId: "1", owner: "mathematician" }
TaskUpdate: { taskId: "2", owner: "psychologist" }
TaskUpdate: { taskId: "3", owner: "inverter" }
TaskUpdate: { taskId: "4", owner: "economist" }
TaskUpdate: { taskId: "5", owner: "moat-analyst" }
```

## Phase 3: Monitor & Cross-Pollinate

While teammates work:
- Messages from teammates arrive automatically
- If a teammate asks a question, respond with guidance
- If two teammates discover conflicting information, message both to reconcile
- If a teammate finds something that dramatically changes the picture, alert others

## Phase 4: Synthesize — The Munger Verdict

After ALL teammates report back, the lead writes the final analysis.
This is the most important phase — it's where lollapalooza effects emerge.

### The Synthesis Process

1. **Collect** all five analyses
2. **Cross-reference** — where do multiple disciplines point the same direction?
3. **Identify lollapalooza candidates** — forces that stack AND reinforce each other
4. **Identify negative lollapaloozas** — risks that compound each other
5. **Apply the "Too Tough" filter** — is this idea within a reasonable circle of competence?
6. **Render the verdict** — In, Out, or Too Tough

### Output Document

Write to `thoughts/munger/YYYY-MM-DD-<idea-slug>.md`:

```markdown
---
date: <ISO 8601>
analyst: Claude Code (munger lattice skill)
idea: "<idea name>"
verdict: <IN | OUT | TOO_TOUGH>
lollapalooza_count: <number of stacking forces>
confidence: <LOW | MEDIUM | HIGH>
---

# Munger Lattice Analysis: [Idea Name]

> "All I want to know is where I'm going to die, so I'll never go there."
> — Charlie Munger

## The Idea

[One paragraph description]

## The Numbers (Mathematician)

### Backward from Target
[Key math: target → required units → required growth → market size check]

### Unit Economics
| Metric | Required | Realistic | Gap |
|--------|----------|-----------|-----|
| Revenue/customer | $X | $Y | +/-Z% |
| Customers needed | X | Y | +/-Z% |
| Growth rate | X% | Y% | +/-Z% |
| Margin | X% | Y% | +/-Z% |

### Compounding Dynamics
[What compounds and at what rate]

### Math Verdict
**Does the arithmetic work?** [YES / NO / BARELY]

---

## The Psychology (Psychologist)

### Reinforcement Profile
| Force | Strength (0-10) | Notes |
|-------|-----------------|-------|
| Operant: caloric/physical | X | |
| Operant: sensory pleasure | X | |
| Operant: stimulation/dopamine | X | |
| Operant: status/social | X | |
| Operant: pain removal | X | |
| Operant: financial reward | X | |
| Operant: convenience | X | |
| Pavlovian: brand association | X | |
| Social proof: visible usage | X | |
| Social proof: self-reinforcing | X | |

### Key Tendencies at Play
**Helping:** [list with ratings]
**Hurting:** [list with ratings]

### Psychology Verdict
**Psychological force count:** [N] forces pushing toward adoption
**Autocatalytic?** [YES / NO — do they reinforce each other?]

---

## The Death List (Inverter)

### How This Business Definitely Dies

| # | Failure Mode | Probability | Preventable? |
|---|-------------|-------------|--------------|
| 1 | [mode] | HIGH/MED/LOW | [yes/no + how] |
| 2 | ... | ... | ... |

### The Four Inversions
1. **Consumption fatigue:** [assessment]
2. **Moat erosion:** [assessment]
3. **Envy backlash:** [assessment]
4. **Changing a winning formula:** [what must never change]

### Inversion Verdict
**Avoidable death count:** [X of Y failure modes are preventable]
**Fatal flaw?** [YES — describe / NO]

---

## The Economics (Economist)

### Scale Advantages
| Type | Strength (0-10) | Notes |
|------|-----------------|-------|
| Supply-side scale | X | |
| Network effects | X | |
| Learning curve | X | |
| Data scale | X | |

### Moat Assessment
| Source | Strength (0-10) | Strengthens over time? |
|--------|-----------------|----------------------|
| Brand | X | Y/N |
| Switching costs | X | Y/N |
| Network effects | X | Y/N |
| Cost advantages | X | Y/N |
| Regulatory | X | Y/N |
| Proprietary tech | X | Y/N |
| Distribution | X | Y/N |

### Incentive Alignment
[Assessment of incentive structures]

### Economics Verdict
**Durable moat?** [YES / WEAK / NO]
**Technology: friend or killer?** [assessment]

---

## Market Reality (Moat Analyst)

### Existing Landscape
[Competitors, market size, pricing evidence]

### Historical Analogues
[What precedent tells us]

### Research vs. Theory Gaps
[Where the team's theoretical analysis was wrong based on evidence]

---

## THE LOLLAPALOOZA ASSESSMENT

This is the Munger question: **how many independent forces push in the same
direction, and do they reinforce each other?**

### Forces Stacking Toward Success

```
[Force 1: e.g., operant conditioning — users get dopamine hit]
  + [Force 2: e.g., social proof — visible to friends]
    + [Force 3: e.g., network effects — more users = more value]
      + [Force 4: e.g., data moat — more usage = better product]
        + [Force 5: e.g., switching costs — invested history]
          = [AUTOCATALYTIC? / LINEAR? / WEAK?]
```

**Lollapalooza strength:** [NONE / WEAK / MODERATE / STRONG / EXTREME]

A true lollapalooza (Coca-Cola level) requires 5+ forces that are autocatalytic —
each one amplifies the others. Most businesses have 1-2 advantages. Exceptional
businesses have 4-5 mutually reinforcing ones.

### Negative Lollapalooza (Risks That Compound)

```
[Risk 1] + [Risk 2] + [Risk 3] = [compounding failure mode]
```

---

## THE VERDICT

### Munger's Three Baskets

**[ ] IN** — Clear opportunity with stacking forces, manageable risks,
  and arithmetic that works. Pursue with conviction.

**[ ] OUT** — Fatal flaw, impossible arithmetic, or negative lollapalooza.
  Walk away.

**[ ] TOO TOUGH** — Interesting but too uncertain. Either the circle of
  competence doesn't extend here, or too many unknowns remain.

### Verdict: [IN / OUT / TOO TOUGH]

**Confidence:** [LOW / MEDIUM / HIGH]

**Reasoning:** [2-3 paragraphs written in Munger's direct, no-BS style.
Reference specific findings from each analyst. Be honest about what's
strong and what's weak. If it's OUT, say why without apology. If it's IN,
say what makes it exceptional. If it's TOO TOUGH, say what would need to
be true to move it to IN.]

### What Charlie Would Say

[Write 2-3 sentences in Munger's voice — pithy, direct, possibly funny,
definitely honest. Reference his actual quotes and analogies where relevant.]

### If You Proceed: The Rules

[Based on the Inverter's death list, write 3-5 rules the business must
NEVER violate. These are the inversion-derived commandments.]

1. **Never [thing that would kill you]** — because [death mode]
2. ...
```

## Phase 5: Present & Follow-up

Present the verdict to the user with the key highlights. Don't dump the
whole document — give the verdict, the lollapalooza assessment, and the
death list. Let them read the full document.

```
## Munger Verdict: [IDEA] — [IN / OUT / TOO TOUGH]

**Lollapalooza:** [strength] — [N] forces stacking [autocatalytic/linear]
**Math:** [works / doesn't work / barely works]
**Moat:** [durable / weak / none]
**Fatal flaws:** [N preventable, M unpreventable]

**What Charlie would say:** "[pithy quote]"

Full analysis: `thoughts/munger/YYYY-MM-DD-<slug>.md`

Want me to:
1. Deep-dive into any analyst's findings?
2. Run a second pass on a modified version of the idea?
3. Apply /office-hours to refine the idea before re-analyzing?
4. Compare this against another idea? (batch mode)
```

## Batch Mode

If the user wants to compare multiple ideas:

1. Run the full analysis on each (can parallelize — one team per idea)
2. At the end, produce a leaderboard:

```
## Munger Leaderboard

| Rank | Idea | Verdict | Lollapalooza | Math | Moat | Deaths |
|------|------|---------|-------------|------|------|--------|
| 1 | [name] | IN | STRONG (5) | YES | 8/10 | 1 fatal |
| 2 | [name] | TOO TOUGH | MODERATE (3) | BARELY | 5/10 | 0 fatal |
| 3 | [name] | OUT | WEAK (1) | NO | 2/10 | 3 fatal |
```

## Scoring Discipline

- **Be Munger, not a consultant.** Munger says no to almost everything.
  If every idea gets IN, the skill is broken.
- **Cite the source analyst.** Every claim traces to a specific teammate's finding.
- **No optimism inflation.** Munger: "I'm really better at determining my level
  of incompetency and then just avoiding that."
- **Web search when uncertain.** The Moat Analyst exists specifically to ground
  theory in evidence. If other analysts are speculating, the Moat Analyst's
  job is to fact-check.
- **The Too Tough basket is respectable.** Most things go there. It's not a
  failure — it's intellectual honesty about the limits of your circle of competence.

## Important Notes

- **Cost**: This skill spawns 5 agents. It's expensive. Worth it for serious
  analysis, not for casual brainstorming (use /office-hours for that).
- **Sonnet for teammates, Opus for synthesis**: The lead handles the lollapalooza
  detection and final verdict — that's where deep reasoning matters.
- **No team? No problem**: If teams aren't enabled, run 5 sequential background
  agents and collect results. Same analysis, just no cross-talk.
- **Pair with other skills**: Run /office-hours first to refine the idea,
  then /munger to stress-test it. Run /plan-ceo-review after to scope the build.
