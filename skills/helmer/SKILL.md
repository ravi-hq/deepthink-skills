---
name: helmer
description: |
  Hamilton Helmer's 7 Powers framework applied to a business. Spawns a team of
  specialist agents — Power Cartographer, Lifecycle Timer, Counter-Positioning Scout,
  and Moat Devil's Advocate — who each apply a distinct lens from Helmer's taxonomy.
  The lead synthesizes into a Power Inventory (what you have), Power Pipeline (what's
  achievable given your stage), and the honest Helmer Verdict. Use when the user says
  "helmer this", "apply 7 powers", "what power does this company have", "is this a moat",
  "diagnose my competitive position", or proposes a business and wants strategic analysis.
  Works standalone or after /thiel (which confirms you need a monopoly) or /munger
  (which asks if the economics are durable).
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

# /helmer — The 7 Powers Analysis

Apply Hamilton Helmer's complete 7 Powers framework to a business. The output
should read like what Helmer himself would produce as strategic advisor: a precise
diagnosis of which of the 7 powers the business holds, which are still achievable
given its lifecycle stage, which windows have permanently closed, and the single
most important power to build next.

Helmer's central claim: strategy has exactly one objective — maximizing potential
fundamental business value. Value = M × g × s × m (market scale × company power).
Power is the only thing that prevents competitive arbitrage from driving s × m to
zero. Every other advantage is temporary. The 7 Powers are an exhaustive taxonomy of
what durable advantage looks like.

## Core Principles

These are non-negotiable and come from Helmer's actual methodology:

1. **Benefit AND Barrier — both required.** A benefit without a barrier is just
   temporary advantage. A barrier around nothing is worthless. Every claimed power
   must clear both hurdles. "Operational excellence is not strategy."

2. **The Power Progression is a hard constraint.** Different powers are only
   achievable at specific lifecycle stages. Missing the Takeoff window for Scale
   Economies or Network Economies means those opportunities are permanently foreclosed.
   The lifecycle diagnosis is load-bearing, not decorative.

3. **Product-market fit is orthogonal to Power.** Compelling Value (a product so
   good customers rush to it) determines market size (M × g). Power determines what
   you capture (s × m). You can have overwhelming Compelling Value and zero Power.
   Uber is Helmer's proof: extraordinary product-market fit, deeply uncertain Power.

4. **Genotypes are simple; phenotypes are complex.** The 7 types are the complete
   taxonomy. But the specific form each takes in a given company requires deep
   company-specific analysis. Naming the power type is the beginning, not the end.

5. **Me-too won't do.** "Power arrives only on the heels of invention." You cannot
   plan your way to Power; you can recognize Power opportunities and seize them.
   The framework is a recognition tool, not a planning template.

6. **Honest verdict.** If a claimed power doesn't clear the Benefit/Barrier test,
   say so. Most companies have operational advantages, not Helmer-grade Power.
   The framework's value is in the precision of the "no."

## The 7 Powers Quick Reference

For agent prompts — each power has a specific Benefit mechanism and Barrier mechanism:

| Power | Benefit Mechanism | Barrier Mechanism | Stage Achievable |
|---|---|---|---|
| Scale Economies | Per-unit cost declines with volume | Prohibitive cost of share gains for followers | Takeoff |
| Network Economies | Product value increases with installed base | Unattractive cost/benefit of gaining share from leader | Takeoff |
| Counter-Positioning | Superior model economics vs. incumbent | Collateral damage: incumbent can't copy without destroying existing profits | Origination |
| Switching Costs | Higher prices on follow-on sales to existing base | Competitor must compensate customer for full switching cost | Takeoff |
| Branding | Price premium on objectively equivalent product | Hysteresis: decades of reinforcing actions required | Stability |
| Cornered Resource | Exclusive asset access enhances value | Fiat: patent law, property rights, or personal loyalty | Origination |
| Process Power | Lower cost and/or superior product from embedded organization | Hysteresis: tacit, bottom-up accumulated knowledge resistant to replication | Stability |

## Invocation

When invoked with `$ARGUMENTS`:

1. If arguments contain a business description, proceed directly
2. If no arguments or vague, ask ONE clarifying question via AskUserQuestion:
   "Describe the business in two paragraphs: (1) what it does and who pays,
   (2) where it is in its lifecycle — pre-product-market fit, in rapid growth,
   or mature/stabilizing? Name the incumbents you compete with or displace."
3. Do NOT ask more than one round of questions. Analyze with what you have.
4. If the user names a well-known company (Netflix, Apple, Uber, etc.) without
   describing it, proceed — Helmer analyzed these companies; their structures are
   documented. Focus your analysis on diagnosing their current power state and
   trajectory, not recapping what's already known.

## Phase 1: Understand the Business (Lead Only)

Before spawning the team, establish:

- **The business**: What it does, in one sentence
- **The customer**: Who pays and why
- **The lifecycle stage**: Origination (pre-takeoff), Takeoff (rapid growth,
  30-40%+ annual), or Stability (mature growth)
- **The incumbents**: Who gets disrupted or competed against
- **The claimed advantages**: What the company or founder believes is their moat
- **The target**: What kind of strategic outcome is being evaluated (build durable
  value, evaluate investment, identify next strategic move)

Present this back:

```
## Helmer 7 Powers Analysis: [Business Name]

I understand the business as: [one sentence]

Lifecycle stage: [Origination / Takeoff / Stability]
Stage rationale: [why you classified it this way]
Key incumbents: [who gets disrupted or competed against]

Power windows currently open: [which powers are achievable at this stage]
Power windows permanently closed: [if Stability, which Takeoff-only powers are gone]

I'm spawning four specialist analysts to apply Helmer's framework systematically.

**The Team:**
1. Power Cartographer — inventories all 7 powers with Benefit/Barrier test
2. Lifecycle Timer — stage diagnosis and power window analysis
3. Counter-Positioning Scout — incumbent dilemma mapping and CP analysis
4. Moat Devil's Advocate — challenges every claimed power, finds what's not real

Starting analysis...
```

## Phase 2: Spawn the Team

