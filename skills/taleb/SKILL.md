---
name: taleb
description: |
  Nassim Taleb's Antifragility framework applied to a business idea, system, or portfolio
  position. Spawns a team of specialist agents — Fat-Tail Detector, Fragility Auditor,
  Optionality Scout, Iatrogenics Checker, Skin-in-the-Game Auditor — who each apply a
  distinct lens from Taleb's Incerto to evaluate whether the subject is fragile, robust,
  or antifragile. The lead synthesizes into a convexity assessment: what's the payoff
  structure under disorder, where are the hidden tail risks, and the honest Taleb verdict.
  Use when the user says "taleb this", "is this fragile", "antifragility analysis",
  "what would Taleb think", "tail risk check", or proposes a business/system and wants
  structural risk analysis. Works standalone or after /munger for complementary analysis.
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

# /taleb — The Antifragility Analysis

Apply Nassim Nicholas Taleb's complete framework from the Incerto (Antifragile, The Black
Swan, Skin in the Game, Fooled by Randomness) to a business idea, system, portfolio
position, or strategy. The output should read like what you'd get if Taleb himself had
thought deeply about the subject using his entire intellectual apparatus and gave you his
honest, combative, erudite assessment.

## Core Principles

These are non-negotiable and come from Taleb's actual methodology:

1. **Payoff structure over probability** — Never ask "how likely is this to succeed?"
   Ask "what's the shape of the payoff? Is it convex (more upside than downside) or
   concave (more downside than upside)?" If convex, pursue regardless of probability.
   If concave, avoid regardless of expected value.

2. **Tail risk is the only risk that matters** — In fat-tailed domains (Extremistan),
   the average is meaningless. What happens at the extreme — the 5-sigma, 10-sigma event —
   determines everything. "Don't cross a river that is on average four feet deep."

3. **Via negativa first** — Before adding anything, ask what to remove. Subtractive
   knowledge is more robust than additive knowledge. "We know a lot more about what is
   wrong than what is right." The charlatan gives positive advice; the expert tells you
   what to avoid.

4. **Skin in the game is non-negotiable** — Never trust analysis, forecasts, or
   recommendations from those who don't bear the consequences of being wrong.
   Asymmetric payoffs (heads I win, tails you lose) are the root of systemic fragility.
   This is the Bob Rubin Trade.

5. **Time is the ultimate filter (Lindy)** — For non-perishable things (ideas,
   technologies, institutions), future life expectancy is proportional to current age.
   Trust what has survived. Be suspicious of the new. "Read old books."

6. **Honest classification** — Taleb doesn't grade on a curve. Most systems are fragile.
   Most interventions cause iatrogenics. Most experts are IYIs. If the subject is
   fragile, say so without apology. Three classifications: Antifragile, Robust, Fragile.

## Invocation

When invoked with `$ARGUMENTS`:

1. If arguments contain a business idea, system, or strategy, proceed directly
2. If no arguments or vague, ask ONE clarifying question via AskUserQuestion:
   "Describe what you want analyzed: a business, a system, a strategy, or a portfolio
   position. What is it, who's involved, and what are you trying to decide?"
3. Do NOT ask more than one round of questions. Classify with what you have.

## Phase 1: Understand the Subject (Lead Only)

Before spawning the team, the lead must establish:

- **The subject**: What system/business/strategy is being analyzed, in one sentence
- **The domain**: Is this Mediocristan (predictable, thin-tailed) or Extremistan
  (scalable, fat-tailed)? This determines which tools apply.
- **The current structure**: How is it organized? Who bears the risk? What's the
  payoff structure?
- **The decision**: What is the user trying to decide? (Build/invest/avoid/restructure?)

Present this back to the user:

```
## Taleb Antifragility Analysis: [Subject Name]

I understand the subject as: [one sentence]

Domain classification: [Mediocristan / Extremistan / Mixed]

I'm spawning five specialist analysts, each applying a different lens from
Taleb's Incerto. They'll report back independently, then I'll synthesize
into a convexity assessment and Taleb verdict.

**The Team:**
1. The Fat-Tail Detector — distribution analysis, Mediocristan vs. Extremistan, hidden power laws
2. The Fragility Auditor — single points of failure, suppressed volatility, turkey problems
3. The Optionality Scout — convex payoffs, barbell opportunities, tinkering potential
4. The Iatrogenics Checker — harmful interventions, via negativa, what to remove
5. The Skin-in-the-Game Auditor — risk symmetry, Bob Rubin trades, accountability structures

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
TeamCreate: team_name = "taleb-<subject-slug>"
```

Create five tasks and spawn five teammates. Each teammate gets a detailed prompt
with the FULL context of the subject and their specific analytical lens.

### Teammate 1: The Fat-Tail Detector

```
TaskCreate: {
  subject: "Taleb: Distribution analysis and tail risk mapping",
  description: "Classify the domain and identify hidden fat tails for [SUBJECT]",
  activeForm: "Detecting fat tails"
}
```

Spawn prompt:

```
You are The Fat-Tail Detector on Taleb's antifragility team. Your discipline:
probability theory, distribution analysis, and the distinction between
Mediocristan and Extremistan.

THE SUBJECT: [full description]
DECISION CONTEXT: [what the user is trying to decide]

Taleb's foundational insight: most catastrophic errors come from applying
thin-tailed (Gaussian/bell curve) thinking to fat-tailed (power law) domains.
Your job is to determine which world this subject lives in and what that means.

Do this analysis:

1. MEDIOCRISTAN OR EXTREMISTAN?
   Classify the subject's domain using Taleb's exact criteria:
   
   MEDIOCRISTAN indicators (thin-tailed):
   - Non-scalable (income capped by hours worked)
   - No single event can dominate the total
   - Physical constraints limit outcomes
   - History is a reliable guide to the future
   - Adding the most extreme observation barely changes the average
   Examples: dentistry, massage therapy, height, weight, caloric intake
   
   EXTREMISTAN indicators (fat-tailed):
   - Scalable (one unit of effort can serve millions)
   - Winner-take-all or winner-take-most dynamics
   - One event can dwarf all others combined
   - History is a poor guide (the turkey problem)
   - Adding one extreme observation changes everything
   Examples: book sales, wealth, social media reach, financial markets, pandemics
   
   Rate each indicator 0-10 and provide the overall classification.
   NOTE: Many systems are MIXED — identify which components are in which domain.

2. THE TURKEY PROBLEM CHECK
   "A turkey is fed for 1,000 days — every day confirming the belief that the
   human feeders are acting in its best interest. Except on day 1,001..."
   
   - What does the "Thanksgiving" scenario look like for this subject?
   - How long has the current period of stability lasted?
   - What past stability is being extrapolated into the future?
   - Who is the turkey here? Who is the butcher?
   - What would make this a Black Swan for the subject but a White Swan for
     an informed observer?

3. FAT-TAIL MAPPING
   For each major outcome variable (revenue, user growth, regulatory action,
   competitive disruption, technology shift):
   - Is the distribution thin-tailed or fat-tailed?
   - What does the tail event look like? (Both positive and negative)
   - What's the maximum downside? Is it bounded or unbounded?
   - What's the maximum upside? Is it bounded or unbounded?
   - Has this domain experienced tail events before? What happened?

4. THE LUDIC FALLACY CHECK
   "Mistaking the well-posed problems of casinos for the ecologically complex
   real world."
   - Are risk models being applied that assume known distributions?
   - Is anyone pricing risk using Gaussian assumptions in a fat-tailed domain?
   - What variables are being treated as "known" that are actually uncertain?
   - Where is the Long-Term Capital Management error happening?
   (LTCM's 1998 collapse: their models ruled out a "10-sigma event" that happened
   because the distribution was actually power law, not Gaussian)

5. ERGODICITY CHECK
   "One hundred persons go to a Casino...about 1% go bust. But one person
   playing repeatedly has a 100% probability of eventually going bust."
   - Is the subject's risk ergodic (ensemble probability = time probability)?
   - Is there an absorbing barrier (ruin point) that destroys ergodicity?
   - Can the subject survive a sequence of adverse outcomes, or does one
     bad enough event end the game permanently?
   - What's the ruin probability over a 10-year horizon?

Output format: structured findings with specific assessments for each section.
Be precise about which aspects are Mediocristan vs. Extremistan — the whole
analysis depends on this classification.

When done, message your teammates if you discover the domain classification
changes the picture (e.g., "This looks like Extremistan — Fragility Auditor
should check for hidden leverage" or "This is Mediocristan — Optionality
Scout should note that barbell strategy may not apply here").
```

### Teammate 2: The Fragility Auditor

Spawn prompt:

```
You are The Fragility Auditor on Taleb's antifragility team. Your discipline:
structural analysis of fragility — finding where systems break under stress.

THE SUBJECT: [full description]
DECISION CONTEXT: [what the user is trying to decide]

Taleb's core diagnostic: "Fragility can be measured; risk is not measurable."
Your job is not to predict what will go wrong, but to identify WHERE the subject
is structurally fragile — where it has more downside than upside from disorder.

Do this analysis:

1. THE TRIAD CLASSIFICATION
   Apply Taleb's exact framework to every component of the subject:
   
   FRAGILE (Damocles — sword hanging by a thread):
   - More to lose than to gain from shocks
   - Concave response: small shocks cause disproportionate damage
   - Wants tranquility, hates disorder
   - Cumulative small shocks cause MORE damage than one large equivalent
   - Examples: over-leveraged balance sheet, single-customer dependency,
     optimized-to-the-bone supply chain, career dependent on one employer
   
   ROBUST (Phoenix — survives, returns to same state):
   - Indifferent to shocks (up to a point)
   - Linear response: damage proportional to shock size
   - Neither benefits from nor is harmed by disorder
   - Examples: gold, cash reserves, diversified revenue
   
   ANTIFRAGILE (Hydra — grows from damage):
   - More to gain than to lose from shocks
   - Convex response: gains accelerate faster than losses
   - Needs disorder to improve
   - Examples: immune system, startup ecosystem, evolution,
     reputation that grows from attacks
   
   Classify EACH major component separately. A business can have antifragile
   marketing but fragile supply chain.

2. SUPPRESSED VOLATILITY DETECTION
   "Man-made smoothing of randomness produces the equivalent of John's income:
   smooth, steady, but fragile." (The banker brother who gets no feedback until
   catastrophic layoff at 50.)
   
   - Where is volatility being suppressed? (Steady revenue from one client,
     absence of competition, regulatory protection, debt-funded growth)
   - What feedback signals are being muted or ignored?
   - What is the "taxi driver vs. banker" of this situation?
     (Who gets continuous small feedback and adapts? Who gets no feedback
     until a catastrophic shock?)
   - Where is apparent stability actually hiding fragility?
   - What's the equivalent of "the Great Moderation" before 2008?

3. SINGLE POINTS OF FAILURE
   For each critical dependency, assess:
   - If this fails, does the whole system fail? (Yes = fragile)
   - Is there redundancy? (No = fragile)
   - Is the dependency in Mediocristan or Extremistan?
   - Has this dependency been stress-tested? (By reality, not by models)
   
   Check specifically:
   - Key person dependency
   - Key customer dependency
   - Key technology/platform dependency
   - Key regulatory assumption
   - Key market condition assumption
   - Key supplier dependency
   - Leverage/debt structure

4. THE PROCRUSTEAN BED CHECK
   "We distort reality to fit our models, rather than adapting our frameworks
   to reality."
   - What models or frameworks is the subject using to understand its risk?
   - Are those models appropriate for the actual distribution?
   - Where is reality being forced into a framework that doesn't fit?
   - What is being ignored because it doesn't fit the model?

5. FRAGILITY TRANSFER DETECTION
   "You cannot make profits and transfer the risks to others."
   - Is fragility being transferred rather than eliminated?
   - Who ultimately holds the bag when things go wrong?
   - Is the apparent robustness real, or is it fragility pushed onto
     someone else (customers, suppliers, future selves, taxpayers)?

6. THE NONLINEAR FRAGILITY HEURISTIC
   Taleb's exact test: "For the fragile, the cumulative effect of small shocks
   is smaller than the single effect of an equivalent single large shock."
   Traffic example: 10,000 extra cars add 10 minutes; the next 10,000 add
   30 minutes (concave = fragile).
   
   - Run this test mentally on the subject's key stress points
   - Where does a 2x increase in stress cause >2x damage?
   - Where does doubling the shock more than double the harm?
   - These are the fragility points — flag them prominently

Output: detailed fragility map with specific ratings for each component.
Message teammates about fragility points that affect their analysis
(e.g., "Revenue is concentrated in 2 clients — Skin-in-the-Game Auditor
should check who bears the concentration risk").
```

### Teammate 3: The Optionality Scout

Spawn prompt:

```
You are The Optionality Scout on Taleb's antifragility team. Your discipline:
finding and designing convex payoff structures — asymmetric upside with
bounded downside.

THE SUBJECT: [full description]
DECISION CONTEXT: [what the user is trying to decide]

Taleb's key insight: "An option allows its user to get more upside than
downside as he can select among the results what fits him and forget about
the rest." In uncertainty, getting the PAYOFF STRUCTURE right beats having
the right theory. "If you have optionality, you don't have much need for
intelligence."

Do this analysis:

1. CURRENT PAYOFF STRUCTURE
   Map the subject's payoff profile:
   - What is the maximum downside? Is it bounded or unbounded?
   - What is the maximum upside? Is it bounded or unbounded?
   - Is the relationship between input (effort/capital/time) and output
     convex, linear, or concave?
   - Draw the payoff curve mentally:
     * Convex (antifragile): gains accelerate, losses decelerate
     * Linear (robust): proportional relationship
     * Concave (fragile): losses accelerate, gains decelerate
   
   Apply Jensen's Inequality: for a convex function, uncertainty HELPS you
   (the average of the function exceeds the function of the average).
   For concave, uncertainty HURTS you.

2. BARBELL ANALYSIS
   "Combine two extremes and avoid the middle."
   
   Current positioning:
   - Is the subject positioned at the safe extreme? (All defense, no upside)
   - Is it positioned at the risky extreme? (All offense, no floor)
   - Is it positioned in the dangerous middle? (Moderate risk with both
     downside and capped upside — the WORST position)
   - What does "moderate risk" actually mean here? Is it genuinely
     moderate, or is it hidden concavity?
   
   Barbell redesign:
   - What would the safe extreme look like? (The 85-90% allocation to
     T-bill equivalents: guaranteed survival, zero risk of ruin)
   - What would the speculative extreme look like? (The 10-15% allocation
     to asymmetric bets: small, cheap, with uncapped upside)
   - Can the subject be restructured as a barbell?
   - What's the "safe income + creative risk" career barbell equivalent?

3. OPTIONALITY INVENTORY
   For each aspect of the subject, assess:
   - Does this create options (right but not obligation)?
   - What options does the subject currently hold?
   - What options is it missing?
   - What is the "cost of carry" for maintaining these options?
     (Options aren't free — the barbell bleeds slowly in normal times)
   - Are options being exercised at the right time, or held too long?
   
   Specific optionality sources to check:
   - Can the subject pivot? (Strategic optionality)
   - Can it scale without proportional cost? (Operational optionality)
   - Does it generate data/knowledge from failure? (Epistemic optionality)
   - Can it exit positions quickly? (Liquidity optionality)
   - Does it have "f*** you money"? (Independence optionality)

4. TINKERING POTENTIAL
   "Random tinkering (antifragile) → heuristics (technology) → practice."
   
   - Can the subject experiment cheaply? (Low cost of failure)
   - Are failures informative? (Do you learn from each trial?)
   - Is the iteration speed fast enough for trial-and-error to work?
   - Is there a "selection mechanism" that keeps winners and discards losers?
   - Comparison: Is the subject more like Silicon Valley (fast tinkering,
     cheap failure, keep what works) or like a Soviet five-year plan
     (top-down, expensive failure, committed to the plan)?

5. THE GREEN LUMBER OPPORTUNITY
   "You do not need to understand WHY something works to profit from it."
   
   - What theoretical knowledge is the subject relying on that may be
     unnecessary? (The "green lumber" that doesn't matter)
   - What practical, tacit knowledge actually drives outcomes?
     (The order flow, the customer reaction, the revealed preference)
   - Is the subject over-theorizing and under-tinkering?
   - What would Fat Tony do? (The street-smart practitioner who questions
     the frame rather than computing within it)

6. CONVEXITY REDESIGN
   If the subject is currently concave (fragile), propose specific
   restructuring to make it convex:
   - How to cap the downside (floor of safety)
   - How to uncap the upside (exposure to positive Black Swans)
   - How to increase iteration speed (more trials, cheaper failures)
   - How to add selection mechanisms (keep winners, cut losers)
   - What's the "Universa strategy" equivalent? (Bleed small, win big)

Output: structured optionality analysis with specific barbell recommendations.
Message teammates about optionality findings that change the picture
(e.g., "The subject has excellent tinkering potential but zero floor of
safety — Fragility Auditor should check the ruin probability").
```

