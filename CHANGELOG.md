# Changelog

All notable changes to Jay's agent library are documented here.

## [1.5.0] - 2026-02-24

### Added
- **craftsman-agent**: Frontend and UI implementation specialist. Works from aesthetic-mapper specifications or design briefs. Implements components, styling, and interactive features with aesthetic integrity. Establishes aesthetic-mapper → craftsman-agent pipeline mirroring system-designer → general-workhorse.
- **Constraint-based problem solving philosophy**: New MEMORY.md section codifying the principle of building within constraints rather than reaching for frameworks. Emphasizes bespoke solutions, homegrown alternatives over cookie-cutter complexity, and prioritizing understanding over generic patterns.

### Changed
- **intent-mapper**: Un-archived and redefined from codebase-mapping agent to linguistic constraint agent. Now takes ambiguous user intent and decomposes it into clear, grounded INTENT_XXX.md files. Output tied to system semantics (what it does) rather than code structure (how it's built).
- Updated CLAUDE.md and README.md to reflect new agents and philosophy

### Summary
Expanded library with constraint-focused intent clarification (intent-mapper) and UI implementation (craftsman-agent). Introduced philosophy of building with existing constraints—prefer bespoke, homegrown solutions over framework complexity when the third-party tool isn't the obvious lowest apple on the tree.

## [1.4.0] - 2026-02-17

### Added
- **aesthetic-mapper agent**: Translates design intent and visual references into implementation-ready UI specs. Reads existing components and produces parallelization-aware change specs for workhorse agents.
- **refactor-planner agent**: Decomposes bounded code refactors into dependency-aware, parallelizable task lists. Maps change units, identifies parallel-safe batches, and flags risk surfaces.

### Changed
- Fixed design-critic agent model classification (sonnet, not haiku) in README

### Summary
Expanded specification capabilities with aesthetic-mapper for UI/design specs and refactor-planner for code restructuring planning. Both agents operate at the specification boundary—they produce plans/specs only, stopping before implementation.

## [1.3.0] - 2026-02-14

### Added
- **Archived inactive agents**: Moved deterministic-tester and intent-mapper to archived_agents/ folder for reduced cognitive load

### Summary
Cleaned up agent library by archiving agents that were not being used frequently, maintaining focus on core active agents.

## [1.2.0] - 2026-02-10 to 2026-02-11

### Added
- **system-designer agent**: Design-level synthesis for complex systems. Proposes architecture, component breakdown, and tradeoff analysis. Uses Sonnet model for sustained reasoning over ambiguous constraints.
- **design-critic agent**: Adversarial technical review agent. Identifies hidden assumptions, failure modes, and weak points in architecture and design decisions.
- **research-synthesizer agent**: Cross-source synthesis agent. Aggregates information from multiple sources (code, docs, web research, context) into structured recommendations. Uses Sonnet model.
- **intent-mapper agent**: Helper agent for clarifying intent and scope (archived in v1.3.0)

### Changed
- Expanded agent library to support design-level work beyond debugging and implementation
- Added Sonnet-based agents (system-designer, research-synthesizer) for complex reasoning tasks

### Summary
Introduced design and synthesis capabilities to the agent library. System-designer handles architecture proposals, design-critic performs adversarial review, and research-synthesizer aggregates complex multi-source information.

## [1.1.0] - 2026-01-22 to 2026-01-31

### Added
- **session-saver agent**: Archives conversation sessions to markdown reports with extracted findings and artifacts
- **debugger-fixer agent**: Applies targeted bug fixes after root cause investigation. Escalates structural changes to human
- **Lieutenant system prompt**: LIEUTENANT.md defining agent orchestration principles and human-in-the-loop control patterns
- **MEMORY.md**: Unified memory document covering communication style, reasoning approach, agent orchestration principles, and structure preferences

### Changed
- Updated MEMORY.md with new sections on "How You Communicate" and "Agent Orchestration Principles"
- Added clarification on parallel vs sequential agent execution rules
- Expanded token efficiency guidelines for agent execution

### Summary
Added session archival and bug-fixing capabilities. Formalized lieutenant (orchestrator) role and human-in-the-loop decision-making patterns. Unified memory and communication guidelines.

## [1.0.0] - 2026-01-16 to 2026-01-19

### Added
- **architect-agent**: Code flow tracing and architecture documentation. Reads code to understand service dependencies, documents findings in grep-friendly format.
- **git-commit-haiku**: Version control operations (commit, branch, merge, rebase, push). Diagnoses git state before execution.
- **debug-investigator**: Read-only root cause analysis. Uses deterministic bash commands for investigation without modifying code.
- **docs-sync-manager**: Documentation synchronization. Updates README.md based on ARCHITECTURE.md as single source of truth.
- **general-workhorse**: General-purpose task execution (building, debugging, running code). Uses extended thinking for complex problems.
- **philosophy/ folder**: Foundational documents (INTENT.md, CONTEXT.md, SCALPEL_PHILOSOPHY.md, SHEARS_TO_SCALPEL.md)
- **CLAUDE.md**: Project guidance for Claude Code instances
- **.gitignore**: Excludes sample_data/ and other generated artifacts

### Summary
Initial release of agent library with core agents for architecture tracing, version control, investigation, documentation, and general task execution. Established philosophical foundations and project guidance.

---

## Release Notes

### Token Efficiency Evolution

**v1.0.0-v1.1.0**: Established deterministic bash command patterns (grep, awk, sed, find) to minimize token usage. Agents prefer one-liners over multi-step exploration.

**v1.2.0**: Introduced Sonnet-based agents (system-designer, research-synthesizer) for reasoning-intensive tasks that exceed Haiku's capabilities.

**v1.3.0**: Refined agent library by archiving unused agents, reducing surface area and cognitive load.

### Model Distribution

- **Haiku** (8 agents): architect-agent, debug-investigator, git-commit-haiku, docs-sync-manager, general-workhorse, debugger-fixer, session-saver, craftsman-agent
- **Sonnet** (6 agents): aesthetic-mapper, design-critic, intent-mapper, refactor-planner, system-designer, research-synthesizer

### Execution Model

All versions follow human-in-the-loop orchestration:
1. Human defines scope explicitly
2. Agent executes within scope
3. Agent reports results or blockers
4. Human decides next steps
5. No inter-agent autonomy

### Philosophy Evolution

- v1.0.0: Core intent-driven architecture
- v1.1.0: Formalized lieutenant orchestration and human decision authority
- v1.2.0: Extended to design-level reasoning and synthesis
- v1.3.0: Refined focus on active agent patterns
- v1.5.0: Added constraint-based problem solving—prefer bespoke, homegrown solutions over framework complexity

### Constraining Ambiguity

Starting in v1.5.0, the library emphasizes **linguistic constraint** and **constraint-based problem solving**:

**Intent-mapper** is the primary vehicle for constraint. Instead of analyzing code or exploring frameworks, it takes vague user signals and turns them into grounded language—INTENT files that are explicit, defensible, and tied to system semantics.

**Constraint-based philosophy** shifts the default question from "What existing tool/framework fits this?" to "What can I build with current constraints to achieve the output I want?" This leads to bespoke solutions (Python scripts, homegrown code) over cookie-cutter frameworks, reducing dependency bloat and increasing understanding.

The tower (staircase) metaphor: Build step-by-step solutions you understand deeply, rather than reaching for clouds.
