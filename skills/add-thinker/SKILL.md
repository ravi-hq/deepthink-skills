---
name: add-thinker
description: |
  Meta-skill: research a thinker's framework deeply, then synthesize a new
  Claude Code skill that applies their thinking to business ideas. Takes a prompt
  like "/add-thinker Andy Grove — Only the Paranoid Survive" and produces a new
  skill at .claude/skills/<thinker>/SKILL.md that works like /munger does.
  Use when the user says "add thinker", "add a new thinker", "codify X's thinking",
  or "make a skill for <person>'s framework".
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
---

# /add-thinker — Codify a Thinker's Framework as a Skill

Takes a thinker (person, book, school of thought) and produces a new Claude Code
skill that applies their thinking to business ideas — the same way /munger applies
Charlie Munger's mental lattice.

## What This Skill Does

1. **Deep research** the thinker's framework — primary sources, talks, books, interviews
2. **Extract the generic form** — the reusable mental models, not just anecdotes
3. **Design specialist agents** — each covering a distinct lens from the framework
4. **Synthesize a SKILL.md** — a fully functional skill that spawns a team and produces
   a structured analysis, verdict, and actionable output

The output is a new skill file at `.claude/skills/<thinker>/SKILL.md` in the current
repo, immediately usable as `/<thinker>`.

## Invocation

`/add-thinker <prompt>`

The prompt can be:
- A person: `Charlie Munger`, `Andy Grove`, `Nassim Taleb`
- A person + work: `Andy Grove — Only the Paranoid Survive`
- A school of thought: `Toyota Production System / Lean Thinking`
- A concept: `Nassim Taleb's Antifragility framework`
- A vague request: `that Intel CEO who wrote about strategic inflection points`

If the prompt is too vague to identify a thinker, ask ONE clarifying question.

## Phase 1: Identify and Scope

Parse the prompt to determine:

- **The thinker**: Name, era, domain
- **The core works**: Books, talks, essays that contain the framework
- **The domain**: Business strategy, psychology, investing, engineering, etc.
- **The slug**: lowercase hyphenated name for the skill directory (e.g., `grove`, `taleb`, `toyota`)

Present back to the user:

```
## Adding Thinker: [Name]

**Core works to research:**
- [Book/talk 1]
- [Book/talk 2]
- [Book/talk 3]

**Domain:** [strategy / investing / psychology / engineering / etc.]
**Skill name:** /[slug]

I'll now deep-research this framework, extract the generic mental models,
and synthesize a skill. This takes a few minutes.

Starting research...
```

## Phase 2: Deep Research (Parallel Agents)

Spawn 3-4 research agents in parallel. Each focuses on a different aspect
of the thinker's framework. Use `model: "sonnet"` for researchers.

### Agent 1: Primary Source Researcher

```
You are researching [THINKER]'s core framework for the purpose of creating
a reusable analytical tool.

Use WebSearch and WebFetch to find:

1. PRIMARY SOURCES
   - Full transcripts or detailed summaries of their key talks/speeches
   - Book summaries with actual frameworks extracted (not just reviews)
   - Interviews where they explain their thinking process
   - Check specific known sources: [list known URLs if any — Stripe Press,
     Farnam Street, personal websites, university lectures]

2. THE CORE FRAMEWORK
   - What are the 3-7 key principles or mental models?
   - How do they structure their analysis of a problem?
   - What questions do they always ask?
   - What is their equivalent of Munger's "inversion" or "lollapalooza"?
   - What is their unique contribution — the thing only THEY see?

3. THEIR VOCABULARY
   - Key terms they coined or use distinctively
   - Metaphors and analogies they rely on
   - Their catchphrases and memorable formulations

Report back with detailed findings including specific quotes and source URLs.
Be thorough — this research becomes the foundation of a permanent skill.
```

### Agent 2: Applied Examples Researcher

```
You are researching how [THINKER] applies their framework to real-world cases.

Use WebSearch and WebFetch to find:

1. CASE STUDIES
   - Specific businesses, decisions, or situations they analyzed
   - How they walked through their framework step by step
   - What conclusions they reached and why
   - Cases where their framework predicted correctly
   - Cases where it failed or had blind spots

2. THE GENERIC PATTERN
   - Across all their case studies, what's the repeated analytical move?
   - What do they always check first?
   - What do they always check last?
   - What's their equivalent of "does the math work" or "what kills this"?

3. COMPARISON TO OTHER THINKERS
   - How does their framework overlap with Munger's lattice?
   - Where does it diverge or add something Munger misses?
   - What's complementary vs. contradictory?

Report with specific examples, quotes, and sources.
```

### Agent 3: Counter-Arguments and Limitations Researcher

