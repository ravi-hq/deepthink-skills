---
name: shannon
description: |
  Claude Shannon's Six Techniques for Creative Problem Transformation. Spawns a team of
  specialist agents — Simplifier, Analogist, Reframer, Decomposer, Inverter — who each
  apply one of Shannon's problem-solving techniques to your stuck problem. The lead
  synthesizes into a transformation assessment: which reframings opened paths, which
  analogies map, and the honest Shannon verdict on whether the problem has been cracked
  open. Use when stuck on any problem — engineering, strategy, design, math, business.
  Works standalone or after other analysis skills surface a hard sub-problem.
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

# /shannon — Six Techniques for Creative Problem Transformation

Apply Claude Shannon's complete problem-solving framework to a problem you're stuck on.
Shannon delivered this framework in his March 20, 1952 Bell Labs lecture "Creative Thinking"
— the only known explanation of HOW he thought. These six techniques produced information
theory, digital computing, and the mathematical foundations of communication. The output
should read like what you'd get if Shannon himself sat down with your problem, juggled
for a while, built a little machine, and then showed you the solution.

Source: "Creative Thinking" by Claude Shannon (1952 Bell Labs lecture).
Biography: *A Mind at Play* by Jimmy Soni & Rob Goodman (2017).
Investing bridge: *Fortune's Formula* by William Poundstone.

## Core Principles

These are non-negotiable and come from Shannon's actual methodology:

1. **Strip to the essence** — Shannon said: "Attempt to eliminate everything from the
   problem except the essentials; that is, cut it down to size." The radical act of
   REMOVING is where insight lives. Shannon stripped "meaning" from communication and
   invented information theory. What can you strip from your problem?

2. **Two small jumps beat one big jump** — Shannon: "It seems to be much easier to make
   two small jumps than the one big jump in any kind of mental thinking." If you can't
   see the solution directly, find an intermediate stepping stone. Or find a solved
   problem nearby and bridge from there.

3. **Break mental ruts by changing words** — Shannon warned that you can "get into ruts
   of mental thinking" and someone "quite green to a problem will sometimes come in and
   find the solution like that." Restate the problem in every form you can imagine.
   Change the vocabulary. Change the viewpoint. The right framing makes the solution obvious.

4. **Invert, always invert** — Shannon: "Turn the problem over. Supposing that S were
   the given proposition and what you are trying to obtain is P." His Nim-playing machine
   became "far simpler than the first design" once he reversed the direction. Work backward
   from the answer.

5. **Generalize after solving** — "If the minute you've found an answer to something,
   the next thing to do is to ask yourself if you can generalize this anymore." Specific
   solutions contain broader principles. Extract them.

6. **Play is not frivolous** — Shannon built flame-throwing trumpets, juggling machines,
   and a maze-solving mouse named Theseus. These weren't distractions; they were him
   applying the six techniques to playful domains. The Bell Labs culture of "play"
   enabled Shannon's method. Approach your problem with curiosity and constructive
   dissatisfaction, not grim determination.

## The Bandwagon Warning

Shannon himself wrote in his 1956 editorial "The Bandwagon": "The use of a few exciting
words like information, entropy, redundancy, do not solve all our problems." This skill
applies Shannon's PROCESS, not his specific theorems. The techniques are domain-general,
but they work best on problems with clear structure. For "wicked" problems where the
definition itself is contested, acknowledge the limits.

## Invocation

When invoked with `$ARGUMENTS`:

1. If arguments contain a problem description, proceed directly
2. If no arguments or vague, ask ONE clarifying question via AskUserQuestion:
   "Describe the problem you're stuck on in one paragraph: what you're trying to
   achieve, what you've already tried, and where specifically you're blocked."
3. Do NOT ask more than one round of questions. Transform with what you have.

## Phase 1: Understand the Problem (Lead Only)

Before spawning the team, the lead must establish:

- **The problem**: What the user is trying to solve, in one sentence
- **The block**: Where specifically they're stuck — what makes it hard
- **What's been tried**: Approaches that have already failed or stalled
- **The domain**: Engineering, strategy, math, design, business, etc.
- **Success criteria**: What would a solution look like?

Present this back to the user:

```
## Shannon Transformation: [Problem Name]

I understand the problem as: [one sentence]

You're stuck because: [the specific block]

I'm spawning five specialist analysts, each applying one of Shannon's
six techniques. (Simplification + Generalization share an agent because
they're inverse operations — strip down and build up.)

**The Team:**
1. The Simplifier — strips away everything non-essential, then generalizes
2. The Analogist — searches for solved problems with similar structure
3. The Reframer — restates the problem in every form imaginable
4. The Decomposer — breaks the big jump into small, tractable sub-problems
5. The Inverter — works backward from the desired solution

Starting transformation...
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
TeamCreate: team_name = "shannon-<problem-slug>"
```

Create five tasks and spawn five teammates. Each teammate gets a detailed prompt
with the FULL context of the problem and their specific technique.

### Teammate 1: The Simplifier (Simplification + Generalization)

```
TaskCreate: {
  subject: "Shannon Simplification: strip to essence and generalize",
  description: "Apply Shannon's simplification and generalization to [PROBLEM]",
  activeForm: "Stripping the problem down"
}
```

Spawn prompt:

```
You are The Simplifier on Shannon's problem transformation team. You apply
TWO of Shannon's six techniques: SIMPLIFICATION and GENERALIZATION. These are
inverse operations — simplification strips down, generalization builds up.

THE PROBLEM: [full description]
THE BLOCK: [where they're stuck]
WHAT'S BEEN TRIED: [prior approaches]

Shannon said: "Attempt to eliminate everything from the problem except the
essentials; that is, cut it down to size. Almost every problem that you come
across is befuddled with all kinds of extraneous data of one sort or another;
and if you can bring this problem down into the main issues, you can see more
clearly what you're trying to do and perhaps find a solution."

Do this analysis:

1. THE RADICAL STRIP
   Shannon's most famous move: he stripped MEANING from communication and
   invented information theory. What is the equivalent radical simplification
   for this problem?

   - List every element of the problem as stated
   - For each element, ask: "If I remove this, does the CORE problem change?"
   - Strip away elements one by one until you reach the irreducible core
   - State the simplified problem in one sentence
   - Is this simplified version actually solvable? If yes, describe the solution.
   - Can you add refinements back to get to the original problem's solution?

2. THE TOY VERSION
   Shannon built toy versions of everything — Theseus the maze-solving mouse,
   juggling machines, Nim-playing machines. What's the "toy version" of this problem?

   - Describe the smallest possible instance of this problem
   - Can you solve the toy version?
   - Does solving the toy version reveal the structure of the full problem?
   - What does the toy version teach you about what's ACTUALLY hard?

3. WHAT'S THE "MEANING" HERE?
   Shannon stripped meaning from communication because meaning was the
   distraction, not the essence. What's the equivalent distraction in this problem?

   - What aspect of this problem seems important but is actually irrelevant
     to the core solution?
   - What assumption is everyone making that, if removed, would simplify everything?
   - What's the "semantic content" that's obscuring the "information content"?

4. GENERALIZATION (after simplification)
   Shannon: "If the minute you've found an answer to something, the next
   thing to do is to ask yourself if you can generalize this anymore."

   - If you solved the simplified version, can you generalize that solution?
   - Does the simplified solution reveal a broader PRINCIPLE?
   - Can the same principle solve a larger class of problems?
   - What's the N-dimensional version of this solution?

5. THE EXISTENCE PROOF
   Shannon proved that good error-correcting codes EXIST without finding
   any specific one. His coding theorem used the probabilistic method —
   showing that random codes are almost certainly good enough.

   - Can you prove a solution EXISTS without constructing it?
   - What would it mean to know a solution exists even if you can't build it yet?
   - Does knowing it exists change how you search for it?

Output format: structured findings with the simplified problem statement,
the toy version, the generalized principle, and whether the simplification
unlocked a path to the solution.

When done, message your teammates about what the simplified form reveals.
The Analogist especially needs to know the stripped-down structure — it's
easier to find analogies to simple forms.
```

### Teammate 2: The Analogist

Spawn prompt:

```
You are The Analogist on Shannon's problem transformation team. You apply
Shannon's technique of SEEKING SIMILAR KNOWN PROBLEMS.

THE PROBLEM: [full description]
THE BLOCK: [where they're stuck]
WHAT'S BEEN TRIED: [prior approaches]

Shannon said: "You have a problem P here and there is a solution S which you
do not know yet. If you have experience in the field, you may know of a
somewhat similar problem, call it P', which has already been solved and which
has a solution, S'. All you need to do is find the analogy from P' to P and
the same analogy from S' to S in order to get back to the solution of the
given problem."

He added: "It seems to be much easier to make two small jumps than the one
big jump in any kind of mental thinking."

Do this analysis:

1. STRUCTURAL MAPPING
   Before finding analogies, identify the STRUCTURE of the problem:
   - What are the inputs? What are the outputs?
   - What are the constraints?
   - What transformation is being attempted?
   - What makes the transformation hard?

   Write this structure abstractly, stripped of domain-specific language.

2. SAME-DOMAIN ANALOGIES
   Within the problem's own field:
   - What similar problems have already been solved?
   - For each: what was the problem (P')? What was the solution (S')?
   - How does P' map to P? How does S' suggest an S?
   - What's different between P' and P that might break the analogy?

   Use WebSearch if needed to find solved problems in the same domain.

3. CROSS-DOMAIN ANALOGIES
   Shannon's master's thesis mapped Boolean algebra (math) onto switching
   circuits (engineering) — the analogy that created digital computing. He
   saw the connection because both had two-valued states.

   Look for structural matches in other domains:
   - Does this problem's structure appear in physics? Biology? Economics?
     Computer science? Game theory? Information theory?
   - For each match: what was the problem there? How was it solved?
   - Can you map that solution back to this problem?

   Think about Shannon's own cross-domain hits:
   - Boolean algebra → circuits (two-valued states)
   - Thermodynamic entropy → information entropy (uncertainty measures)
   - Communication channels → gambling/investing (Kelly Criterion)
   - Maze-solving → machine learning (Theseus)

4. THE BRIDGING STRATEGY
   Shannon noted that "two small jumps" beat "one big jump." For each
   promising analogy:
   - What's the first small jump? (from P to the analogous domain)
   - What's the second small jump? (from the analogous solution back to S)
   - Where does the analogy break? What refinement is needed?

5. FALSE ANALOGY CHECK
   Shannon himself warned about the danger of false analogies. For each
   proposed analogy:
   - What structural features are shared? (valid mapping)
   - What structural features differ? (where the analogy breaks)
   - Is the analogy superficial (same words, different structure) or
     deep (different words, same structure)?
   - Could following this analogy actively mislead?

Output format: For each analogy found, provide: the source problem P',
its solution S', the mapping to the current problem, and an honest assessment
of whether the analogy is deep or superficial.

Message teammates — especially the Reframer — about which framings of the
problem yield the best analogies. Also message the Decomposer if an analogy
solves part but not all of the problem.
```

### Teammate 3: The Reframer

Spawn prompt:

```
You are The Reframer on Shannon's problem transformation team. You apply
Shannon's technique of RESTATEMENT — the most creatively demanding of
the six techniques.

THE PROBLEM: [full description]
THE BLOCK: [where they're stuck]
WHAT'S BEEN TRIED: [prior approaches]

Shannon said: "Another approach for a given problem is to try to restate it
in just as many different forms as you can. Change the words. Change the
viewpoint. Look at it from every possible angle. After you've done that, you
can try to look at it from several angles at the same time and perhaps you
can get an insight into the real basic issues of the problem."

He warned: "It is very easy to get into ruts of mental thinking. You start
with a problem here and you go around a circle... and if you could only get
over to this point, perhaps you would see your way clear; but you can't break
loose from certain mental blocks."

He noted: "That is the reason why very frequently someone who is quite green
to a problem will sometimes come in and look at it and find the solution like
that, while you have been laboring for months over it."

Do this analysis:

1. THE VOCABULARY SWAP
   Restate the problem using completely different words:
   - Replace every technical term with a plain-language equivalent
   - Replace every plain-language term with a technical equivalent from
     a DIFFERENT field
   - State the problem as a story / narrative
   - State the problem as a mathematical equation
   - State the problem as a physical system
   - State the problem as a game with players, moves, and payoffs

   For each restatement, note: does a new insight appear?

2. THE VIEWPOINT ROTATION
   Look at the problem from every stakeholder's perspective:
   - The person trying to solve it (current viewpoint)
   - The "customer" / end user of the solution
   - The adversary / competitor / obstacle
   - A complete newcomer seeing it for the first time
   - An alien intelligence with no domain assumptions
   - Shannon himself (a playful tinkerer who builds toy models)

   For each viewpoint: what does the problem look like? What's obvious
   from this angle that's invisible from the original angle?

3. THE CONSTRAINT FLIP
   For each constraint in the problem:
   - State the constraint explicitly
   - Ask: "What if this constraint didn't exist?"
   - Ask: "What if this constraint were the OPPOSITE?"
   - Ask: "What if this constraint were 10x stronger?"
   - Ask: "What if this constraint IS the solution?"

   Sometimes the thing blocking you is actually the key to the answer.
   Shannon used the noise in communication channels — the very thing
   engineers wanted to eliminate — as the basis for his channel capacity
   theorem. The constraint DEFINED the solution.

4. THE FRESH EYES TEST
   Shannon noted that newcomers often solve problems that veterans can't.
   Simulate the newcomer:
   - What would someone with NO context ask about this problem?
   - What "obvious" questions would they raise that experts dismiss?
   - What would a child say about this?
   - What's the dumbest possible restatement? (Sometimes it's the best.)

5. THE VON NEUMANN RENAME
   Von Neumann told Shannon to call his uncertainty function "entropy"
   because "nobody knows what entropy really is, so in a debate you will
   always have the advantage." Sometimes renaming a concept completely
   changes how you think about it.

   - Give the problem a completely new name — something evocative
   - Give the key variables new names from a different domain
   - Does the renamed version suggest different approaches?

6. SYNTHESIS: THE BEST REFRAMING
   After all restatements:
   - Which reframing changed your understanding the most?
   - Which reframing made the problem feel solvable?
   - Which reframing revealed a hidden assumption?
   - State the BEST reframing as the new problem statement.

Output format: All restatements, with the best ones flagged and explained.
The goal is to find the ONE reframing that breaks the mental rut.

Message teammates about which reframings opened new angles — especially
the Analogist (new framings suggest new analogies) and the Inverter
(new framings suggest new directions to invert).
```

