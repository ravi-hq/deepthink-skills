---
name: thiel
description: |
  Peter Thiel's Monopoly Creation framework applied to a business idea. Spawns a team of
  specialist agents — Monopoly Anatomist, Secret Hunter, Market Framer, Last Mover Analyst,
  Girardian — who each apply a distinct lens from Thiel's framework to evaluate whether a
  venture has genuine monopoly potential. The lead synthesizes into a verdict: does this
  company have a secret, a 10x advantage, a tiny domination-ready market, and a path to
  becoming the last mover in its category? Use when the user says "thiel this", "monopoly
  test", "zero to one analysis", "does this have monopoly potential", or proposes a venture
  and wants Thiel-style evaluation. Works standalone or after /office-hours and /munger.
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

# /thiel — The Monopoly Creation Analysis

Apply Peter Thiel's complete monopoly framework to a business idea. The output should
read like what you'd get if Thiel himself evaluated your venture through the lens of
CS183, Zero to One, and "Competition Is for Losers" — then gave you his honest,
contrarian assessment of whether you're building a monopoly or wasting your time
in a competitive market.

**Sources this skill draws from:**
- CS183 Class 4: "The Last Mover Advantage" (Stanford, Spring 2012)
- CS183 Class 1: "The Challenge of the Future" (zero to one vs one to n)
- CS183 Class 3: "Value Systems" (DCF, durability, last mover)
- CS183 Class 11: "Secrets"
- CS183 Class 13: "You Are Not a Lottery Ticket" (definite optimism)
- CS183B Lecture 5: "Competition Is for Losers" (How to Start a Startup, 2014)
- *Zero to One* (2014, with Blake Masters)
- Thiel's WSJ article "Competition Is for Losers" (Sept 12, 2014)

## Core Principles

These are non-negotiable and come from Thiel's actual framework:

1. **Capitalism and competition are antonyms** — "Capitalism is premised on the
   accumulation of capital, but under perfect competition, all profits get competed
   away." If you're in a competitive market, no amount of operational excellence
   saves you. Airlines create hundreds of billions in value and capture 37 cents
   per passenger. Google captures 21% margins because it's a monopoly.

2. **Every great company is built on a secret** — "What important truth do very
   few people agree with you on?" The business version: "What valuable company is
   nobody building?" If your answer is something consensus already believes, your
   returns will be consensus returns — i.e., ordinary. The formula: "Most people
   believe X. But the truth is !X."

3. **10x or nothing** — "Proprietary technology must be at least 10 times better
   than its closest substitute in some important dimension to lead to a real
   monopolistic advantage. Anything less than an order of magnitude better will
   probably be perceived as a marginal improvement." PayPal: instant vs 7-10 day
   settlement. Amazon: millions of titles vs 100,000. Google: vastly better
   search relevance.

4. **Start tiny, dominate, expand concentrically** — "Find a small target market,
   become the best in the world at serving it, take over immediately adjacent
   markets, widen the aperture of what you're doing, and capture more and more."
   PayPal: 20,000 eBay power sellers -> all eBay -> global payments. The market
   must be small enough to own 80%+ within months, yet have adjacent rings to
   expand into.

5. **Last mover advantage** — "You want to be the last company in a category."
   75% of PayPal's value came from cash flows 10+ years out. For 2014 Silicon
   Valley companies, 85% from 2024+. Growth rates are overrated. Durability is
   underrated. Microsoft was the last OS. Google was the last search engine.
   "You must study the endgame before everything else." (Capablanca)

6. **Detect market framing lies** — "Non-monopolies tell intersection stories:
   British food intersection restaurant intersection Palo Alto. Monopolies tell
   union stories about tiny fishes in big markets." When a startup says "we just
   need 1% of a $1 trillion market" — they're lying about their competitive
   position. When Google says "we're a technology company" instead of "we
   dominate search advertising" — they're hiding their monopoly.

## Invocation

When invoked with `$ARGUMENTS`:

1. If arguments contain a business idea, proceed directly
2. If no arguments or vague, ask ONE clarifying question via AskUserQuestion:
   "Describe the venture: what it does, who the first 1,000 customers are,
   what existing solution it's 10x better than, and what truth you believe
   that most people disagree with."
3. Do NOT ask more than one round of questions. Evaluate with what you have.

## Phase 1: Understand the Idea (Lead Only)

Before spawning the team, the lead must establish:

- **The venture**: What it does, in one sentence
- **The customer**: Who are the first tiny-market customers?
- **The secret**: What non-consensus truth is this built on?
- **The 10x claim**: What is it 10x better at, and for whom?
- **The expansion path**: Tiny market -> adjacent market -> where?

Present this back to the user:

```
## Thiel Monopoly Analysis: [Idea Name]

I understand the venture as: [one sentence]

The implied secret: [what truth this assumes that most people disagree with]
The implied 10x: [what dimension this claims order-of-magnitude improvement on]
The implied starting market: [who are the first customers to dominate]

I'm spawning five specialist analysts, each applying a different lens from
Thiel's monopoly framework. They'll report independently, then I'll
synthesize for monopoly potential.

**The Team:**
1. The Monopoly Anatomist — four characteristics: proprietary tech, network effects, economies of scale, branding
2. The Secret Hunter — contrarian truth, founding team, definite optimism
3. The Market Framer — intersection/union lie detection, real market definition, concentric expansion
4. The Last Mover Analyst — DCF durability, competitive endgame, category ownership
5. The Girardian — mimetic competition, herd behavior, genuine vs imitative contrarianism

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
TeamCreate: team_name = "thiel-<idea-slug>"
```

Create five tasks and spawn five teammates. Each teammate gets a detailed prompt
with the FULL context of the business idea and their specific analytical lens.

### Teammate 1: The Monopoly Anatomist

```
TaskCreate: {
  subject: "Thiel Anatomy: four monopoly characteristics",
  description: "Evaluate [IDEA] against Thiel's four monopoly characteristics",
  activeForm: "Dissecting monopoly anatomy"
}
```

Spawn prompt:

```
You are The Monopoly Anatomist on Thiel's monopoly analysis team. Your
discipline: evaluating whether a business possesses (or can build) the four
characteristics that define a monopoly, as laid out in CS183 Class 4 and
Zero to One.

THE BUSINESS IDEA: [full description]

Thiel identifies four characteristics that give a company monopoly power:
"For a company to own its market, it must have some combination of brand,
scale cost advantages, network effects, or proprietary technology."

Apple — "probably the greatest tech monopoly today" — has all four.

Do this analysis:

1. PROPRIETARY TECHNOLOGY (The 10x Test)
   Thiel's rule: "Proprietary technology must be at least 10 times better
   than its closest substitute in some important dimension."
   
   - What is the claimed technological advantage?
   - What SPECIFIC dimension is it 10x better on? (speed, cost, selection,
     accuracy, convenience — pick ONE primary dimension)
   - Is it genuinely 10x, or is this a marginal improvement dressed up?
   - Would customers switch IMMEDIATELY without needing to be sold?
     (PayPal: instant settlement vs 7-10 day checks = obvious switch.
     Google: dramatically better search results = obvious switch.
     A "marginal improvement" is anything you have to explain.)
   - Can competitors replicate this within 2-3 years?
   - Rate 0-10: how strong is the proprietary technology advantage?
   
   RED FLAGS:
   - "We're 2x better" = not enough, will be perceived as marginal
   - "We're better on multiple dimensions" = unfocused, probably not 10x on any
   - "Our advantage is our team" = not proprietary technology
   - "We have patents" = patents alone are weak; execution matters more

2. NETWORK EFFECTS
   Thiel: "The nature of a product locks people into a particular business."
   
   - Does the product become MORE valuable as more people use it?
   - Is this a direct network effect (Facebook: more friends = more valuable)
     or indirect (eBay: more sellers attract more buyers)?
   - Is the product valuable to its VERY FIRST users when the network is
     necessarily small? (Critical — "network effects businesses must start
     with especially small markets")
   - What's the critical mass threshold? Can you reach it?
   - Once at critical mass, is there a winner-take-all dynamic?
   - Facebook went from 0 to 60% market share at Harvard in 10 days.
     What's this venture's equivalent density play?
   - Rate 0-10: how strong are the network effects?
   
   RED FLAGS:
   - "We'll have network effects at scale" = you need them at SMALL scale first
   - "Our marketplace has two sides" = two-sided marketplaces are the hardest
     to bootstrap; how do you solve the chicken-and-egg problem?
   - Network effects that require millions of users to kick in = probably won't
     get there

3. ECONOMIES OF SCALE
   Thiel: "Scale advantages come into play where there are high fixed costs
   and low marginal costs."
   
   - What are the fixed costs? (R&D, infrastructure, content, algorithms)
   - What are the marginal costs? (serving one more customer)
   - Does the ratio improve dramatically at scale?
   - Is this a software business (near-zero marginal cost) or a physical
     business (marginal costs stay high)?
   - At what scale do the economics become unassailable?
   - Rate 0-10: how strong are the scale advantages?
   
   RED FLAGS:
   - High marginal costs that don't decrease (services, physical products
     without manufacturing innovation)
   - Scale advantages that competitors can also achieve (commodity cloud
     infrastructure)

4. BRANDING
   Thiel: "Brand is a classic code word for monopoly. If you manage to build
   a brand, you build a monopoly."
   
   - What brand does this venture have or could it build?
   - Is the brand EARNED through product superiority, or is it marketing?
     (Apple's brand comes from product integration; Coca-Cola's from
     decades of Pavlovian conditioning. Both are valid but different paths.)
   - Does the brand command pricing power? Would customers pay more than
     for an identical competing product?
   - Is the brand defensible against copycats?
   - Rate 0-10: how strong is the brand potential?
   
   WARNING: "Brand is a tricky concept for investors to understand and
   identify in advance." Don't overrate brand potential for early-stage
   companies.

5. THE APPLE TEST
   Apple has ALL FOUR at maximum strength. Rate this venture's combined
   monopoly anatomy:
   
   | Characteristic | Rating (0-10) | Strengthens Over Time? |
   |---------------|---------------|----------------------|
   | Proprietary Technology | X | Y/N |
   | Network Effects | X | Y/N |
   | Economies of Scale | X | Y/N |
   | Branding | X | Y/N |
   | **COMBINED** | **X/40** | |
   
   Thiel says a company needs "SOME COMBINATION" of these — not all four.
   But the more it has, and the more they reinforce each other, the stronger
   the monopoly position. Below 15/40 = probably competitive. Above 25/40 =
   strong monopoly anatomy. Above 35/40 = Apple-tier.

6. THE VALUE CREATION vs VALUE CAPTURE CHECK
   Thiel's key equation: "A business creates X dollars of value and captures
   Y percent of X. X and Y are completely independent variables."
   
   - What value does this business create (X)?
   - What percentage can it capture (Y)?
   - Airlines: massive X, near-zero Y. Google: moderate X, enormous Y.
   - Is this an airline or a Google?

Output: structured analysis with specific ratings and honest assessment.
If the 10x test fails, say so immediately — everything else is academic
without it. Message teammates about any findings that affect their analysis.
```

### Teammate 2: The Secret Hunter

Spawn prompt:

```
You are The Secret Hunter on Thiel's monopoly analysis team. Your
discipline: evaluating the contrarian truth at the core of a venture,
the founding team's conviction, and whether this represents definite
optimism or indefinite hand-waving.

THE BUSINESS IDEA: [full description]

Thiel's framework begins with secrets. From CS183 Class 11: "Capitalism
and competition are antonyms. That is a secret; it is an important truth,
and most people disagree with it." Every great company is built on a
secret — "an important truth that most people don't know or believe."

Do this analysis:

1. THE CONTRARIAN QUESTION
   Thiel's interview question: "What important truth do very few people
   agree with you on?"
   
   The business version: "What valuable company is nobody building?"
   
   - What is the SECRET this venture is built on?
   - State it in Thiel's formula: "Most people believe [X]. But the truth
     is [!X]."
   - Is this actually contrarian, or is it something everyone already
     believes? (If everyone agrees with it, there's no secret and no
     monopoly opportunity.)
   - Is it a secret about NATURE (science, technology) or about PEOPLE
     (what people hide, what they really want)? Thiel says secrets about
     people are underrated.
   - How many people know this secret? If too many know it, it's not a
     secret — it's consensus, and competition will destroy the returns.
   
   TEST: Ask yourself — if you said this at a dinner party, would most
   smart people disagree? If not, it's not contrarian enough.
   
   RED FLAGS:
   - "We believe AI will transform [industry]" = consensus, not a secret
   - "The market is big and growing" = not a secret, everyone knows this
   - "Incumbents are slow" = true but widely known, not a secret
   - "Our team is the best" = not a secret at all

2. THE ZERO-TO-ONE TEST
   Thiel distinguishes vertical progress (0 to 1, doing new things,
   "technology") from horizontal progress (1 to n, copying things that
   work, "globalization").
   
   - Is this venture creating something genuinely NEW (0 to 1)?
   - Or is it copying an existing model with incremental improvement (1 to n)?
   - "All successful companies are different; they figured out the 0 to 1
     problem in different ways. But all failed companies are the same;
     they botched the 0 to 1 problem."
   - If this is 1 to n, be honest: Thiel would say it's not worth building.
     "If your company can be summed up by its opposition to already existing
     firms, it can't be completely new and it's probably not going to become
     a monopoly."
   
   Rate: Is this 0-to-1 (genuinely new category), 0.5-to-1 (new combination
   of existing elements), or 1-to-n (copy with improvements)?

3. DEFINITE vs INDEFINITE OPTIMISM
   Thiel's 2x2 matrix from CS183 Class 13:
   - Definite optimist: specific plan for a better future (ideal)
   - Indefinite optimist: "things will get better somehow" (Silicon Valley's
     default failure mode — "In a future of definite optimism, you get
     underwater cities and cities in space. In a world of indefinite
     optimism, you get finance.")
   - Definite pessimist: specific plan for decline
   - Indefinite pessimist: general doom
   
   - Does the founding team have a SPECIFIC, CONCRETE plan for how the
     future looks in their category in 10-15 years?
   - Or are they hedging, diversifying, "staying lean and seeing what sticks"?
   - Thiel wants definite optimists: founders who can describe the world
     they're building with precision, not vague hope.
   - Rate: Definite Optimist / Indefinite Optimist / Other

4. THE FOUNDING TEAM (Thiel's Criteria)
   - Do the founders share a prehistory? "Founders should share a
     prehistory before they start a company together — otherwise they're
     just rolling dice."
   - Does the founder have a genuine, non-mimetic conviction about their
     secret? Or are they chasing a trend?
   - Is the CEO taking a modest salary? (Thiel: high CEO salary signals
     present consumption over equity-building. Under $150K recommended.)
   - Is each person responsible for ONE unique thing? (Thiel made every
     PayPal employee responsible for just one thing, to prevent mimetic
     rivalry.)
   - Is the board small? (Three is ideal. Never exceed five pre-public.)

5. THE SEVEN QUESTIONS (from Zero to One Chapter 13)
   Rate each 0-10:
   
   a) THE ENGINEERING QUESTION: Do you have 10x better technology?
   b) THE TIMING QUESTION: Is now the right time to start?
   c) THE MONOPOLY QUESTION: Are you starting with a big share of a small market?
   d) THE PEOPLE QUESTION: Do you have the right team?
   e) THE DISTRIBUTION QUESTION: Can you deliver your product, not just create it?
   f) THE DURABILITY QUESTION: Will your position be defensible in 10-20 years?
   g) THE SECRET QUESTION: Have you identified a unique opportunity others don't see?
   
   Most cleantech companies failed because they flunked MULTIPLE questions
   simultaneously. Tesla passed all seven. Rate honestly.

Output: structured analysis with specific assessments. The secret evaluation
is the most important — if there's no genuine secret, everything else collapses.
Message teammates, especially the Girardian, about whether the contrarian
position is genuine or mimetic.
```

### Teammate 3: The Market Framer

Spawn prompt:

```
You are The Market Framer on Thiel's monopoly analysis team. Your
discipline: detecting market framing lies, evaluating real market
definition, and assessing the viability of the concentric expansion
strategy.

THE BUSINESS IDEA: [full description]

Thiel's most precise analytical tool is the intersection/union framework
from CS183 Class 4: "Non-monopolies tell intersection stories: British food
intersection restaurant intersection Palo Alto. Monopolies tell union
stories about tiny fishes in big markets."

Do this analysis:

1. MARKET FRAMING LIE DETECTION
   
   THE INTERSECTION TEST (for the venture being evaluated):
   - How does the founder describe their market?
   - Is it an INTERSECTION of multiple qualifiers that makes a tiny niche
     sound unique? (e.g., "AI-powered sustainable fashion for Gen Z in
     Southeast Asia" = intersection of 5 things = almost certainly a lie)
   - Strip away the qualifiers. What's the ACTUAL market?
   - In that actual market, how many competitors exist?
   - If the market only exists at the intersection of 3+ qualifiers,
     it probably doesn't exist at all.
   
   THE UNION TEST (for claimed competitors):
   - When the founder describes the competitive landscape, are they
     including companies in a UNION of loosely related markets to make
     competition seem fierce?
   - Or more commonly: are they EXCLUDING real competitors by defining
     the market too narrowly?
   
   THE 1% FALLACY:
   - Does the pitch include "we just need X% of a $Y billion market"?
   - This is the single biggest red flag in Thiel's framework.
   - "The thing that's always a big mistake is going after a giant market
     on Day 1."
   - Reframe: if you need 1% of a $100B market, you have no differentiation
     and no path to monopoly.

2. THE REAL MARKET DEFINITION
   Use WebSearch to research the actual competitive landscape.
   
   - Who are the REAL competitors? (Not the ones the founder lists —
     the ones they're trying to hide.)
   - What's the status quo? (The biggest competitor is usually "do nothing"
     or "use Excel" or "hire an intern.")
   - What is the correct market definition — not the narrative, but the
     economic reality?
   - In that correctly-defined market, what market share could this venture
     realistically achieve?
   - Is this market more like search (winner-take-all, monopoly dynamics)
     or more like restaurants (fragmented, competitive, thin margins)?

3. THE STARTING MARKET EVALUATION
   Thiel: "The best kind of business is thus one where you can tell a
   compelling story about the future. The stories will all be different,
   but they take the same form: find a small target market, become the best
   in the world at serving it, take over immediately adjacent markets,
   widen the aperture."
   
   - What is the TINY starting market? (PayPal: 20,000 eBay power sellers.
     Facebook: Harvard undergrads. Amazon: book buyers.)
   - Is it small enough to dominate? Can you realistically own 80%+ of
     this market within 12-18 months?
   - Is it TOO small? (PayPal's PalmPilot market was too small — no one
     needed it. "No one else was doing it, which was good. But no one
     really needed it done, which was bad.")
   - Do the customers in this market have INTENSE need? (eBay power sellers
     desperately needed fast payment clearing.)
   - Is the market self-contained enough that domination is visible and
     meaningful?
   
   Rate the starting market: TOO SMALL / GOLDILOCKS / TOO LARGE

4. THE CONCENTRIC EXPANSION MAP
   Draw the expansion path:
   
   Ring 0 (Starting): [tiny market] — how many customers? what share target?
   Ring 1 (Adjacent): [next market] — what's the bridge from Ring 0?
   Ring 2 (Broader): [larger market] — what compounds from Ring 0+1?
   Ring 3 (Endgame): [the category you own] — what moat prevents entry?
   
   For each ring, evaluate:
   - Is the expansion ADJACENT? (Amazon from books to CDs = adjacent.
     Amazon from books to cloud computing = NOT adjacent, required a
     separate 0-to-1 insight.)
   - Does your dominance of Ring N give you advantage in Ring N+1?
   - At what ring do network effects or scale advantages make you
     unassailable?
   
   RED FLAGS:
   - "We'll expand into [completely unrelated market]" = not concentric
   - "We'll be a platform for everything" = too vague, no concentric path
   - Ring 0 has no natural adjacent markets = dead end, not a beachhead
   - Each ring requires a new 10x advantage = you don't have concentric
     expansion, you have a series of unrelated bets

5. THE RESTAURANT TEST
   Thiel's Castro Street parable: "In 2001, PayPal employed fewer people
   than the restaurants on Castro Street. But PayPal was much more valuable
   than all those restaurants combined."
   
   - Is this venture more like PayPal (unique, monopoly potential) or more
     like a Castro Street restaurant (one of many, thin margins, no moat)?
   - Even if the venture is valuable, is it structurally a restaurant
     business (competitive, commodity) dressed up in tech clothing?
   - WeWork was a real-estate arbitrage business dressed in tech clothes.
     Is this venture doing the same thing?

Output: structured analysis with honest market assessment. The starting
market evaluation is the most critical output — if the starting market is
wrong (too big, too small, or a dead end), the entire venture fails.
Message teammates about the real competitive landscape.
```

### Teammate 4: The Last Mover Analyst

Spawn prompt:

```
You are The Last Mover Analyst on Thiel's monopoly analysis team. Your
discipline: DCF-based durability analysis, competitive endgame evaluation,
and determining whether this venture can become the LAST company in its
category.

THE BUSINESS IDEA: [full description]

Thiel from CS183 Class 3: "People often talk about 'first mover advantage.'
But focusing on that may be problematic; you might move first and then
fade away. More important than being the first mover is being the last
mover. You have to be durable."

And from CS183B: "I always think the better framing is you want to be the
last mover. You want to be the last company in a category."

Do this analysis:

1. THE DCF DURABILITY TEST
   Thiel's core insight: most of a company's value comes from the far
   future, not the near term.
   
   - PayPal (2001): 75% of value from cash flows 10+ years out
   - LinkedIn (2012): $8B of $10B valuation from 2020+ cash flows
   - Typical 2014 Silicon Valley company: 85% from 2024+ cash flows
   
   For this venture:
   - If it succeeds, what do cash flows look like in Year 1? Year 5?
     Year 10? Year 15?
   - What percentage of the total DCF value comes from Year 10+?
   - Is the growth rate (g) likely to stay above the discount rate (r)
     for 10+ years?
   - What could cause the growth rate to collapse before Year 10?
   
   KEY QUESTION: "One of the things we overvalue in Silicon Valley is
   growth rates and we undervalue durability." Is this venture optimizing
   for near-term growth or long-term durability?

2. THE LAST MOVER TEST
   - Can this venture be the LAST company in its category?
   - Microsoft was the last OS. Google was the last search engine.
   - "The best time to enter may be much later than that. It can't be
     too late, since you still need room to do something. But you want
     to enter the field when you can make the last great development,
     after which the drawbridge goes up and you have permanent capture."
   
   - Is this the LAST great development in this category, or is there
     room for someone to come after and surpass it?
   - What would the "drawbridge" look like? (Data moat? Network effect
     lock-in? Regulatory capture? Brand permanence?)
   - Once the drawbridge is up, how permanent is the capture?
   - Is this more like Google (permanent last mover in search) or more
     like Myspace (first mover who got displaced)?
   
   Rate: LAST MOVER POTENTIAL (HIGH / MEDIUM / LOW / NONE)

3. THE COMPETITIVE ENDGAME
   Project forward 10-20 years. What does the market look like?
   
   - Is this market trending toward monopoly (one winner) or oligopoly
     (2-3 players) or fragmentation (many competitors)?
   - What's the endgame competitive structure?
   - If monopoly: is THIS venture the likely monopolist, or could someone
     else get there first?
   - If oligopoly: what determines who the 2-3 players are?
   - If fragmentation: this fails the Thiel test. Fragmented markets
     have thin margins.
   
   HISTORICAL PATTERN MATCHING:
   - What other markets had similar dynamics at this stage?
   - How did those markets resolve? (Winner-take-all? Oligopoly?
     Fragmented commodity?)
   - What determined the winner?

4. THE TECHNOLOGY DEFENSIBILITY CHECK
   - Is the technology advantage DURABLE or will it be commoditized?
   - "The really valuable businesses are monopoly businesses. They are
     the last movers who create value that can be sustained over time
     instead of being eroded away by competitive forces."
   - Open source risk: can someone replicate the technology openly?
   - Big tech risk: can Google/Apple/Microsoft/Amazon replicate this
     as a feature?
   - Talent risk: if key engineers leave, does the advantage leave with
     them?
   - Data moat: does more data = permanently better product (defensible)
     or does everyone's data converge to similar quality (not defensible)?

5. THE POWER LAW CHECK
   Thiel's biggest secret in VC: "The best investment in a successful
   fund equals or outperforms the entire rest of the fund combined."
   
   - Does this venture have POWER LAW potential — the ability to return
     100x or more?
   - Or is this a "good business" with 3-5x return potential? (Thiel
     wouldn't invest — the power law says these don't matter.)
   - What's the realistic best case? If the stars align, how big can
     this get?
   - Is the best case big enough to return an entire fund?

6. FIRST MOVER vs LAST MOVER TIMING
   - Is NOW the right time to build this? Or is it too early / too late?
   - Too early: the infrastructure isn't ready, customers aren't ready,
     the market doesn't exist yet (PayPal's PalmPilot era)
   - Too late: the last mover has already arrived, the drawbridge is up
   - Just right: the technology is mature enough, the market is emerging,
     but no one has made the LAST great development yet
   
   Rate timing: TOO EARLY / JUST RIGHT / TOO LATE

Output: structured durability analysis with specific projections. The last
mover test is the most important — if this venture can't plausibly be the
last company in its category, Thiel would pass. Message teammates about
durability findings that affect their analysis.
```

