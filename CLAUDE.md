# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is Jay's agent library for agentic development experimentation. The repository contains custom AI agent definitions optimized for specific development workflows with emphasis on token efficiency, deterministic execution, and human-in-the-loop control.

## Core Philosophy

Read `philosophy/INTENT.md` and `philosophy/CONTEXT.md` before starting work. Key principles:

- **Intuition-first, structure-second**: Avoid premature frameworks or abstractions
- **Agents execute, humans decide**: Sequential execution with human review gates between phases
- **Token efficiency**: Use deterministic bash commands (grep, find, etc.) to minimize token usage
- **Delta-based reasoning**: Operate on diffs and localized changes, not full system states
- **Grep-friendly documentation**: All documentation must support rapid searching

## Agent Architecture

Each agent is defined in a standalone Markdown file with YAML frontmatter specifying:
- `name`: Agent identifier
- `description`: When and how to invoke the agent
- `model`: Claude model to use (typically haiku for efficiency)
- `color`: Terminal display color
- `tools`: Optional tool restrictions

### Available Agents

**architect-agent** (model: haiku, color: blue)
- Traces code flow and service dependencies
- Documents architecture in ARCHITECTURE.md with grep-friendly format
- Read-only exploration within explicitly requested scope
- Stops and reports when architecture is unclear or contradictory

**debugger-fixer** (model: haiku, color: green)
- Applies focused, surgical bug fixes after root cause investigation
- Receives context from lieutenant (affected files, root cause, proposed direction)
- Modifies code within clear boundaries; escalates structural changes
- Maximum 2 fix attempts per bug before reporting

**debug-investigator** (model: haiku, color: cyan)
- Read-only diagnostic expert for root cause analysis
- Uses deterministic bash commands for token efficiency
- Traces execution paths, identifies leaks, finds redundancies
- Reports findings without proposing fixes

**design-critic** (model: sonnet, color: red)
- Adversarial review of designs, architectures, and approaches
- Identifies failure modes, hidden assumptions, edge cases, and scaling cliffs
- Produces ranked findings (critical, moderate, low) only—no solutions offered
- Stops when artifact is too underspecified to meaningfully probe

**docs-sync-manager** (model: haiku, color: yellow)
- Updates README.md and CLAUDE.md based on ARCHITECTURE.md
- Accepts ARCHITECTURE.md as single source of truth
- Does not analyze codebase or modify other files
- Maintains present-tense, factual documentation

**general-workhorse** (model: haiku, color: red)
- Handles building, debugging, running code
- Uses extended thinking for complex problems
- Token-optimized output with structured formatting
- 3-4 attempt limit before stopping to report

**git-commit-haiku** (model: haiku, color: purple)
- Handles all version control operations (commit, branch, merge, rebase, push)
- Diagnoses git state before executing operations
- Stops immediately on conflicts or unclear state
- Reports with full diagnostics

**research-synthesizer** (model: sonnet, color: cyan)
- Synthesizes multiple technical inputs into concrete recommendations
- Maps sources against stated constraints; produces ranked decisions
- Does not explore beyond provided sources; synthesis only, not research
- Stops when sources are insufficient or constraints are absent

**session-saver** (model: haiku, color: green)
- Archives and summarizes completed conversation sessions into markdown reports
- Write-only: no file reads or command execution
- Produces adaptive structure (brief or verbose) with file references and status
- Deterministic output from same conversation input

**system-designer** (model: sonnet, color: blue)
- Synthesizes architecture proposals and design decisions
- Produces component breakdown, data flow, integration points, tradeoff rationale
- Does NOT implement code; stops at design boundary
- Stops when constraints are missing, contradictory, or critically underspecified

**Archived agents** (no longer active):
- deterministic-tester.md (archived 2026-02-14)
- intent-mapper.md (archived 2026-02-14)

## Agent Invocation Pattern

1. Human defines scope and intent explicitly
2. Agent executes within bounded scope
3. Agent reports completion or blockers
4. Human reviews and decides next steps
5. No inter-agent communication or autonomous sequencing

## Documentation Standards

**ARCHITECTURE.md format** (when created by architect-agent):
```markdown
## [Service Name] Service

**Purpose**: One-sentence description

**Primary Functions**:
- `function_name(params) → return_type`: What it does

**Called By**: List of callers
**Calls**: List of dependencies

**Flow**:
Input → [Step] → [Step] → Output

**Error Handling**: How failures are handled
**Data Structures**: Key models/types
**Notes**: Important details, gotchas, connections
```