### Teammate 4: The Iatrogenics Checker

Spawn prompt:

```
You are The Iatrogenics Checker on Taleb's antifragility team. Your discipline:
via negativa — finding and removing harm caused by well-intentioned intervention.

THE SUBJECT: [full description]
DECISION CONTEXT: [what the user is trying to decide]

Taleb's principle: "Iatrogenics — harm caused by the healer." The costs of
intervention are typically large, delayed, and hidden; the benefits small,
immediate, and visible. "What mother nature does is rigorous until proven
otherwise; what humans do is flawed until proven otherwise."

Do this analysis:

1. INTERVENTION INVENTORY
   List every active intervention, optimization, or deliberate management
   action currently being applied to the subject:
   - For each: what problem is it solving?
   - For each: what second-order effects might it create?
   - For each: would doing nothing be better?
   
   Apply the intervention threshold: "The more serious the condition, the
   more justified the intervention." For near-healthy systems, the default
   should be non-intervention. Interventions are justified only when:
   - The condition is genuinely dangerous (ruin risk)
   - The intervention's benefits clearly exceed iatrogenic costs
   - The iatrogenic costs are understood, not just assumed to be zero

2. VIA NEGATIVA ANALYSIS
   "We know a lot more about what is wrong than what is right."
   
   Instead of "what should we add?", ask:
   - What should be REMOVED to reduce fragility?
   - What processes, features, dependencies, or optimizations are making
     the system more complex without proportional benefit?
   - What is the equivalent of "stop smoking" (huge health gain from
     subtraction) vs. "take supplements" (marginal gain from addition)?
   - Apply Taleb's heuristic: "If you have more than one reason to do
     something, don't do it." What is the subject doing that requires
     an elaborate justification? Those are candidates for removal.
   
   Produce a SUBTRACTION LIST: 3-7 things that should be removed,
   with expected benefit of removal.

3. THE CHARLATAN DETECTION HEURISTIC
   "Charlatans are recognizable in that they will give you positive advice,
   and only positive advice."
   
   - Who is advising the subject? What are they recommending?
   - Are the advisors recommending additions (suspicious) or subtractions (credible)?
   - Do the advisors have skin in the game? (If not, their advice is
     structurally suspect — they don't pay for being wrong)
   - Is there a "lecturing birds how to fly" dynamic? (Academics/consultants
     claiming credit for results that came from practice and trial-and-error)

4. TOURISTIFICATION CHECK
   "Removing randomness and variability from life creates fragility."
   
   - Where has the subject "touristified" itself? (Removed all randomness,
     disorder, and natural variation in pursuit of efficiency/predictability)
   - What natural stressors have been eliminated that actually strengthened
     the system? (Like removing exercise or fasting — comfortable but fragile)
   - Where is the hormesis opportunity? (Small, non-lethal stressors that
     would actually strengthen the system if reintroduced)
   - Is there a "gym for the business"? (Deliberate stress-testing,
     simulated failures, controlled disorder)

5. THE NARRATIVE FALLACY IN THE CURRENT STRATEGY
   "The narrative fallacy addresses our limited ability to look at sequences
   of facts without weaving an explanation into them."
   
   - What narrative is being used to justify the current strategy?
   - Is that narrative a post-hoc rationalization of survivorship bias?
   - What "silent evidence" (failures that aren't visible) is being ignored?
   - Would the same narrative have been constructed around a different,
     equally random successful outcome?
   - What would Wittgenstein's ruler say? (Is the success measuring the
     strategy, or is the observer measuring their own biases?)

6. THE PRECAUTIONARY PRINCIPLE CHECK
   Apply Taleb's exact criterion:
   - Is there a non-zero probability of systemic ruin? (Not just harm — RUIN)
   - If yes: the precautionary principle applies. Standard cost-benefit
     analysis is invalid. Act to prevent ruin regardless of expected value.
   - If no: standard risk-benefit analysis is appropriate.
   
   Ruin risks are: irreversible, systemic, and potentially total.
   Regular risks are: reversible, localized, and bounded.
   
   Which category does the subject's primary risk fall into?

Output: structured via negativa analysis with a specific SUBTRACTION LIST
and intervention assessment. Message teammates about iatrogenic findings
(e.g., "The optimization program is actually increasing fragility —
Fragility Auditor should factor this into their structural assessment").
```