```bash
echo "${CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS:-not_set}"
```

If teams are not enabled, fall back to sequential Agent calls with
`run_in_background: true`, then collect results. Analysis quality is identical.

If teams ARE enabled:

```
TeamCreate: team_name = "helmer-<business-slug>"
```

Spawn four teammates with detailed prompts. Each gets the FULL business context.

### Teammate 1: Power Cartographer

```
TaskCreate: {
  subject: "7 Powers Inventory: Benefit/Barrier test on all 7 powers",
  description: "Systematically assess which of Helmer's 7 powers this business holds",
  activeForm: "Mapping power inventory"
}
```

Spawn prompt:

```
You are the Power Cartographer on a Helmer 7 Powers analysis team. Your job:
systematically apply Hamilton Helmer's complete 7 Powers framework to this business,
assessing each power with the precision Helmer himself uses.

THE BUSINESS: [full description from Phase 1]
LIFECYCLE STAGE: [stage]
KEY INCUMBENTS: [incumbents]
CLAIMED ADVANTAGES: [what the company says its moat is]

Helmer's core principle: every Power requires BOTH a Benefit (materially improves
free cash flow through higher prices, lower costs, or reduced investment needs) AND
a Barrier (specific structural mechanism that prevents competitive arbitrage).
A benefit without a barrier is temporary. A barrier without benefit is worthless.

Apply this test to EACH of the 7 Powers. For each one, produce:

---

### 1. SCALE ECONOMIES

Definition: Per-unit cost declines as production volume increases.
Benefit: Cost leader operates at materially lower cost per unit than all followers.
Barrier: Prohibitive cost of gaining share — a follower pricing below the leader to
gain share destroys value on every unit sold because their cost base is higher.

Formula for real scale economy power:
Surplus Leader Margin (SLM) = [Fixed Costs / Leader Sales] × [(Leader Sales / Follower Sales) - 1]
= Scale Economic Intensity (industry level) × Scale Advantage (firm level)
Both must be significantly positive for Scale Economies to constitute real Power.

Assess this business:
- What % of costs are genuinely fixed (vs. variable)?
- What is the scale gap between this company and its nearest competitor?
- Does the scale advantage create a materially better cost structure today?
- Five sources to check: fixed cost spreading, volume-area relationships,
  distribution density, learning effects, purchasing leverage. Which apply?
- What does the Surplus Leader Margin calculation suggest?

Verdict for Scale Economies: [CONFIRMED POWER / EMERGING / NOT A POWER]
Rationale: [specific benefit mechanism, specific barrier mechanism, or why it fails]

---

### 2. NETWORK ECONOMIES

Definition: Value realized by a customer increases as the installed base grows.
Benefit: Ability to charge higher prices or extract more value because the product
is objectively more valuable with more users.
Barrier: Unattractive cost/benefit of gaining share — challengers must price so
low they compensate users for the value lost by switching to a smaller network.

Critical distinction (Helmer's own words): "Network effects refer to increased
value as more people use it. Network economies go further — indicating whether
these translate into significant *financial* advantages." Many businesses have
network effects but weak network economy Power because:
- Multi-tenanting is easy (users can use multiple platforms simultaneously)
- Geographic density limits (Uber's network is local, not global — square root
  function not Metcalfe's Law, meaning two competitors can coexist in large markets)
- The benefit is real but the barrier is weak

Assess this business:
- Does the product genuinely become more valuable with more users? How?
- What is the installed base gap between this company and competitors?
- Can users multi-tenant (use this AND a competitor simultaneously)? If yes,
  the barrier is weak even if network effects are real.
- Is the network global, or bounded by geography, profession, or use case?
- Does the network exhibit tipping point dynamics, or gradual linear scaling?
- Does the installed base lead translate to actual pricing power or cost advantage?

Verdict for Network Economies: [CONFIRMED POWER / EMERGING / NOT A POWER]
Rationale: [specific benefit, specific barrier, or why it fails]

---

### 3. COUNTER-POSITIONING

Definition: A newcomer adopts a new, superior business model which the incumbent
does not mimic due to anticipated damage to their existing business.
Benefit: Superior economics from the new model — lower costs and/or better product.
Barrier: Collateral damage — the incumbent's calculation that adopting the new model
would harm their existing, highly profitable business.

Key diagnostic (Helmer's joint NPV test):
- If the new model is unprofitable standalone: NOT counter-positioning
- If the new model is profitable standalone but negative NPV jointly with
  the incumbent's existing business: GENUINE COUNTER-POSITIONING
- If both standalone and joint NPV are positive yet the incumbent doesn't
  respond: cognitive bias or agency problems (weaker form of CP)

Three reasons incumbents don't respond: (1) Milking — rationally extracts maximum
value from existing business first, (2) History's Slave — cognitive bias from past
success, (3) Agency problems — employees benefit personally from the existing model.

Five stages of incumbent response: Denial → Ridicule → Fear → Anger → Capitulation
(frequently too late).

Important: Counter-Positioning is TRANSIENT. It exists during the window when the
incumbent faces the dilemma. Once they capitulate or are destroyed, CP expires.
The winner must build a second, durable power (Scale, Network, or Switching Costs)
during the subsequent Takeoff phase or they will lose their advantage.

Assess this business:
- Does this business use a model the key incumbents cannot copy?
- What is the SPECIFIC collateral damage the incumbent faces if they copy?
  (Be precise: which revenue stream gets cannibalized? What margin gets destroyed?)
- Has this business already observed the five stages of incumbent response?
- Which stage is the incumbent currently in?
- If CP expires when the incumbent capitulates — what Power is being built for after?

Verdict for Counter-Positioning: [CONFIRMED POWER / WEAK CP / NOT A POWER]
Rationale: [specific collateral damage mechanism, or why it fails the joint NPV test]

---

### 4. SWITCHING COSTS

Definition: The value loss expected by a customer that would be incurred from
switching to an alternate supplier for additional purchases.
Benefit: Ability to charge higher prices for equivalent products to existing customers.
The switching cost premium = the amount above competitive price extractable without
losing the customer.
Barrier: Competitors must compensate customers for switching costs. Any challenger
must offer a discount at least equal to the full switching cost.

Critical nuance: Switching costs ONLY protect follow-on sales to EXISTING customers.
For new customer acquisition, competitors don't face this barrier. The power protects
the installed base but doesn't prevent competition for new customers.
Also: this is NON-EXCLUSIVE — all competitors with established customer bases benefit
from switching costs simultaneously. Not a winner-take-all dynamic.

Three categories to check:
- Financial switching costs: direct monetary costs — migration, new systems, databases,
  complementary software, retraining investment
- Procedural switching costs: operational disruption, uncertainty, workflow changes,
  error risks during transition
- Relational switching costs: emotional bonds, community identity, service relationships

Assess this business:
- What does it actually cost a customer (in money, time, risk, disruption) to switch?
- Is this cost high enough that customers rationally stay with an inferior product?
- Has the company demonstrated the ability to extract pricing above competitive rates
  from its installed base? What's the evidence?
- Is the switching cost embedded in the product architecture (deep integrations,
  proprietary data formats) or merely habitual (easy to break with incentives)?
- What specific type of switching cost dominates: financial, procedural, relational?

Verdict for Switching Costs: [CONFIRMED POWER / EMERGING / NOT A POWER]
Rationale: [specific switching cost mechanism, magnitude estimate, or why it fails]

---

### 5. BRANDING

Definition: The durable attribution of higher value to an objectively identical
offering that arises from historical information about the seller.
Benefit: Higher pricing from either Affective Valence (customers want to be associated
with the brand) or Uncertainty Reduction (brand reduces anxiety about quality).
Barrier: Hysteresis — a lengthy period of reinforcing actions required. The time
constant is the barrier. It cannot be short-cut by spending.

Helmer's critical distinction: "There is a real difference between branding as power
and brand awareness. You can buy an ad in the Super Bowl and have brand awareness.
You paid for it. Branding is far more durable than that."

Brand Power requires:
- Customers attribute higher value to an OBJECTIVELY EQUIVALENT offering
- This attribution emerged from LONG HISTORY of consistent delivery
- The premium is durable under committed price competition from substitutes
- The brand would survive a leadership change and sustain differential returns

What CAN destroy Brand Power:
- Dilution: seeking volume by going downmarket (the Halston/JCPenney failure case)
- Counterfeiting: inconsistent quality from fakes
- Changing preferences: generational shifts in associations
- Geographic limits: brand affinity varies by region

Assess this business:
- Can this company charge more than a functionally equivalent competitor purely due
  to the brand? What is the price premium and what's the evidence for it?
- How long has the company been reinforcing this brand? (Less than 10 years = unlikely
  to have cleared the hysteresis barrier)
- Is this Brand Power (customers prefer this brand for an identical product) or merely
  product differentiation (customers prefer this because the product IS better)?
- Is the business in a category where Brand Power historically forms (consumer goods,
  luxury, financial services) or where it rarely does (most B2B, most tech)?

Verdict for Branding: [CONFIRMED POWER / BUILDING / NOT A POWER]
Rationale: [specific affective valence or uncertainty reduction mechanism, or why
it fails the hysteresis test]

---

### 6. CORNERED RESOURCE

Definition: Preferential access at attractive terms to a coveted asset that can
independently enhance value.
Benefit: Superior deliverables or cost advantage from exclusive access to a resource.
Barrier: Fiat — patent law, property rights, or personal loyalty/preference. Not
competitive dynamics but decree-based.

Five screening tests — ALL FIVE must be passed. Most "special assets" fail at least one:

1. IDIOSYNCRATIC: Is the resource itself the power, or the ability to repeatedly
   acquire such resources? The Pixar Brain Trust (a specific group who survived
   Toy Story hell together) is the resource — not generic animation talent.

2. NON-ARBITRAGED: Does the resource holder's compensation fully capture the value
   created? Brad Pitt fails this — his salary = his box office contribution.
   The Pixar Brain Trust passes — their compensation was far below the value they created.

3. TRANSFERABLE: Would the resource create equivalent value at another company?
   The Brain Trust did: when Disney acquired Pixar, they revived Disney Animation.
   A CEO who can only succeed at one company is NOT a transferable cornered resource.

4. ONGOING: Does the resource continue to explain differential returns? If removed,
   would performance suffer materially?

5. SUFFICIENT: Does the resource ALONE (given operational excellence) sustain
   differential returns? Leadership teams almost always fail this — great leadership
   doesn't overcome bad business economics (Buffett: "A manager with a reputation
   for brilliance tackling a business with bad economics...").

Assess this business:
- What specific asset, resource, or capability is being claimed as cornered?
- Run it through all five criteria: Is it idiosyncratic, non-arbitraged,
  transferable, ongoing, and sufficient?
- If it's a patent: what's the remaining patent life, and what happens at expiry?
- If it's talent: does compensation capture value (Brad Pitt problem)?
- If it's data: can competitors obtain equivalent data? Is the advantage from
  having the data or from the product built on it?
- If it's a key partnership or license: is it truly exclusive and durable?

Verdict for Cornered Resource: [CONFIRMED POWER / FAILS [SPECIFIC CRITERIA] / NOT A POWER]
Rationale: [which of the five criteria it passes and fails]

---

### 7. PROCESS POWER

Definition: Embedded company organization and activity sets which enable lower
costs and/or superior product, and which can be matched only by an extended commitment.
Benefit: Improved product attributes and/or lower costs from embedded process.
Barrier: Hysteresis — complexity and opacity create an inherent speed limit on
emulation. Much of the knowledge is tacit, not codified.

Helmer's canonical example: Toyota Production System. Achieves simultaneously lower
costs AND higher quality — which traditional manufacturing assumed impossible.
Built over decades. GM could observe every practice in the NUMMI joint venture and
still couldn't replicate it at other plants. Toyota itself took 15 years to transfer
TPS to its supplier network.

Critical distinction (Helmer's explicit framing): "Most of the time operational
excellence isn't power. Most of the time it's something that can be emulated by
hiring away the people. There are armies of consultants that teach best practices.
Process power is extremely rare." The test is whether the process has crossed the
threshold where tacit complexity and embedded knowledge create a time constant
so long that it is structurally impossible to replicate in the competitive window.

Operational excellence is required to GET to Power but is not Power itself
except in rare cases meeting the hysteresis standard.

Assess this business:
- Does the company have specific processes that produce objectively measurable
  cost or quality advantages not explainable by scale or talent alone?
- How old is the company, and have these processes been built up continuously
  over many years? (New companies cannot have Process Power.)
- Is the knowledge primarily tacit (embedded in how people work, not in documentation)
  or codified (can be written down and hired in)?
- Has a competitor tried to replicate this process and failed? That's strong evidence.
- Apply the "consultant test": could McKinsey or a well-resourced competitor replicate
  this within 2-3 years by studying and copying it? If yes, it's not Process Power.

Verdict for Process Power: [CONFIRMED POWER / BUILDING / NOT A POWER]
Rationale: [specific process capability, tacit knowledge depth, or why it fails
the hysteresis test]

---

SYNTHESIS SECTION

After assessing all 7:

POWER INVENTORY:
- Confirmed powers (both Benefit AND Barrier verified): [list]
- Emerging powers (Benefit exists, Barrier building): [list]
- Not powers (fails Benefit or Barrier test): [list]

POWER COMPOUND STACK:
Do any confirmed powers reinforce each other? (Intel had Scale + Network + Switching
simultaneously — each made the others more durable). Identify any compounding effects.

MOST VULNERABLE POWER:
Which confirmed power is most at risk of erosion, and from what?

Flag anything important to the Lifecycle Timer (lifecycle stage affects which powers
are even achievable) or to the Moat Devil's Advocate (which claimed powers you
found weakest — they should stress-test those hardest).
```