### Teammate 4: The Decomposer

Spawn prompt:

```
You are The Decomposer on Shannon's problem transformation team. You apply
Shannon's technique of STRUCTURAL DECOMPOSITION — breaking big jumps into
small jumps with intermediate solutions.

THE PROBLEM: [full description]
THE BLOCK: [where they're stuck]
WHAT'S BEEN TRIED: [prior approaches]

Shannon said: "Suppose you have your problem here and a solution here. You
may have too big a jump to take. What you can try to do is to break down
that jump into a large number of small jumps. If this were a set of
mathematical axioms and this were a theorem you were trying to prove, it
might be too much to try to prove this thing in one fell swoop. But perhaps
I can visualize a number of subsidiary theorems or propositions such that
if I could prove those, in turn I would eventually arrive at this solution."

Shannon also noted: "Many proofs in mathematics have been actually found by
extremely roundabout processes. A man starts to prove this theorem and he
finds that he wanders all over the map... and very often when that's done,
when you've found your solution, it may be very easy to simplify."

And: "If you can design a way of doing something which is obviously clumsy
and cumbersome, uses too much equipment; but after you've really got something
you can get a grip on, you can start cutting out components and seeing some
parts were really superfluous."

Do this analysis:

1. THE GAP ANALYSIS
   Map the distance between where we are and where we need to be:
   - Current state (P): what do we have / know / can do?
   - Desired state (S): what do we need?
   - The gap: what's missing? What makes the jump too big?
   - Is the gap a single chasm or multiple smaller gaps?

2. INTERMEDIATE STATIONS
   Shannon's key insight: find stepping stones between P and S.
   - What intermediate results, if achieved, would make the final
     jump trivial?
   - Work forward: what's the FIRST sub-problem you could solve?
   - Work backward: what's the LAST step before the solution?
   - Can you find a chain of intermediate results connecting P to S?

   For each intermediate station:
   - State it clearly
   - Assess: is THIS sub-problem tractable?
   - If not, can IT be further decomposed?

3. THE SOURCE/CHANNEL SEPARATION
   Shannon's greatest decomposition: he separated the communication problem
   into source coding (compression) and channel coding (error correction).
   Each could be solved independently. This separation theorem is one of
   the most powerful in engineering history.

   - Can this problem be separated into independent sub-problems?
   - Which parts can be solved in isolation?
   - Which parts MUST be solved together?
   - Are there dependencies between sub-problems, or are they truly
     independent?

4. THE ROUNDABOUT PATH
   Shannon acknowledged that solutions are often found "by extremely
   roundabout processes." The first path through doesn't have to be elegant.

   - What's the brute-force, ugly, "too much equipment" solution?
   - Can you describe ANY solution, no matter how inefficient?
   - Once you have something that works (however poorly), what can be
     simplified away?
   - What components turn out to be "really superfluous"?

5. THE DEPENDENCY GRAPH
   Draw the problem's structure:
   - What depends on what?
   - What can be parallelized?
   - What's the critical path?
   - Where are the bottlenecks?
   - Is there a single hardest sub-problem that, if solved, unblocks everything?

6. THE MINIMUM VIABLE PROOF
   Shannon proved theorems before building systems. Can you:
   - Prove the solution IS possible (existence proof) before constructing it?
   - Build the simplest possible prototype that demonstrates feasibility?
   - Identify the ONE hardest sub-problem and focus all effort there?

Output format: The decomposition tree — problem → sub-problems → sub-sub-problems,
with each node assessed as TRACTABLE, HARD, or BLOCKED. Highlight the
critical path and the single hardest sub-problem.

Message teammates — especially the Inverter — about which sub-problems
might be easier to solve backward. Also alert the Analogist about
tractable sub-problems that might have known solutions.
```

