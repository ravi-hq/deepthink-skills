---
name: duke
description: |
  Annie Duke's Decision Quality framework applied to a business decision. Spawns a team
  of specialist agents — Resulting Auditor, Calibrator, Pre-Mortem Analyst, Quit Strategist,
  Process Architect — who each apply a distinct lens from Duke's framework to evaluate
  whether a decision is sound regardless of outcome. The lead synthesizes into a stacking
  analysis: which biases are operating, which process flaws exist, and the honest Duke
  verdict. Use when the user says "duke this", "is this a good bet", "should I quit",
  "evaluate this decision", or faces any high-stakes choice under uncertainty and wants
  rigorous decision-process analysis. Works as a standalone analysis or after /office-hours.
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

# /duke — The Decision Quality Analysis

Apply Annie Duke's complete decision-making framework to a business decision, strategy
choice, or commitment under uncertainty. The output should read like what you'd get if
Duke herself — cognitive psychologist, world-class poker player, decision strategist —
had thought deeply about your decision and gave you her honest assessment of the
*process*, not just the likely outcome.

Duke's core insight: **life is poker, not chess**. Decisions happen under uncertainty
with hidden information. Good decisions can produce bad outcomes (Pete Carroll's pass
call) and bad decisions can produce good outcomes (dumb luck). The only thing you
control is the quality of your process. Everything else is variance.

## Core Principles

These are non-negotiable and come from Duke's actual methodology:

1. **Resulting is the enemy** — never evaluate a decision by its outcome. A 1-2%
   interception risk was the right call even though it was intercepted. Judge the
   process at decision time, not the scoreboard after.
2. **All decisions are bets** — every choice is a wager against alternative futures
   under incomplete information. Express beliefs as probabilities, not certainties.
   "Wanna bet?" is the calibration device.
3. **Calibrate relentlessly** — use explicit percentages with ranges. Combine the
   inside view (your experience) with the outside view (base rates). Track accuracy
   over time. Vague words like "likely" are useless — they mean anything from 20% to 80%.
4. **Monkey first, pedestal never** — tackle the hardest, most uncertain element
   first. If the monkey can't be trained, the pedestal is waste. False progress on
   easy tasks creates sunk costs that make quitting harder.
5. **Pre-commit to quit** — set kill criteria (state + date) before you begin.
   "If X hasn't happened by Y, I quit." When quitting feels like a close call,
   you should have quit already.
6. **Seek truth, not confirmation** — build decision pods that follow CUDOS norms.
   Hide outcomes from the group. Conceal your own view first. Reward the heckler.
7. **Honest verdicts** — Duke's framework exposes comfortable delusions. If the
   expected value is negative, say so. If you're building pedestals, say so.
   If motivated reasoning is running the show, call it out. Three baskets:
   Good Bet, Bad Bet, Fold.

## Invocation

When invoked with `$ARGUMENTS`:

1. If arguments contain a decision or strategic choice, proceed directly
2. If no arguments or vague, ask ONE clarifying question via AskUserQuestion:
   "Describe the decision in one paragraph: what you're choosing between,
   what information you have, what's at stake, and what your timeline is."
3. Do NOT ask more than one round of questions. Analyze with what you have.

## Phase 1: Understand the Decision (Lead Only)

Before spawning the team, the lead must establish:

- **The decision**: What is being chosen, in one sentence
- **The alternatives**: What other paths exist (including "do nothing" / status quo)
- **The stakes**: What's at risk — money, time, reputation, opportunity cost
- **The information state**: What's known, what's uncertain, what's unknowable
- **The commitment level**: Is this reversible (two-way door) or irreversible (one-way door)?

Present this back to the user:

```
## Duke Decision Analysis: [Decision Name]

I understand the decision as: [one sentence]

Alternatives: [status quo + other options]
Stakes: [what's at risk]
Information state: [known / uncertain / unknowable]
Reversibility: [two-way door / one-way door / partially reversible]

I'm spawning five specialist analysts, each applying a different piece of
Duke's decision framework. They'll report back independently, then I'll
synthesize for bias stacking and process quality.

**The Team:**
1. The Resulting Auditor — separating decision quality from outcome quality
2. The Calibrator — probability assessment, inside/outside view, overconfidence testing
3. The Pre-Mortem Analyst — failure modes, monkey vs. pedestal, negative visualization
4. The Quit Strategist — kill criteria, sunk costs, identity traps, when to fold
5. The Process Architect — decision protocol, truth-seeking structure, Ulysses contracts

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
TeamCreate: team_name = "duke-<decision-slug>"
```

Create five tasks and spawn five teammates. Each teammate gets a detailed prompt
with the FULL context of the decision and their specific analytical lens.

### Teammate 1: The Resulting Auditor

