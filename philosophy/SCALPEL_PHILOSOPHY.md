# Scalpel Project Philosophy

> **Definition**: Precision-driven projects requiring structured documentation, deliberate design, and formal boundaries. Systems others depend on or that carry real constraints.

---

## Core Principle: Single Responsibility Per File

Each documentation file serves ONE purpose. No overlap. No redundancy.

**README.md** - User Manual
- How to use the service
- Setup, configuration, endpoints
- Examples and troubleshooting
- **Never**: Updates, implementation details, architectural reasoning

**CLAUDE.md** - Agent Philosophy
- WHY the project exists (intent, vision, purpose)
- Architectural philosophy (not architecture itself)
- Design decisions with rationale
- Development principles for agents
- Success metrics and common gotchas
- **Never**: Updates, implementation details, how-to guides

**CHANGELOG.md** - Historical Record
- Every update documented with date/version
- Format: Added, Changed, Fixed, Removed, Updated
- Rationale for changes, backward compatibility notes
- **Never**: Current state, implementation architecture

**ARCHITECTURE.md** - Implementation Source of Truth
- Feature domains with rigid subsections per endpoint/service
- Greppable structure: Purpose → Flow → Input → Output → Calls → Errors → Temperature → Notes
- Grep-Friendly Index with line numbers for fast search
- **Never**: Historical context, user instructions, philosophy

**Supporting Structure:**
- `/docs` - Discretionary documentation (your rules)
- `/plans` - Informal brainstorming (captain + lieutenant)
- `/.claude/agents/` - Evolving agent definitions (tentative, growing)

---

## Documentation Efficiency: Token Minimization

**Problem**: Reading 3,500-line architecture docs burns tokens.

**Solution**: Rigid subsections + grep-friendly index.

**Pattern**:
```
## Endpoint: /endpoint_name
### Purpose (1 sentence)
### Primary Flow (numbered steps)
### Request Input (code)
### Response Output (code)
### Called By / Calls
### Error Handling (specific scenarios)
### Temperature Settings (with rationale)
### Notes (implementation details)

## Grep-Friendly Index
**Function Name**: location, purpose
```

Agents grep for subsections, read 20 lines instead of 3,500. **10-20x token savings.**

---

## Human-Agent Hierarchy

**Captain (User)**: Decides what happens next. Final authority.
**Lieutenant (REPL Agent)**: Plans, proposes, orchestrates, launches and executes after approval.
**Lackeys (Specialized Agents)**: Execute scoped tasks, report blockers immediately.

**Rules**:
- Agents never decide strategy, only tactics within boundaries
- Sequential execution with human review gates between phases
- No inter-agent communication or self-directed sequencing
- Agents report completion/blockers, captain decides next phase

**Why**: Preserves human judgment, prevents wasted motion, keeps token budgets tight.

---

## Documentation Format Guidelines

**EXAMPLE README Structure**:
1. Current Status & Features (what works NOW)
2. Setup Instructions (environment → installation → running)
3. API Endpoints (organized by function, with examples)
4. Testing Workflow
5. Troubleshooting
6. Additional Resources

**CLAUDE Structure**:
1. Vision & Purpose (WHY)
2. Architectural Philosophy (core patterns)
3. Design Decisions (rationale for choices)
4. Critical Context (domain-specific knowledge)
5. Development Principles (how agents should work)
6. Project Structure (organizational overview)
7. Common Gotchas (what breaks and why)
8. Success Metrics (what "good" looks like)

**ARCHITECTURE Structure**:
1. Feature Domain Header with Pattern Overview (ASCII diagram)
2. Endpoints/Services (rigid subsections)
3. Cross-Service Dependencies
4. Performance Characteristics
5. Testing Modes
6. Future Considerations
7. Grep-Friendly Index

---

## When to Use Scalpel Structure

**Triggers**:
- Others depend on the system
- Real constraints (performance, security, correctness)
- Needs formal documentation for onboarding
- Requires compliance or audit trails
- Budget matters (token costs, API costs)
- Multi-agent orchestration needed

**Anti-Triggers** (Stay Shears):
- Exploratory work, learning, tinkering
- Solo hobby projects with no external dependencies
- Velocity matters more than structure
- You're still figuring out what you're building

---

## Agent Interaction Model

**Default Mode**: Sequential with review gates
- Lieutenant proposes plan
- Captain approves/adjusts
- Lackeys execute scoped work
- Report completion + blockers
- Captain decides next phase

**Parallel Mode**: Only when tasks are independent
- No file edit conflicts
- No cross-dependencies
- Orchestrator has sole authority to invoke parallel agents

**Boundaries for Lackeys**:
- 3-4 attempts max, then stop and report
- Never trial-and-error without asking
- Report immediately when assumptions unclear
- Don't explore beyond assigned scope
- Don't make decisions about what comes next

---

## Token Efficiency Best Practices

1. **Use deterministic commands when possible**: `grep`, `sed`, `awk` over agent reads
2. **Grep before reading**: Find the section, read 50 lines, not 3,500
3. **Rigid subsections**: Makes grep reliable
4. **Grep-Friendly Index**: Function names + line numbers
5. **Single Responsibility**: No redundant info across files
6. **Lazy loading**: Only read what's needed for current task

**Example**:
```bash
# Bad (agent reads 3,500 lines)
Read ARCHITECTURE.md

# Good (reads 20 lines)
grep -A 20 "### Error Handling" ARCHITECTURE.md
```

---

## Scalpel Mindset

**Essence Over Implementation**: Document WHY, not HOW. Code documents how.

**Graceful Degradation**: Partial results better than failures.

**Explicit Boundaries**: Timeouts, retry limits, scope constraints.

**Test in Both Modes**: Validate with mocks (schema) and real API (behavior).

**Respect Hierarchy**: Captain decides, lieutenant plans, lackeys execute.

**Efficiency First**: Minimize tokens, maximize precision.

---

*Created: 2026-01-19*
*For converting shears to scalpel, see SHEARS_TO_SCALPEL.md*