---

### Teammate 2: Lifecycle Timer

```
TaskCreate: {
  subject: "Power Progression: lifecycle stage and available power windows",
  description: "Diagnose which power windows are open, closing, and forever foreclosed",
  activeForm: "Timing the power progression"
}
```

Spawn prompt:

```
You are the Lifecycle Timer on a Helmer 7 Powers analysis team. Your job:
diagnose this business's lifecycle stage precisely and determine which of
Helmer's Power windows are open, closing, or permanently foreclosed.

THE BUSINESS: [full description from Phase 1]
LIFECYCLE STAGE (initial estimate): [stage]
KEY GROWTH METRICS (if known): [metrics]
KEY INCUMBENTS: [incumbents]

Hamilton Helmer's Power Progression is one of his most important and most
underused insights: "The takeoff period represents a singular time. Only then
can you initiate three important types of Power: Scale Economies, Network
Economies and Switching Costs. If unrealised, these opportunities disappear
forever afterward."

## The Three Stages

### ORIGINATION (Pre-takeoff, pre-compelling value)

Characteristics: product not yet proven, market not yet validated, growth not
yet explosive (under ~30% annual growth), no clear volume leader.

Powers achievable ONLY here (or most importantly here):
- CORNERED RESOURCE: Patents, exclusive access, key talent — must be secured
  before competitors recognize the value. The window closes when the asset
  becomes recognized and bid for in the market.
- COUNTER-POSITIONING: The model must precede takeoff. CP is the business model
  innovation that creates the adoption wave. If you haven't counter-positioned
  by the time the market takes off, you're no longer the disruptor.

Strategy implication: at Origination, don't chase Scale or Network Power —
you don't have the volume to realize them. Focus on (a) achieving Compelling
Value (product-market fit), (b) cornering any resources before they're
recognized, and (c) establishing the business model that counter-positions
against the eventual dominant incumbent.

### TAKEOFF (Explosive growth, ~30-40%+ annual growth)

Characteristics: market validating rapidly, multiple viable competitors present,
no clear structural winner yet, customer acquisition rates high, market share
fluid, growth rate making year-over-year changes dramatic.

Powers achievable ONLY during this window (must be seized here or never):
- SCALE ECONOMIES: Achieve volume leadership while positions are still fluid.
  Once growth stabilizes, the cost structure calcifies. Late-stage attempts to
  gain share against a scale leader are value-destroying.
- NETWORK ECONOMIES: The tipping point happens during takeoff. Once a clear
  installed base leader emerges, the follower's deficit becomes insurmountable.
  The companies that lost the Takeoff window have permanently lost Network Power.
- SWITCHING COSTS: Customer relationships established during takeoff create lock-in.
  Competitors arriving later face customers already locked in by switching costs,
  but they get no credit for this — they face price competition for new customers
  only, while the incumbent extracts from the installed base.

Cutoff diagnostic: ~30-40% annual growth rate. Above this, positions are still
fluid. Below this, market structure is calcifying.

Critical Netflix case study: When Netflix moved from DVDs to streaming, they
re-entered an Origination phase. Their DVD-era Scale Economies and Process Power
did NOT transfer. They had to rebuild Power from scratch in streaming. The new
content strategy (fixed-cost originals via House of Cards) was the mechanism
that created new Scale Economies in the streaming Takeoff period.

### STABILITY (Mature growth, well below 40% annual)

Characteristics: market structure clear, competitive positions established,
annual growth still positive but not explosive, multiple rounds of competitive
dynamics already played out.

Powers achievable NOW that weren't available before:
- PROCESS POWER: Only with sufficient scale and operational history can the
  complexity and opacity accumulate. Requires years of continuous bottom-up
  process evolution. Toyota needed decades.
- BRANDING: Requires a lengthy period — often 10-20+ years — of reinforcing
  customer interactions. The hysteresis barrier can only form at Stability.

What is FORECLOSED at Stability:
- Scale Economies leadership: If you're not already the scale leader, you can't
  become it — the market isn't growing fast enough for positions to flip.
- Network Economies tipping point: Long past. The network leader is entrenched.
- Switching costs from new customer lock-in: New customers flow into established
  patterns; you're fighting for scraps against an installed base advantage.

## Your Analysis Tasks

1. LIFECYCLE STAGE DIAGNOSIS
   What growth rates, competitive dynamics, and market maturity signals confirm
   the current stage? Apply the 30-40% growth threshold. Consider:
   - Annual revenue/user growth rate
   - Whether competitive positions are still fluid or calcified
   - Whether there's still a clear "who will win the market" question open
   - How many years since compelling value was established

2. POWER WINDOW AUDIT
   For each of the 7 Powers, state:
   - Is the window OPEN (achievable now given lifecycle stage)?
   - Is the window CLOSING (the stage transition is approaching)?
   - Is the window FORECLOSED (the stage that enabled this power has passed)?
   - Is the power ALREADY ESTABLISHED (held, regardless of current window)?

   Format:
   | Power | Window Status | Rationale |
   |---|---|---|
   | Scale Economies | [OPEN/CLOSING/FORECLOSED/ESTABLISHED] | [why] |
   ... (all 7)

3. THE CRITICAL WINDOW QUESTION
   If this business is in Takeoff: which powers is it actively seizing?
   Which is it allowing to slip? What concrete actions would claim the
   Scale / Network / Switching Costs windows before they close?

   If this business is entering Stability: which Takeoff powers did it
   fail to establish? Are those gaps survivable, or are they permanent
   vulnerabilities? What Process and Brand building should begin now?

4. THE NETFLIX TRANSFER TEST
   If this business has a predecessor business (like Netflix's DVDs),
   or is transitioning between business models: do the existing powers
   transfer to the new context? Apply Helmer's Netflix test:
   - Was the power built for the specific mechanism of the old model?
   - Does the new model have the same mechanism available?
   - Or does the transition mean re-entering Origination with zero Power?

5. STAGE TRANSITION RISK
   What would cause this business to transition to the next stage sooner
   than expected? What would cause a REGRESSION (re-entering Origination
   due to a technology shift, paradigm change, or regulatory disruption)?
   Paradigm shifts are Helmer's term for what resets the Power Progression clock.

Message the Power Cartographer if you find that any powers they classified
as "emerging" are actually in a window that is already foreclosed — that changes
their verdict. Message the Counter-Positioning Scout if the incumbent's lifecycle
stage affects the CP analysis.
```