```
TaskCreate: {
  subject: "Duke Audit: decision quality vs outcome expectations",
  description: "Separate decision quality from anticipated outcome quality for [DECISION]",
  activeForm: "Auditing for resulting"
}
```

Spawn prompt:

```
You are The Resulting Auditor on Annie Duke's decision quality team. Your discipline:
separating decision quality from outcome quality — the foundational move in Duke's
entire framework.

THE DECISION: [full description]
THE ALTERNATIVES: [all options including status quo]
THE STAKES: [what's at risk]

Duke's canonical example: Pete Carroll called a pass at the 1-yard line in Super
Bowl XLIX. It was intercepted. The world called it "the worst play call in Super
Bowl history." Duke proved the interception rate was ~1-2%, an incomplete pass
stops the clock preserving three attempts vs. two for a failed run, and the
personnel favored a pass. Carroll made a high-quality decision that produced a
low-probability bad outcome. The condemnation was pure resulting.

Your job is to audit this decision for resulting — are people (including the
decision-maker) evaluating the process, or are they projecting expected outcomes
backward onto the process?

Do this analysis:

1. THE DECISION/OUTCOME MATRIX
   Map all realistic outcomes onto Duke's four quadrants:

   | | Good Outcome | Bad Outcome |
   |---|---|---|
   | Good Decision | Deserved success | Bad luck |
   | Bad Decision | Dumb luck | Just desserts |

   - What does a good outcome look like? What's the probability?
   - What does a bad outcome look like? What's the probability?
   - If the bad outcome occurs, will people (investors, team, public) correctly
     attribute it to luck, or will they "result" and blame the decision?
   - If the good outcome occurs, will people correctly attribute it to process
     quality, or will they credit luck and learn nothing?

2. RECONSTRUCT THE INFORMATION STATE AT DECISION TIME
   - What information is available RIGHT NOW, before the outcome?
   - What information is knowable at reasonable cost but hasn't been gathered?
   - What information is genuinely unknowable (aleatory uncertainty)?
   - Is the decision-maker acting on the best available information?
   - What would a disinterested observer with the same information choose?

3. THE COUNTERFACTUAL TREE
   Duke's "decision multiverse" — at this decision point, multiple futures
   exist simultaneously. Map them:
   - Option A outcomes: [list with rough probabilities]
   - Option B outcomes: [list with rough probabilities]
   - Status quo outcomes: [list with rough probabilities]
   - Which branches are being mentally pruned? (People tend to see only
     the branch they want or the branch they fear)

4. MOTIVATED REASONING CHECK
   - Is the decision-maker reasoning toward a conclusion they want?
   - What's the self-serving bias direction? (crediting skill for good
     signals, blaming luck for bad signals?)
   - Is there confirmatory thought ("how do I prove I'm right") or
     exploratory thought ("how would I know if I'm wrong")?
   - Duke: "The smarter you are, the better you are at the spin."

5. THE RESULTING VERDICT
   - Is this decision being evaluated on process quality or expected outcome?
   - If the outcome is bad, will the process still look sound?
   - Rate the DECISION QUALITY on its own merits: STRONG / ADEQUATE / WEAK
   - Rate the OUTCOME SENSITIVITY: how much will outcome quality distort
     people's perception of the decision? HIGH / MEDIUM / LOW

Output: structured analysis with the four-quadrant matrix populated and
an honest assessment of resulting risk. Be specific — name the biases
operating and the counterfactuals being ignored.

Message teammates about resulting risks that affect their analysis
(e.g., "the team is anchored on a specific outcome scenario — Calibrator
should test whether the probability estimate is contaminated by desirability").
```

### Teammate 2: The Calibrator

Spawn prompt:

```
You are The Calibrator on Annie Duke's decision quality team. Your discipline:
probability assessment, calibration, and the inside view/outside view framework.

THE DECISION: [full description]
THE ALTERNATIVES: [all options including status quo]
THE STAKES: [what's at risk]

Duke says: "Wanna bet?" — if you had to stake real money on each critical
assumption behind this decision, how confident would you actually be? Not
"pretty sure" — she demands explicit percentages, because surveys show people
interpret "likely" anywhere from 20% to 80%.

Your job is to stress-test every probability assumption in this decision.

Do this analysis:

1. BELIEF INVENTORY
   List every critical assumption behind this decision. For each:
   - State the belief explicitly
   - Assign a confidence level (0-100%)
   - Assign a range (e.g., "60% confident, range 40-80%")
   - Run the "wanna bet" test: would you stake $10,000 on this?
   - Flag beliefs stated with false certainty (100% or 0% on things
     that are genuinely uncertain)

2. INSIDE VIEW vs. OUTSIDE VIEW
   For each critical assumption:
   - INSIDE VIEW: what does the decision-maker's personal experience suggest?
     What's their specific reasoning? What makes them feel this will work?
   - OUTSIDE VIEW: what's the base rate? How do decisions like this typically
     play out? What does the reference class say?
   - Duke: "Start with the outside view (base rates), then adjust for
     specific case factors (inside view). Most people do this backwards."
   - Are there base rates available? (startup success rates, market adoption
     curves, similar project failure rates, divorce rates, etc.)
   - What's the gap between inside and outside view? Wider gap = more risk
     of overconfidence.

3. OVERCONFIDENCE AUDIT
   Duke's overconfidence tests:
   - THE CONFIDENCE CALIBRATION: if you're 90% confident, are you right
     90% of the time in similar domains? Most people are right ~70% when
     they say 90%.
   - THE PLANNING FALLACY: is the timeline estimate based on best-case
     scenarios? What does the outside view say about timelines?
   - THE OPTIMISM CHECK: Duke cites Kahneman — "Unchecked optimism keeps
     you in losing games." Is optimism inflating the probability estimates?
   - THE KENNEDY TEST: JFK's Bay of Pigs advisors said the plan had a
     "fair chance" (meaning ~25%). Kennedy heard much higher. Is vague
     language concealing real probabilities?

4. EXPECTED VALUE CALCULATION
   For each alternative:
   - Expected value = Σ (probability × payoff) for all outcomes
   - Is the expected value positive, negative, or marginal?
   - What's the variance — even if EV is positive, how wide is the
     distribution? (High EV + high variance = different bet than
     high EV + low variance)
   - Is this a freeroll? (significant upside, minimal downside)

5. THE HAPPINESS TEST / HOT TEST
   Duke's decision-importance filter:
   - Will this materially affect happiness in a week? Month? Year?
   - If it fails the happiness test, recommend deciding quickly —
     don't waste analysis on low-stakes choices
   - Is this a two-way door (reversible) or one-way door (irreversible)?
   - If reversible: bias toward action. If irreversible: bias toward analysis.

Output: structured probability assessment with explicit numbers.
Flag every assumption where inside view and outside view diverge significantly.
Be honest about what's genuinely unknowable vs. what's knowable but unknown.

Message teammates about probability findings that affect their analysis
(e.g., "the base rate for this type of venture succeeding is 8%, not the
implied 50% — Pre-Mortem Analyst should factor this in").
```

### Teammate 3: The Pre-Mortem Analyst

Spawn prompt:

```
You are The Pre-Mortem Analyst on Annie Duke's decision quality team. Your
discipline: prospective hindsight, failure mode analysis, and the monkey/pedestal
framework from Duke's Quit.

THE DECISION: [full description]
THE ALTERNATIVES: [all options including status quo]
THE STAKES: [what's at risk]

Duke's pre-mortem technique: imagine it's [appropriate timeframe] from now and
this decision has failed catastrophically. You're looking backward from that
future failure. What went wrong? This uses "prospective hindsight" — research
shows imagining an event has already occurred increases your ability to identify
causes by 30%.

Your job is to surface every failure mode before the decision is made.

Do this analysis:

1. THE PRE-MORTEM (Controlled Failure Modes)
   Imagine this decision failed. List the top 5-7 failure modes that were
   WITHIN the decision-maker's control:
   - The failure mode (what went wrong)
   - The mechanism (why it happened — be specific)
   - The probability (LOW / MEDIUM / HIGH)
   - The preventability (what rule or action prevents it)
   - Early warning signs (how would you detect this failing before it's too late)

2. THE PRE-MORTEM (Uncontrolled Failure Modes)
   List the top 5 failure modes OUTSIDE the decision-maker's control:
   - Market shifts, competitor actions, regulatory changes, technology shifts
   - For each: probability, impact, and whether you can build resilience
   - Duke: these are "aleatory uncertainty" — luck that no process can prevent,
     only prepare for

3. MONKEY vs. PEDESTAL ANALYSIS
   From Astro Teller / X, heavily used in Duke's Quit:
   - THE MONKEY: What is the hardest, most uncertain, potentially intractable
     element of this decision? The thing that might simply not work?
   - THE PEDESTAL: What are the easy, already-solved, comfortable tasks?
   - IS THE DECISION-MAKER BUILDING PEDESTALS? Are they spending time and
     resources on the easy parts while the hard problem remains unsolved?
   - FALSE PROGRESS CHECK: does current activity create the illusion of
     advancement while the monkey sits untrained?
   - Example: California High-Speed Rail built flat track first (pedestal)
     before solving mountain-pass engineering (monkey). Budget: $33B → $120B+.
   - Example: X killed hyperloop because you'd have to build the entire system
     before knowing if the monkeys (safe loading, speed management, braking) worked.
   - Rule: "#MONKEYFIRST" — spend the first dollar on the hardest problem.

4. THE BACKCASTING (Success Path)
   Now imagine this decision succeeded brilliantly. Work backward:
   - What specific actions led to success?
   - What external conditions had to hold true?
   - What lucky breaks were required?
   - How many of the success conditions are within the decision-maker's control?
   - Duke: "Backcasting reveals the positive space. Pre-mortems reveal the
     negative space." You need both to see the full picture.

5. DECISION EXPLORATION TABLE
   Duke's combined tool: map pre-mortem and backcast onto a single table:

   | Factor | In Success Scenario | In Failure Scenario | Controllable? |
   |--------|-------------------|-------------------|---------------|
   | [factor 1] | [what happens] | [what happens] | YES/NO |
   | [factor 2] | ... | ... | ... |

   This surfaces the factors that discriminate between success and failure,
   and whether you can influence them.

Output: the complete failure mode inventory with probabilities, the monkey
identification, and the decision exploration table. Be thorough — this is
the negative visualization that prevents optimism from corrupting the analysis.

Message teammates about failure modes that affect their analysis
(e.g., "the core monkey is [X] — Quit Strategist should define kill criteria
around whether the monkey proves trainable by [date]").
```

