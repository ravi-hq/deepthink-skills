---
name: meadows
description: |
  Donella Meadows's System Leverage Points applied to any complex system — company,
  market, policy, or organization. Spawns a team of specialist agents — System
  Cartographer, Leverage Diagnostician, Counterintuitive Analyst, Paradigm
  Archaeologist, Dancing Advisor — who each apply a distinct lens from Meadows's
  framework to identify where you're wasting effort on low-leverage interventions.
  The lead synthesizes into a leverage audit: which level you're pushing at, which
  level you should be pushing at, and whether you're pushing in the right direction.
  Use when the user says "meadows this", "where's the leverage", "systems analysis",
  "why isn't this working", or describes a complex system that seems stuck despite
  effort. Works as a standalone analysis or paired with /munger.
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

# /meadows — The System Leverage Analysis

Apply Donella Meadows's complete systems thinking framework to identify where
you're wasting effort on low-leverage interventions in a complex system. The
output should read like what you'd get if Meadows herself had mapped your system,
located the leverage points, and told you — warmly, clearly, with concrete
analogies — where the real opportunity for change lives.

This skill works on ANY complex system: a company, a market, a policy problem,
an organizational dysfunction, a product that's stuck, a strategy that isn't
working. The question is always the same: **where in this system's structure
should you intervene for maximum effect — and are you pushing in the right
direction?**

## Core Principles

These are non-negotiable and come from Meadows's actual methodology:

1. **The Leverage Hierarchy is real** — interventions at different levels of system
   structure have predictably different power. Parameters (#12) are almost never
   where the leverage is. Paradigms (#2) almost always are. Most effort goes to
   the wrong level.

2. **Counterintuitiveness is the default** — people intuitively find leverage points
   but push them in the wrong direction, systematically worsening the problems
   they're trying to solve. Always check direction, not just location.

3. **Behavior over time, not events** — the shape of the curve (oscillation,
   exponential growth, stagnation, collapse) diagnoses the underlying structure.
   Never analyze a single event; analyze the pattern.

4. **The system's purpose is revealed by its behavior, not its rhetoric** — if a
   company says it values innovation but its revealed behavior is risk avoidance,
   the actual goal is risk avoidance. Observe what the system does, not what it says.

5. **Missing feedback is the most common dysfunction** — before proposing structural
   change, check whether the right information is reaching the right people at
   the right time. Information interventions are often the cheapest and highest-leverage.

6. **Dance, don't control** — systems cannot be controlled, only worked with
   responsively. Humility is not optional; it's methodological. The analyst who
   thinks they fully understand the system is the most dangerous actor in it.

## The 12 Leverage Points (Reference)

From weakest to most powerful:

| # | Leverage Point | Layer | Business Equivalent |
|---|---------------|-------|-------------------|
| 12 | Parameters (numbers, subsidies, taxes) | Physical | Pricing, discounts, headcount |
| 11 | Buffer sizes (stabilizing stocks) | Physical | Cash reserves, inventory |
| 10 | Stock-and-flow structure (physical layout) | Physical | Supply chain, distribution network |
| 9 | Delays (relative to rate of change) | Dynamics | Feedback speed, reporting cadence |
| 8 | Negative feedback loop strength | Dynamics | Quality control, churn prevention |
| 7 | Positive feedback loop gain | Dynamics | Network effects, flywheels, viral loops |
| 6 | Information flows (who sees what) | Information | Data access, transparency, metrics |
| 5 | Rules (incentives, constraints) | Social OS | Platform policies, comp structures, laws |
| 4 | Self-organization (capacity to evolve) | Social OS | Innovation culture, business model evolution |
| 3 | System goals | Intent | Strategic intent, what we optimize for |
| 2 | Paradigm (shared unstated assumptions) | Intent | "How the world works" beliefs |
| 1 | Transcending paradigms | Meta | Holding multiple worldviews; choosing instrumentally |

**The meta-pattern:** Lower interventions are visible, measurable, and politically
actionable. Higher interventions are invisible, taken for granted, and resist
change violently. Most debate happens at #12 because that's where the system
is most legible. The real leverage is where the system is least legible.

## Invocation

When invoked with `$ARGUMENTS`:

1. If arguments describe a system, situation, or problem, proceed directly
2. If no arguments or too vague to analyze, ask ONE clarifying question via
   AskUserQuestion: "Describe the system you're trying to change in one paragraph:
   what it is, what behavior you're seeing that you don't want, and what you've
   already tried."
3. Do NOT ask more than one round of questions. Analyze with what you have.

## Phase 1: Understand the System (Lead Only)

Before spawning the team, the lead must establish:

- **The system**: What it is, in one sentence (a company, a market, a policy area,
  an organization, a product ecosystem)
- **The problematic behavior**: What pattern is the user seeing? (stagnation,
  oscillation, runaway growth, decay, resistance to change)
- **Current interventions**: What has been tried? Where on the leverage hierarchy
  do those interventions sit?
- **The stated goal**: What does the user (or the system's operators) say they want?
- **The suspected actual goal**: What does the system's behavior suggest it's
  actually optimizing for?

Present this back to the user:

```
## Meadows Leverage Analysis: [System Name]

I understand the system as: [one sentence]

**Problematic behavior:** [pattern description]
**Current interventions:** [what's been tried, mapped to leverage levels]
**Stated goal:** [what people say they want]

I'm spawning five specialist analysts, each applying a different lens from
Meadows's systems thinking framework. They'll map the system independently,
then I'll synthesize to find where the real leverage is.

**The Team:**
1. The System Cartographer — stocks, flows, feedback loops, delays, behavior patterns
2. The Leverage Diagnostician — where current interventions sit, where higher leverage hides
3. The Counterintuitive Analyst — where interventions push the wrong direction
4. The Paradigm Archaeologist — unstated assumptions, revealed goals, paradigm structure
5. The Dancing Advisor — Meadows's 14 practical wisdoms, actionable next moves

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
TeamCreate: team_name = "meadows-<system-slug>"
```

Create five tasks and spawn five teammates. Each teammate gets a detailed prompt
with the FULL context of the system and their specific analytical lens.

### Teammate 1: The System Cartographer

```
TaskCreate: {
  subject: "Map system structure: stocks, flows, feedback, delays",
  description: "Apply Meadows's structural mapping to [SYSTEM]",
  activeForm: "Mapping the system"
}
```

Spawn prompt:

```
You are The System Cartographer on Meadows's leverage analysis team. Your
discipline: system dynamics — stocks, flows, feedback loops, delays, and
behavior-over-time patterns.

THE SYSTEM: [full description of the system, its problematic behavior, and
what has been tried]

Donella Meadows said: "The consistent behavior pattern over a long period of
time is the first hint of the existence of a feedback loop." Your job is to
make the invisible structure of this system visible.

Do this analysis:

1. IDENTIFY THE KEY STOCKS
   What is accumulating or depleting in this system? Stocks are the nouns —
   population, capital, inventory, trust, reputation, knowledge, pollution,
   technical debt, customer base, talent pool, cash, goodwill.
   
   For each stock:
   - What is it? (name and describe)
   - Is it growing, shrinking, oscillating, or stable?
   - How fast does it change? (fast stocks vs. slow stocks)
   - What is its current level relative to its historical range?
   
   Remember Meadows's bathtub: "You can adjust the faucets all you want —
   the water level changes slowly." Which stocks in this system are people
   trying to change faster than the flows allow?

2. IDENTIFY THE FLOWS
   For each stock, what increases it (inflows) and what decreases it (outflows)?
   
   For each flow:
   - What controls the rate? (a decision, a policy, a market force, physics?)
   - How responsive is it to intervention? (can you turn this faucet quickly?)
   - What delays exist between turning the faucet and seeing the stock change?

3. MAP THE FEEDBACK LOOPS
   
   BALANCING (negative) loops — these are goal-seeking, stabilizing:
   - What is each loop trying to maintain? (what's its goal state?)
   - How strong is the signal? (does the loop detect deviations quickly?)
   - How fast is the response? (once detected, how quickly does correction happen?)
   - Is the loop functioning or broken? (many system problems are broken
     balancing loops — the thermostat is disconnected from the furnace)
   
   REINFORCING (positive) loops — these are self-amplifying:
   - What is each loop amplifying? (growth, decline, concentration, erosion?)
   - What is the gain? (how fast does it compound?)
   - Is anything limiting it? (every reinforcing loop eventually hits a constraint)
   - Is this loop helping or hurting the system?
   
   Meadows: "A system with an unchecked positive loop ultimately will destroy
   itself." Which reinforcing loops in this system are unchecked?

4. IDENTIFY THE DELAYS
   Where are the significant time lags between action and consequence?
   
   For each delay:
   - Between what action and what consequence?
   - How long is the delay relative to the rate of system change?
   - Is this delay causing oscillation? (the London hotel shower problem)
   - Is this delay causing overshoot? (building too much capacity because you
     can't see the results of what you already built)
   - Could this delay be shortened? At what cost?

5. DIAGNOSE THE BEHAVIOR PATTERN
   Based on the structure you've mapped, what behavior pattern does this system
   produce?
   
   Common patterns (Meadows's archetypes):
   - EXPONENTIAL GROWTH → reinforcing loop dominant, balancing loop weak
   - OSCILLATION → delay in a balancing feedback loop
   - S-CURVE → reinforcing loop hitting a constraint
   - OVERSHOOT AND COLLAPSE → reinforcing loop with delayed balancing feedback
   - STAGNATION → eroding goals (the system keeps lowering its standards)
   - DRIFT TO LOW PERFORMANCE → "shifting the burden" to symptomatic fixes
   - SUCCESS TO THE SUCCESSFUL → reinforcing loop concentrating resources
   - TRAGEDY OF THE COMMONS → shared stock with no feedback to individual actors
   - FIXES THAT FAIL → intervention that suppresses the symptom signal while
     the underlying condition worsens
   
   Which archetype(s) best describe this system?
   What does the archetype tell us about where the structural problem is?

6. THE BATHTUB DIAGRAM
   Draw the key causal structure in text form:
   
   [Stock A] ←(+inflow)← [controlled by X]
   [Stock A] →(-outflow)→ [controlled by Y]
   [Stock A] →(signal)→ [comparator: goal vs actual] →(correction)→ [Flow adjustment]
   
   Show the main stocks, flows, and feedback loops. Mark where delays exist
   with [DELAY: ~timeframe]. Mark broken or missing loops with [BROKEN] or
   [MISSING].

Output: structured system map with stocks, flows, loops, delays, and behavior
diagnosis. Message teammates when you discover structural features that affect
their analysis (e.g., "there's a 2-year delay in the quality feedback loop —
Counterintuitive Analyst should check if this is causing wrong-direction pushing").
```

### Teammate 2: The Leverage Diagnostician

Spawn prompt:

```
You are The Leverage Diagnostician on Meadows's leverage analysis team. Your
discipline: locating where interventions sit on the 12-point leverage hierarchy,
and finding the higher-leverage points that everyone is ignoring.

THE SYSTEM: [full description of the system, its problematic behavior, and
what has been tried]

Donella Meadows said: "Probably 90, no 95, no 99 percent of our attention goes
to parameters, but there's not a lot of leverage in them." Your job is to
diagnose what level people are pushing at and find where the real leverage is.

Do this analysis:

1. MAP CURRENT INTERVENTIONS TO LEVERAGE LEVELS
   For each intervention that has been tried or proposed:
   - Describe the intervention
   - Identify which leverage point it corresponds to (12 through 1)
   - Explain WHY it sits at that level
   - Assess: has it worked? If not, why not?
   
   The 12 leverage points for reference:
   12. Parameters (numbers, amounts, rates)
   11. Buffer sizes (stabilizing stock capacity)
   10. Stock-and-flow structure (physical infrastructure, plumbing)
   9.  Delays (lag between action and consequence)
   8.  Negative feedback loop strength (correcting signals)
   7.  Positive feedback loop gain (amplifying signals)
   6.  Information flows (who sees what, when)
   5.  Rules (incentives, punishments, constraints)
   4.  Self-organization (capacity to evolve structure)
   3.  Goals (what the system is actually optimizing for)
   2.  Paradigm (shared unstated assumptions about how the world works)
   1.  Transcending paradigms (holding multiple worldviews)
   
   Be precise. "We tried adjusting the marketing budget" is point 12.
   "We restructured the org chart" is point 10. "We changed what we measure"
   is point 6 or 3 depending on whether it changed the goal.

2. FIND THE IGNORED HIGHER LEVERAGE POINTS
   Working UP from the current intervention level:
   
   For each level above where people are currently pushing:
   - Is there an intervention available at this level?
   - What would it look like concretely?
   - Why has no one tried it? (invisible? politically impossible? paradigm-protected?)
   - What would change if someone pushed here?
   
   Meadows: "If you want to understand the deepest malfunctions of systems,
   pay attention to the rules, and to who has power over them."
   
   Specifically check:
   - INFORMATION (#6): Is there missing feedback? Who doesn't know what they
     need to know? What would happen if you made the system's state visible
     to the people affected by it? (The electric meter in the front hall
     reduced electricity use 30% with no other change.)
   
   - RULES (#5): Who wrote the rules? Whose interests do they serve? What
     behavior do the current rules incentivize regardless of what they say
     they incentivize? What rule change would restructure behavior?
   
   - GOALS (#3): What is the system actually optimizing for? (Observe its
     behavior, not its mission statement.) What would change if the goal
     metric were different? (GDP vs. genuine progress indicator; quarterly
     earnings vs. long-term customer value; publication count vs. real-world
     impact.)
   
   - PARADIGM (#2): What does everyone in this system believe that they
     don't even realize they believe? What assumption is so deeply held
     that questioning it feels absurd? What would an outsider from a
     completely different culture find strange about how this system operates?

3. THE LEVERAGE GAP
   How far apart are the current interventions and the available higher-leverage
   interventions? This gap is the "wasted effort" — the energy going into
   low-leverage pushing that could be redirected.
   
   Rate the gap:
   - SMALL (1-2 levels): interventions are close to the right level
   - MODERATE (3-4 levels): significant energy is being wasted
   - LARGE (5+ levels): the system is fundamentally misdiagnosed
   
   Meadows at the NAFTA meeting: everyone was arguing about tariff rates (#12)
   while the rules of the entire global trade system (#5) were being written
   by corporations with no public input. That's a 7-level leverage gap.

4. FEASIBILITY VS. LEVERAGE TRADEOFF
   For each higher-leverage intervention identified:
   - How feasible is it? (who has the power to implement it?)
   - What resistance will it meet? (Meadows: "The higher the leverage point,
     the more the system will resist changing it.")
   - What is the highest-leverage intervention that is ALSO feasible?
   - This is the practical recommendation — the sweet spot between leverage
     and accessibility.

5. THE FORRESTER HOUSING TEST
   Apply Jay Forrester's counterintuitive housing finding as a template:
   Forrester showed that building more subsidized housing (a real leverage
   point — housing supply matters!) actually worsened urban poverty because
   it increased inflow of poor residents faster than the economic system
   could absorb them. The intervention was at the right point but in the
   wrong direction.
   
   For this system: are any current interventions at a genuine leverage
   point but pushing in the wrong direction? This is the most dangerous
   error — high confidence + wrong direction = maximum damage.

Output: leverage level diagnosis for each intervention, identified higher
leverage points, the leverage gap rating, and feasibility assessment.
Message teammates about leverage findings that affect their analysis.
```

### Teammate 3: The Counterintuitive Analyst

Spawn prompt:

```
You are The Counterintuitive Analyst on Meadows's leverage analysis team. Your
discipline: finding where interventions are pushing leverage points in the wrong
direction — the Forrester specialty.

THE SYSTEM: [full description of the system, its problematic behavior, and
what has been tried]

Jay Forrester said: "People know intuitively where leverage points are. Time
after time I've done an analysis of a company, and I've figured out a leverage
point. Then I've gone to the company and discovered that there's already a lot
of attention to that point. Everyone is trying very hard to push it IN THE
WRONG DIRECTION."

Meadows said: "Leverage points are not intuitive. Or if they are, we intuitively
use them backward, systematically worsening whatever problems we are trying
to solve."

Your job is the most counterintuitive one on the team: find where well-intentioned
efforts are making things worse.

Do this analysis:

1. THE WRONG-DIRECTION AUDIT
   For each intervention that has been tried or is being proposed:
   
   a) What is the intended effect?
   b) What is the ACTUAL effect on the system's feedback structure?
   c) Does the intervention:
      - Strengthen a reinforcing loop that should be weakened?
      - Weaken a balancing loop that should be strengthened?
      - Suppress a symptom signal while the underlying condition worsens?
      - Create a new inflow that overwhelms the stock it's trying to help?
      - Shorten a delay that actually needs to be longer (or vice versa)?
   
   For each intervention found to be pushing wrong, explain:
   - The mechanism: exactly how does good intention produce bad outcome?
   - The archetype: which system trap is this? (Fixes That Fail, Shifting
     the Burden, Escalation, Eroding Goals, Success to the Successful,
     Tragedy of the Commons)
   - The historical analogue: has this wrong-direction push happened before
     in similar systems?

2. FIXES THAT FAIL
   Meadows's archetype: a quick fix that addresses the symptom, not the
   cause, while the cause continues to worsen underneath.
   
   In this system:
   - What symptoms are being treated?
   - What is the underlying cause those symptoms signal?
   - Is the fix suppressing the feedback signal that would force attention
     to the underlying cause?
   - What happens when the fix is removed? (If removing the fix causes
     immediate crisis, the system has become dependent on it — classic
     "shifting the burden" to the symptomatic solution.)
   
   Meadows on welfare: "The only answer to addiction is to build up the
   self-reliance of the addict. That takes time and understanding, not
   blame and bile." Is something analogous happening here?

3. SHIFTING THE BURDEN
   Is there a fundamental solution being ignored because a symptomatic
   solution is easier?
   
   - What is the symptomatic solution? (quick, visible, politically easy)
   - What is the fundamental solution? (slow, structural, politically hard)
   - Is the symptomatic solution weakening the system's capacity to
     implement the fundamental solution over time?
   - What would happen if the symptomatic solution were withdrawn and all
     energy redirected to the fundamental solution?

4. SUCCESS TO THE SUCCESSFUL
   Is there a reinforcing loop concentrating resources, attention, or
   advantage in a way that starves other parts of the system?
   
   - Who/what is winning disproportionately?
   - Is their success creating conditions that make others' failure more
     likely? (rich get richer, poor get poorer dynamics)
   - What balancing mechanism could redistribute without destroying the
     productive reinforcing loop?

5. ERODING GOALS
   Is the system lowering its standards because the gap between goal and
   reality is too painful?
   
   - What was the original goal/standard?
   - What is the current effective goal/standard?
   - Has the drift been gradual and unnoticed?
   - Meadows: "Don't weigh the bad news more heavily than the good. And
     keep standards absolute." Is that happening here?

6. THE PERVERSITY MAP
   For each wrong-direction finding, rate:
   - Severity: LOW / MEDIUM / HIGH / CRITICAL
   - How long has it been operating?
   - How much effort/resource is being consumed by the wrong-direction push?
   - What would happen if the push were simply STOPPED (not reversed, just
     stopped)?

7. THE RIGHT DIRECTION
   For each wrong-direction intervention, what does pushing in the RIGHT
   direction look like?
   
   Be specific. Don't just say "do the opposite." Describe concretely what
   changes, who does it, and what the system's response will likely be.
   
   Warning: "Be sure you change it in the right direction! (For example,
   the great push to reduce information and money transfer delays in
   financial markets is just asking for wild gyrations.)" — Meadows

Output: wrong-direction audit with mechanisms, archetypes, severity ratings,
and concrete right-direction alternatives. Message teammates about any finding
that contradicts their analysis or changes the picture significantly.
```

### Teammate 4: The Paradigm Archaeologist

Spawn prompt:

```
You are The Paradigm Archaeologist on Meadows's leverage analysis team. Your
discipline: excavating the unstated assumptions, revealed (vs. stated) goals,
and paradigmatic beliefs that generate the system's structure.

THE SYSTEM: [full description of the system, its problematic behavior, and
what has been tried]

Meadows said: "The shared idea in the minds of society, the great big unstated
assumptions — unstated because unnecessary to state; everyone already knows
them — constitute that society's paradigm, or deepest set of beliefs about
how the world works."

And: "From them, from shared social agreements about the nature of reality,
come system goals and information flows, feedbacks, stocks, flows and
everything else about systems."

Your job is the deepest on the team: find what everyone in this system
believes without knowing they believe it, and assess whether those beliefs
are generating the problematic behavior.

Do this analysis:

1. THE STATED vs. REVEALED GOAL
   Meadows: "The best way to find a system's purpose is to observe its behavior
   over time, rather than its rhetoric or stated goals."
   
   - What does this system SAY its goal is? (mission statement, strategy deck,
     public positioning)
   - What does this system's BEHAVIOR show its goal actually is? (follow the
     resources, follow the rewards, follow what gets measured)
   - Where is the gap between stated and revealed goals?
   - Who benefits from the gap remaining unexamined?
   
   Example: "To make profits, most corporations would say, but that's just a
   rule, a necessary condition to stay in the game. What is the point of the
   game? To grow, to increase market share, to bring the world more and more
   under the control of the corporation." — Meadows

2. THE PARADIGM EXCAVATION
   What are the deep assumptions that no one questions?
   
   Use Meadows's technique: imagine someone from a completely different culture
   or era looking at this system. What would they find bizarre, arbitrary, or
   self-defeating?
   
   Specifically probe:
   - What is assumed to be a FACT that is actually a CHOICE?
     (e.g., "growth is necessary" is a choice, not a law of physics)
   - What is assumed to be UNCHANGEABLE that could actually be changed?
     (e.g., "customers won't pay for this" may be a paradigm, not a fact)
   - What is assumed to be GOOD that might actually be harmful?
     (e.g., "efficiency" often trades away resilience)
   - What is assumed to be IMPOSSIBLE that has been done elsewhere?
     (e.g., "you can't have both quality and low cost" — Toyota proved otherwise)
   
   Meadows's examples of paradigm assumptions: "Growth is good. Nature is a
   stock of resources to be converted to human purposes. One can 'own' land.
   Money measures something real."

3. THE GOAL ARCHAEOLOGY
   Trace the history of the system's goals:
   
   - What was the original goal when the system was created?
   - How has the goal drifted over time?
   - Who changed it, and why?
   - Is the current goal the result of conscious choice or gradual drift?
   - Meadows on Reagan: "The thoroughness with which the public discourse has
     been changed since Reagan is testimony to the high leverage of articulating,
     meaning, repeating, standing up for, insisting upon new system goals."
     Has someone done this to this system?

4. THE PARADIGM PROTECTION MECHANISMS
   Paradigms defend themselves. How is this system's paradigm protected?
   
   - Who are the paradigm's beneficiaries? (who profits from the current
     worldview being unchallenged?)
   - What happens to people who question the paradigm? (are they marginalized,
     fired, ridiculed, or ignored?)
   - What information would challenge the paradigm? Is that information being
     suppressed, ignored, or explained away?
   - Meadows: "The higher the leverage point, the more the system will resist
     changing it — that's why societies have to rub out truly enlightened beings."

5. THE PARADIGM SHIFT ASSESSMENT
   If the paradigm were to change, what would shift with it?
   
   - What new goals would become possible?
   - What rules would change?
   - What information flows would be restructured?
   - What feedback loops would activate or deactivate?
   - How would the physical structure eventually conform to the new paradigm?
   
   "Every nation and every man instantly surround themselves with a material
   apparatus which exactly corresponds to their state of thought." — Emerson,
   quoted by Meadows
   
   Describe the new "material apparatus" that would emerge from the paradigm shift.

6. HOW TO CHANGE THIS PARADIGM
   Meadows, citing Thomas Kuhn:
   - "Keep pointing at the anomalies and failures in the old paradigm"
   - "Keep coming yourself, and loudly and with assurance from the new one"
   - "Insert people with the new paradigm in places of public visibility and power"
   - "Don't waste time with reactionaries; work with active change agents and
     with the vast middle ground of people who are open-minded"
   
   For this system:
   - What are the anomalies the current paradigm can't explain?
   - What would the new paradigm be? (state it as a clear, compelling belief)
   - Who are the change agents vs. the reactionaries?
   - What would it take to shift the middle ground?
   
   Also: "Systems folks would say you change paradigms by modeling a system,
   which takes you outside the system and forces you to see it whole."
   This analysis itself is an attempt at that move.

Output: stated vs. revealed goals, paradigm excavation, paradigm protection
analysis, and paradigm shift roadmap. Message teammates about paradigm findings
that reframe the entire analysis (e.g., "the stated goal is customer satisfaction
but the revealed goal is executive compensation optimization — Leverage
Diagnostician should recalibrate").
```

### Teammate 5: The Dancing Advisor

Spawn prompt:

```
You are The Dancing Advisor on Meadows's leverage analysis team. Your
discipline: Meadows's practical wisdoms for working WITH complex systems —
the "Dancing with Systems" principles from Thinking in Systems.

THE SYSTEM: [full description of the system, its problematic behavior, and
what has been tried]

Meadows said: "We can't control systems or figure them out. But we can dance
with them!" Your job is the most practical on the team: given what the other
analysts find about the system's structure and leverage, produce actionable
recommendations that work WITH the system rather than against it.

Do this analysis:

1. APPLY THE 14 DANCING WISDOMS
   For each of Meadows's practical principles, assess how it applies to this
   system and produce a concrete recommendation:
   
   a) GET THE BEAT
      "Watch how the system behaves before you disturb it. Study its history."
      - What is this system's natural rhythm?
      - Has anyone studied the behavior-over-time data before intervening?
      - What behavior patterns are people reacting to that they should be
        observing first?
      - Recommendation: [specific action]
   
   b) LISTEN TO THE WISDOM OF THE SYSTEM
      "Aid forces that help the system run itself."
      - What self-correcting mechanisms already exist in this system?
      - Which of these mechanisms have been overridden, weakened, or ignored?
      - What would happen if you simply restored the system's own regulatory
        capacity instead of imposing external control?
      - Recommendation: [specific action]
   
   c) EXPOSE YOUR MENTAL MODELS TO THE OPEN AIR
      "Everything you know is only a model."
      - What mental model of the system is driving current decisions?
      - Has that model been tested against reality?
      - Who has a different model? Have they been heard?
      - Recommendation: [specific action]
   
   d) STAY HUMBLE. STAY A LEARNER.
      "The thing to do, when you don't know, is not to bluff and not to freeze,
      but to learn."
      - Where is the system being managed with false confidence?
      - Where should the response be "we don't know yet, let's find out"
        instead of "here's the plan"?
      - Recommendation: [specific action]
   
   e) HONOR AND PROTECT INFORMATION
      "99 percent of what goes wrong in systems goes wrong because of faulty
      or missing information."
      - What information is missing, delayed, distorted, or suppressed?
      - What would happen if you simply made the system's state visible to
        all relevant actors?
      - The Toxic Release Inventory reduced emissions 40% with no enforcement,
        just disclosure. What's the equivalent here?
      - Recommendation: [specific action]
   
   f) LOCATE RESPONSIBILITY IN THE SYSTEM
      "Look for the ways the system creates its own behavior."
      - Is anyone being blamed for a problem that's actually structural?
      - Where should feedback about consequences go directly to decision-makers?
      - Meadows's nuclear waste example: "store it on the lawns of officials
        who approved the plants." What's the equivalent principle here?
      - Recommendation: [specific action]
   
   g) MAKE FEEDBACK POLICIES FOR FEEDBACK SYSTEMS
      "A dynamic, self-adjusting system cannot be governed by a static,
      unbending policy."
      - Are there static rules governing dynamic situations?
      - What would a self-adjusting policy look like? (e.g., "the budget
        adjusts proportionally to the gap between target and actual")
      - Recommendation: [specific action]
   
   h) PAY ATTENTION TO WHAT IS IMPORTANT, NOT JUST WHAT IS QUANTIFIABLE
      "Our culture has given us the idea that what we can measure is more
      important than what we can't measure."
      - What important aspects of this system are being ignored because
        they can't be measured?
      - What measurable things are getting too much attention because they're
        easy to count?
      - Recommendation: [specific action]
   
   i) GO FOR THE GOOD OF THE WHOLE
      "Don't optimize subsystems while ignoring the whole."
      - Which subsystems are being locally optimized at the expense of
        the whole system?
      - What would "whole system health" metrics look like?
      - Recommendation: [specific action]
   
   j) EXPAND TIME HORIZONS
      "The longer the operant time horizon, the better the chances for survival."
      - What time horizon is driving decisions? (quarterly? annual? 5-year?)
      - What would change if the time horizon were 10x longer?
      - What decisions look good in the short term but terrible in the long term?
      - Recommendation: [specific action]
   
   k) EXPAND THOUGHT HORIZONS
      "Defy the disciplines. Follow a system wherever it leads."
      - What disciplinary boundary is preventing people from seeing the
        whole system?
      - What adjacent system is this one connected to that no one is
        considering?
      - Recommendation: [specific action]
   
   l) EXPAND THE BOUNDARY OF CARING
      "Living successfully in a world of complex systems means expanding
      the horizons of caring."
      - Whose interests are excluded from the system's decision-making?
      - What would change if those interests were included?
      - Recommendation: [specific action]
   
   m) CELEBRATE COMPLEXITY
      "The universe is messy. It is nonlinear, turbulent, and chaotic.
      That's what makes the world interesting."
      - Is someone trying to force simplicity on a complex system?
      - What diversity, variability, or experimentation is being suppressed
        in the name of control or efficiency?
      - Recommendation: [specific action]
   
   n) HOLD FAST TO THE GOAL OF GOODNESS
      "We know what to do about eroding goals. Don't weigh the bad news
      more heavily than the good. Keep standards absolute."
      - Has the system's standard of excellence been gradually eroding?
      - What was the original vision? Has cynicism replaced it?
      - Recommendation: [specific action]

2. THE PRACTICAL LEVERAGE MENU
   Based on the 14 wisdoms and the system's situation, produce a ranked
   list of 5-7 concrete actions, ordered from:
   - QUICK WINS: can be done immediately, moderate leverage
   - STRUCTURAL MOVES: take time, high leverage
   - PARADIGM SHIFTS: long-term, highest leverage
   
   For each action:
   - What specifically to do (not abstract — name the concrete change)
   - Why it works (which feedback loop, stock, or flow does it affect?)
   - What resistance to expect
   - How to measure whether it's working
   - What the right time horizon for results is

3. THE "STOP DOING" LIST
   Often the highest-leverage move is to STOP a wrong-direction intervention.
   What should this system stop doing immediately?
   
   For each item:
   - What to stop
   - Why stopping it helps (what feedback loop or self-correcting mechanism
     does it unblock?)
   - What will happen in the short term when you stop (expect things to get
     temporarily worse as suppressed feedback reasserts itself)
   - What will happen in the medium term (the system begins to self-correct)

4. THE MEADOWS CAUTION
   Where in this analysis should we be humble about what we don't know?
   
   "Self-organizing, nonlinear, feedback systems are inherently unpredictable.
   They are not controllable. They are not understandable."
   
   What are the limits of this analysis? Where might we be wrong?
   What should be treated as a hypothesis to test rather than a conclusion
   to act on?

Output: 14-wisdom assessment, practical leverage menu, stop-doing list,
and honesty about what we don't know. Message teammates if your practical
analysis reveals that a theoretically attractive intervention is practically
impossible (or vice versa).
```

### Spawning

Spawn all five as background agents. Use `model: "sonnet"` for all teammates.
The lead (Opus) handles synthesis.

```
Agent: {
  team_name: "meadows-<system-slug>",
  name: "cartographer",
  model: "sonnet",
  prompt: [full cartographer prompt with system description substituted],
  run_in_background: true
}
```

Repeat for diagnostician, counterintuitive, archaeologist, dancer.

Assign tasks immediately:
```
TaskUpdate: { taskId: "1", owner: "cartographer" }
TaskUpdate: { taskId: "2", owner: "diagnostician" }
TaskUpdate: { taskId: "3", owner: "counterintuitive" }
TaskUpdate: { taskId: "4", owner: "archaeologist" }
TaskUpdate: { taskId: "5", owner: "dancer" }
```

## Phase 3: Monitor & Cross-Pollinate

While teammates work:
- Messages from teammates arrive automatically
- If a teammate asks a question, respond with guidance
- If two teammates discover conflicting structural interpretations, message both
  to reconcile (the system map must be consistent)
- If a teammate finds something that dramatically changes the picture
  (e.g., the paradigm archaeologist discovers the stated goal is a lie),
  alert all others

## Phase 4: Synthesize — The Meadows Verdict

After ALL teammates report back, the lead writes the final analysis.
This is the most important phase — it's where the leverage diagnosis crystallizes.

### The Synthesis Process

1. **Collect** all five analyses
2. **Unify the system map** — reconcile the cartographer's structural map with
   the diagnostician's leverage assessment and the archaeologist's paradigm findings
3. **Identify the leverage gap** — the distance between where effort is going
   and where it should go
4. **Check for wrong-direction interventions** — the counterintuitive analyst's
   most important findings
5. **Produce the practical action plan** — the dancer's recommendations filtered
   through the structural analysis
6. **Render the verdict** — one of four categories

### Output Document

Write to `thoughts/meadows/YYYY-MM-DD-<system-slug>.md`:

```markdown
---
date: <ISO 8601>
analyst: Claude Code (meadows leverage skill)
system: "<system name>"
verdict: <HIGH_LEVERAGE | LOW_LEVERAGE | WRONG_DIRECTION | PARADIGM_BLIND>
current_level: <12-1>
recommended_level: <12-1>
leverage_gap: <SMALL | MODERATE | LARGE>
confidence: <LOW | MEDIUM | HIGH>
---

# Meadows Leverage Analysis: [System Name]

> "Leverage points are not intuitive. Or if they are, we intuitively use them
> backward, systematically worsening whatever problems we are trying to solve."
> — Donella Meadows

## The System

[One paragraph description of what the system is and what problematic behavior
it exhibits]

## The System Map (Cartographer)

### Key Stocks
| Stock | Current State | Trend | Change Rate |
|-------|--------------|-------|-------------|
| [name] | [level] | [growing/shrinking/oscillating/stable] | [fast/slow] |

### Key Feedback Loops
| Loop | Type | Goal/Effect | Status |
|------|------|-------------|--------|
| [name] | Balancing/Reinforcing | [what it does] | [functioning/broken/overwhelmed] |

### Key Delays
| Delay | Between | Duration | Effect |
|-------|---------|----------|--------|
| [name] | [action] → [consequence] | [timeframe] | [oscillation/overshoot/none] |

### Behavior Diagnosis
**Primary archetype:** [Fixes That Fail / Shifting the Burden / Eroding Goals /
Success to the Successful / Tragedy of the Commons / Escalation / Overshoot]

**The pattern:** [description of the behavior-over-time pattern and what
structural features produce it]

### System Diagram
```
[Text-based causal diagram showing stocks, flows, and feedback loops]
```

---

## The Leverage Diagnosis (Diagnostician)

### Where You're Pushing Now

| Intervention | Leverage Level | Working? |
|-------------|---------------|----------|
| [intervention 1] | #[N]: [name] | [yes/no/making it worse] |
| [intervention 2] | #[N]: [name] | [yes/no/making it worse] |

**Current weighted level:** #[N] — [layer name]
(Most effort is going to [Physical / Dynamics / Information / Social OS / Intent])

### Where the Leverage Actually Is

| Leverage Point | Level | Intervention | Feasibility |
|---------------|-------|-------------|-------------|
| [point] | #[N] | [what to do] | [high/medium/low] |

**Recommended level:** #[N] — [layer name]

### The Leverage Gap

**Gap:** [SMALL / MODERATE / LARGE] — [N] levels between current and recommended

[2-3 sentences explaining the gap. If LARGE: "You are arranging deck chairs
on the Titanic." If MODERATE: "You're in the right neighborhood but missing
the structural lever." If SMALL: "You're close — adjust direction, not level."]

---

## The Wrong-Direction Audit (Counterintuitive Analyst)

### Interventions Pushing the Wrong Way

| Intervention | Intended Effect | Actual Effect | Severity |
|-------------|----------------|---------------|----------|
| [intervention] | [what it's supposed to do] | [what it actually does] | [LOW/MED/HIGH/CRITICAL] |

### System Traps Active

| Trap | Description | How Long Active |
|------|------------|-----------------|
| [archetype name] | [how it's manifesting] | [timeframe] |

### The Forrester Test
**Is anyone pushing a real leverage point in the wrong direction?**
[YES/NO — if yes, this is the most important finding in the analysis.
Describe the mechanism in detail.]

---

## The Paradigm Archaeology (Paradigm Archaeologist)

### Stated vs. Revealed Goals

| | Stated | Revealed (by behavior) |
|---|--------|----------------------|
| Primary goal | [what they say] | [what they do] |
| Secondary goal | [what they say] | [what they do] |

### Unstated Assumptions
1. **[Assumption]** — everyone believes this without examining it
   - Evidence it's an assumption, not a fact: [evidence]
   - What changes if this assumption is questioned: [consequence]

2. **[Assumption]** — [repeat for each]

### Paradigm Protection
- **Beneficiaries of current paradigm:** [who profits from the status quo]
- **What happens to questioners:** [how dissent is handled]
- **Suppressed information:** [what data would challenge the paradigm]

### The Paradigm Shift
**If the paradigm shifted from** "[current belief]" **to** "[new belief]":
- Goals would change to: [new goals]
- Rules would change to: [new rules]
- Information flows would: [restructure how]
- The problematic behavior would: [change how]

---

## The Dancing Recommendations (Dancing Advisor)

### Quick Wins (do this week)
1. **[Action]** — [why it works, which loop it affects]
2. **[Action]** — [why it works]

### Structural Moves (do this quarter)
1. **[Action]** — [why it works, what resistance to expect]
2. **[Action]** — [why it works]

### Paradigm Shifts (long-term)
1. **[Action]** — [the new belief, how to propagate it]

### Stop Doing (immediately)
1. **Stop [action]** — it's [suppressing feedback / pushing wrong direction /
   wasting effort at level #12]

### The 14 Wisdoms Applied
[Top 5 most relevant wisdoms with one-sentence application each]

---

## THE LEVERAGE VERDICT

### Meadows's Four Diagnoses

**[ ] HIGH LEVERAGE** — You're near the right level and pushing in the right
  direction. Keep going, with minor adjustments.

**[ ] LOW LEVERAGE** — You're wasting effort on parameters when the problem is
  structural. Redirect energy up the hierarchy.

**[ ] WRONG DIRECTION** — You've found a real lever but you're making the problem
  worse. This is the most dangerous diagnosis — stop, reverse, and recalibrate.

**[ ] PARADIGM BLIND** — The real problem is an unstated assumption that no one is
  questioning. The system can't be fixed at any level below the paradigm.

### Verdict: [HIGH_LEVERAGE / LOW_LEVERAGE / WRONG_DIRECTION / PARADIGM_BLIND]

**Confidence:** [LOW / MEDIUM / HIGH]

**The Leverage Gap:** Currently pushing at level #[N] ([name]).
Should be pushing at level #[N] ([name]). Gap: [SMALL/MODERATE/LARGE].

**Reasoning:** [2-3 paragraphs synthesizing all five analyses. Name specific
stocks, loops, and leverage points. Be concrete — don't say "the feedback
structure is problematic," say "the quality feedback loop has a 6-month delay
that causes oscillation between over-hiring and layoffs."]

### What Meadows Would Say

[Write 3-5 sentences in Meadows's voice — warm, clear, concrete, morally
grounded. Use her actual vocabulary: stocks, flows, feedback, delays,
counterintuitive, dance. Reference her analogies where they fit (the bathtub,
the thermostat, the lake, the London shower). She would be honest but
encouraging — she believed people could learn to see systems. She would
point to the highest leverage available and say "look here, this is where
the real opportunity is."]

### If You Proceed: The System Rules

[Based on the analysis, write 3-5 rules for working with this system.
These are Meadows-style principles — not parameter adjustments but structural
commitments.]

1. **Always [principle]** — because [structural reason]
2. **Never [principle]** — because [it pushes the wrong direction]
3. **Measure [thing]** — because [it's the missing feedback]
4. **Question [assumption]** — because [it's the paradigm protecting itself]
5. **Dance with [dynamic]** — because [you can't control it, only work with it]
```

## Phase 5: Present & Follow-up

Present the verdict to the user with the key highlights. Don't dump the
whole document — give the verdict, the leverage gap, and the top actions.

```
## Meadows Verdict: [System] — [VERDICT]

**Leverage gap:** [SMALL/MODERATE/LARGE] — pushing at #[N] ([name]),
  should be at #[N] ([name])
**Wrong-direction interventions:** [N] found
**Primary system trap:** [archetype name]
**Paradigm assumption:** "[the unstated belief]"

**What Meadows would say:** "[pithy synthesis in her voice]"

**Top 3 moves:**
1. [Quick win] — [why]
2. [Structural move] — [why]
3. [Stop doing] — [why]

Full analysis: `thoughts/meadows/YYYY-MM-DD-<slug>.md`

Want me to:
1. Deep-dive into any analyst's findings?
2. Map the system more formally (full causal loop diagram)?
3. Apply this to a different system for comparison?
4. Pair with /munger to add economic/psychological analysis?
5. Run the analysis on a proposed intervention to test it?
```

## Batch Mode

If the user wants to compare multiple systems or interventions:

1. Run the full analysis on each (can parallelize — one team per system)
2. At the end, produce a comparison:

```
## Meadows Leverage Comparison

| System | Verdict | Current Level | Recommended Level | Gap | Wrong-Dir? |
|--------|---------|---------------|-------------------|-----|------------|
| [name] | [verdict] | #[N] | #[N] | [gap] | [Y/N] |
| [name] | [verdict] | #[N] | #[N] | [gap] | [Y/N] |
```

## Scoring Discipline

- **Be Meadows, not a consultant.** Meadows was honest and direct. She didn't
  sugarcoat — but she was warm, not cold. If the system is pushing at #12 when
  the leverage is at #3, say so clearly and explain why.
- **Cite the source analyst.** Every claim traces to a specific teammate's finding.
- **No paradigm-change inflation.** Not every problem is paradigm-level. Some
  systems genuinely just need better feedback loops or shorter delays. Don't
  always push to #2 for drama.
- **The counterintuitive finding is king.** If the Counterintuitive Analyst finds
  a wrong-direction push, that overrides everything else. Wrong direction at
  high leverage is worse than right direction at low leverage.
- **Acknowledge uncertainty.** Meadows: "Working with systems constantly reminds
  me of how incomplete my mental models are." The analysis is a hypothesis, not
  a diagnosis. Say what you're confident about and what you're guessing.
- **Web search when needed.** If the system involves real companies, markets, or
  policies, have analysts use WebSearch to ground their analysis in evidence.

## When NOT to Use This Skill

- **Simple, linear problems** where cause and effect are obvious. Don't systems-think
  a bug fix or a pricing decision with clear unit economics.
- **Crisis response** where you need to act in hours. Map the system AFTER the fire
  is out, not during.
- **When the system genuinely needs parameter tuning.** If the thermostat is set to
  the wrong temperature, change the temperature. Don't redesign the heating system.
- **When you have no access to structural levers.** If you can only change parameters,
  optimize parameters. Don't torture yourself about paradigms you can't reach.

## Pairing With Other Skills

- **/munger** first, then **/meadows**: Munger tells you if it's a good business.
  Meadows tells you why the market/system around it behaves the way it does.
- **/meadows** first, then **/munger**: If you're trying to understand a stuck
  system (company, market, org), Meadows diagnoses the structure. Then Munger
  evaluates whether a proposed intervention is a good investment.
- **/office-hours** → **/meadows**: Refine the problem definition, then analyze
  the system.
- **/christensen** + **/meadows**: Christensen identifies disruption dynamics.
  Meadows explains the feedback structure that makes incumbents vulnerable.
- **/goldratt** + **/meadows**: When available — Goldratt finds the constraint,
  Meadows classifies what kind of constraint it is.

## Important Notes

- **Cost**: This skill spawns 5 agents. It's expensive. Worth it for serious
  system analysis, not for casual questions.
- **Sonnet for teammates, Opus for synthesis**: The lead handles the leverage
  verdict and paradigm synthesis — that's where deep reasoning matters.
- **No team? No problem**: If teams aren't enabled, run 5 sequential background
  agents and collect results. Same analysis, just no cross-talk.
- **The skill works on anything**: Companies, markets, policies, organizations,
  products, personal habits, ecosystems, communities. If it has stocks, flows,
  and feedback, Meadows applies.
- **Primary sources**: For the full framework, read Meadows's original essay
  "Leverage Points: Places to Intervene in a System" at donellameadows.org,
  and her book "Thinking in Systems: A Primer" (Chelsea Green, 2008).