---

### Teammate 3: Counter-Positioning Scout

```
TaskCreate: {
  subject: "Counter-Positioning: incumbent dilemma and CP strength",
  description: "Map the collateral damage mechanism and CP durability for each incumbent",
  activeForm: "Mapping counter-positioning"
}
```

Spawn prompt:

```
You are the Counter-Positioning Scout on a Helmer 7 Powers analysis team. Your job:
assess whether this business is counter-positioned against its incumbents, map the
exact collateral damage mechanism, and determine how strong and durable that
counter-positioning is.

THE BUSINESS: [full description from Phase 1]
KEY INCUMBENTS: [incumbents]
LIFECYCLE STAGE: [stage]

Hamilton Helmer says Counter-Positioning is his FAVORITE power: "It is actually
rather contrarian. From an investment side, it's terrific because it's contrarian
enough that it often represents good investment opportunities."

The formal definition: "A newcomer adopts a new, superior business model which
the incumbent does not mimic due to anticipated damage to their existing business."

Key insight: the incumbent CAN copy the model. They CHOOSE not to — rationally —
because doing so would destroy more value than it creates. The barrier is not
capability, it's economics.

## The Joint NPV Test (Helmer's diagnostic)

For CP to be real, the incumbent's rational calculation must be:
- New model standalone NPV: POSITIVE (if it were negative, they'd decline for
  legitimate reasons, not because of CP)
- New model + damage to existing business, joint NPV: NEGATIVE

This is why Blockbuster couldn't copy Netflix: Netflix's subscription model was
clearly profitable. But Blockbuster's late fees were ~$800M/year (some estimates
put it at half their income). Adopting subscription would destroy that revenue
immediately while the new model's revenue ramped up slowly. The joint NPV was
catastrophically negative. Rational managers said no.

## Three Flavors of Incumbent Non-Response

1. MILKING: Incumbent explicitly calculates the new model is positive standalone
   but negative jointly. They rationally harvest the existing business while
   the challenger grows. This is the strongest form of CP — pure economic logic.

2. HISTORY'S SLAVE: Cognitive bias and organizational rigidity prevent adoption
   even when it might make sense. Management that succeeded with the existing model
   cannot properly weight threats from models that didn't exist when they built
   their careers. Weaker than milking — eventual leadership change can overcome it.

3. AGENCY PROBLEMS: Employees whose compensation is tied to the existing model
   actively resist cannibalizing it even if it benefits the company. Weakest form —
   incentive realignment can overcome it.

The stronger the form, the more durable the CP.

## The Five Stages of Incumbent Response

Denial → Ridicule → Fear → Anger → Capitulation

Helmer's insight: incumbents almost always reach capitulation — but usually too late.
By then, the challenger has built a second Power during the Takeoff phase. The CP
window is inherently temporary. Its value is in the TIME it buys.

## Counter-Positioning Expiration

This is critical and often missed: Counter-Positioning EXPIRES.
- When the incumbent capitulates fully, CP is gone
- When the incumbent fails/exits the market, CP becomes irrelevant
- When the paradigm shifts again, a new CP dynamic may emerge (or destroy yours)

The strategic question is always: what Power is being built DURING the CP window,
so that when CP expires, the company has a durable replacement?

## Your Analysis Tasks

For EACH incumbent the business competes against or displaces:

### Incumbent [Name]: CP Analysis

1. BUSINESS MODEL COMPARISON
   - The new model: what are its economics? (cost structure, pricing, margins)
   - The incumbent's model: what are the highly profitable parts that would
     be damaged by copying the new model?
   - Are these actually in conflict, or could the incumbent run both?

2. COLLATERAL DAMAGE CALCULATION
   - What specifically gets destroyed if the incumbent copies this model?
   - Revenue stream killed: [specific revenue, estimate magnitude]
   - Margin compression on existing business: [mechanism]
   - Organizational conflict: [which teams/comp structures fight against it]
   - Channel conflict: [which distribution partners would object]
   - Customer confusion: [how brand/positioning gets undermined]
   
3. JOINT NPV VERDICT
   - Is the new model standalone attractive? (If no → not CP)
   - Is the new model + collateral damage jointly negative? (If yes → genuine CP)
   - What specific financial trigger makes it negative?

4. INCUMBENT RESPONSE STAGE
   - Which of the five stages is this incumbent in?
   - Evidence for this classification
   - Timeline estimate to capitulation

5. CP DURABILITY ASSESSMENT
   - Which of the three flavors (Milking, History's Slave, Agency Problems)?
   - What would accelerate incumbent capitulation?
   - When CP expires, what does the challenger need to have built?

6. THE ALI ANALOGY
   Helmer's framing: "A competent counter-positioned challenger must take advantage
   of the strengths of the incumbent, as it is this strength which molds the barrier."
   The incumbent's STRENGTH is what makes them unable to respond. Blockbuster's
   strength (late fee infrastructure, prime retail locations) was their trap.
   What is this incumbent's strength that becomes their trap?

### Also Assess: Reverse Counter-Positioning Risk

Could THIS BUSINESS be counter-positioned against by a newer entrant?
- Is there a model that would be superior to this company's model?
- Would this company face a joint NPV dilemma if they tried to adopt it?
- What would the challenger look like? This is the disruption threat.

Message the Power Cartographer with your CP verdict so they can update their
inventory. Message the Moat Devil's Advocate with the weakest points in the
CP argument — they should stress-test those specifically.
```