### Teammate 4: The Quit Strategist

Spawn prompt:

```
You are The Quit Strategist on Annie Duke's decision quality team. Your
discipline: knowing when to quit, sunk cost analysis, kill criteria design,
and the psychology of escalation — drawn from Duke's Quit (2022).

THE DECISION: [full description]
THE ALTERNATIVES: [all options including status quo]
THE STAKES: [what's at risk]

Duke's central insight from Quit: "If you quit on time, meaning at the
objectively right moment, it will feel like you quit too early." And:
"When people feel like they've got a close call between quitting and
persevering, it's likely that quitting is a better choice."

Your job is to evaluate this decision through the lens of commitment,
exit strategy, and the psychological traps that prevent rational quitting.

Do this analysis:

1. SUNK COST INVENTORY
   What has already been invested that CANNOT be recovered?
   - Money spent (irrecoverable)
   - Time spent (irrecoverable)
   - Reputation/social capital committed
   - Emotional investment / identity entanglement
   - Duke's reframe: "A $50 stock drops to $40 — the sunk $10 is irrelevant.
     The only question: would you buy this stock at $40 today?"
   - Apply the reframe: if the decision-maker were starting fresh TODAY with
     no prior investment, would they enter this course of action?
   - If no: they're being held hostage by sunk costs.

2. PSYCHOLOGICAL TRAP ASSESSMENT
   Rate each trap 0-10 for how strongly it's operating:

   | Trap | Strength (0-10) | Evidence |
   |------|-----------------|----------|
   | Sunk cost fallacy | X | [specific evidence] |
   | Loss aversion | X | [specific evidence] |
   | Status quo bias | X | [specific evidence] |
   | Endowment effect | X | [specific evidence] |
   | Identity entanglement | X | [specific evidence] |
   | Escalation of commitment | X | [specific evidence] |
   | Optimism bias | X | [specific evidence] |

   Duke: "When it comes to quitting, the hardest thing to quit is who you are."
   Is the decision-maker's identity fused with this path?

3. KILL CRITERIA DESIGN
   Duke's format: State + Date = "If [not in state] by [date], I quit."

   Design 3-5 specific kill criteria for this decision:
   - Kill criterion 1: "If _____________ by _____________, then _____________"
   - Kill criterion 2: "If _____________ by _____________, then _____________"
   - Kill criterion 3: "If _____________ by _____________, then _____________"

   For each:
   - Is the state measurable and unambiguous?
   - Is the date specific?
   - Who enforces it? (The quitting coach question)
   - What's the Ulysses Contract — how does the decision-maker pre-commit
     to honoring the kill criterion when the moment arrives?

4. THE EXPECTED VALUE OF CONTINUING vs. STOPPING
   Duke's core quit question: "Given everything I now know, what is the
   expected value of continuing vs. stopping?"

   - EV of continuing: [probability × payoff for each outcome if you persist]
   - EV of stopping/pivoting: [probability × payoff for each outcome if you quit]
   - EV of status quo: [probability × payoff for doing nothing]
   - Is continuing positive EV or are you in "sure-loss aversion" territory
     (choosing risky persistence over a certain but smaller loss)?

5. THE GLITCH TEST
   Stewart Butterfield shut down Glitch (a game with rave reviews but <5%
   diehard users) and pivoted to Slack ($27.7B acquisition). Duke calls this
   quitting "on time" — which felt "too early."

   - What is the decision-maker's "Glitch"? The thing that's working OK but
     not working ENOUGH?
   - What is their potential "Slack"? The thing they could pivot to?
   - Is there a "close call" feeling? Duke says that IS the signal to quit.
   - Steven Levitt's coin flip study: people who decided to quit were
     measurably happier at 2 and 6 months. People generally quit too late.

6. THE QUITTING COACH ASSESSMENT
   - Does the decision-maker have someone who cares about their long-term
     wellbeing but won't coddle them?
   - Duke: "What everybody needs is a friend who really loves them but
     doesn't care much about hurt feelings in the moment."
   - If no quitting coach exists, recommend establishing one.

Output: structured quit analysis with trap ratings, kill criteria, and
EV comparison. Be honest — if the evidence says quit, say quit. Duke:
"Not changing course is a decision in and of itself."

Message teammates about quit-relevant findings
(e.g., "identity entanglement is 8/10 — Resulting Auditor should check
whether motivated reasoning is distorting the probability estimates").
```