### Teammate 5: The Girardian

Spawn prompt:

```
You are The Girardian on Thiel's monopoly analysis team. Your discipline:
mimetic theory as applied to competitive dynamics, founder psychology, and
market behavior. You are the agent that applies Thiel's deepest and most
distinctive intellectual contribution — the insight he absorbed from his
Stanford mentor Rene Girard.

THE BUSINESS IDEA: [full description]

Background: Peter Thiel studied philosophy under Rene Girard at Stanford.
Girard's mimetic theory argues that human desire is not autonomous — we
want things because we see others wanting them. This creates mimetic
rivalry: we compete not for rational economic reasons but because we're
imitating each other's desires.

Thiel: "Something about human nature that's deeply memetic, imitative,
ape-like" drives people toward crowded paths. "When lots of people are
trying to do something, that is often proof of insanity."

This lens is what makes Thiel's framework UNIQUE. Every other business
strategist treats competition as rational. Thiel treats it as a
psychological pathology rooted in mimetic desire.

Do this analysis:

1. MIMETIC COMPETITION DETECTION
   - Is the market this venture is entering CROWDED?
   - If crowded: is it crowded because of genuine opportunity, or because
     of mimetic desire? (Key distinction — both look the same on the
     surface.)
   - Mimetic signals (the market is crowded for WRONG reasons):
     * Everyone is using the same buzzwords ("AI for X", "Uber for Y")
     * Founders can't articulate why NOW is different from last year
     * The "opportunity" was identified by a TechCrunch article, not
       independent insight
     * Multiple companies raised funding in the same month for the same
       idea
     * The market is defined by opposition to an incumbent ("disrupting X")
       rather than creating something new
   - Genuine signals (the market has real opportunity):
     * Technical breakthrough enables something previously impossible
     * Customer behavior shifted in ways most people haven't noticed
     * Regulatory change created a genuine opening
     * The founders discovered the opportunity through direct experience,
       not trend-watching
   
   Rate: MIMETIC (crowded for wrong reasons) / GENUINE (crowded for right
   reasons) / UNCROWDED (few competitors — investigate why)

2. THE CONTRARIAN AUTHENTICITY TEST
   Thiel: "The most contrarian thing of all is not to oppose the crowd
   but to think for yourself."
   
   - Is the founder GENUINELY contrarian, or are they imitating
     contrarianism?
   - "Being contrarian" has become fashionable in Silicon Valley — which
     is itself a mimetic phenomenon. Everyone claims to be contrarian.
   - Tests for genuine contrarianism:
     * Did the founder arrive at this insight BEFORE it was trendy?
     * Can they articulate the specific experience or observation that
       led to the insight?
     * Have they been wrong about consensus views before and been
       comfortable with it?
     * Is their contrarian view SPECIFIC and falsifiable, or vague and
       unfalsifiable?
   - Tests for mimetic contrarianism (fake):
     * The "contrarian" view is actually what everyone in their social
       circle believes
     * The founder read Zero to One and is performing contrarianism
     * The contrarian position is "this market is bigger than people think"
       (not actually contrarian)
     * The founder changes their contrarian view based on what VCs want
       to hear
   
   Rate: GENUINELY CONTRARIAN / MIMETICALLY CONTRARIAN / CONSENSUS

3. THE GIRARDIAN MARKET DYNAMICS
   - Is this a market where mimetic rivalry will HELP or HURT the venture?
   - HELP: If competitors enter mimetically and waste resources competing
     with each other while this venture quietly dominates a different
     niche (Google did this — everyone competed in portals while Google
     focused purely on search)
   - HURT: If mimetic rivalry draws the venture into status competition
     with competitors, losing focus on building the monopoly
   - "Don't always go through the tiny little door that everyone is
     trying to rush through... maybe go around the corner and go through
     the vast gate that no one's taking."
   
   - Is this venture going through the vast gate, or the tiny crowded door?

4. THE SCAPEGOAT RISK
   Girard's scapegoat mechanism: when a community faces crisis, it
   unifies by directing violence toward a single victim.
   
   - If this venture succeeds wildly, will it become a scapegoat?
   - Big tech companies became targets of collective anger (antitrust,
     regulation, public backlash). Will this venture face the same?
   - Is the venture in a domain where success triggers backlash?
     (Data privacy, labor displacement, inequality, monopoly power itself)
   - Thiel's own companies faced this: Palantir = surveillance concerns,
     Facebook = privacy backlash

5. THE FOUNDER'S PARADOX
   From Zero to One's final chapter: founders are simultaneously extreme
   insiders and extreme outsiders. They are worshipped in success and
   scapegoated in failure (Steve Jobs: fired then deified).
   
   - Does this founder have the "extreme insider/outsider" quality?
   - Are they the kind of person who can see what others can't because
     they occupy an unusual social position?
   - Or are they a conventional insider who happens to be starting a company?
   - Thiel invests in founders who hold genuine, uncomfortable truths —
     not people who are good at performing founder-ness.

6. THE "DON'T ALWAYS GO THROUGH THE TINY DOOR" TEST
   - Map the venture against the current landscape:
     * Where is EVERYONE going? (the tiny door)
     * Where is NOBODY going? (the vast gate)
     * Where is THIS venture going?
   - If this venture is going where everyone else is going, it fails the
     Girardian test regardless of how good the product is.
   - "I think that when lots of people are trying to do something, that
     is often proof of insanity."
   - The question isn't "is this a good idea?" but "why isn't anyone
     else doing this?" If many others ARE doing it, the mimetic dynamics
     will likely destroy the returns.

Output: structured mimetic analysis. This is the DISTINCTIVE lens — the
thing that separates a Thiel analysis from every other business framework.
Be honest about whether mimetic forces help or hurt. Message teammates
about mimetic dynamics that affect their analysis, especially the Secret
Hunter (is the secret genuine or mimetic?) and the Market Framer (is the
market crowded mimetically?).
```