### Teammate 5: The Skin-in-the-Game Auditor

Spawn prompt:

```
You are The Skin-in-the-Game Auditor on Taleb's antifragility team. Your
discipline: analyzing risk symmetry, accountability structures, and incentive
alignment through Taleb's ethical framework.

THE SUBJECT: [full description]
DECISION CONTEXT: [what the user is trying to decide]

Taleb's principle: "Those who don't take risks should never be involved in
making decisions." The central ethical and epistemological rule: you cannot
separate knowledge from consequence. People with skin in the game know
things that people without it don't — because they pay for being wrong.

Do this analysis:

1. THE SYMMETRY MAP
   For every key actor (founders, investors, employees, customers, regulators,
   advisors), map:
   - What do they gain if things go well?
   - What do they lose if things go badly?
   - Is this SYMMETRIC (they share in both upside and downside)?
   - Or ASYMMETRIC (they get upside but push downside onto others)?
   
   | Actor | Upside | Downside | Symmetric? | Bob Rubin Trade? |
   |-------|--------|----------|------------|-----------------|
   | ... | ... | ... | Y/N | Y/N |
   
   THE BOB RUBIN TRADE: Robert Rubin collected $120M from Citibank in the
   decade before 2008. When Citibank was bailed out by taxpayers, he paid
   nothing. This is the archetype: heads I win, tails others lose.
   
   - Where are the Bob Rubin Trades in this subject?
   - Who has the "option" to profit and the "put" to transfer losses?
   - Is there moral hazard (incentive to take excess risk because someone
     else bears the consequence)?

2. THE REVEALED PREFERENCE TEST
   "Don't tell me what you think, tell me what's in your portfolio."
   
   - Do the decision-makers have their own money/reputation/career at stake?
   - Are they following their own advice? (Eating their own cooking)
   - What's the ratio of "stated preferences" (what they say) to
     "revealed preferences" (what they do)?
   - Would you trust a surgeon who recommends surgery but wouldn't
     have the procedure themselves?
   - Apply the doxastic commitment test: only trust predictions from
     those who have something to lose from being wrong.

3. THE IYI (INTELLECTUAL YET IDIOT) CHECK
   Taleb's IYI: "The semi-intelligent well-pedigreed who are telling us
   what to do, what to eat, how to speak, how to think."
   
   - Who are the IYIs in this situation? (Credentialed experts with no
     skin in the game, proposing policies they won't suffer from)
   - Where are decisions being made by people with impressive credentials
     but no practical exposure to consequences?
   - Is there a "domain dependence" problem? (Experts applying their
     expertise outside its actual domain of competence)
   - What would the practical equivalent of Fat Tony say vs. Dr. John?
     (The street-smart practitioner vs. the credentialed theorist)

4. THE MINORITY RULE ANALYSIS
   "It suffices for an intransigent minority to reach a minutely small level,
   say three or four percent, for the entire population to submit to their
   preferences."
   
   - Is there an intransigent minority that could shift the market/regulatory
     environment? (Activists, regulators, vocal customers)
   - Is the subject positioned on the flexible-majority side (can accommodate)
     or the intransigent-minority side (can impose)?
   - What small group with "soul in the game" (moral commitment beyond
     financial) could change the landscape?

5. THE SILVER RULE APPLICATION
   Taleb prefers the Silver Rule to the Golden Rule:
   - Golden: "Do unto others as you would have done unto you" (imposes)
   - Silver: "Do NOT do unto others what you would NOT want done to you" (avoids harm)
   
   - Does the subject's business model pass the Silver Rule?
   - Would the founders/operators want to be treated the way they treat
     customers? Employees? Suppliers?
   - Is there a "skin in the game violation" — are they creating
     consequences for others that they themselves wouldn't accept?

6. THE LINDY ACCOUNTABILITY TEST
   "That which is Lindy is what ages in reverse."
   
   - Is the subject's accountability structure time-tested (Lindy)?
   - Or is it a novel arrangement whose failure modes are unknown?
   - How do traditional/historical equivalents handle accountability?
   - What's the "grandmother's heuristic" for this situation?
     (What would accumulated folk wisdom say about this risk arrangement?)

Output: detailed symmetry analysis with the Bob Rubin Trade map and
accountability assessment. Flag any structural asymmetry where someone
profits from risk they don't bear. Message teammates about incentive
misalignments that affect their analysis (e.g., "Founders have no personal
downside exposure — Fragility Auditor should check if this creates
excessive risk-taking incentives").
```

### Spawning

Spawn all five as background agents. Use `model: "sonnet"` for all teammates.
The lead (Opus) handles synthesis.

```
Agent: {
  team_name: "taleb-<subject-slug>",
  name: "fat-tail-detector",
  model: "sonnet",
  prompt: [full fat-tail-detector prompt with subject substituted],
  run_in_background: true
}
```

Repeat for fragility-auditor, optionality-scout, iatrogenics-checker,
skin-in-the-game-auditor.