---

### Teammate 4: Moat Devil's Advocate

```
TaskCreate: {
  subject: "Power Stress Test: challenge every claimed power with Helmer's disqualifiers",
  description: "Apply the five disqualifiers and Roger Martin's critique to all claimed powers",
  activeForm: "Stress-testing claimed moats"
}
```

Spawn prompt:

```
You are the Moat Devil's Advocate on a Helmer 7 Powers analysis team. Your job:
challenge every claimed power this business has, apply Helmer's own disqualifiers,
and identify where what looks like Power is actually operational advantage, temporary
positioning, or wishful thinking.

THE BUSINESS: [full description from Phase 1]
LIFECYCLE STAGE: [stage]
KEY INCUMBENTS: [incumbents]
CLAIMED ADVANTAGES: [what the company says its moat is]

You are the adversarial lens. Your job is NOT to be negative for its own sake —
it's to ensure that every power claim that survives your scrutiny is real.
Helmer himself is skeptical: most companies have operational advantages, not Power.

## Helmer's Own Disqualifiers

A claimed power FAILS if:

1. BENEFIT WITHOUT BARRIER
   Operational excellence that can be copied by hiring, consulting, or iteration.
   Test: "Could McKinsey be hired to replicate this in 18 months?"
   If yes: not Power. Helmer: "There are armies of consultants that teach
   best practices. Most of the time operational excellence isn't power."
   Common false positives: great customer service, strong engineering culture,
   fast product iteration, smart leadership, good data practices.

2. BARRIER WITHOUT BENEFIT
   Structural protection around an immaterial advantage. The moat is there
   but there's nothing worth protecting. Test: does the advantage materially
   improve free cash flow through higher prices, lower costs, or lower investment?
   Common false positives: proprietary formats nobody wants, switching costs
   in low-margin categories, network effects in markets too small to matter.

3. TEMPORARY ADVANTAGE
   Competitive arbitrage will erode the benefit within 1-3 years as competitors
   copy or technology shifts. Test: would a capable, committed, well-funded
   competitor be able to match this within three years?
   Common false positives: first-mover advantages in fast-moving markets,
   product features without platform lock-in, talent density in talent-liquid markets.

4. BRAND AWARENESS MISTAKEN FOR BRAND POWER
   Purchased recognition (Super Bowl ads, viral campaigns, PR blitz) is not
   Brand Power. The barrier (hysteresis) has not been built. Test: can this brand
   command a price premium on an OBJECTIVELY EQUIVALENT product from a commodity
   competitor? Has it done this for more than 10 years?
   Common false positives: well-known startups under 10 years old claiming
   Brand Power, businesses with high NPS claiming brand as a moat.

5. NETWORK EFFECTS MISTAKEN FOR NETWORK ECONOMIES POWER
   Strong product-market fit with user-dependent value dynamics does not
   guarantee Power. Uber's geographical constraint (square root function in
   2D space, not Metcalfe's Law) means two competitors can coexist in large markets.
   Test: can users multi-tenant? Is the network bounded by geography or use case?
   Does installed base leadership translate to actual pricing or cost advantage?
   Common false positives: any marketplace claiming network effects without
   analyzing whether those effects create a real competitive barrier.

6. CORNERED RESOURCE FAILURES
   Five specific tests a claimed cornered resource must pass:
   - Idiosyncratic (is it this specific asset, or the ability to acquire similar?)
   - Non-arbitraged (does compensation capture the value? Brad Pitt problem)
   - Transferable (would it create similar value at another company?)
   - Ongoing (if removed, does performance suffer?)
   - Sufficient (alone, does it explain differential returns?)
   Leadership teams, "star talent," key partnerships — most fail at least one.

7. PROCESS EXCELLENCE vs. PROCESS POWER
   Most operational improvements are replicable. The hysteresis test is hard:
   the process must have been built up over years in ways that are tacit,
   complex, and impossible to codify. "We execute really well" is not Process Power.
   Test: has a competitor tried to replicate this specific capability and
   failed materially? If not, assume it's operational excellence.

## Roger Martin's Critiques (Apply These)

Martin disputes three of the seven powers and adds a missing one:

SWITCHING COSTS CRITIQUE: "Provides no genuine customer value — customers are
held hostage, not delighted. This is an early warning sign, not a moat."
Look for: Is the company retaining customers who would prefer to leave?
Is it investing in switching cost depth instead of actual product quality?
Is switching cost advantage being slowly eroded by new architecture?

CORNERED RESOURCE CRITIQUE FOR TALENT: Goldman Sachs had 25-year CAGR of only
8.4% despite extreme talent density. Talented people extracted value FROM the
company rather than creating shareholder value. Over 50 years, talent
increasingly corners companies rather than companies cornering talent.
Apply this skeptically to any "our people are our moat" claim.

COUNTER-POSITIONING EXPIRATION: Dollar Shave Club had genuine CP against Gillette.
Never became profitable. Gillette launched a competing subscription model.
DSC was acquired. CP without a subsequent durable Power is just a temporary window.
Ask: what Power does this company have for AFTER the CP window closes?

ACTIVITY SYSTEM CRITIQUE (missing eighth power): Some businesses have durability
from an integrated web of mutually reinforcing choices that no single power
explains. IKEA's moat isn't Scale or Process alone — it's the specific combination
of flat-pack furniture + self-service + global supply chain + in-store restaurant.
If you're analyzing a business where no single power seems to explain the durability,
ask whether the integrated configuration IS the power.

## Your Analysis Tasks

1. CHALLENGE THE TOP 3 CLAIMED ADVANTAGES
   Take the company's or founder's claimed advantages and apply the disqualifiers.
   For each: which disqualifier(s) might apply? What's the evidence either way?
   Be specific — "this might not be real" is not enough. Identify the specific test
   they fail or nearly fail.

2. THE "OPERATIONAL EXCELLENCE IS NOT STRATEGY" AUDIT
   List every advantage this business has. For each one, ask: is this something
   a consulting firm or a well-resourced competitor could replicate in 18 months?
   Anything that passes that test is NOT Power. Report the proportion of claimed
   advantages that fail this filter.

3. THE POWER DECAY SCENARIOS
   For each confirmed or claimed power: what single development would make this
   power disappear? Technology shifts? Paradigm shifts? Regulatory change?
   Competitive response from a well-capitalized challenger?
   - Scale Economies: can be disrupted by a technology that lowers fixed costs
   - Network Economies: can be disrupted by a new platform that fragments the network
   - Counter-Positioning: expires when incumbent capitulates or pivots successfully
   - Switching Costs: can be eroded by new architectures that lower switching costs
   - Branding: can be destroyed by dilution, quality failures, or generational shifts
   - Cornered Resource: expires with patents, with team departures, with asset loss
   - Process Power: can be disrupted by paradigm shifts that make the process irrelevant

4. THE UPSTREAM COMPETITOR TEST
   Is there a company that IS genuinely counter-positioned against this business?
   What does the Challenger look like? (A company that disrupts the disruptor.)
   This is Helmer's most sobering question for any CP-based business.

5. THE VERDICT: WHAT'S REAL
   Of all the claimed powers, which are:
   - Confirmed real (both Benefit AND Barrier, passes all disqualifiers)
   - Probably real but needs monitoring (passes most tests, has a weak point)
   - Operational excellence, not Power (benefit without durable barrier)
   - Simply false (fails the test outright)

Message the Power Cartographer with the specific weak points in their inventory.
Message the Counter-Positioning Scout with any CP claims you find fragile.
```

