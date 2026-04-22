# HQ Skills

A collection of reusable [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills from Ravi HQ.

## Skills

### Thinking & Analysis (`deepthink` plugin)

| Skill | Description |
|-------|-------------|
| [think](skills/think/) | Multi-framework intelligence brief — selects 4-7 frameworks and synthesizes an actionable analysis |
| [red-team](skills/red-team/) | Adversarial stress-test of a `/think` brief — finds kill shots and failure modes |
| [deepthink](skills/deepthink/) | End-to-end pipeline: web research → think → red-team → gap research → iterate until convergence |
| [brainstorm](skills/brainstorm/) | Generative ideation engine — 6 specialist agents produce 15-30 novel ideas |
| [prospect](skills/prospect/) | Adaptive exploration: brainstorm → think → red-team with intelligent pivoting |
| [garrytan](skills/garrytan/) | YC Office Hours — startup mode (6 forcing questions) or builder mode (design thinking) |
| [add-thinker](skills/add-thinker/) | Meta-skill: research a thinker's framework and generate a new skill from it |
| [feynman](skills/feynman/) | Richard Feynman's Integrity Audit — hunts for unverified assumptions and cargo cult reasoning |
| [kahneman](skills/kahneman/) | Daniel Kahneman's Cognitive Diagnostic — identifies biases, substitutions, and framing effects |
| [shannon](skills/shannon/) | Claude Shannon's Six Techniques for Creative Problem Transformation |
| [tetlock](skills/tetlock/) | Philip Tetlock's Superforecasting — calibrated forecasts with explicit uncertainties |
| [duke](skills/duke/) | Annie Duke's Decision Quality — separates decision quality from outcome quality |
| [munger](skills/munger/) | Charlie Munger's Mental Lattice — multi-disciplinary elementary models |
| [thiel](skills/thiel/) | Peter Thiel's Monopoly Creation — zero-to-one, contrarian truths, 10x advantages |
| [helmer](skills/helmer/) | Hamilton Helmer's 7 Powers — scale economies, network effects, counter-positioning |
| [christensen](skills/christensen/) | Clayton Christensen's Disruption Analysis — disruptor/disrupted dynamics and trajectories |
| [meadows](skills/meadows/) | Donella Meadows's System Leverage Points — 12 levels of system intervention |
| [taleb](skills/taleb/) | Nassim Taleb's Antifragility — fragile/robust/antifragile classification and tail risk |
| [bezos](skills/bezos/) | Jeff Bezos's Decision-Making OS — Type 1/2 doors, flywheel logic, Day 1 diagnostics |

## Installation

### Via Claude Code Plugin Marketplace (Recommended)

Add the marketplace in Claude Code:

```
/plugin marketplace add ravi-hq/hq-skills
```

Then install the plugin:

```
/plugin install deepthink@hq-skills
```

Or browse and install interactively:

```
/plugin marketplace
```

### Update skills

```
/plugin update
```

### Remove skills

```
/plugin remove deepthink
/plugin marketplace remove hq-skills
```

### Manual Installation

If you prefer to manage skills manually:

```bash
git clone git@github.com:ravi-hq/hq-skills.git
cp -r hq-skills/skills/* ~/.claude/skills/
```

For project-scoped install:

```bash
mkdir -p .claude/skills
cp -r /path/to/hq-skills/skills/<skill-name> .claude/skills/
```

## Thinking Skills Usage Guide

The thinking skills are a suite of multi-agent analytical frameworks for strategic analysis, business evaluation, and decision-making. They work standalone or together as pipelines.

### Quick Start

```
# Analyze a business idea with the best-fit frameworks
/think Should we build an AI-native CRM?

# Stress-test the analysis
/red-team

# Generate ideas in a domain
/brainstorm Agent-native infrastructure plays

# Full end-to-end deep research + analysis pipeline
/deepthink Is vertical SaaS for dentists a good venture bet?

# Explore, analyze, and pivot adaptively
/prospect What are the most promising AI agent infrastructure opportunities?
```

### Individual Thinker Frameworks

Each framework skill spawns 3-5 specialist agents who apply their discipline's models in parallel, then synthesizes findings into a structured verdict.

```
/feynman <topic>      # Integrity audit — unverified assumptions, cargo cult reasoning
/kahneman <topic>     # Cognitive diagnostic — biases, framing effects, System 1/2
/shannon <topic>      # Problem transformation — simplify, reframe, decompose, invert
/tetlock <topic>      # Superforecasting — calibrated probabilities, base rates
/duke <topic>         # Decision quality — resulting bias, pre-mortems, quit criteria
/munger <topic>       # Mental lattice — multi-disciplinary elementary models
/thiel <topic>        # Monopoly creation — contrarian truths, 10x, last mover
/helmer <topic>       # 7 Powers — scale economies, network effects, switching costs
/christensen <topic>  # Disruption analysis — foothold markets, improvement trajectories
/meadows <topic>      # System leverage points — 12 levels of intervention
/taleb <topic>        # Antifragility — fragile/robust/antifragile, tail risks
/bezos <topic>        # Decision-making OS — Type 1/2 doors, flywheel, Day 1
```

### Pipelines

The skills are designed to compose:

| Pipeline | What it does |
|----------|-------------|
| `/think` → `/red-team` | Analyze, then adversarially stress-test |
| `/brainstorm` → `/think` → `/red-team` | Generate ideas, analyze the best, stress-test |
| `/deepthink` | Automated: research → think → red-team → gap research → iterate |
| `/prospect` | Automated: brainstorm → think → red-team with intelligent pivoting |

### Office Hours

```
/garrytan <startup idea>   # YC-style forcing questions (startup mode)
/garrytan <project idea>   # Design thinking brainstorm (builder mode)
```

### Adding New Thinkers

```
/add-thinker Andy Grove — Only the Paranoid Survive
```

This meta-skill researches a thinker's framework and generates a new skill file automatically.

### Output

All skills produce structured markdown documents in `thoughts/` with frontmatter metadata, executive summaries, and actionable findings.

## Contributing

1. Skills go in `skills/<skill-name>/SKILL.md` (with optional `references/` subdirectory)
2. Each skill must have YAML frontmatter with `name` and `description`
3. Add the skill to a plugin bundle in `.claude-plugin/marketplace.json`