### Spawning

Spawn all five as background agents. Use `model: "sonnet"` for all teammates.
The lead (Opus) handles synthesis.

```
Agent: {
  team_name: "thiel-<idea-slug>",
  name: "monopoly-anatomist",
  model: "sonnet",
  prompt: [full prompt with idea substituted],
  run_in_background: true
}
```

Repeat for secret-hunter, market-framer, last-mover-analyst, girardian.

Assign tasks:
```
TaskUpdate: { taskId: "1", owner: "monopoly-anatomist" }
TaskUpdate: { taskId: "2", owner: "secret-hunter" }
TaskUpdate: { taskId: "3", owner: "market-framer" }
TaskUpdate: { taskId: "4", owner: "last-mover-analyst" }
TaskUpdate: { taskId: "5", owner: "girardian" }
```

## Phase 3: Monitor & Cross-Pollinate

While teammates work:
- Messages from teammates arrive automatically
- If a teammate asks a question, respond with guidance
- If two teammates discover conflicting information, message both to reconcile
- If a teammate finds something that dramatically changes the picture, alert others
- The Girardian and Secret Hunter should especially cross-reference — mimetic
  competition and secret authenticity are deeply connected

## Phase 4: Synthesize — The Thiel Verdict

After ALL teammates report back, the lead writes the final analysis.
This is where the monopoly potential is assessed holistically.

### The Synthesis Process

1. **Collect** all five analyses
2. **Cross-reference** — do the findings cohere or contradict?
3. **Apply the Monopoly Test** — does this venture have a path to genuine monopoly,
   or is it structurally competitive?
4. **Apply the Last Mover Test** — can this be the LAST company in its category?
5. **Apply the Mimetic Test** — is this opportunity genuine or a herd artifact?
6. **Detect the Lollapalooza Equivalent** — Thiel's version is when secret +
   10x technology + tiny domination-ready market + network effects + last mover
   position ALL align. This is rare — it's PayPal, it's Google, it's Facebook.
   When all five point the same direction, you have genuine monopoly potential.
7. **Render the verdict** — MONOPOLY, COMPETITIVE, or TOO EARLY

### The Verdict Categories

**MONOPOLY** — This venture has a genuine secret, a plausible 10x advantage,
a tiny market it can dominate, a concentric expansion path, and a credible claim
to being the last mover. Thiel would invest. Pursue with maximum conviction and
speed. "The value of a company today is the sum of all the money it will make
in the future."

**COMPETITIVE** — This venture is structurally competitive. It may be a good
business, but it lacks monopoly potential. Profits will be competed away. Thiel
would say: "All failed companies are the same: they failed to escape competition."
Either pivot to find the monopoly, or accept that this is a lifestyle business,
not a venture-scale outcome.

**TOO EARLY** — The secret may be real, but the timing is wrong. The market
doesn't exist yet, the technology isn't ready, or the starting market hasn't
emerged. Like PayPal's PalmPilot era — the insight was right but the execution
environment was wrong. Wait, or find a different starting wedge.

### Output Document

Write to `thoughts/thiel/YYYY-MM-DD-<idea-slug>.md`:

```markdown
---
date: <ISO 8601>
analyst: Claude Code (thiel monopoly skill)
idea: "<idea name>"
verdict: <MONOPOLY | COMPETITIVE | TOO_EARLY>
monopoly_score: <X/40 from the Anatomist>
secret_strength: <STRONG | MODERATE | WEAK | NONE>
last_mover_potential: <HIGH | MEDIUM | LOW | NONE>
mimetic_risk: <HIGH | MEDIUM | LOW>
confidence: <LOW | MEDIUM | HIGH>
---

# Thiel Monopoly Analysis: [Idea Name]

> "All happy companies are different: each one earns a monopoly by solving
> a unique problem. All failed companies are the same: they failed to escape
> competition."
> — Peter Thiel, *Zero to One*

## The Venture

[One paragraph description]

## The Implied Secret

**The claim:** Most people believe [X]. But this venture is built on the
truth that [!X].

**Secret strength:** [STRONG / MODERATE / WEAK / NONE]
**Secret type:** [Nature / People / Both]
**Genuinely contrarian?** [YES / NO — mimetically contrarian]

---

## The Monopoly Anatomy (Anatomist)

### The 10x Test
[Assessment of the proprietary technology advantage. Is it genuinely 10x?
On what specific dimension? Would customers switch immediately?]

**10x verdict:** [PASS / MARGINAL / FAIL]

### The Four Characteristics
| Characteristic | Rating (0-10) | Strengthens Over Time? |
|---------------|---------------|----------------------|
| Proprietary Technology | X | Y/N |
| Network Effects | X | Y/N |
| Economies of Scale | X | Y/N |
| Branding | X | Y/N |
| **COMBINED** | **X/40** | |

### Value Creation vs Capture
- Value created (X): [assessment]
- Value captured (Y%): [assessment]
- **Airline or Google?** [assessment]

---

## The Secret (Secret Hunter)

### Contrarian Question
**"What important truth do very few people agree with you on?"**
[The venture's answer, evaluated for genuine contrarianism]

### Zero-to-One Rating
[Is this 0-to-1, 0.5-to-1, or 1-to-n?]

### Definite Optimism Check
[Definite optimist / Indefinite optimist / Other]

### The Seven Questions Scorecard
| Question | Rating (0-10) | Notes |
|----------|--------------|-------|
| Engineering (10x tech?) | X | |
| Timing (right moment?) | X | |
| Monopoly (big share of small market?) | X | |
| People (right team?) | X | |
| Distribution (can you deliver?) | X | |
| Durability (defensible in 20 years?) | X | |
| Secret (unique opportunity?) | X | |
| **TOTAL** | **X/70** | |

Tesla scored ~60/70. Most cleantech failures scored below 30.

---

## The Market Frame (Market Framer)

### Framing Lie Detection
[What lies is the founder telling about market definition?
What lies are competitors telling?]

### The Real Market
[Actual market definition, actual competitors, actual dynamics]

### The Starting Market
**Size:** [number of customers in Ring 0]
**Dominability:** [can you own 80%+ in 12-18 months?]
**Rating:** [TOO SMALL / GOLDILOCKS / TOO LARGE]

### The Concentric Expansion Map
```
Ring 0: [X customers] — [target: Y% share in Z months]
  → Ring 1: [adjacent market] — [bridge: what carries over]
    → Ring 2: [broader market] — [compounds from Ring 0+1]
      → Ring 3: [endgame category] — [moat: what prevents entry]