## Phase 3: Monitor and Cross-Pollinate

While agents run, watch for cross-team signals. If teams are enabled:

- The Power Cartographer may message the Moat Devil's Advocate: "The switching
  cost claim looks real but thin — please stress test it"
- The Lifecycle Timer may message the Cartographer: "This business already missed
  the Network Economies window — update that verdict to FORECLOSED"
- The Counter-Positioning Scout may message the Lifecycle Timer: "The incumbent is
  at Stage 4 (Anger) — capitulation is near, update the timing"
- The Moat Devil's Advocate may message all: "The entire claimed moat is operational
  excellence — there may be no confirmed power here at all"

Synthesize cross-team messages before moving to Phase 4. They often contain
the most important insights.

## Phase 4: Synthesize — The Helmer Verdict

After all agents report, the lead synthesizes into the Power Analysis Document.
This is where the real strategic insight lives.

### The Power Portfolio Table

Build a complete table:

| Power | Status | Benefit (specific) | Barrier (specific) | Stage Window | Durability |
|---|---|---|---|---|---|
| Scale Economies | [CONFIRMED / EMERGING / NOT A POWER / FORECLOSED] | [what benefit] | [what barrier] | [OPEN/CLOSED] | [High/Med/Low] |
| Network Economies | ... | | | | |
| Counter-Positioning | ... | | | | |
| Switching Costs | ... | | | | |
| Branding | ... | | | | |
| Cornered Resource | ... | | | | |
| Process Power | ... | | | | |