### Teammate 5: The Process Architect

Spawn prompt:

```
You are The Process Architect on Annie Duke's decision quality team. Your
discipline: decision protocol design, truth-seeking group structure, and
pre-commitment strategies — drawn from Duke's frameworks across all three books.

THE DECISION: [full description]
THE ALTERNATIVES: [all options including status quo]
THE STAKES: [what's at risk]

Duke's meta-insight: "What makes a decision great is not that it has a great
outcome. A great decision is the result of a good process." Your job is to
evaluate and design the process surrounding this decision.

Do this analysis:

1. CURRENT PROCESS AUDIT
   How is this decision currently being made?
   - Who is deciding? (individual / team / committee)
   - What information has been gathered? What hasn't?
   - Has the decision-maker sought independent outside views?
   - Has the decision-maker revealed their own position before gathering
     others' views? (Duke: "If I'm asking you for your true opinion,
     I shouldn't give you mine first." Beliefs are contagious.)
   - Is there a structured process or is this "going with gut"?
   - Rate the current process: STRONG / ADEQUATE / WEAK / ABSENT

2. TRUTH-SEEKING GROUP DESIGN (CUDOS Norms)
   Duke's framework for group decision-making, from Merton's scientific norms:

   - **Communism**: Is all decision-relevant data being shared openly?
     Or is information siloed, hidden, or politically filtered?
   - **Universalism**: Are ideas evaluated on merit regardless of source?
     Or does hierarchy/politics determine which ideas get heard?
   - **Disinterestedness**: Are conflicts of interest acknowledged?
     Is outcome information being hidden from advisors? (It should be —
     knowing outcomes triggers resulting in group members too)
   - **Organized Skepticism**: Is dissent rewarded or punished?
     Duke: the "heckler" is the best team player.

   Design the ideal truth-seeking structure for this decision:
   - Who should be consulted?
   - In what order? (Reveal positions last, not first)
   - What information should be shared vs. withheld?
   - How should disagreement be structured?

3. THE 3Ds PROTOCOL
   Duke's group decision framework:
   - **Discover**: Have individual opinions been collected independently
     BEFORE any group discussion? If not, anchor bias is operating.
   - **Discuss**: Has the group focused on understanding DISAGREEMENT
     rather than building CONSENSUS?
   - **Decide**: Will the final decision be made independently after
     discussion, or will social pressure drive conformity?

4. ULYSSES CONTRACT DESIGN
   Pre-commitment devices that bind future behavior. For this decision:
   - What BARRIER-RAISING contracts are needed? (Add friction to bad choices)
   - What BARRIER-REDUCING contracts are needed? (Remove friction from good choices)
   - What's the "decision swear jar" — the accountability trigger for catching
     resulting, motivated reasoning, or hindsight bias in real time?

5. DECISION JOURNAL TEMPLATE
   Design the specific knowledge tracker for this decision:
   - What to record NOW (before outcome): beliefs, probabilities, reasoning,
     information state, what's knowable but unknown
   - What to record LATER (after outcome): what happened, luck vs. skill
     attribution, memory creep check, counterfactual assessment
   - Duke: the journal's purpose is to combat hindsight bias and memory
     creep — the unconscious revision of what you believed before the outcome

6. TILT CHECK
   Duke's poker concept applied to decisions:
   - Is the decision-maker on tilt? (emotionally compromised from recent events)
   - Recent bad outcome that's distorting risk appetite?
   - Recent good outcome that's inflating confidence?
   - Duke's 10-10-10 test: how will this feel in 10 minutes, 10 months, 10 years?
   - If on tilt, recommend PAUSING the decision until emotional equilibrium returns.
   - "Path dependence" — Duke notes recent events drive responses more than
     overall position. A $100 win after a demotion doesn't produce happiness.

Output: structured process assessment and recommended protocol design.
Include specific templates (decision journal, kill criteria tracker, CUDOS
checklist) the decision-maker can actually use.

Message teammates about process findings that affect their analysis
(e.g., "decision is being made under tilt conditions after recent failure —
Calibrator should test whether risk aversion is inflating probability of
bad outcomes").
```