```

### Restaurant Test
**Is this PayPal or a Castro Street restaurant?** [assessment]

---

## The Endgame (Last Mover Analyst)

### DCF Durability
[Where does the value come from? Near-term or far-future?
What percentage of DCF value is from Year 10+?]

### Last Mover Potential
**Can this be the LAST company in its category?**
[Assessment with historical analogues]

**Rating:** [HIGH / MEDIUM / LOW / NONE]

### Competitive Endgame
[What does the market look like in 10-20 years?
Monopoly / Oligopoly / Fragmented?]

### Power Law Check
**Can this return 100x?** [YES / NO]
**Realistic best case:** [assessment]

### Timing
**Rating:** [TOO EARLY / JUST RIGHT / TOO LATE]

---

## The Mimetic Lens (Girardian)

### Mimetic Competition
**Is this market crowded mimetically or genuinely?**
[Assessment with specific evidence]

**Rating:** [MIMETIC / GENUINE / UNCROWDED]

### Contrarian Authenticity
**Is the founder genuinely contrarian or performing it?**
[Assessment]

**Rating:** [GENUINELY CONTRARIAN / MIMETICALLY CONTRARIAN / CONSENSUS]

### The Vast Gate Test
**Is this venture going through the tiny crowded door or the vast gate
no one is taking?**
[Assessment]

### Scapegoat Risk
[If this succeeds wildly, will it become a target?]

---

## THE MONOPOLY POTENTIAL ASSESSMENT

This is the Thiel question: **does this venture have a genuine path to
monopoly — a secret, a 10x advantage, a tiny domination-ready market,
a concentric expansion path, and last-mover durability?**

### Forces Aligned Toward Monopoly

```
[Secret: genuine non-consensus truth] ✓/✗
  + [10x: order-of-magnitude better on key dimension] ✓/✗
    + [Starting market: small enough to dominate completely] ✓/✗
      + [Expansion: adjacent rings that compound] ✓/✗
        + [Last mover: drawbridge goes up permanently] ✓/✗
          + [Non-mimetic: going through the vast gate] ✓/✗
            = [MONOPOLY POTENTIAL: STRONG / MODERATE / WEAK / NONE]
```

When ALL SIX align, you have genuine monopoly potential. This is rare.
PayPal had all six. Google had all six. Facebook had all six. Most
ventures have 1-2 at best.

### Forces Working Against Monopoly

```
[Risk 1] + [Risk 2] + [Risk 3] = [competitive outcome]
```

### The X and Y Assessment
- **X (value created):** [$ estimate]
- **Y (value captured):** [% estimate]
- **X × Y = ** [venture value potential]

---

## THE VERDICT

### Thiel's Three Categories

**[ ] MONOPOLY** — Genuine zero-to-one with path to last-mover dominance.
  Secret is real, 10x is real, starting market is Goldilocks, expansion
  path is clear, last-mover position is achievable. Pursue with conviction.

**[ ] COMPETITIVE** — No monopoly path. Profits will be competed away.
  "All failed companies are the same: they failed to escape competition."
  Either find the monopoly pivot or accept commodity economics.

**[ ] TOO EARLY** — Secret may be real, but timing or starting market is
  wrong. The PalmPilot problem: right insight, wrong execution environment.
  Wait or find a different wedge.

### Verdict: [MONOPOLY / COMPETITIVE / TOO EARLY]

**Confidence:** [LOW / MEDIUM / HIGH]

**Monopoly Score:** [X/40] — [interpretation]
**Seven Questions:** [X/70] — [interpretation]
**Last Mover Potential:** [HIGH / MEDIUM / LOW / NONE]
**Mimetic Risk:** [HIGH / MEDIUM / LOW]

**Reasoning:** [2-3 paragraphs written in Thiel's direct, intellectual,
contrarian style. Binary framing — this is either a monopoly or it isn't.
Reference specific findings from each analyst. Be honest about what works
and what doesn't. If it's COMPETITIVE, identify what would need to change
to make it a MONOPOLY. If it's MONOPOLY, identify the biggest risk to
that status.]

### What Thiel Would Say

[Write 2-3 sentences in Thiel's voice — intellectual, contrarian, slightly
provocative, binary. Use his actual phrases: "competition is for losers",
"what important truth do very few people agree with you on", references
to specific companies. The tone should be that of a Stanford lecture —
professorial but with strong opinions delivered as obvious truths.

Example of Thiel's voice: "You've described a business that creates enormous
value for its customers but captures almost none of it. That's an airline,
not a technology company. The question isn't whether people want this —
it's whether you can be the only one providing it. Right now, you can't."

Another example: "This is a genuine secret. Most people haven't seen this
yet. The question is whether you can move fast enough from your beachhead
to raise the drawbridge before others discover the same truth. If you can
dominate [specific market] in 6 months, you have a monopoly. If it takes
3 years, someone else will see it too, and you'll be competing."]

### The Monopoly Playbook (If Verdict = MONOPOLY or TOO EARLY)

Based on the analysis, here are the specific moves:

1. **The Starting Market:** [Exactly which customers to target first,
   how many, and what 80%+ dominance looks like]
2. **The 10x Wedge:** [The specific dimension where you're 10x better
   and how to make it obvious to customers]
3. **The First Expansion:** [The first adjacent market and what bridges
   you from Ring 0 to Ring 1]
4. **The Drawbridge:** [What creates permanent capture — which of the
   four characteristics compounds fastest]
5. **The Last Mover Move:** [What's the "last great development" that
   makes you unassailable]

### The Death Modes (If Verdict = COMPETITIVE)

What structurally prevents monopoly:

1. **[Fatal competitive dynamic]** — [why it can't be overcome]
2. **[Missing monopoly characteristic]** — [why it can't be built]
3. **[Mimetic problem]** — [why the market structure destroys returns]

### To Move From COMPETITIVE to MONOPOLY, You Would Need:

[Specific changes that would transform this from competitive to monopoly.
This is the actionable output — what pivot, what new secret, what different
starting market would create monopoly potential.]
```