### Teammate 5: The Inverter

Spawn prompt:

```
You are The Inverter on Shannon's problem transformation team. You apply
Shannon's technique of INVERSION — working backward from the desired
solution to the given starting point.

THE PROBLEM: [full description]
THE BLOCK: [where they're stuck]
WHAT'S BEEN TRIED: [prior approaches]

Shannon said: "You are trying to obtain the solution S on the basis of the
premises P and then you can't do it. Well, turn the problem over supposing
that S were the given proposition, the given axioms, or the given numbers in
the problem and what you are trying to obtain is P. Just imagine that that
were the case. Then you will find that it is relatively easy to solve the
problem in that direction."

He demonstrated this with his Nim-playing machine: "It turned out that it
seemed to be quite difficult. It took quite a number of relays to do this
particular calculation. But then I got the idea that if I inverted the
problem, it would have been very easy to do — if the given and required
results had been interchanged; and that idea led to a way of doing it which
was far simpler than the first design. The way of doing it was doing it by
feedback; that is, you start with the required result and run it back until
it matches the given input."

Do this analysis:

1. THE FULL INVERSION
   Swap P and S completely:
   - Original: Given [P], find [S]
   - Inverted: Given [S], find [P]
   - Is the inverted problem easier? If so, WHY?
   - Can the inverted solution be reversed into a forward solution?
   - What does the ease of the inverted problem tell you about the
     structure of the original?

2. THE FEEDBACK APPROACH
   Shannon's Nim machine used feedback — start with the desired output
   and iterate until you match the input. This is the essence of:
   - Gradient descent (start at answer, adjust until you match data)
   - Binary search (start in the middle, narrow until you find it)
   - Feedback control (compare output to desired, correct the error)

   Can this problem be solved by:
   - Starting with a GUESS at the solution and iteratively refining?
   - Starting with the DESIRED output and working backward step by step?
   - Setting up a FEEDBACK LOOP that converges on the answer?

3. THE DEATH INVERSION (from Munger, who shares Shannon's love of inversion)
   Munger: "All I want to know is where I'm going to die, so I'll never
   go there." Shannon's version: instead of asking "how do I succeed?",
   ask "how do I definitely fail?"

   - What would GUARANTEE failure for this problem?
   - What would make the problem IMPOSSIBLE to solve?
   - What's the OPPOSITE of the desired solution?
   - Now: are any of these failure modes present in the current approach?
   - Can you construct the solution by systematically avoiding all failure modes?

4. THE DUAL PROBLEM
   In mathematics, many problems have a "dual" — an equivalent problem
   in a transformed space that's often easier to solve. Shannon's channel
   capacity theorem can be viewed as the dual of the source coding theorem.

   - What's the dual of this problem?
   - Is there an equivalent problem in a different representation?
   - Can you solve in the dual space and transform back?

5. THE OUTPUT-FIRST DESIGN
   Shannon designed his Nim machine output-first: "You start with the
   required result and run it through its value until it matches the
   given input."

   - If you already HAD the solution, what would it look like?
   - Describe the solution in detail, even if you don't know how to get there
   - Now: given that detailed picture, can you work backward to a construction?
   - What's the first step backward from the solution?

6. INVERSION IN SMALL BATCHES
   Shannon noted: "It's often possible to invert it in small batches.
   In other words, you've got a path marked out... you can see how to
   invert these things in small stages."

   - Can you invert the problem partially — just the hardest step?
   - Can you alternate between forward and backward reasoning?
   - For each sub-problem from the Decomposer: is forward or backward easier?

Output format: The inverted problem statement, the feedback approach if
applicable, the death inversion (what guarantees failure), and the
output-first design. Highlight which inversion technique opened the
most productive path.

Message teammates about inversions that suggest new decompositions
(tell the Decomposer) or new analogies (tell the Analogist). The
Simplifier should know if inverting revealed that some elements
of the problem are irrelevant.
```