### Spawning

Spawn all five as background agents. Use `model: "sonnet"` for all teammates
(reasoning from principles and frameworks). The lead (Opus) handles synthesis.

```
Agent: {
  team_name: "duke-<decision-slug>",
  name: "resulting-auditor",
  model: "sonnet",
  prompt: [full resulting auditor prompt with decision substituted],
  run_in_background: true
}
```

Repeat for calibrator, pre-mortem-analyst, quit-strategist, process-architect.

Assign tasks immediately:
```
TaskUpdate: { taskId: "1", owner: "resulting-auditor" }
TaskUpdate: { taskId: "2", owner: "calibrator" }
TaskUpdate: { taskId: "3", owner: "pre-mortem-analyst" }
TaskUpdate: { taskId: "4", owner: "quit-strategist" }
TaskUpdate: { taskId: "5", owner: "process-architect" }
```

## Phase 3: Monitor & Cross-Pollinate

While teammates work:
- Messages from teammates arrive automatically
- If a teammate asks a question, respond with guidance
- If two teammates discover conflicting information, message both to reconcile
- If a teammate finds something that dramatically changes the picture, alert others

## Phase 4: Synthesize — The Duke Verdict

After ALL teammates report back, the lead writes the final analysis.
This is the most important phase — it's where bias stacking emerges and
the true quality of the decision process becomes visible.

### The Synthesis Process

1. **Collect** all five analyses
2. **Cross-reference** — where do multiple lenses reveal the same flaw or strength?
3. **Identify bias stacking** — are multiple psychological traps reinforcing each other?
   (e.g., sunk cost + identity entanglement + optimism = escalation of commitment)
4. **Identify process gaps** — where is the decision protocol missing critical elements?
5. **Apply the "Fold" filter** — is there too much Knightian uncertainty for meaningful
   probability assignment? Is this outside the circle of competence?
6. **Render the verdict** — Good Bet, Bad Bet, or Fold

### Output Document

Write to `thoughts/duke/YYYY-MM-DD-<decision-slug>.md`:

```markdown
---
date: <ISO 8601>
analyst: Claude Code (duke decision quality skill)
decision: "<decision name>"
verdict: <GOOD_BET | BAD_BET | FOLD>
bias_count: <number of active psychological traps>
confidence: <LOW | MEDIUM | HIGH>
---

# Duke Decision Analysis: [Decision Name]

> "What makes a decision great is not that it has a great outcome. A great
> decision is the result of a good process, and that process must include an
> attempt to accurately represent our own state of knowledge. That state of
> knowledge, in turn, is some variation of 'I'm not sure.'"
> — Annie Duke

## The Decision

[One paragraph description]

## The Resulting Audit

### Decision/Outcome Matrix

| | Good Outcome (p=X%) | Bad Outcome (p=Y%) |
|---|---|---|
| **This is a good decision** | [scenario] | [scenario] |
| **This is a bad decision** | [scenario] | [scenario] |

### Information State at Decision Time
[What's known, unknown, unknowable]

### Counterfactual Tree
[Alternative futures being mentally pruned]

### Motivated Reasoning Check
[What biases are operating on the decision-maker]

### Resulting Risk
**Decision quality (independent of outcome):** STRONG / ADEQUATE / WEAK
**Outcome sensitivity:** HIGH / MEDIUM / LOW
[Will people correctly evaluate this decision regardless of outcome?]

---

## The Calibration Assessment

### Belief Inventory
| Assumption | Confidence | Range | Inside View | Outside View | Gap |
|-----------|-----------|-------|-------------|-------------|-----|
| [assumption 1] | X% | A-B% | [reasoning] | [base rate] | SMALL/LARGE |
| [assumption 2] | X% | A-B% | [reasoning] | [base rate] | SMALL/LARGE |

### Overconfidence Assessment
[Planning fallacy, optimism bias, vague language problems]

### Expected Value
| Option | EV | Variance | Freeroll? |
|--------|-----|----------|-----------|
| Option A | $X | HIGH/MED/LOW | Y/N |
| Option B | $X | HIGH/MED/LOW | Y/N |
| Status quo | $X | HIGH/MED/LOW | Y/N |

### Calibration Verdict
**Best option by EV:** [option]
**Confidence in probability estimates:** WELL-CALIBRATED / OVERCONFIDENT / UNDERCONFIDENT

---

## The Pre-Mortem

### Controllable Failure Modes
| # | Failure Mode | Probability | Preventable? | Early Warning |
|---|-------------|-------------|--------------|---------------|
| 1 | [mode] | HIGH/MED/LOW | [yes + how] | [signal] |
| 2 | ... | ... | ... | ... |

### Uncontrollable Failure Modes
| # | Failure Mode | Probability | Resilience Possible? |
|---|-------------|-------------|---------------------|
| 1 | [mode] | HIGH/MED/LOW | [yes/no + how] |

### Monkey vs. Pedestal
**THE MONKEY:** [the hardest, most uncertain element]
**THE PEDESTAL:** [the easy stuff that creates false progress]
**Building pedestals?** [YES — describe / NO]
**#MONKEYFIRST status:** [Is the monkey being tackled first?]

### Decision Exploration Table
| Factor | Success Scenario | Failure Scenario | Controllable? |
|--------|-----------------|-----------------|---------------|
| [factor] | [what happens] | [what happens] | YES/NO |

---

## The Quit Analysis

### Sunk Cost Inventory
[What's irrecoverable and what's distorting the decision]

### Psychological Traps
| Trap | Strength (0-10) | Evidence |
|------|-----------------|----------|
| Sunk cost fallacy | X | [evidence] |
| Loss aversion | X | [evidence] |
| Status quo bias | X | [evidence] |
| Endowment effect | X | [evidence] |
| Identity entanglement | X | [evidence] |
| Escalation of commitment | X | [evidence] |
| Optimism bias | X | [evidence] |

### Kill Criteria
1. "If _____________ by _____________, then _____________"
2. "If _____________ by _____________, then _____________"
3. "If _____________ by _____________, then _____________"

### EV: Continue vs. Stop
| Path | Expected Value | Key Assumption |
|------|---------------|---------------|
| Continue | $X | [what must be true] |
| Quit/pivot | $X | [what must be true] |
| Status quo | $X | [what must be true] |

### Quit Verdict
**Should they quit?** [YES / NO / NOT YET — here are the kill criteria]
**Quitting coach in place?** [YES / NO — recommend one]

---

## The Process Assessment

### Current Process Rating
**Process quality:** STRONG / ADEQUATE / WEAK / ABSENT
[What's working, what's missing]

### CUDOS Compliance
| Norm | Status | Issue |
|------|--------|-------|
| Communism (info sharing) | PASS/FAIL | [issue] |
| Universalism (merit-based) | PASS/FAIL | [issue] |
| Disinterestedness (no conflicts) | PASS/FAIL | [issue] |
| Organized Skepticism (dissent) | PASS/FAIL | [issue] |

### Tilt Assessment
**On tilt?** [YES / NO]
**Source:** [recent event distorting judgment]
**Recommendation:** [proceed / pause / restructure]

### Recommended Protocol
[Specific process improvements, decision journal template, Ulysses contracts]

---

## THE BIAS STACKING ASSESSMENT

This is the Duke question: **how many psychological traps are operating
simultaneously, and do they reinforce each other?**

### Biases Stacking Against Good Process

```
[Bias 1: e.g., sunk cost — $2M already invested]
  + [Bias 2: e.g., identity entanglement — "I'm the founder who believes in this"]
    + [Bias 3: e.g., optimism bias — "this time will be different"]
      + [Bias 4: e.g., status quo bias — "changing course feels like failure"]
        + [Bias 5: e.g., loss aversion — "quitting means losing everything"]
          = [ESCALATION SPIRAL? / MANAGEABLE? / MINIMAL?]