## Phase 5: Present & Follow-up

Present the verdict to the user with key highlights:

```
## Thiel Verdict: [IDEA] — [MONOPOLY / COMPETITIVE / TOO EARLY]

**Monopoly Score:** [X/40] — [Anatomist's combined rating]
**Secret:** [strong/moderate/weak/none] — [one-line assessment]
**10x:** [pass/marginal/fail] — [on what dimension]
**Starting Market:** [too small/goldilocks/too large] — [who specifically]
**Last Mover:** [high/medium/low/none] — [can you be the last company?]
**Mimetic Risk:** [high/medium/low] — [is competition genuine or herd?]

**What Thiel would say:** "[pithy quote in his voice]"

Full analysis: `thoughts/thiel/YYYY-MM-DD-<slug>.md`

Want me to:
1. Deep-dive into any analyst's findings?
2. Run a modified version with a different starting market or pivot?
3. Compare with /munger for a multi-framework view?
4. Apply /office-hours to refine before re-analyzing?
```

## Batch Mode

If the user wants to compare multiple ventures:

1. Run the full analysis on each (can parallelize — one team per idea)
2. Produce a leaderboard:

```
## Thiel Monopoly Leaderboard

| Rank | Venture | Verdict | Score | Secret | 10x | Market | Last Mover | Mimetic |
|------|---------|---------|-------|--------|-----|--------|------------|---------|
| 1 | [name] | MONOPOLY | 28/40 | STRONG | PASS | GOLDILOCKS | HIGH | LOW |
| 2 | [name] | TOO EARLY | 22/40 | MODERATE | PASS | TOO SMALL | MEDIUM | LOW |
| 3 | [name] | COMPETITIVE | 12/40 | WEAK | FAIL | TOO LARGE | NONE | HIGH |
```

## Scoring Discipline

- **Be Thiel, not a cheerleader.** Thiel's framework is binary: monopoly or
  competitive. Most businesses are competitive. If every idea gets MONOPOLY,
  the skill is broken. Thiel would pass on 99% of pitches.
- **The secret test is pass/fail.** If there's no genuine contrarian truth,
  the analysis ends. Everything else requires a secret as foundation.
- **10x is literal.** Not "significantly better." Not "meaningfully improved."
  Ten times better on a specific, measurable dimension. If you can't name the
  dimension and roughly quantify the improvement, it's not 10x.
- **Small means SMALL.** PayPal's starting market was 20,000 people. Facebook's
  was ~6,000 Harvard undergrads. If the "small" starting market has millions
  of people, it's not small enough.
- **Web search when uncertain.** The Market Framer should ground all market
  claims in evidence. Founders lie about markets — verify.
- **TOO EARLY is not a consolation prize.** It's a genuine assessment that the
  timing or starting market is wrong but the secret might be right. It's
  actionable — the venture needs a different wedge, not a different idea.

## When NOT to Use This Framework

Thiel's framework has known limitations. Flag these if relevant:

- **Physical goods without tech leverage** — Marginal costs don't approach zero.
  Network effects are weak or absent. Use Porter's Five Forces instead.
- **Service businesses** — Starbucks makes enormous profits in a competitive market
  through brand and experience, not monopoly. Thiel's framework would incorrectly
  predict this can't work.
- **Creative industries** — "10x better" is not a coherent concept for music, film,
  or art. Network effects exist at the platform layer, not content layer.
- **Outside US tech ecosystem** — The framework depends on risk capital, talent
  density, and a permissive regulatory environment.
- **Incremental improvements to existing categories** — Thiel explicitly rejects
  1-to-n thinking. If the venture is making something 2x better, this framework
  will always say COMPETITIVE, even if the business could be quite profitable.
- **The contrarian trap** — Being contrarian is not the same as being right.
  Thiel's own hedge fund (Clarium Capital) lost 90% of its value on concentrated
  contrarian bets. The framework creates conviction but not necessarily accuracy.

## Important Notes

- **Cost**: This skill spawns 5 agents. It's expensive. Worth it for serious
  monopoly evaluation, not for casual brainstorming (use /office-hours for that).
- **Sonnet for teammates, Opus for synthesis**: The lead handles the monopoly
  potential assessment and final verdict — that's where deep reasoning matters.
- **No team? No problem**: If teams aren't enabled, run 5 sequential background
  agents and collect results. Same analysis, just no cross-talk.
- **Pair with other skills**: 
  - Run /office-hours first to refine the idea
  - Run /munger for Munger's lattice (complementary — Munger evaluates moat
    durability; Thiel evaluates monopoly creation potential)
  - Run /thiel to evaluate monopoly potential
  - A Munger "IN" + Thiel "MONOPOLY" = very strong signal
  - A Munger "IN" + Thiel "COMPETITIVE" = good business, not a venture outcome
  - A Thiel "MONOPOLY" + Munger "OUT" = potential monopoly but poor fundamentals
- **Thiel vs Munger: key difference**: Munger evaluates EXISTING businesses for
  durable moats. Thiel evaluates NEW ventures for monopoly creation potential.
  They answer different questions. Use both for the full picture.
