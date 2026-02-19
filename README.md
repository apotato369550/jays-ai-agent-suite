# Jay's Agent Library

A personal agent library for agentic development experimentation. This repository contains custom AI agent definitions optimized for specific development workflows with emphasis on token efficiency, deterministic execution, and human-in-the-loop control.

## Quick Start

This is a documentation-only repository. Agents are invoked in Claude Code via the Task tool or @agent-name mentions. Each agent is defined as a standalone Markdown file with YAML frontmatter specifying name, model, color, and capabilities.

**Key principle**: Humans decide. Agents execute within explicit scope boundaries.

## Core Agents

### Architecture & Documentation

**architect-agent** (model: haiku, color: blue)
- Traces code flow and service dependencies
- Documents architecture in project format (grep-friendly)
- Read-only exploration within explicitly requested scope
- Invoke when: "Document the X architecture" or "Trace how Y works"

**docs-sync-manager** (model: haiku, color: yellow)
- Updates README.md and project documentation
- Accepts ARCHITECTURE.md as single source of truth
- Maintains present-tense, factual documentation
- Invoke when: Documentation files need to be synced with codebase state

### Investigation & Diagnosis

**debug-investigator** (model: haiku, color: cyan)
- Read-only diagnostic expert for root cause analysis
- Uses deterministic bash commands for token efficiency
- Traces execution paths, identifies leaks, finds redundancies
- Never modifies code; only reports findings
- Invoke when: "Debug this memory leak" or "Why is X not working?"

**design-critic** (model: sonnet, color: red)
- Adversarial technical review of architecture and design decisions
- Identifies hidden assumptions, failure modes, and tradeoff implications
- Probes weak points in reasoning and unexamined dependencies
- Invoke when: Design review or critique needed before implementation

### Specification & Planning

**aesthetic-mapper** (model: sonnet, color: magenta)
- Translates design intent, taste, and visual references into implementation-ready UI specs
- Reads existing components and tokens; produces parallelization-aware change specs
- Not a frontend implementer—produces specs only, stops at boundary
- Invoke when: "What should this look like?" or converting design vision to concrete specs

**refactor-planner** (model: sonnet, color: yellow)
- Decomposes bounded code refactors into dependency-aware, parallelizable task lists
- Maps change units and internal dependencies; flags parallel-safe vs. sequenced batches
- Not an implementer—produces the plan only, stops at boundary
- Invoke when: "I need to restructure this before adding X" or "Split out the data layer"

### Design & Planning

**system-designer** (model: sonnet, color: blue)
- Design-level synthesis of complex systems
- Proposes architecture, component breakdown, data flow, tradeoff analysis
- Produces design artifacts and decision memos
- Does not implement—stops at design boundary
- Invoke when: "How should this be built?" rather than "How does this work?"

**research-synthesizer** (model: sonnet, color: cyan)
- Cross-source synthesis of information into recommendations
- Aggregates from multiple sources (code, docs, web research, context)
- Produces structured synthesis documents
- Invoke when: Complex problem requiring multi-source analysis

### Version Control

**git-commit-haiku** (model: haiku, color: purple)
- Handles all version control operations (commit, branch, merge, rebase, push)
- Diagnoses git state before executing operations
- Stops immediately on conflicts or unclear state
- Invoke when: Version control operations needed

### Execution & Building

**general-workhorse** (model: haiku, color: red)
- Handles building, debugging, running code
- Uses extended thinking for complex problems
- Token-optimized output with structured formatting
- Invoke when: General technical tasks not covered by specialized agents

**debugger-fixer** (model: haiku, color: green)
- Applies targeted, surgical bug fixes after investigation
- Works from explicit root cause diagnosis
- Escalates structural changes back to lieutenant
- Invoke when: Fix direction is clear and root cause identified

### Session Management

**session-saver** (model: haiku, color: orange)
- Archives conversation sessions to markdown reports
- Extracts key findings, decisions, and artifacts
- Invoke when: Session results need to be preserved as artifacts

## How Agents Work

Each agent has:
- **Explicit scope** defined by human request (never self-directed)
- **Hard failure boundaries** where agent stops and reports
- **Token efficiency** constraints (prefer deterministic bash over exploration)
- **Clear reporting** format for results or blockers

### Execution Pattern

1. Human defines scope and intent explicitly
2. Agent executes within bounded scope
3. Agent reports completion or blockers
4. Human reviews and decides next steps
5. No inter-agent communication or autonomous sequencing

## Philosophy

Read the philosophy/ folder for detailed guidance:

- **INTENT.md** - Jay's core intent and decision-making authority
- **CONTEXT.md** - Foundational concepts (Scalpel vs Shears projects, parallel vs sequential execution)
- **LIEUTENANT.md** - How agents are orchestrated and coordinated
- **MEMORY.md** - Communication style, reasoning approach, structure preferences

Key principles:
- Intuition-first, structure-second
- Token efficiency is a design parameter
- Documentation reflects reality, not aspiration
- Humans preserve final intent; agents amplify capacity

## Agent Invocation

In Claude Code, invoke agents via:
- Task tool: `Task tool invoked with [agent-name]`
- Direct mention: `@architect-agent trace the X service`

When calling agents, provide:
- Clear problem statement or scope
- Any constraints or requirements
- Context files to read (if needed)
- Definition of success or expected output

## Repository Structure

```
/home/jay/.claude/agents/
├── README.md                      (this file)
├── CHANGELOG.md                   (version history)
├── CLAUDE.md                      (project guidance)
├── MEMORY.md                      (communication style)
├── [agent-name].md                (agent definitions)
├── philosophy/                    (foundational documents)
│   ├── INTENT.md
│   ├── CONTEXT.md
│   ├── LIEUTENANT.md
│   ├── SCALPEL_PHILOSOPHY.md
│   └── SHEARS_TO_SCALPEL.md
├── archived_agents/               (inactive agents)
└── sample_data/                   (session examples)
```

## Common Patterns

**Debugging a complex issue:**
1. Call debug-investigator to trace and diagnose
2. Call debugger-fixer to apply targeted fix (given root cause)
3. Validate with general-workhorse or manual testing

**Designing a new system:**
1. Call system-designer to synthesize architecture (given constraints)
2. Call design-critic to review and probe weak points
3. Call architect-agent to document existing state (if refactoring)

**Documenting a codebase:**
1. Call architect-agent to trace service chains
2. Call docs-sync-manager to update README and documentation

## Troubleshooting

**Agent stops unexpectedly:** Check the failure boundaries section in the agent's documentation. Agents stop and report when scope is unclear, state is ambiguous, or constraints are contradictory.

**Token usage too high:** Agents use deterministic bash commands (grep, awk, find) to minimize token cost. If an agent is reading too many files, request a narrower scope.

**Multiple agents needed:** Use sequential execution with human review gates between phases. Call agents in order, review findings between calls, then decide next steps.

**Structural changes needed:** debug-investigator and debugger-fixer escalate structural changes back to the human. system-designer handles architecture-level decisions.

## Additional Resources

- **CLAUDE.md**: Full project guidance and agent architecture details
- **philosophy/SCALPEL_PHILOSOPHY.md**: When and how to use this library for serious projects
- **philosophy/SHEARS_TO_SCALPEL.md**: Converting hobby projects into production-ready systems
- **CHANGELOG.md**: Version history and evolution of the agent library