```
You are researching the limitations, critiques, and failure modes of
[THINKER]'s framework.

Use WebSearch and WebFetch to find:

1. KNOWN CRITIQUES
   - Academic or practitioner criticism of their framework
   - Cases where following their advice led to bad outcomes
   - Blind spots they acknowledge themselves
   - What types of problems does their framework NOT apply to?

2. FAILURE MODES
   - When does this thinking lead you astray?
   - What biases does the thinker themselves exhibit?
   - What does the framework miss that other frameworks catch?

3. CIRCLE OF COMPETENCE
   - What domains is this framework strongest in?
   - What domains should it NOT be applied to?
   - What's the thinker's own circle of competence vs. where they opine?

This is critical — every skill needs a "when NOT to use this" section.
Report with specific examples and honest assessment.
```

### Agent 4: Adjacent Thinkers and Synthesis (optional, spawn if the framework is broad)

```
You are researching thinkers adjacent to [THINKER] who extend, complement,
or challenge their framework.

Use WebSearch and WebFetch to find:

1. INTELLECTUAL LINEAGE
   - Who influenced this thinker?
   - Who did this thinker influence?
   - What's the "school of thought" this belongs to?

2. COMPLEMENTARY FRAMEWORKS
   - Other thinkers whose models stack well with this one
   - Specific models from other disciplines that strengthen this framework
   - What would a "lattice" look like that includes this thinker?

3. SYNTHESIS OPPORTUNITIES
   - How could this framework be combined with Munger's lattice?
   - What does this thinker add to the /munger analysis that's missing?
   - Could this be an "add-on module" to /munger rather than standalone?

Report with specific frameworks and how they interconnect.
```

## Phase 3: Extract the Generic Form

After all research agents report back, the lead synthesizes the findings into
a structured framework. This is the most important step — it's where raw
research becomes a reusable analytical tool.

### The Extraction Template

For each thinker, extract:

```
1. THE CORE QUESTION
   What single question does this thinker's framework answer?
   - Munger: "Is this a good business to own for decades?"
   - Grove: "Are we at a strategic inflection point?"
   - Taleb: "Is this fragile, robust, or antifragile?"

2. THE KEY PRINCIPLES (3-7)
   The reusable mental models, stated as actionable rules.
   Each principle needs:
   - Name (their term or a clear label)
   - One-sentence rule
   - The mechanism (why it works)
   - How to apply it (specific questions to ask)
   - Example from the thinker's own work

3. THE ANALYTICAL PROCESS
   The step-by-step sequence they follow:
   - What do they check first? (the "no-brainer" equivalent)
   - What math do they run? (the numerical check)
   - What do they invert? (the "how does this die" check)
   - What's their synthesis move? (the "lollapalooza" equivalent)
   - What's their verdict framework? (the "In/Out/Too Tough" equivalent)

4. THE SPECIALIST LENSES
   Map to 3-5 agent roles, each covering a distinct analytical lens:
   - What discipline does each lens draw from?
   - What specific questions does each lens ask?
   - What output format does each lens produce?
   - How do the lenses interact? (cross-references)

5. THE VERDICT FRAMEWORK
   How does this thinker make a final call?
   - What are their "baskets" (equivalent to In/Out/Too Tough)?
   - What evidence tips the verdict?
   - What's their signature voice/style for delivering it?

6. THE FAILURE MODES
   When should you NOT use this framework?
   - Domain limitations
   - Known blind spots
   - Types of problems it misleads on

7. THE VOICE
   How does this thinker communicate?
   - Direct/indirect? Technical/colloquial? Serious/humorous?
   - Signature phrases, metaphors, rhetorical moves
   - What would they actually SAY about your idea?
```

Present this extraction to the user:

```
## Framework Extraction: [Thinker]

**Core question:** [one sentence]

**Key principles:**
1. [Name] — [one-line rule]
2. [Name] — [one-line rule]
3. ...

**Specialist lenses (will become agents):**
1. [Agent name] — [what they analyze]
2. [Agent name] — [what they analyze]
3. ...

**Verdict framework:** [how the thinker makes a final call]

**Voice:** [how they communicate]

**Not for:** [when to NOT use this]

Does this capture the framework correctly? Anything to add or adjust?
```

Wait for user confirmation before generating the skill.

## Phase 4: Generate the Skill

Using the extracted framework, generate a SKILL.md that follows the same
architecture as /munger. The skill MUST include:

### Required Sections

1. **Frontmatter** — name, description, allowed-tools (same set as /munger)

2. **Header** — skill name, one-paragraph description of what it does

