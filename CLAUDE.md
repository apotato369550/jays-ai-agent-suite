# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is Jay's agent library for agentic development experimentation. The repository contains custom AI agent definitions optimized for specific development workflows with emphasis on token efficiency, deterministic execution, and human-in-the-loop control.

## Core Philosophy

Read INTENT.md and CONTEXT.md before starting work. Key principles:

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

**git-commit-haiku** (model: haiku, color: purple)
- Handles all version control operations (commit, branch, merge, rebase, push)
- Diagnoses git state before executing operations
- Stops immediately on conflicts or unclear state
- Reports with full diagnostics

**debug-investigator** (model: haiku, color: cyan)
- Read-only diagnostic expert for root cause analysis
- Uses deterministic bash commands for token efficiency
- Traces execution paths, identifies leaks, finds redundancies
- Reports findings without proposing fixes

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

**debug-investigator**:
- Read-only commands only (no rm, mv, cp, sed -i, etc.)
- Never execute destructive operations

**git-commit-haiku**:
- Stop on merge conflicts → report details, ask for resolution
- Stop on unclear git state → report full diagnostics
- Stop on ambiguous requests → ask for clarification

**architect-agent**:
- Stop when architecture is undocumented or contradictory
- Report gaps rather than inventing documentation
- Checkpoint at 50% token usage for complex traces

**general-workhorse**:
- Maximum 3-4 debugging attempts before reporting
- Never trial-and-error - ask when uncertain
- Report immediately on ambiguity

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