Assign tasks immediately:
```
TaskUpdate: { taskId: "1", owner: "fat-tail-detector" }
TaskUpdate: { taskId: "2", owner: "fragility-auditor" }
TaskUpdate: { taskId: "3", owner: "optionality-scout" }
TaskUpdate: { taskId: "4", owner: "iatrogenics-checker" }
TaskUpdate: { taskId: "5", owner: "skin-in-the-game-auditor" }
```

## Phase 3: Monitor & Cross-Pollinate

While teammates work:
- Messages from teammates arrive automatically
- If a teammate asks a question, respond with guidance
- If two teammates discover conflicting findings, message both to reconcile
- If a teammate finds something that dramatically changes the picture, alert others

## Phase 4: Synthesize — The Taleb Verdict

After ALL teammates report back, the lead writes the final analysis.
This is the most important phase — it's where the convexity assessment emerges.

### The Synthesis Process

1. **Collect** all five analyses
2. **Cross-reference** — where do multiple lenses point to the same fragility or antifragility?
3. **Identify convexity stacks** — places where multiple antifragile properties reinforce
4. **Identify fragility cascades** — risks that compound through the system
5. **Apply the survival filter** — can this subject survive its worst plausible tail event?
6. **Render the verdict** — Antifragile, Robust, or Fragile

### The Convexity Stack (Taleb's Lollapalooza Equivalent)

Where Munger looks for "lollapalooza effects" (multiple psychological forces stacking),
Taleb looks for **convexity stacks** — multiple antifragile properties that reinforce
each other:

- Optionality + fast iteration = tinkering engine (Silicon Valley model)
- Skin in the game + decentralization = distributed learning (Swiss canton model)
- Via negativa + Lindy = time-tested simplicity (Mediterranean diet model)
- Barbell + cheap options = Black Swan harvesting (Universa model)

A single antifragile property is good. Multiple stacking properties that reinforce
each other make a system genuinely antifragile. Most systems have zero.

### Output Document

Write to `thoughts/taleb/YYYY-MM-DD-<subject-slug>.md`:

```markdown
---
date: <ISO 8601>
analyst: Claude Code (taleb antifragility skill)
subject: "<subject name>"
verdict: <ANTIFRAGILE | ROBUST | FRAGILE>
domain: <MEDIOCRISTAN | EXTREMISTAN | MIXED>
ruin_risk: <NONE | LOW | MEDIUM | HIGH | CRITICAL>
convexity_stack: <number of stacking antifragile properties>
confidence: <LOW | MEDIUM | HIGH>
---

# Taleb Antifragility Analysis: [Subject Name]

> "Wind extinguishes a candle and energizes fire. Likewise with randomness,
> uncertainty, chaos: you want to use them, not hide from them."
> — Nassim Nicholas Taleb, Antifragile

## The Subject

[One paragraph description]

## Domain Classification (Fat-Tail Detector)

### Mediocristan or Extremistan?
| Dimension | Classification | Evidence |
|-----------|---------------|----------|
| [Revenue/growth/risk dimension] | MED/EXT | [why] |
| ... | ... | ... |

### The Turkey Problem
[What's the Thanksgiving scenario? How long has apparent stability lasted?]

### Tail Risk Map
| Variable | Distribution | Positive Tail | Negative Tail | Bounded? |
|----------|-------------|---------------|---------------|----------|
| [var] | thin/fat | [what happens] | [what happens] | Y/N |
| ... | ... | ... | ... | ... |

### Ergodicity Assessment
**Absorbing barrier?** [YES — describe the ruin point / NO]
**Ruin probability (10yr)?** [estimate]

---

## The Fragility Map (Fragility Auditor)

### Triad Classification by Component
| Component | Classification | Key Evidence |
|-----------|---------------|-------------|
| [component] | FRAGILE/ROBUST/ANTIFRAGILE | [why] |
| ... | ... | ... |

### Suppressed Volatility
[Where is disorder being artificially smoothed? Where is the banker-vs-taxi-driver dynamic?]

### Single Points of Failure
| Dependency | Severity if Failed | Redundancy | Fat-Tailed? |
|-----------|-------------------|------------|-------------|
| [dep] | TOTAL/HIGH/MEDIUM/LOW | Y/N | Y/N |
| ... | ... | ... | ... |

### Nonlinear Fragility Points
[Where does doubling the stress more than double the damage?]

---

## The Optionality Profile (Optionality Scout)

### Payoff Structure
**Current shape:** [CONVEX / LINEAR / CONCAVE]
**Maximum downside:** [bounded at X / unbounded]
**Maximum upside:** [bounded at X / unbounded]

### Barbell Assessment
**Current position:** [SAFE EXTREME / DANGEROUS MIDDLE / RISKY EXTREME]

**Proposed barbell redesign:**
- Safe extreme (85-90%): [what the floor of safety looks like]
- Speculative extreme (10-15%): [what the asymmetric upside bets look like]
- What to ELIMINATE from the middle: [the moderate-risk positions to exit]

### Optionality Inventory
| Option Type | Present? | Quality (0-10) | Cost of Carry |
|-------------|----------|----------------|---------------|
| Strategic (can pivot) | Y/N | X | [cost] |
| Operational (can scale) | Y/N | X | [cost] |
| Epistemic (learns from failure) | Y/N | X | [cost] |
| Liquidity (can exit) | Y/N | X | [cost] |
| Independence ("f-you money") | Y/N | X | [cost] |

### Tinkering Potential
**Experiment cost:** [HIGH / MEDIUM / LOW]
**Iteration speed:** [FAST / MEDIUM / SLOW]
**Selection mechanism:** [EXISTS / WEAK / ABSENT]
**Silicon Valley or Soviet?** [assessment]

---

## The Via Negativa Report (Iatrogenics Checker)

### The Subtraction List
What should be REMOVED to reduce fragility:

| # | Remove This | Expected Benefit | Iatrogenic Cost of Keeping |
|---|-------------|-----------------|---------------------------|
| 1 | [thing] | [benefit of removal] | [harm it's causing] |
| ... | ... | ... | ... |

### Intervention Assessment
| Intervention | Justified? | Iatrogenic Risk | Net Effect |
|-------------|-----------|----------------|------------|
| [intervention] | Y/N | LOW/MED/HIGH | POSITIVE/NEGATIVE |
| ... | ... | ... | ... |

### Narrative Fallacy Alert
[What post-hoc story is being told? What silent evidence is being ignored?]

### Precautionary Principle
**Ruin risk category:** [SYSTEMIC RUIN / REGULAR RISK]
**Standard cost-benefit valid?** [YES / NO — precautionary principle applies]

---

## The Accountability Structure (Skin-in-the-Game Auditor)

### Symmetry Map
| Actor | Upside | Downside | Symmetric? | Bob Rubin Trade? |
|-------|--------|----------|------------|-----------------|
| [actor] | [what they gain] | [what they lose] | Y/N | Y/N |
| ... | ... | ... | ... | ... |

### Revealed Preferences
[Do decision-makers eat their own cooking? Portfolio vs. advice alignment?]

### IYI Check
[Who is advising without skin in the game? Domain dependence issues?]

### Minority Rule Exposure
[What intransigent minority could shift the landscape?]

---

## THE CONVEXITY ASSESSMENT

This is the Taleb question: **What's the payoff structure under disorder,
and do the antifragile properties reinforce each other?**

### Antifragile Properties Stacking

```
[Property 1: e.g., cheap experimentation with fast feedback]
  + [Property 2: e.g., selection mechanism that keeps winners]
    + [Property 3: e.g., skin in the game for decision-makers]
      + [Property 4: e.g., decentralized structure with redundancy]
        = [CONVEXITY STACK STRENGTH: NONE / WEAK / MODERATE / STRONG]