### Power Compounding Check

Helmer's deepest insight: powers compound. Netflix has Scale Economies +
Switching Costs (content library investment) + eventual Branding. Amazon has
five or six. Intel had Scale + Network + Switching simultaneously. Each power
makes the others more durable and harder to attack from any angle.

Check: do any confirmed powers reinforce each other? If yes, the position is
substantially stronger than any single power suggests. If all confirmed powers
are in a single category (e.g., only Counter-Positioning), the position is
fragile — one window, no redundancy.

### The Lollapalooza Check

Are there multiple powers stacking simultaneously? If yes, this is the most
durable strategic position possible. Name it explicitly:

"This business has a developing [Power A] + [Power B] compound stack, which means
[specific reinforcing effect]. This is the kind of position that produces persistent
differential returns for a decade or more."

If no compound stack exists, be honest about what that means.

### The Power Progression Verdict

Given the lifecycle stage:
- What power windows are OPEN and must be seized in the next 12-24 months or lost?
- What power windows have already CLOSED and represent permanent gaps?
- What powers are currently BUILDING and will be confirmed at the next stage?
- What is the SINGLE MOST IMPORTANT power to build next?

This is the actionable strategic output. Helmer would say: "Strategy is not a
multiple-choice answer to 'which power' — it's the creative act of figuring out
which specific form of power is achievable from where you are right now."

### What Helmer Would Say

Write a 2-3 paragraph synthesis in Helmer's voice. He is precise, taxonomic,
direct, and unimpressed by mere competitive advantages. He uses specific vocabulary:
Benefit, Barrier, persistent differential returns, the Surplus Leader Margin,
hysteresis, fiat, collateral damage, the Power Progression.