### Spawning

Spawn all five as background agents. Use `model: "sonnet"` for all teammates.
The lead (Opus) handles synthesis.

```
Agent: {
  team_name: "shannon-<problem-slug>",
  name: "simplifier",
  model: "sonnet",
  prompt: [full simplifier prompt with problem substituted],
  run_in_background: true
}
```

Repeat for analogist, reframer, decomposer, inverter.

## Phase 3: Monitor & Cross-Pollinate

While teammates work:
- Messages from teammates arrive automatically
- If a teammate asks a question, respond with guidance
- If two teammates discover conflicting framings, message both to reconcile
- If a teammate finds a breakthrough reframing, alert all others immediately
- The techniques are meant to INTERACT: a simplification suggests analogies,
  a reframing suggests inversions, a decomposition reveals tractable sub-problems

## Phase 4: Synthesize — The Shannon Transformation

After ALL teammates report back, the lead writes the final analysis.
This is where the six techniques converge into a transformed view of the problem.

### The Synthesis Process

1. **Collect** all five analyses
2. **Cross-reference** — where do multiple techniques point the same direction?
3. **Identify the breakthrough reframing** — which technique opened the most productive path?
4. **Map the solution paths** — which routes to the solution are now visible?
5. **Assess completeness** — is the problem fully transformed, or are hard cores remaining?
6. **Render the verdict** — TRANSFORMED, PARTIALLY TRANSFORMED, or STUCK

### Output Document

Write to `thoughts/shannon/YYYY-MM-DD-<problem-slug>.md`:

```markdown
---
date: <ISO 8601>
analyst: Claude Code (shannon transformation skill)
problem: "<problem name>"
verdict: <TRANSFORMED | PARTIALLY_TRANSFORMED | STUCK>
technique_that_cracked_it: <simplification | analogy | restatement | decomposition | inversion | combination>
confidence: <LOW | MEDIUM | HIGH>
---

# Shannon Transformation: [Problem Name]

> "It seems to be much easier to make two small jumps than the one big jump
> in any kind of mental thinking."
> — Claude Shannon, "Creative Thinking" (1952)

## The Problem As Given

[One paragraph: what the user is stuck on]

## The Block

[One paragraph: what specifically makes this hard]

---

## Technique 1: Simplification (The Simplifier)

### The Radical Strip
[What was removed. What's the irreducible core.]

### The Toy Version
[The smallest instance of the problem. What it reveals.]

### The "Meaning" That Was Obscuring the "Information"
[The distraction that, once removed, clarified everything]

### Generalized Principle
[If a solution was found, the broader principle it embodies]

### Simplification Verdict
**Core problem in one sentence:** [the stripped-down version]
**Solvable in simplified form?** [YES / PARTIALLY / NO]

---

## Technique 2: Analogy (The Analogist)

### Structural Map of the Problem
[Abstract structure: inputs, outputs, constraints, transformation]

### Best Same-Domain Analogy
**Source problem (P'):** [description]
**Source solution (S'):** [description]
**Mapping to our problem:** [how P'→P and S'→S]
**Analogy depth:** [DEEP — same structure / SURFACE — same words only]

### Best Cross-Domain Analogy
**Domain:** [the foreign field]
**Source problem:** [description]
**Source solution:** [description]
**Mapping:** [how it maps back]
**Analogy depth:** [DEEP / SURFACE]

### Shannon's Own Analogies That Apply
[Does this problem resemble any of Shannon's own cross-domain moves?]

### Analogy Verdict
**Best analogy found:** [the most productive mapping]
**Does it suggest a solution?** [YES / PARTIALLY / NO]

---

## Technique 3: Restatement (The Reframer)

### The Restatements
| # | Framing | New Insight? |
|---|---------|-------------|
| 1 | [plain language version] | [what it reveals] |
| 2 | [technical version from different field] | [what it reveals] |
| 3 | [story/narrative version] | [what it reveals] |
| 4 | [mathematical version] | [what it reveals] |
| 5 | [game-theoretic version] | [what it reveals] |
| 6 | [inverted constraint version] | [what it reveals] |

### The Breakthrough Reframing
[The ONE restatement that changed understanding the most]

### Hidden Assumption Exposed
[What assumption was invisible until the reframing revealed it?]

### Restatement Verdict
**Best reframing:** [the version that makes the problem feel solvable]
**Mental rut broken?** [YES / NO — describe the rut]

---

## Technique 4: Structural Decomposition (The Decomposer)

### Decomposition Tree
```
[PROBLEM]
├── [Sub-problem 1] — TRACTABLE
│   ├── [Sub-sub 1a] — SOLVED
│   └── [Sub-sub 1b] — TRACTABLE
├── [Sub-problem 2] — HARD
│   └── [Sub-sub 2a] — THE BOTTLENECK
└── [Sub-problem 3] — TRACTABLE
```

### The Critical Path
[Which sub-problems must be solved in order?]

### The Single Hardest Sub-Problem
[The one thing that, if solved, unblocks everything]

### The Roundabout Path
[The ugly brute-force solution that at least WORKS]

### Source/Channel Separation
[Can independent parts be solved independently?]

### Decomposition Verdict
**Tractable sub-problems:** [N of M]
**Bottleneck:** [the hardest remaining piece]
**Roundabout solution exists?** [YES / NO]

---

## Technique 5: Inversion (The Inverter)

### The Inverted Problem
**Original:** Given [P], find [S]
**Inverted:** Given [S], find [P]
**Easier inverted?** [YES / NO — why]

### The Feedback Approach
[Can we start with a guess at S and iterate?]

### The Death Inversion
**What guarantees failure:**
1. [failure mode 1]
2. [failure mode 2]
3. [failure mode 3]

**Are any present in the current approach?** [assessment]

### Output-First Design
[Description of what the solution looks like, working backward]

### Inversion Verdict
**Best inversion technique:** [full inversion / feedback / death inversion / output-first]
**Path revealed?** [YES / PARTIALLY / NO]

---

## THE TRANSFORMATION ASSESSMENT

This is the Shannon question: **has the problem been transformed into one
we can solve?**

### Techniques That Opened Paths

```
[Technique 1: what it revealed]
  + [Technique 2: what it added]
    + [Technique 3: how they combine]
      = [THE TRANSFORMED PROBLEM / SOLUTION PATH]
```

### The Transformed Problem Statement

[If the techniques worked: the problem restated in a form where the
solution is visible or nearly so. This is the key output — the original
problem, but seen from the angle that makes it tractable.]

### Remaining Hard Cores

[Sub-problems that resist all six techniques — genuinely hard pieces
that may require new insight, more domain knowledge, or accepting that
this is in Shannon's "too tough" category.]

---

## THE VERDICT

### Shannon's Assessment

**[ ] TRANSFORMED** — The six techniques cracked the problem open. A clear
  path to the solution is now visible. The problem has been reduced to
  tractable sub-problems with known or findable solutions.

**[ ] PARTIALLY TRANSFORMED** — Some techniques opened productive paths,
  but hard cores remain. The problem is better understood but not fully
  solved. Specific next steps are clear.

**[ ] STUCK** — The problem resists all six techniques. This may be
  genuinely hard (Shannon's "too tough" basket), may require domain
  knowledge the team doesn't have, or may need a fundamentally new
  insight that systematic technique can't provide.

### Verdict: [TRANSFORMED / PARTIALLY TRANSFORMED / STUCK]

**Confidence:** [LOW / MEDIUM / HIGH]

**Technique that contributed most:** [which of the six was most productive]

**Reasoning:** [2-3 paragraphs explaining what the transformation revealed,
which techniques combined productively, and what remains. Written in
Shannon's style — concrete, playful, with physical analogies.]

### What Shannon Would Say

[Write 2-3 sentences in Shannon's voice — playful, concrete, probably
involving a physical analogy or a toy machine. Shannon might say something
like "You know, this reminds me of the Nim machine. Everyone was trying to
compute the winning move directly, which needed dozens of relays. But if
you just start with the answer and run it backward through feedback, the
whole thing collapses to something simple. Your problem is the same — you're
computing forward when you should be designing backward."]

### Next Steps: The Path Forward

[Based on the transformation, write 3-5 concrete next steps. These should
be specific, actionable, and ordered by priority.]

1. **[Most important action]** — because [which technique revealed this]
2. **[Second action]** — because [reasoning]
3. **[Third action]** — because [reasoning]

### Shannon's Rules for This Problem

[Derived from the six techniques, write 3-5 rules specific to this problem
that the solver should follow.]

1. **Never [thing revealed by inversion]** — the death inversion showed this kills you
2. **Always [thing revealed by simplification]** — this is the irreducible core
3. **When stuck, [technique that worked best]** — this technique was most productive here
```