```

**Convexity stack strength:** [NONE / WEAK / MODERATE / STRONG]

A genuinely antifragile system (evolution, restaurant ecosystem, Swiss cantons)
has 4+ antifragile properties that reinforce each other. Most businesses have
zero genuine antifragile properties — they are merely not-yet-tested fragile.

### Fragility Cascade (Risks That Compound)

```
[Fragility 1] → triggers → [Fragility 2] → amplifies → [Fragility 3]
  = [cascade severity: MANAGEABLE / DANGEROUS / CATASTROPHIC]
```

---

## THE VERDICT

### Taleb's Three Classifications

**[ ] ANTIFRAGILE** — Benefits from disorder. Convex payoff structure with
  multiple reinforcing antifragile properties. Position to capture positive
  Black Swans while surviving negative ones. Pursue with the barbell.

**[ ] ROBUST** — Survives disorder without benefiting. Acceptable if the goal
  is preservation, not growth. Consider restructuring for convexity if growth
  is needed.

**[ ] FRAGILE** — Harmed by disorder. Concave payoff structure with hidden
  tail risks, suppressed volatility, or structural asymmetries. Restructure
  immediately or walk away. The longer the apparent calm, the worse the
  eventual break.

### Verdict: [ANTIFRAGILE / ROBUST / FRAGILE]

**Confidence:** [LOW / MEDIUM / HIGH]
**Ruin risk:** [NONE / LOW / MEDIUM / HIGH / CRITICAL]
**Domain:** [MEDIOCRISTAN / EXTREMISTAN / MIXED]

**Reasoning:** [2-3 paragraphs synthesizing all five analysts' findings.
Reference specific evidence. Identify the key fragility points and the
key antifragile properties. Be honest about the tail risk. If it's FRAGILE,
say so without apology — most things are. If it's ANTIFRAGILE, explain
exactly what makes it so — genuine antifragility is rare.]

### What Taleb Would Say

[Write 3-5 sentences in Taleb's voice — combative, erudite, peppered with
his vocabulary. Use his actual terms: "concave," "IYI," "skin in the game,"
"Lindy," "via negativa," "Mediocristan," "Extremistan," "lecturing birds
how to fly." Reference Mediterranean wisdom, Seneca, ancient heuristics.
Be dismissive of credentials and respectful of practitioners. If the subject
is fragile, be merciless. If antifragile, be grudgingly impressed. Taleb
doesn't praise easily.]

Example voice:
"This has the payoff structure of a Thanksgiving turkey — smooth feeding for
years, then the butcher arrives. The 'risk management' here is an IYI exercise
in lecturing birds how to fly. Anyone with actual skin in the game would have
noticed the concavity in the revenue structure and the Bob Rubin trade in the
governance. Via negativa: remove the consultants, remove the optimization,
and let the system encounter small stressors before the big one arrives
uninvited."

### If You Proceed: The Antifragility Rules

[Based on the analysis, write 3-7 rules for making this subject less fragile
or more antifragile. Each rule should be a SPECIFIC, ACTIONABLE directive
derived from the framework.]

1. **[Via negativa rule]** — Remove [specific fragility source]
2. **[Barbell rule]** — Restructure as [safe extreme] + [speculative extreme]
3. **[Skin in the game rule]** — Ensure [specific actor] bears [specific risk]
4. **[Optionality rule]** — Create [specific option with bounded downside]
5. **[Lindy rule]** — Replace [new unproven thing] with [time-tested equivalent]
6. **[Anti-turkey rule]** — Introduce [specific stressor/feedback mechanism]
7. **[Tail risk rule]** — Hedge against [specific tail event]
```

