---
name: kahneman
description: |
  Daniel Kahneman's Cognitive Diagnostic applied to a decision, strategy, or business
  evaluation. Spawns a team of specialist agents — System Detector, Substitution Mapper,
  Prospect Theorist, Noise Auditor, Outside Viewer — who each apply a different lens
  from Kahneman's cognitive architecture to audit the decision for bias, noise, and
  cognitive traps. The lead synthesizes into a contamination assessment: which cognitive
  systems are operating, which substitutions are active, and whether the decision should
  proceed, be corrected, or be restructured. Use when the user says "kahneman this",
  "check my thinking", "am I biased", "audit this decision", "what am I missing",
  or presents any decision, strategy, or evaluation they want cognitively stress-tested.
  Works standalone or as a companion to /munger (Munger evaluates the business;
  Kahneman audits the thinking about the business).
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

# /kahneman -- The Cognitive Diagnostic

Audit any decision through Daniel Kahneman's complete cognitive architecture.
The output should read like what you'd get if Kahneman himself had examined
your thinking process -- quietly demonstrating where System 1 has hijacked
the reasoning, mapping the specific substitutions at play, checking for noise,
and prescribing the corrective tools from his toolkit (premortem, reference
class forecasting, decision hygiene).

This is not a business evaluation tool (use /munger for that). This is a
**thinking evaluation tool**. It answers: "Am I reasoning clearly about this,
or has my cognitive machinery introduced errors I can't see?"

## Core Principles

These are non-negotiable and come from Kahneman's actual framework:

1. **Attribute substitution is the meta-mechanism** -- Most biases are instances
   of System 1 replacing a hard question (target attribute) with an easier one
   (heuristic attribute) without the thinker's awareness. Every analysis must
   map the specific substitutions operating.

2. **WYSIATI governs confidence** -- "What You See Is All There Is." System 1
   builds coherent stories from available evidence and is blind to missing
   evidence. Confidence reflects narrative coherence, not evidence quality.
   Every analysis must identify what information is absent.

3. **Show, don't just tell** -- Kahneman's method is demonstrative. He makes
   you experience the bias before explaining it. Where possible, the analysis
   should reveal the bias in action, not just label it.

4. **Noise equals bias in damage** -- MSE = Bias^2 + Noise^2. Random variability
   in judgment is as destructive as systematic error. Every analysis must check
   for noise, not just bias.

5. **Debiasing individuals is hard; restructuring decisions works** -- Kahneman
   is honest that knowing about biases rarely prevents them. The prescriptions
   are structural: premortems, reference class forecasting, decision hygiene,
   algorithms where possible. Don't just diagnose -- prescribe the structural fix.

6. **Honest about the framework's limits** -- System 1 is sometimes right
   (expert intuition in kind environments). The framework doesn't apply to
   fat-tailed domains, motivated reasoning, group dynamics, or creative work.
   Say so when relevant.

## Invocation

When invoked with `$ARGUMENTS`:

1. If arguments contain a decision, strategy, evaluation, or business idea, proceed
2. If no arguments or vague, ask ONE clarifying question via AskUserQuestion:
   "Describe the decision you're making or evaluating, what options you're
   considering, and what you're currently leaning toward (and why)."
3. Do NOT ask more than one round of questions. Diagnose with what you have.

## Phase 1: Understand the Decision (Lead Only)

Before spawning the team, the lead must establish:

- **The decision**: What is being decided, in one sentence
- **The options**: What alternatives are on the table
- **The current lean**: What the decision-maker is inclined toward
- **The stated reasoning**: Why they lean that way
- **The stakes**: What's at risk if the decision is wrong
- **The context**: Is this being paired with /munger? If so, note that Kahneman
  audits the thinking, not the business itself.

Present this back to the user:

```
## Kahneman Cognitive Diagnostic: [Decision]

I understand the decision as: [one sentence]

Current lean: [what you're inclined toward]
Stated reasoning: [why]

I'm spawning five cognitive analysts, each applying a different layer of
Kahneman's architecture to audit your thinking process.

**The Team:**
1. The System Detector -- which cognitive system is driving this decision
2. The Substitution Mapper -- which hard questions have been replaced by easier ones
3. The Prospect Theorist -- reference points, loss aversion, framing effects
4. The Noise Auditor -- variability, occasion noise, decision hygiene
5. The Outside Viewer -- reference classes, base rates, premortem

Starting diagnostic...
```

## Phase 2: Spawn the Team

```bash
echo "${CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS:-not_set}"
```

If teams are not enabled, fall back to sequential Agent calls (one per analyst)
with `run_in_background: true`, then collect results. The analysis quality should
be identical -- teams just enable cross-talk.

If teams ARE enabled:

```
TeamCreate: team_name = "kahneman-<decision-slug>"
```

Create five tasks and spawn five teammates.

### Teammate 1: The System Detector

Spawn prompt:

```
You are The System Detector on Kahneman's cognitive diagnostic team. Your job
is to determine which cognitive system -- System 1 or System 2 -- is primarily
driving this decision, and what that implies about its reliability.

THE DECISION: [full description]
CURRENT LEAN: [what the person is inclined toward]
STATED REASONING: [why they lean that way]
STAKES: [what's at risk]

Kahneman's System 1 operates automatically, quickly, with no effort and no
sense of voluntary control. System 2 allocates attention to effortful mental
activities. System 2 is lazy -- it often rubber-stamps System 1's output
without checking. "Most of what you think and do originates in your System 1,
but System 2 takes over when things get difficult."

Do this analysis:

1. SYSTEM 1 SIGNATURE DETECTION
   Check for each telltale sign that System 1 is driving:

   | Signal | Present? | Evidence |
   |--------|----------|----------|
   | Speed -- answer appeared immediately, without deliberation | | |
   | Confidence without traceable reasoning -- feels certain but can't show the logic chain | | |
   | Emotional charge -- the answer feels right/wrong/good/bad before reasons emerge | | |
   | Cognitive ease -- everything fits, no friction, coherent story | | |
   | Resistance to alternatives -- other options feel obviously wrong | | |
   | Answer to a different question -- may be responding to an easier question than the one asked | | |

   Rate overall System 1 dominance: 0-10

2. SYSTEM 2 ENGAGEMENT ASSESSMENT
   Check for signs that System 2 is genuinely engaged (not just rationalizing):

   | Signal | Present? | Evidence |
   |--------|----------|----------|
   | Effortful reasoning -- aware of working through sequential steps | | |
   | Genuine doubt -- uncertainty acknowledged before concluding | | |
   | Checking behavior -- actively sought disconfirming evidence | | |
   | Considered alternatives -- genuinely weighed other options | | |
   | Numerical reasoning -- did actual math, not just intuitive estimates | | |
   | Delayed judgment -- took time before forming a view | | |

   Rate genuine System 2 engagement: 0-10

3. THE LAZY SYSTEM 2 CHECK
   Kahneman's critical insight: System 2 often endorses System 1's conclusion
   without actually checking it. Like the bat-and-ball problem -- people who
   can do the math still get it wrong because System 2 never engages.

   Signs that System 2 is rubber-stamping rather than genuinely checking:
   - The "reasoning" was constructed AFTER the conclusion (rationalization)
   - The stated reasons are post-hoc justifications for a gut feeling
   - No alternative was seriously considered -- just rejected for being "wrong"
   - The person can articulate why their lean is right but not why it might be wrong

   Is System 2 genuinely checking, or just rubber-stamping? Rate 0-10
   (0 = pure rubber stamp, 10 = genuine independent check)

4. COGNITIVE EASE VS. STRAIN ASSESSMENT
   Kahneman showed that cognitive ease (fluency, familiarity, good mood)
   reduces vigilance and increases gullibility, while cognitive strain
   (unfamiliarity, difficulty, bad mood) activates System 2.

   What is the cognitive ease level of this decision environment?
   - Is this a familiar type of decision? (ease +)
   - Is the information presented clearly and fluently? (ease +)
   - Is the decision-maker in a good mood or positive state? (ease +)
   - Are there time pressures reducing deliberation? (ease +)
   - Is there any cognitive strain forcing deeper processing? (strain +)

   Overall: Is the environment promoting ease (danger) or strain (safer)?

5. ENVIRONMENT VALIDITY CHECK (Kahneman-Klein Framework)
   Kahneman and Gary Klein agreed: intuition is trustworthy only in "kind"
   learning environments with stable patterns and clear, timely feedback.
   It fails in "wicked" environments with low predictability and delayed
   or absent feedback.

   Classify this decision environment:
   - Kind environment (regular patterns, rapid feedback, expert can learn):
     Trust System 1 more. Examples: chess, firefighting, ER triage.
   - Wicked environment (irregular patterns, delayed/absent feedback, low
     predictability): Distrust System 1. Examples: stock picking, hiring,
     political forecasting, long-range strategy.

   Is this a kind or wicked environment? How confident can we be in
   intuitive judgment here?

6. SYSTEM DIAGNOSIS
   Based on all the above, write a clear diagnostic:
   - Which system is primarily driving this decision?
   - Is that appropriate given the environment?
   - What specific risks does this create?
   - What would genuine System 2 engagement look like here?

Output: Structured diagnostic with specific evidence. Do not speculate --
only flag what the evidence supports. Message teammates if you find something
that changes the diagnostic (e.g., "System 1 is clearly dominant here --
Substitution Mapper should look hard for heuristic replacements").
```

### Teammate 2: The Substitution Mapper

Spawn prompt:

```
You are The Substitution Mapper on Kahneman's cognitive diagnostic team.
Your discipline: attribute substitution -- Kahneman's meta-mechanism that
explains most cognitive biases.

THE DECISION: [full description]
CURRENT LEAN: [what the person is inclined toward]
STATED REASONING: [why they lean that way]
STAKES: [what's at risk]

Kahneman and Frederick (2002): "A judgment is mediated by a heuristic when
the individual assesses a specified TARGET ATTRIBUTE by substituting a related
HEURISTIC ATTRIBUTE that comes more readily to mind." The person answers
the easier question but believes they answered the harder one.

Do this analysis:

1. THE TARGET QUESTION
   What is the actual hard question this decision requires answering?
   State it precisely. Often the person hasn't articulated the real question --
   they've already substituted without knowing it.

   Examples of hard questions:
   - "What is the probability of success for this specific venture?"
   - "What will the actual return on this investment be?"
   - "Is this person the best candidate for this role?"
   - "What is the expected cost and timeline for this project?"

2. THE SUBSTITUTION MAP
   For each major judgment involved in this decision, map the substitution:

   | Hard Question (Target) | Easy Question (Heuristic) | Heuristic Type |
   |----------------------|-------------------------|---------------|
   | [what they should be answering] | [what they're actually answering] | [availability/representativeness/affect/anchoring] |

   Check for each classic substitution:

   a) AVAILABILITY SUBSTITUTION
      Target: "How frequent/probable is this?"
      Heuristic: "How easily does an example come to mind?"

      - Is the decision-maker estimating probability based on vivid examples
        rather than base rates?
      - Has recent news, a dramatic story, or personal experience made certain
        outcomes feel more likely than they are?
      - Are they confusing "I can easily imagine this" with "this is likely"?

   b) REPRESENTATIVENESS SUBSTITUTION
      Target: "What is the probability this belongs to category X?"
      Heuristic: "How similar is this to the prototype of X?"

      - Is the decision-maker judging probability by how well something
        matches a stereotype or mental prototype?
      - Are they ignoring base rates in favor of narrative fit?
      - Is there a "Linda problem" here -- where a more detailed/specific
        scenario feels more probable than a general one?
      - Tom W. effect: Are they rating likelihood based on similarity to
        a description rather than actual frequencies?

   c) AFFECT SUBSTITUTION
      Target: "What is the objective risk/benefit/probability?"
      Heuristic: "How do I FEEL about this?"

      - Has the decision-maker confused their emotional reaction with an
        assessment of probability or risk?
      - Slovic's finding: if you like something, you perceive it as low risk
        AND high benefit. If you dislike it, high risk AND low benefit.
        Is this inverse correlation operating here?
      - Are they paying more for "terrorism insurance" than "any cause of
        death insurance" -- letting fear override probability?

   d) ANCHORING
      Target: "What is the correct value/estimate?"
      Heuristic: "What number was already on the table?"

      - Was there an early number (price, valuation, timeline, percentage)
        that now anchors all subsequent estimates?
      - Even obviously irrelevant anchors affect judgment. The Gandhi question:
        "Was Gandhi older or younger than 144 when he died?" produces higher
        age estimates than anchoring at 35.
      - Has the decision-maker adjusted insufficiently from an initial anchor?

3. WYSIATI ANALYSIS (What You See Is All There Is)
   System 1 builds the best coherent story from available information and
   makes NO allowance for information it doesn't have.

   - What information is PRESENT that's driving the narrative?
   - What information is ABSENT that should be considered?
   - Is the decision-maker's confidence proportional to evidence quality,
     or proportional to narrative coherence?
   - Would the story change dramatically if a specific missing fact were added?

   Kahneman: "We are confident when the story we tell ourselves comes easily
   to mind, with no contradiction and no competing scenario."

   List the top 3-5 pieces of missing information that, if known, might
   change the decision.

4. THE HALO EFFECT CHECK
   System 1 extends a positive (or negative) impression from one domain
   to all domains. First impressions contaminate everything that follows.

   - Has one positive attribute (charismatic founder, beautiful product,
     prestigious brand) contaminated the evaluation of unrelated attributes?
   - Has one negative attribute (bad first impression, ugly interface,
     unknown brand) unfairly suppressed positive evaluation?

5. NARRATIVE FALLACY CHECK
   System 1 prefers coherent stories to messy data. "Stories are simpler
   and more coherent than data justifies; luck is replaced by cause."

   - Is there a compelling narrative driving this decision?
   - Does the narrative assign causation where correlation (or luck) is
     more likely?
   - Is the decision-maker saying "because" when they should say
     "and also, separately"?

6. SUBSTITUTION DIAGNOSIS
   Summarize: What hard questions have been replaced, what heuristic
   attributes are doing the answering, and what would it look like to
   actually answer the original hard questions?

Output: The substitution map with specific evidence. Flag confidence level
for each mapping (certain / probable / possible). Message teammates about
substitutions that affect their analysis.
```

### Teammate 3: The Prospect Theorist

Spawn prompt:

```
You are The Prospect Theorist on Kahneman's cognitive diagnostic team.
Your discipline: prospect theory -- how the human value function distorts
evaluation of gains, losses, risk, and probability.

THE DECISION: [full description]
CURRENT LEAN: [what the person is inclined toward]
STATED REASONING: [why they lean that way]
STAKES: [what's at risk]

Prospect theory (Kahneman & Tversky, 1979/1992) shows that people evaluate
outcomes relative to a reference point, not in absolute terms. The value
function is S-shaped: concave for gains (risk aversion), convex for losses
(risk seeking), and steeper for losses than gains (lambda ~= 2.25).

Do this analysis:

1. REFERENCE POINT IDENTIFICATION
   Everything in prospect theory depends on what the decision-maker treats
   as the reference point -- the "zero" from which gains and losses are measured.

   - What is the implicit reference point for this decision?
   - Is it the status quo? An expected outcome? A social comparison?
   - Has the reference point shifted? (Expected gains become the new baseline,
     making actual outcomes feel like losses)
   - Would reframing the reference point change the decision?

   Example: A founder who raised at a $50M valuation now treats $50M as the
   reference point. A $40M outcome feels like a $10M loss, not a $40M gain
   from zero. The reference point distorts everything downstream.

2. LOSS AVERSION ANALYSIS (lambda ~= 2.25)
   Losses loom roughly 2-2.5x larger than equivalent gains.

   - Is loss aversion driving this decision? Is the person choosing to avoid
     a definite loss rather than pursuing a larger expected-value gain?
   - Is the endowment effect operating? (Valuing what they already have
     more than they'd pay to acquire the same thing)
   - Is status quo bias at work? (Preferring the current state because
     change involves perceived losses that loom larger than gains)
   - Quantify if possible: What is the actual expected value of each option?
     Does loss aversion explain why the person prefers the lower-EV option?

3. FRAMING EFFECTS
   The Asian Disease Problem: identical options presented as "lives saved"
   vs "lives lost" produce opposite risk preferences. 72% chose the certain
   option in the gain frame; 78% chose the gamble in the loss frame.

   - How is this decision currently framed -- as a gain or a loss?
   - Would reframing it (gain <-> loss) change the preference?
   - Is someone (a salesperson, advisor, colleague, media) framing this
     decision in a way that exploits prospect theory?
   - What does the decision look like in BOTH frames?

   Construct both frames explicitly:
   - Gain frame: "If you choose X, you gain [outcome]"
   - Loss frame: "If you choose Y, you lose [outcome]"
   - Do these produce different intuitive preferences? If so, framing
     is contaminating the decision.

4. THE FOUR-FOLD PATTERN
   Prospect theory predicts four distinct risk attitudes:

   |                    | High Probability | Low Probability |
   |--------------------|-----------------|-----------------|
   | Gains              | Risk Averse     | Risk Seeking    |
   | Losses             | Risk Seeking    | Risk Averse     |

   Which cell describes this decision? What risk attitude does prospect
   theory predict, and does the decision-maker's behavior match?

   - Are they paying too much for a small probability of a big gain?
     (lottery behavior -- low-probability gain, risk seeking)
   - Are they paying too much to avoid a small probability of a big loss?
     (insurance behavior -- low-probability loss, risk averse)
   - Are they gambling to avoid a certain loss when they should accept it?
     (high-probability loss, risk seeking -- the "double down" trap)

5. PROBABILITY WEIGHTING
   People overweight small probabilities and underweight large ones.

   - Are small probabilities being treated as larger than they are?
   - Are near-certainties being treated as less certain than they are?
   - Is "possibility effect" (overweighting unlikely outcomes) or
     "certainty effect" (overweighting sure things) at play?

6. NARROW VS. BROAD FRAMING
   Kahneman: System 1 frames "decision problems narrowly, in isolation
   from one another." Broad framing -- evaluating all concurrent decisions
   as a portfolio -- is superior because it reveals that individual losses
   are offset by gains elsewhere.

   - Is this decision being evaluated in isolation?
   - Would viewing it as part of a portfolio of decisions change the calculus?
   - Is loss aversion on this single decision causing the person to miss
     the portfolio-level expected value?

7. EXPERIENCING SELF VS. REMEMBERING SELF
   The remembering self evaluates by peak intensity + ending, ignoring
   duration. It dominates future decisions.

   - Is the decision-maker optimizing for remembered happiness or
     experienced happiness?
   - Is peak-end rule distorting the evaluation of past similar experiences?
   - Duration neglect: is the length of a past experience being ignored
     in favor of its most intense moment?

8. PROSPECT THEORY DIAGNOSIS
   Summarize which prospect theory mechanisms are active, how they're
   distorting the decision, and what the decision looks like when you
   correct for them.

Output: Structured analysis with specific identifications. Quantify
distortions where possible. Message teammates about framing effects
or reference points that change their analysis.
```

### Teammate 4: The Noise Auditor

Spawn prompt:

```
You are The Noise Auditor on Kahneman's cognitive diagnostic team.
Your discipline: noise -- the overlooked dimension of judgment error
from Kahneman, Sibony, & Sunstein's "Noise" (2021).

THE DECISION: [full description]
CURRENT LEAN: [what the person is inclined toward]
STATED REASONING: [why they lean that way]
STAKES: [what's at risk]

Kahneman's key insight: MSE = Bias^2 + Noise^2. Both contribute equally
to total error. Bias gets all the attention; noise is invisible because
it requires comparing multiple judgments on the same case.

"Wherever there is judgment, there is noise, and more of it than you think."

Do this analysis:

1. NOISE VULNERABILITY ASSESSMENT
   How vulnerable is this decision to noise -- random variability unrelated
   to the merits?

   a) OCCASION NOISE (within the same person)
      The same person makes different decisions at different times.
      Factors that introduce occasion noise:
      - Time of day (judges are harsher before lunch)
      - Mood (weather affects stock returns; sports losses affect sentencing)
      - Fatigue / cognitive depletion
      - Recent unrelated events (priming from previous decisions)
      - Physical state (hunger, pain, sleep quality)

      Would this decision likely be different if made:
      - Tomorrow morning instead of today?
      - After a good night's sleep vs. after a stressful week?
      - Before lunch vs. after lunch?
      - On a sunny day vs. a rainy day?

      Rate occasion noise vulnerability: 0-10

   b) LEVEL NOISE (between different judges)
      Different people making this judgment would set different baselines.
      Some are hawks; some are doves.

      If multiple qualified people evaluated this independently:
      - Would they reach the same conclusion?
      - Would they set similar numerical estimates?
      - Insurance underwriters showed 55% median variance on identical cases.
        What would the variance be here?

      Rate level noise vulnerability: 0-10

   c) PATTERN NOISE (different judges weight different factors)
      Two decision-makers might agree on the overall approach but weight
      specific factors very differently.

      - Which factors in this decision are weighted subjectively?
      - Would different decision-makers weight them differently?
      - Is there a "correct" weighting, or is it inherently subjective?

      Rate pattern noise vulnerability: 0-10

2. DECISION HYGIENE AUDIT
   Kahneman prescribes six components of "decision hygiene" to reduce noise:

   | Hygiene Practice | Applied? | Assessment |
   |-----------------|----------|------------|
   | Decomposition -- broken into independent dimensions scored separately | | |
   | Delayed holistic judgment -- global impression formed AFTER dimension scores | | |
   | Independent assessment -- each evaluator judged independently before discussion | | |
   | Relative scales -- comparing options against each other, not rating on absolute scale | | |
   | Statistical aggregation -- averaging independent judgments | | |
   | Information independence -- evaluators didn't see each other's reasoning | | |

   Overall decision hygiene score: 0-10 (0 = no structure, pure gut;
   10 = fully structured with all six practices)

3. THE MEDIATING ASSESSMENTS PROTOCOL (MAP) CHECK
   Kahneman's integrated procedure for complex decisions:
   1. Identify key dimensions
   2. Assign assessors to dimensions independently
   3. Score dimensions independently before combining
   4. Aggregate statistically
   5. Allow final gut-check only AFTER systematic scoring

   Is anything like MAP being used? If not, what would it look like
   for this specific decision?

4. ALGORITHM VS. CLINICAL JUDGMENT
   Kahneman (drawing on Meehl 1954): Even simple linear models outperform
   expert clinical judgment in domains with measurable outcomes.

   - Does an algorithm or decision rule exist for this type of decision?
   - If not, could a simple model be constructed? (Even a weighted checklist
     of 3-5 factors typically beats unaided judgment)
   - Is the decision-maker resisting algorithmic input due to the preference
     for human agency and authenticity in decisions?

5. INTUITION-ON-TOP-OF-STRUCTURE
   Kahneman's finding from hiring research: after systematic scoring of
   independent dimensions, allowing a final intuitive judgment ADDS value.
   But intuition INSTEAD of structure is just noise.

   - Is intuition being used on top of structure (good)?
   - Or instead of structure (dangerous)?

6. NOISE DIAGNOSIS
   Summarize the noise profile:
   - Total noise vulnerability (0-10)
   - Primary noise sources (occasion / level / pattern)
   - Decision hygiene gaps
   - Specific structural improvements that would reduce noise

Output: Structured noise audit. Flag high-noise areas with specific evidence.
Message teammates about noise factors that affect their analysis (e.g.,
"this decision was made under time pressure and fatigue -- System Detector
should factor this into the System 1 dominance assessment").
```

### Teammate 5: The Outside Viewer

Spawn prompt:

```
You are The Outside Viewer on Kahneman's cognitive diagnostic team.
Your discipline: the outside view, reference class forecasting, and
the premortem -- Kahneman's primary corrective tools for cognitive bias.

THE DECISION: [full description]
CURRENT LEAN: [what the person is inclined toward]
STATED REASONING: [why they lean that way]
STAKES: [what's at risk]

Kahneman's most actionable distinction: the Inside View vs. the Outside View.
The inside view focuses on the specific case, building a detailed narrative.
The outside view asks: "What happened when other people made similar decisions?"
The inside view systematically produces overconfidence and the planning fallacy.
The outside view corrects it.

Use WebSearch and WebFetch to ground this analysis in evidence where possible.

Do this analysis:

1. INSIDE VIEW DETECTION
   The decision-maker is taking the inside view when they:
   - Focus on the unique features of their specific situation
   - Build a detailed scenario of how things will unfold
   - Dismiss base rates because "this time is different"
   - Feel that their situation is special/unprecedented
   - Can explain in detail why this will succeed but not why it might fail

   How strongly is the inside view operating? Rate 0-10.
   What specific features of the reasoning mark it as inside-view?

2. REFERENCE CLASS IDENTIFICATION
   Kahneman's corrective: before analyzing the specific case, identify
   the reference class of comparable past situations.

   Step 1: What class does this decision belong to?
   - If it's a business: what category of businesses? What stage?
   - If it's a project: what type of projects? What scale?
   - If it's a hire: what type of role? What level?
   - If it's a strategy: what type of strategic move? In what industry?

   Step 2: What is the base rate for this reference class?
   Use WebSearch to find actual statistics where possible:
   - What % of similar decisions/ventures/projects succeed?
   - What is the median outcome (time, cost, return)?
   - What is the distribution? (The 10th percentile and 90th percentile)

   Step 3: Where does this specific case fall in the distribution?
   - Are there genuine distinguishing features that justify deviation
     from the base rate?
   - How much should we adjust? (Kahneman says: much less than you think)

3. THE PLANNING FALLACY CHECK
   If this decision involves any forecast (timeline, cost, growth, return):

   - Is the estimate based on the inside view (specific plan simulation)?
   - What does the reference class say about actual outcomes?
   - Bent Flyvbjerg's infrastructure research: cost overruns are the norm.
     Is this the kind of estimate where overruns are standard?

   The curriculum developer example: An expert predicted 18-30 months for
   a project. When asked about similar teams, he admitted 40% never finished,
   and successful ones took 7-10 years. He had never used this information.

   Is the decision-maker's estimate similarly disconnected from base rates?

4. THE PREMORTEM
   Kahneman calls this "his most valuable technique" (developed by Gary Klein).

   "Imagine it is one year from now. This decision was implemented. The
   outcome was a disaster. Write a brief history of that disaster."

   Generate 5-7 specific failure scenarios. For each:
   - What went wrong (the failure mode)
   - Why it went wrong (the mechanism)
   - How likely is it (probability estimate)
   - Whether it's preventable (and how)
   - Whether the current plan accounts for it

   Also run the inverse: "The decision succeeded beyond expectations.
   What specifically caused the extraordinary success?" Generate 3-5
   success scenarios.

5. OVERCONFIDENCE CALIBRATION
   Kahneman demonstrated that subjective confidence is a poor predictor
   of accuracy. It reflects cognitive ease and narrative coherence, not
   evidence quality.

   - How confident is the decision-maker? (stated or implied)
   - Is this confidence calibrated to the evidence?
   - What would a well-calibrated probability estimate look like?
   - What should the confidence interval be? (People's 90% confidence
     intervals contain the true answer only about 50% of the time)

6. THE "WHAT WOULD CHANGE YOUR MIND?" TEST
   If nothing could change the decision-maker's mind, that's not conviction --
   it's System 1 lock-in. Genuine System 2 engagement includes knowing
   what evidence would reverse the conclusion.

   - What specific evidence would (or should) change this decision?
   - Has the decision-maker articulated a falsification condition?
   - If no evidence could change their mind, flag this as a diagnostic
     red flag -- it suggests the conclusion preceded the analysis.

7. OUTSIDE VIEW DIAGNOSIS
   Summarize:
   - How dominated is this decision by inside-view thinking?
   - What does the reference class actually predict?
   - What did the premortem reveal?
   - What is a well-calibrated confidence level?

Output: Evidence-based analysis with actual base rates where findable.
The premortem failure scenarios should be specific and vivid -- that's
what makes them useful. Message teammates about base rates and failure
modes that change their analysis.
```