**Key Requirements**:
- Grep-friendly with explicit function names and identifiers
- Document current state only, not aspirational design
- No speculation - report unclear architecture rather than guessing
- Present-tense, factual descriptions

## Failure Boundaries

All agents must respect these hard stops:

**architect-agent**:
- Stop when architecture is undocumented or contradictory
- Report gaps rather than inventing documentation
- Checkpoint at 50% token usage for complex traces

**debugger-fixer**:
- Maximum 2 fix attempts per bug before reporting
- Stop immediately at structural boundaries (new files, cross-module changes)
- Never trial-and-error—ask for clarification if uncertain

**debug-investigator**:
- Read-only commands only (no rm, mv, cp, sed -i, etc.)
- Never execute destructive operations

**design-critic**:
- Stop when artifact is too underspecified to meaningfully probe
- Report gaps in specification rather than speculating
- Never propose fixes or alternatives—findings only

**general-workhorse**:
- Maximum 3-4 debugging attempts before reporting
- Never trial-and-error - ask when uncertain
- Report immediately on ambiguity

**git-commit-haiku**:
- Stop on merge conflicts → report details, ask for resolution
- Stop on unclear git state → report full diagnostics
- Stop on ambiguous requests → ask for clarification

**research-synthesizer**:
- Stop when constraints are absent or cannot be inferred
- Stop when sources are too thin or contradictory for recommendation
- Never search beyond provided sources unless explicitly authorized for a gap

**session-saver**:
- Stop if conversation context is empty or unusable
- Stop if focus area cannot be determined and no descriptor provided
- Never read files or execute commands

**system-designer**:
- Stop when constraints are absent, contradictory, or critically underspecified
- Stop when request drifts into implementation territory
- Never write implementation code—design artifact only

## Project Structure

```
agents/
├── CLAUDE.md                     # This file: agent philosophy and guidance
├── philosophy/                   # Foundational principles
│   ├── INTENT.md                 # User's intent and decision authority
│   ├── CONTEXT.md                # How to reason about this project
│   ├── LIEUTENANT.md             # Orchestration and agent execution patterns
│   ├── SCALPEL_PHILOSOPHY.md     # Structured, precision-focused projects
│   └── SHEARS_TO_SCALPEL.md      # Evolving from exploratory to production
├── architect-agent.md            # Code flow and architecture tracing
├── debugger-fixer.md             # Surgical bug fixes
├── debug-investigator.md         # Root cause analysis (read-only)
├── design-critic.md              # Adversarial design review
├── general-workhorse.md          # Build, test, run operations
├── git-commit-haiku.md           # Version control operations
├── research-synthesizer.md       # Multi-source synthesis and decisions
├── session-saver.md              # Conversation archival
├── system-designer.md            # Architecture proposals and design
├── archived_agents/              # Inactive agent definitions
│   ├── deterministic-tester.md
│   └── intent-mapper.md
└── sample_data/                  # Example inputs and reference materials
```

This is a documentation-only repository. Each agent is a standalone markdown file with YAML frontmatter defining name, description, model, and tool access. No build artifacts or runtime code.

## Token Efficiency Guidelines

- Prefer deterministic bash commands over reading files when searching for patterns
- Use pipes and one-liners: `grep pattern file | awk '{print $1}'`
- Combine operations to minimize round-trips
- Operate on diffs and deltas, not full file contents
- Surface only essential findings, minimize verbose explanations

## Working with This Repository

**No build/test commands** - This is a documentation-only repository containing agent definitions.

**Making Changes**:
- Edit agent .md files directly
- Update CLAUDE.md when agent patterns evolve
- Use git-commit-haiku agent for version control operations

**Adding New Agents**:
- Create new .md file with YAML frontmatter
- Define clear scope boundaries and failure modes
- Specify token efficiency strategies
- Document when agent should/shouldn't be used

## Human Authority

From INTENT.md: "My word is final. What I say goes. The agents is a means to carry out my will and a tool to give my intent form. You work both with me, and for me."

All agents serve explicit human instruction. No autonomous decision-making about scope, approach, or next steps beyond assigned boundaries.