3. **Core Principles** — the thinker's key principles, stated as non-negotiable
   rules for the analysis (equivalent to Munger's "five notions")

4. **Invocation** — how to trigger, what arguments to provide

5. **Phase 1: Understand the Idea** — lead gathers context, presents understanding

6. **Phase 2: Spawn the Team** — detailed prompts for each specialist agent.
   Each agent prompt MUST include:
   - Role and discipline
   - The business idea (substituted at runtime)
   - Specific analytical questions from the framework
   - Output format
   - Cross-reference instructions for messaging teammates
   - The thinker's actual vocabulary and framing

7. **Phase 3: Monitor & Cross-Pollinate** — same as /munger

8. **Phase 4: Synthesize — The [Thinker] Verdict** — the lead's synthesis process.
   MUST include:
   - How to combine agent findings
   - The framework's equivalent of "lollapalooza detection"
   - The verdict framework (the thinker's version of In/Out/Too Tough)
   - A "What [Thinker] Would Say" section written in their voice
   - Actionable rules derived from the analysis

9. **Phase 5: Present & Follow-up** — summary, verdict, next steps

10. **Batch Mode** — how to compare multiple ideas

11. **Scoring Discipline** — honesty rules, evidence requirements

12. **Important Notes** — cost, model selection, pairing with other skills

### Quality Requirements

- **Agent prompts must be LONG and SPECIFIC** — not "analyze the economics"
  but detailed questions with the thinker's actual vocabulary and examples.
  Look at the /munger agent prompts for the standard. Each should be 30-50 lines.

- **The verdict must be HONEST** — capture the thinker's actual standards.
  If they're a harsh critic (like Munger), the skill should reject most ideas.
  If they're an optimist, the skill should reflect that — but still have rigor.

- **The voice must be AUTHENTIC** — the "What [Thinker] Would Say" section
  should sound like them, using their actual phrases and rhetorical style.

- **Cross-references to /munger** — note where this framework overlaps with
  or complements Munger's lattice. Suggest pairing where appropriate.

### File Output

Write the skill to:
```
.claude/skills/<slug>/SKILL.md
```

Also copy it to the global skills directory so it's available everywhere:
```
~/.claude/skills/<slug>/SKILL.md
```

## Phase 5: Verify and Present

After writing the skill:

1. **Read it back** — verify it's syntactically correct and complete
2. **Check it appears** — the skill should show up in the skills list
3. **Present to the user:**

```
## New Thinker Skill: /<slug>

**Framework:** [one-sentence description]
**Core question:** [what it answers]
**Agents:** [N] specialists
  1. [Agent] — [lens]
  2. [Agent] — [lens]
  ...
**Verdict:** [framework's decision categories]
**Voice:** [how it communicates]

Installed at:
  - .claude/skills/<slug>/SKILL.md (this repo)
  - ~/.claude/skills/<slug>/SKILL.md (global)

Try it: /<slug> [your business idea]

**Suggested workflow:**
1. /garrytan — refine the idea
2. /munger — Munger's lattice
3. /<slug> — [thinker]'s framework
```

## Architecture Notes

- **Research agents use sonnet** — they're doing web search and extraction, not
  deep reasoning. The lead (opus) handles synthesis and skill generation.
- **3-4 research agents max** — more than that produces diminishing returns and
  the lead can't synthesize well beyond 4 perspectives.
- **The generated skill follows /munger's architecture exactly** — same phase
  structure, same agent spawning pattern, same verdict format. This makes all
  thinker skills composable and familiar.
- **Each thinker skill is standalone** — it doesn't depend on /munger being
  installed. But the output format is compatible, so you can run both and
  compare verdicts.
- **The global install means the skill persists** — even if you delete this repo,
  the thinker skill remains available in `~/.claude/skills/`.

## Examples of Thinkers This Should Work For

| Prompt | Skill | Core Question |
|--------|-------|---------------|
| Andy Grove | /grove | Are we at a strategic inflection point? |
| Nassim Taleb antifragility | /taleb | Is this fragile or antifragile? |
| Peter Thiel Zero to One | /thiel | Is this a 0-to-1 or 1-to-n business? |
| Toyota Production System | /toyota | Where is the waste and how do we eliminate it? |
| Ben Thompson Stratechery | /thompson | What's the aggregation theory play here? |
| Clayton Christensen | /christensen | Is this disruptive or sustaining innovation? |
| Hamilton Helmer 7 Powers | /helmer | Which of the 7 powers does this business have? |
| Jeff Bezos | /bezos | Is this a one-way or two-way door decision? |
| Ray Dalio Principles | /dalio | What principles govern this situation? |
| Eliyahu Goldratt Theory of Constraints | /goldratt | What's the bottleneck? |

## What This Skill Does NOT Do

- It does not evaluate business ideas itself — it creates tools that do.
- It does not replace reading the thinker's actual work — the research phase
  extracts the framework, but the skill description should reference primary
  sources so users can go deeper.
- It does not guarantee the generated skill is perfect on first pass — complex
  thinkers may need iteration. The user can edit the SKILL.md after generation.