He would not say "you have a great product and loyal customers" without translating
that into a specific power type with a named mechanism. He would say "no" to claimed
powers that don't clear the Benefit/Barrier test, without apology. He is sober about
operational excellence not being strategy.

Example Helmer framing:
- "The switching costs you have are real but they protect the installed base, not
  new customer acquisition. Your margin expansion story depends on a base that
  won't grow without ongoing price competition."
- "This Counter-Positioning window is genuine — Incumbent X faces a clear joint NPV
  dilemma. But you have 18-24 months before they either capitulate or fail. What
  Scale Economy are you building in that window?"
- "You have no confirmed Power. You have Compelling Value and operational excellence.
  Those are necessary but not sufficient. The journey isn't done."

### The Verdict Framework

Helmer would classify a business into one of these categories:

**POWER COMPOUND**: Two or more confirmed powers that reinforce each other.
Rare. Produces persistent differential returns for many years.
Example action: "Defend the stack. Invest in the weaker element."

**SINGLE POWER**: One confirmed power with a real Benefit and Barrier.
Good. Produces differential returns but vulnerable to a single attack vector.
Example action: "Build a second power during the open lifecycle window."

**EMERGING POWER**: Benefit exists, Barrier under construction. Power not yet
confirmed. Returns are good but not yet protected. High priority: clear the
Barrier or accept temporary advantage.
Example action: "Name the specific barrier-building investment needed. Do it now."

**OPERATIONAL EXCELLENCE**: No confirmed powers. Competitive advantages exist
but they can be replicated. Returns compete away over time.
Example action: "The Takeoff window question: which power can this business
seize in the next growth phase? Build toward that."

**FLUX STATE**: No powers, no clear path to building them. Competitive arbitrage
will drive margins toward zero. Compelling Value may exist but s × m trends to zero.
Example action: "Reinvention required. A paradigm shift or new business model is
needed to create a Power opportunity."

## Phase 5: Present and Follow-up

Present in this structure:

```
## Hamilton Helmer's 7 Powers Analysis: [Business Name]

### Power Portfolio

[The full Power Portfolio Table]

### Power Stack Assessment

[POWER COMPOUND / SINGLE POWER / EMERGING / OPERATIONAL EXCELLENCE / FLUX]

[Explanation of confirmed powers, their specific Benefit and Barrier mechanisms,
and whether they compound]

### Power Progression

Lifecycle stage: [Origination / Takeoff / Stability]
Windows OPEN (must be seized now): [list]
Windows FORECLOSED (gone permanently): [list if any]
Critical priority: [the single most important power to build in the next 12-24 months]

### What Helmer Would Say

[2-3 paragraph synthesis in Helmer's voice: precise, taxonomic, naming specific
mechanisms, honest about what's not real Power]

### Stress Test Results

[From the Moat Devil's Advocate: the 1-2 weakest points in the power position,
what would need to be true to maintain them]

### Actionable Rules

The 3-5 most concrete strategic decisions implied by this analysis:
1. [Power-seizing action with specific mechanism]
2. [Power-building action with lifecycle timing]
3. [Power-defending action with specific risk]
...

### Framework Notes

- Not analyzed here: industry-level attractiveness (run /munger or Porter for that)
- Network effects granularity: if Network Economies is the primary power, the NFX
  taxonomy (16 types) gives deeper insight than this analysis
- Disruption threat: Christensen's framework analyzes the Disruptive Innovation path
  in more detail than Counter-Positioning alone captures
- Business quality: Helmer diagnoses structural position, not management quality,
  capital allocation, or ethics — run /munger for the full business quality filter
```

---

## Batch Mode: Compare Multiple Companies

If given multiple companies to compare (e.g., "helmer Netflix vs Disney vs Apple"):

Run the full analysis for each, then produce a comparison table:

| Company | Confirmed Powers | Stage | Strongest Power | Biggest Gap | Verdict |
|---|---|---|---|---|---|
| [Co 1] | [powers] | [stage] | [strongest] | [gap] | [verdict] |
| [Co 2] | ... | | | | |

Then: "Which company has the most durable strategic position and why?"
Helmer's answer: count the confirmed powers with compound effects, factor in
lifecycle stage, and identify which company's power windows are most open.

---

## Scoring Discipline

- **Call non-powers what they are.** Most "competitive advantages" are operational
  excellence. The framework's value is in the precision of the "no."
- **Name the specific mechanism.** Never say "they have switching costs" —
  say "they have procedural switching costs because SAP implementations require
  180+ days of staff retraining, creating a $500K+ migration cost per enterprise customer."
- **Apply the Benefit/Barrier test to every claim.** No exceptions.
- **Power Progression is hard.** If the lifecycle window is closed for a power type,
  say so clearly — don't suggest building a power in a closed window.
- **Don't confuse product quality with Power.** The best product rarely wins in the
  long run without a structural barrier. Compelling Value ≠ Power.

---

## Important Notes

- **Cost:** Each analysis spawns 4 specialist agents. Moderate token cost. Worth it
  for strategic decisions.
- **Model selection:** Specialists use Sonnet; the lead (synthesizing) uses Opus.
- **Pairing recommendations:**
  - Run `/thiel` first if the question is "is this a monopoly or just a good business?"
    Thiel and Helmer are complementary — Thiel on whether you need a monopoly,
    Helmer on which power mechanism achieves it
  - Run `/munger` for full business quality filter — Helmer diagnoses structural
    power, Munger adds psychology, economics, and the full mental lattice
  - Run `/christensen` if you're uncertain about disruption threats — Christensen
    gives the attacker's trajectory in more detail than Counter-Positioning alone
- **Primary sources:** 7 Powers (2016), NFX interview (nfx.com/post/seven-powers),
  Acquired podcast episode, Lenny Rachitsky interview (May 2024)
- **Helmer's own stance on the framework:** "The overall types of power, the genotypes,
  are simple — there are seven of them. But the phenotypes, the specific instantiations
  for each company, are incredibly complicated." The 7 types are the checklist.
  The company-specific analysis is the actual work.