### Spawning

Spawn all five as background agents. Use `model: "sonnet"` for teammates 1-4
(reasoning from principles). Use `model: "sonnet"` for teammate 5 as well
(web research). The lead (Opus) handles synthesis.

Assign tasks immediately after spawning.

## Phase 3: Monitor & Cross-Pollinate

While teammates work:
- Messages from teammates arrive automatically
- If a teammate asks a question, respond with guidance
- If two teammates discover conflicting findings, message both to reconcile
- If a teammate finds something that dramatically changes the picture, alert others

## Phase 4: Synthesize -- The Kahneman Diagnostic

After ALL teammates report back, the lead writes the final analysis.
This is where the cognitive contamination profile emerges.

### The Synthesis Process

1. **Collect** all five analyses
2. **Cross-reference** -- where do multiple lenses identify the same contamination?
3. **Identify the primary contamination** -- what's the dominant cognitive error?
4. **Identify contamination cascades** -- biases that compound each other
   (like Munger's lollapalooza, but for cognitive errors)
5. **Apply the structural correction** -- what would a debiased version of
   this decision look like?
6. **Render the diagnostic verdict** -- Clear, Contaminated, or Compromised

### Output Document

Write to `thoughts/kahneman/YYYY-MM-DD-<decision-slug>.md`:

```markdown
---
date: <ISO 8601>
analyst: Claude Code (kahneman diagnostic skill)
decision: "<decision name>"
verdict: <CLEAR | CONTAMINATED | COMPROMISED>
primary_system: <SYSTEM_1 | SYSTEM_2 | MIXED>
contamination_count: <number of active biases/substitutions>
noise_level: <LOW | MEDIUM | HIGH>
confidence_calibration: <OVER | CALIBRATED | UNDER>
---

# Kahneman Cognitive Diagnostic: [Decision]

> "When faced with a difficult question, we often answer an easier one
> instead, usually without noticing the substitution."
> -- Daniel Kahneman

## The Decision

[One paragraph description]

---

## System Analysis (System Detector)

### Which System Is Driving?

| Signal | System 1 | System 2 |
|--------|----------|----------|
| Speed of conclusion | [assessment] | [assessment] |
| Confidence source | [narrative coherence / evidence chain] | |
| Emotional charge | [present/absent] | |
| Checking behavior | [present/absent] | |
| Alternative consideration | [genuine/pro-forma] | |

**Primary system:** [System 1 / System 2 / Mixed]
**System 2 engagement quality:** [Genuine / Rubber-stamping / Rationalizing]
**Environment type:** [Kind / Wicked] -- [implications for trusting intuition]

---

## The Substitution Map (Substitution Mapper)

### Hard Questions Replaced by Easy Ones

| # | Target Attribute (Hard Question) | Heuristic Attribute (Easy Answer) | Type | Confidence |
|---|--------------------------------|----------------------------------|------|------------|
| 1 | [what should be answered] | [what's actually being answered] | [type] | [H/M/L] |
| 2 | ... | ... | ... | ... |

### WYSIATI: What's Missing

Information present and driving the narrative:
- [list]

Information absent that should be considered:
- [list -- top 3-5 missing pieces]

### Narrative Coherence vs. Evidence Quality
[Assessment of whether confidence reflects evidence or just a good story]

---

## Prospect Theory Analysis (Prospect Theorist)

### Reference Point
**Current reference point:** [what]
**Effect:** [how it distorts gain/loss perception]

### Loss Aversion Impact
[Is loss aversion driving the decision? Quantify if possible]

### Framing
**Current frame:** [gain / loss]
**Alternative frame:** [the other frame]
**Does reframing change the preference?** [yes / no]

### Four-Fold Pattern Position
| | This Decision |
|---|---|
| Domain | [Gains / Losses] |
| Probability | [High / Low] |
| Predicted attitude | [Risk Averse / Risk Seeking] |
| Actual behavior | [matches / contradicts] |

### Experiencing Self vs. Remembering Self
[Is peak-end rule distorting evaluation of past experience relevant to this decision?]

---

## Noise Profile (Noise Auditor)

### Noise Vulnerability

| Type | Score (0-10) | Primary Source |
|------|-------------|---------------|
| Occasion noise | X | [what's causing it] |
| Level noise | X | [what's causing it] |
| Pattern noise | X | [what's causing it] |
| **Total** | **X** | |

### Decision Hygiene Score

| Practice | Applied? | Gap |
|----------|----------|-----|
| Decomposition into dimensions | Y/N | [what's missing] |
| Delayed holistic judgment | Y/N | |
| Independent assessment | Y/N | |
| Relative scales | Y/N | |
| Statistical aggregation | Y/N | |
| Information independence | Y/N | |

**Overall hygiene:** [score] / 10

---

## Outside View (Outside Viewer)

### Inside View Dominance
**Inside view strength:** [score] / 10
**Key inside-view markers:** [list]

### Reference Class
**Class:** [what category this decision belongs to]
**Base rate:** [actual statistics if findable]
**Current estimate vs. base rate:** [gap]

### The Premortem: How This Fails

| # | Failure Mode | Mechanism | Probability | Preventable? |
|---|-------------|-----------|-------------|--------------|
| 1 | [scenario] | [why] | H/M/L | [yes/no + how] |
| 2 | ... | ... | ... | ... |

### Confidence Calibration
**Stated/implied confidence:** [X%]
**Calibrated confidence:** [Y%]
**Gap:** [over/under by how much]

---

## THE CONTAMINATION ASSESSMENT

This is the Kahneman question: **How many independent cognitive errors are
operating, and do they compound each other?**

### Active Contaminations

```
[Bias 1: e.g., availability -- recent vivid example inflating probability]
  + [Bias 2: e.g., anchoring -- first number heard still dominating]
    + [Bias 3: e.g., loss aversion -- framed as loss, driving risk-seeking]
      + [Bias 4: e.g., WYSIATI -- missing information not registered]
        + [Bias 5: e.g., high occasion noise -- decided under fatigue]
          = [CASCADE / INDEPENDENT / MINOR]
```

**Contamination severity:** [NONE / MILD / MODERATE / SEVERE / CRITICAL]

A contamination cascade occurs when multiple biases reinforce each other --
availability makes the scenario vivid, representativeness makes it match
a prototype, affect makes it feel right, WYSIATI makes the missing data
invisible, and cognitive ease prevents System 2 from checking any of it.

### Negative vs. Positive Contamination
Some contaminations push TOWARD the current lean (dangerous -- confirming
the decision for wrong reasons). Others push AWAY (less dangerous --
the decision may be right despite the noise).

Direction of net contamination: [CONFIRMING / OPPOSING / MIXED]

---

## THE VERDICT

### Kahneman's Three Diagnostic Categories

**[ ] CLEAR** -- System 2 is genuinely engaged, major biases accounted for,
  decision hygiene adequate, reference class consulted, confidence calibrated.
  Proceed with the decision as reasoned.

**[ ] CONTAMINATED** -- Specific biases identified but correctable. The decision
  may still be right, but the reasoning process has identifiable errors that
  should be addressed before proceeding. Apply the prescribed corrections
  and re-evaluate.

**[ ] COMPROMISED** -- Multiple compounding biases, high noise, no decision
  hygiene, strong inside-view dominance, uncalibrated confidence. The
  decision-making process is too contaminated for the conclusion to be
  trusted. Restructure the decision process before proceeding.

### Verdict: [CLEAR / CONTAMINATED / COMPROMISED]

**Confidence in diagnostic:** [LOW / MEDIUM / HIGH]

**Primary contamination:** [the single biggest cognitive error identified]

**Reasoning:** [2-3 paragraphs written in Kahneman's precise, demonstrative
style. Show the contamination in action -- don't just label it. Reference
specific findings from each analyst. Be honest about what's clean and
what's contaminated. If it's CLEAR, say what's working. If it's COMPROMISED,
say what specifically makes the process untrustworthy.]

### What Kahneman Would Say

[Write 2-3 sentences in Kahneman's voice -- quiet, precise, slightly rueful,
demonstrative. He shows you the error and lets you sit with it. He doesn't
shout; he makes you notice. He is characteristically pessimistic about
debiasing: "I'm not very optimistic about people's ability to change
their minds." His characteristic move: "Notice that you have just done X.
Now let me explain what happened in your System 1."]

### Prescribed Corrections

Based on the diagnostic, apply these structural fixes:

1. **[Correction 1]** -- [specific structural change to the decision process]
   Addresses: [which contamination]
   Tool: [premortem / reference class / decision hygiene / reframing / etc.]

2. **[Correction 2]** -- ...

3. **[Correction 3]** -- ...

### The Debiased Decision

If the prescribed corrections were applied, what would the decision
look like? Would the conclusion change, or would it be the same
conclusion reached through a cleaner process?

[Describe what the decision looks like after correction. Sometimes the
same decision is reached -- but with calibrated confidence and structural
support. Sometimes the corrections reveal that the decision should change.]

### Decision Rules (If You Proceed)

Based on the diagnostic, these rules protect against the identified
contaminations:

1. **Never [action that triggers identified bias]** -- because [contamination mode]
2. **Always [structural check]** -- because [noise/bias source]
3. ...
```

## Phase 5: Present & Follow-up

Present the verdict to the user with key highlights:

```
## Kahneman Diagnostic: [DECISION] -- [CLEAR / CONTAMINATED / COMPROMISED]

**Primary system:** [System 1 / System 2] driving the decision
**Environment:** [Kind / Wicked]
**Substitutions found:** [N] hard questions replaced by easier ones
**Contaminations:** [N] active biases, [cascade/independent]
**Noise level:** [LOW / MEDIUM / HIGH]
**Confidence:** Stated [X%], Calibrated [Y%]

**What Kahneman would say:** "[pithy diagnostic observation]"

**Top 3 corrections:**
1. [most important structural fix]
2. [second most important]
3. [third]

Full diagnostic: `thoughts/kahneman/YYYY-MM-DD-<slug>.md`

Want me to:
1. Deep-dive into any analyst's findings?
2. Run the premortem in more detail?
3. Apply /munger to evaluate the business itself (not the thinking)?
4. Re-run the diagnostic on a modified version of the decision?
5. Compare the cognitive profile across multiple decisions? (batch mode)
```

## Batch Mode

If the user wants to compare cognitive profiles across multiple decisions:

1. Run the full diagnostic on each
2. Produce a comparison:

```
## Kahneman Diagnostic Comparison

| | Decision A | Decision B | Decision C |
|---|---|---|---|
| Verdict | CONTAMINATED | CLEAR | COMPROMISED |
| Primary system | System 1 | System 2 | System 1 |
| Substitutions | 3 | 0 | 5 |
| Noise level | MEDIUM | LOW | HIGH |
| Confidence gap | +30% over | Calibrated | +50% over |
| Top contamination | Availability | -- | Cascade |
```

## Scoring Discipline

- **Be Kahneman, not a therapist.** Kahneman is precise, not reassuring. If the
  thinking is contaminated, say so clearly. Don't soften the diagnosis.
- **Cite the source analyst.** Every contamination traces to a specific teammate.
- **No optimism about debiasing.** Kahneman: "Understanding a bias does not
  make you immune to it." Prescribe structural fixes, not "just be more aware."
- **Honest about framework limits.** If this is a kind environment where
  expert intuition is reliable, say so. If it's a domain where Kahneman's
  framework doesn't apply (fat tails, motivated reasoning, creative work),
  say so. Don't force every decision through a bias lens.
- **The CLEAR verdict is real.** Not every decision is contaminated. Good
  decision-making exists. If the thinking is clean, say so and explain why.

## Framework Limitations (Always Acknowledge When Relevant)

From Kahneman's own work and his critics:

1. **Replication concerns** -- Social priming effects (Chapter 4 of TF&S) have
   largely failed to replicate. Ego depletion is effectively debunked. The core
   framework (prospect theory, anchoring, availability, representativeness,
   planning fallacy) remains robust. This skill uses only the robust components.

2. **Gigerenzer's critique** -- In uncertain environments, simple heuristics
   sometimes outperform complex analysis. "Less can be more." Don't treat
   all heuristic reasoning as error.

3. **Motivated reasoning gap** -- Kahneman's framework underestimates how
   System 2 can be recruited in service of System 1 conclusions. Dan Kahan
   showed that the best System 2 thinkers show the MOST ideologically
   motivated cognition on political topics. If the decision involves identity
   or tribal loyalty, this framework may underdiagnose the contamination.

4. **Fat tails / Extremistan** -- Prospect theory was built on lab gambles
   with known payoffs. Kahneman himself acknowledged it doesn't apply to
   fat-tailed domains where rare events dominate. In such domains, the
   prescriptions (use base rates, calibrate probabilities) can be dangerous.

5. **Individual only** -- The framework has no theory of group decision-making,
   institutional design, or power dynamics. For those, complement with other tools.

## Pairing With Other Skills

- **Run /munger first, then /kahneman**: Munger evaluates the business opportunity;
  Kahneman audits whether you're thinking clearly about Munger's findings.
  They compose perfectly: "Is this a good business?" + "Am I analyzing this clearly?"

- **Run /kahneman before major decisions**: Use as a pre-decision checklist
  regardless of the domain. Works for hiring, strategy, investment, product
  decisions, partnerships.

- **Run /garrytan to refine the idea, /munger to evaluate it, /kahneman to
  audit your thinking about it**: The full stack.

## Important Notes

- **Cost**: This skill spawns 5 agents. Use for decisions that matter, not
  casual brainstorming.
- **Sonnet for teammates, Opus for synthesis**: The lead handles contamination
  cascade detection and the final diagnostic -- that requires deep reasoning.
- **No team? No problem**: If teams aren't enabled, run 5 sequential background
  agents. Same analysis, just no cross-talk.
- **Not a replacement for domain expertise**: This tool audits your cognitive
  process, not the substance of the decision. You still need domain knowledge
  to make good decisions -- this just checks that your cognitive machinery
  isn't sabotaging you.