## Phase 5: Present & Follow-up

Present the verdict to the user with the key highlights. Don't dump the
whole document — give the verdict, the convexity assessment, and the
fragility map. Let them read the full document.

```
## Taleb Verdict: [SUBJECT] — [ANTIFRAGILE / ROBUST / FRAGILE]

**Domain:** [Mediocristan / Extremistan / Mixed]
**Ruin risk:** [level]
**Convexity stack:** [strength] — [N] antifragile properties [stacking/isolated]
**Turkey problem:** [yes/no — what's the Thanksgiving scenario?]
**Key fragility:** [the biggest structural weakness]
**Key optionality:** [the biggest asymmetric upside opportunity]
**Skin in the game:** [aligned / misaligned — who has the Bob Rubin trade?]

**What Taleb would say:** "[pithy Talebian quote]"

Full analysis: `thoughts/taleb/YYYY-MM-DD-<slug>.md`

Want me to:
1. Deep-dive into any analyst's findings?
2. Restructure this as a barbell? (specific redesign)
3. Run /munger to complement with competitive/valuation analysis?
4. Compare this against an alternative? (batch mode)
```

## Batch Mode

If the user wants to compare multiple subjects:

1. Run the full analysis on each (can parallelize — one team per subject)
2. At the end, produce a comparison:

```
## Taleb Antifragility Leaderboard

| Rank | Subject | Verdict | Domain | Ruin Risk | Convexity | Skin |
|------|---------|---------|--------|-----------|-----------|------|
| 1 | [name] | ANTIFRAGILE | EXT | LOW | STRONG (4) | Aligned |
| 2 | [name] | ROBUST | MED | NONE | NONE | Aligned |
| 3 | [name] | FRAGILE | EXT | HIGH | NONE | Bob Rubin |
```

## Scoring Discipline

- **Be Taleb, not a consultant.** Taleb classifies most things as fragile. If
  every subject gets ANTIFRAGILE, the skill is broken. Genuine antifragility
  is RARE — even robust is uncommon.
- **Cite the source analyst.** Every claim traces to a specific teammate's finding.
- **No false precision.** Taleb hates fake numerical precision in fat-tailed domains.
  Don't put decimals on things that can't be measured that precisely. Use
  categories (HIGH/LOW) not numbers (73.2%) for tail risk.
- **The survival test is primary.** Before anything else: can this survive its
  worst plausible scenario? If not, everything else is irrelevant.
  "In order to succeed, you must first survive."
- **Web search when uncertain.** The Skin-in-the-Game Auditor and Fat-Tail
  Detector should use WebSearch to ground their analysis in real-world evidence.

## Framework Limitations (Built-in Honest Warning)

Include this at the end of every analysis:

```
### Framework Limitations

This analysis applies Taleb's antifragility framework, which is strongest for:
- Structural risk assessment in fat-tailed domains
- Identifying hidden fragilities and suppressed volatility
- Evaluating payoff asymmetries and skin-in-the-game alignment
- Portfolio/system design for survival under extreme scenarios

It is weakest for (and should be complemented with other frameworks for):
- Competitive dynamics and moat analysis (use /munger)
- Go-to-market timing and execution risk
- Product-market fit and customer development
- Team quality and management assessment
- Valuation and unit economics (use /munger)
- Creative and early-stage product decisions

Taleb's framework is primarily diagnostic (identifying fragility) rather than
generative (creating value). Use it to set structural constraints on strategy,
then use other frameworks to fill in the positive vision.
```

## Important Notes

- **Cost**: This skill spawns 5 agents. It's expensive. Worth it for serious
  structural analysis, not for casual questions (for quick fragility checks,
  just ask directly).
- **Sonnet for teammates, Opus for synthesis**: The lead handles the convexity
  assessment and final verdict — that's where deep reasoning matters.
- **No team? No problem**: If teams aren't enabled, run 5 sequential background
  agents and collect results. Same analysis, just no cross-talk.
- **Pair with /munger**: The strongest analysis runs both: /munger for competitive
  dynamics, valuation, and psychological forces; /taleb for structural fragility,
  tail risk, and payoff asymmetry. Munger asks "is this a great business?"
  Taleb asks "can this survive what it can't predict?"
- **Sources**: This framework synthesizes concepts from Taleb's Incerto series:
  *Antifragile* (2012), *The Black Swan* (2007), *Skin in the Game* (2018),
  *Fooled by Randomness* (2001), and *Statistical Consequences of Fat Tails* (2020).
  For deeper understanding, read the originals — especially Antifragile Ch. 1-4
  and Skin in the Game Ch. 1-3.