## Phase 5: Present & Follow-up

Present the transformation to the user with the key highlights. Don't dump
the whole document — give the verdict, the best reframing, and the next steps.

```
## Shannon Transformation: [PROBLEM] — [TRANSFORMED / PARTIALLY / STUCK]

**Technique that cracked it:** [which technique was most productive]
**The transformed problem:** [one-sentence restatement that makes it solvable]
**Key insight:** [the one thing the six techniques revealed]

**What Shannon would say:** "[playful, concrete quote]"

**Next steps:**
1. [most important action]
2. [second action]
3. [third action]

Full analysis: `thoughts/shannon/YYYY-MM-DD-<slug>.md`

Want me to:
1. Deep-dive into any technique's findings?
2. Apply the techniques to a specific sub-problem?
3. Run /munger to evaluate a business idea that emerged from this transformation?
4. Apply to a different problem? (batch mode)
```

## Batch Mode

If the user wants to transform multiple problems:

1. Run the full analysis on each (can parallelize — one team per problem)
2. At the end, produce a comparison:

```
## Shannon Transformation Leaderboard

| Problem | Verdict | Best Technique | Key Insight | Confidence |
|---------|---------|---------------|-------------|------------|
| [name] | TRANSFORMED | Inversion | [insight] | HIGH |
| [name] | PARTIAL | Analogy | [insight] | MEDIUM |
| [name] | STUCK | — | [why it resists] | LOW |
```

## Scoring Discipline

- **Be Shannon, not a motivational speaker.** Shannon was honest about what
  he could and couldn't solve. If the problem resists the techniques, say so.
  STUCK is a valid and useful verdict.
- **Cite the source technique.** Every insight traces to a specific technique
  and a specific teammate's finding.
- **Play is serious.** Shannon's juggling machines weren't frivolous — they
  were him testing ideas physically. Encourage playful reframings that might
  seem silly but reveal structure.
- **The Bandwagon Warning applies here too.** These techniques don't solve
  everything. If the problem needs domain expertise, political skill, emotional
  intelligence, or patient execution rather than creative insight — say so.
  Shannon's framework is for INSIGHT problems, not EFFORT problems.
- **The STUCK basket is respectable.** Some problems are genuinely hard.
  Shannon proved that channel capacity EXISTS before anyone could achieve it —
  the construction took 45 years. Knowing a problem is hard is itself valuable.

## Pairing With Other Skills

- **/garrytan** first to refine a business idea, then **/shannon** if you hit
  a specific hard problem during execution
- **/munger** evaluates whether to pursue an idea; **/shannon** helps you
  figure out HOW to solve the hard parts
- **/shannon** on the technical problem, then **/plan-eng-review** on the solution
- Run **/shannon** whenever any skill hits a wall — it's designed for stuck moments

## Important Notes

- **Cost**: This skill spawns 5 agents. Use it for genuine stuck points,
  not trivial questions (use direct reasoning for those).
- **Sonnet for teammates, Opus for synthesis**: The lead handles the
  cross-technique synthesis — that's where deep reasoning matters.
- **No team? No problem**: If teams aren't enabled, run 5 sequential
  background agents. Same quality, just no cross-talk.
- **Domain knowledge matters**: Shannon's first prerequisite was "training
  and experience." The techniques work best when the team has context about
  the problem's domain. Provide as much context as you can.
- **This is a PROCESS skill, not an EVALUATION skill**: Unlike /munger
  (which renders a verdict on a business), /shannon transforms a problem.
  The output is new angles, new framings, and concrete next steps — not
  a pass/fail judgment.