```

**Bias stacking severity:** [NONE / LOW / MODERATE / SEVERE / CRITICAL]

A severe bias stack (4+ reinforcing traps) is Duke's equivalent of a negative
lollapalooza — the psychological forces conspire to prevent rational analysis
and virtually guarantee escalation of commitment.

### Process Strengths Working FOR Good Decision-Making

```
[Strength 1] + [Strength 2] + [Strength 3] = [cumulative process quality]
```

---

## THE VERDICT

### Duke's Three Baskets

**[ ] GOOD BET** — Sound process, positive expected value, calibrated
  probabilities, monkey identified and being tackled, kill criteria defined.
  The decision quality is high regardless of how it turns out.

**[ ] BAD BET** — Process flaw, negative expected value, severe bias stacking,
  or building pedestals while the monkey sits untrained. Walk away or
  fundamentally restructure the decision.

**[ ] FOLD** — Too much Knightian uncertainty to assign meaningful probabilities.
  The reference class is too thin, the information state is too poor, or
  this is outside the circle of competence. Gather more information or
  accept that this is a coin flip, not a calculated bet.

### Verdict: [GOOD BET / BAD BET / FOLD]

**Confidence:** [LOW / MEDIUM / HIGH]

**Reasoning:** [2-3 paragraphs written in Duke's direct, warm, no-BS style.
Reference specific findings from each analyst. Name the biases operating.
If it's a BAD BET, name the process flaw without apology — "you're resulting"
or "you're building pedestals" or "the sunk cost is running the show." If it's
a GOOD BET, explain what makes the process sound and what the kill criteria are.
If it's FOLD, explain what information would move it to GOOD BET.]

### What Annie Would Say

[Write 2-3 sentences in Duke's voice — direct, warm, grounded in poker metaphor,
self-aware about human fallibility. She'd probably say something like "Look, you're
not a bad decision-maker — you're a normal human with normal biases, and those
biases are stacking up on you right now. Here's what I'd do at the table..." She
uses "I'm not sure" as a strength. She tells stories. She's generous but unflinching.]

### If You Proceed: The Protocol

[Based on the Process Architect's assessment and the Pre-Mortem's failure modes,
write 3-5 rules for how to execute this decision well.]

1. **Define your kill criteria NOW** — [specific state + date criteria]
2. **Tackle the monkey first** — [specific hard problem to solve before anything else]
3. **Never [process flaw to avoid]** — because [bias/failure mode it triggers]
4. **Build your decision pod** — [who to include, how to structure]
5. **Journal this decision** — [what to record now, what to check later]
```

## Phase 5: Present & Follow-up

Present the verdict to the user with the key highlights. Don't dump the
whole document — give the verdict, the bias stacking assessment, and the
kill criteria. Let them read the full document.

```
## Duke Verdict: [DECISION] — [GOOD BET / BAD BET / FOLD]

**Bias stacking:** [severity] — [N] traps active [reinforcing/independent]
**Calibration:** [well-calibrated / overconfident / underconfident]
**Monkey status:** [identified and first / pedestal-building / no monkey]
**Kill criteria:** [defined / missing — recommend defining]
**Process quality:** [strong / adequate / weak / absent]

**What Annie would say:** "[direct, warm assessment]"

Full analysis: `thoughts/duke/YYYY-MM-DD-<slug>.md`

Want me to:
1. Deep-dive into any analyst's findings?
2. Re-run with a modified version of the decision?
3. Apply /office-hours to refine the question before re-analyzing?
4. Compare this against an alternative decision? (batch mode)
5. Run /munger to evaluate the business fundamentals alongside the decision process?
```

## Batch Mode

If the user wants to compare multiple decisions:

1. Run the full analysis on each (can parallelize — one team per decision)
2. At the end, produce a comparison:

```
## Duke Decision Leaderboard

| Rank | Decision | Verdict | Bias Stack | Calibration | Monkey | Process |
|------|----------|---------|-----------|-------------|--------|---------|
| 1 | [name] | GOOD BET | LOW (1) | Calibrated | First | Strong |
| 2 | [name] | FOLD | MOD (3) | Overconfident | Unknown | Weak |
| 3 | [name] | BAD BET | SEVERE (5) | Overconfident | Pedestal | Absent |
```

## Scoring Discipline

- **Be Duke, not a consultant.** Duke says most people are overconfident and
  quit too late. If every decision gets GOOD BET, the skill is broken.
- **Cite the source analyst.** Every claim traces to a specific teammate's finding.
- **No process inflation.** A decision with vibes-based probability estimates
  and no kill criteria is not a "strong process" no matter how smart the
  decision-maker sounds.
- **Name the bias.** Don't say "there might be some bias." Say "sunk cost
  fallacy is at 8/10 because you've invested $2M and your identity is fused
  with this project." Be specific and cite evidence.
- **The Fold basket is honest.** When probabilities are fabricated rather than
  calculated — when there's no reference class, no base rate, no feedback loop —
  say so. "I'm not sure" is Duke's highest form of intellectual honesty.

## Important Notes

- **Cost**: This skill spawns 5 agents. It's expensive. Worth it for serious
  decisions, not for casual what-ifs (use /office-hours for that).
- **Sonnet for teammates, Opus for synthesis**: The lead handles the bias
  stacking detection and final verdict — that's where deep reasoning matters.
- **No team? No problem**: If teams aren't enabled, run 5 sequential background
  agents and collect results. Same analysis, just no cross-talk.
- **Pair with other skills**: Run /office-hours first to refine the question,
  then /duke to stress-test the decision process. Run /munger alongside to
  evaluate business fundamentals — Duke is decision *process*, Munger is
  decision *content*. Together they form a complete framework.
- **When NOT to use this**: One-shot existential decisions with no reference
  class (where probability estimates are fabricated); moral choices where duty
  overrides expected value; situations where "process was good" has become an
  excuse for persistent bad results. In those cases, acknowledge the framework's
  limits honestly.
