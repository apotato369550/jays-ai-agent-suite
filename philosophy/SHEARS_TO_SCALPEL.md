# Shears to Scalpel: Conversion Guide

> **Goal**: Transform informal exploratory projects into structured precision systems without losing momentum.

---

## When to Convert

**Signals**:
- Project proven valuable, needs to scale
- Others want to use/contribute
- You're rebuilding same context repeatedly
- Token costs climbing (re-reading same files)
- Can't remember why X was built that way
- Need to hand off work to agents efficiently

**Don't Convert If**:
- Still exploring, learning, tinkering
- Solo hobby with no external dependencies
- You're comfortable with chaos
- Structure would slow you down

---

## Pre-Conversion Assessment

**Ask yourself**:
1. What does this project DO? (purpose in one sentence)
2. Who uses it? (just me, others, APIs, etc.)
3. What's critical vs nice-to-have? (constraints, requirements)
4. What keeps breaking? (gotchas, edge cases)
5. What would I tell someone learning this? (key concepts)

**Write answers in `/plans/conversion_notes.md`** - This becomes your source material.

---

## Conversion Order (4-Phase Approach)

### Phase 1: Extract Intent (CLAUDE.md)

**Goal**: Capture WHY before HOW.

**Process**:
1. Create `/plans/conversion_notes.md` if not done
2. Answer these questions:
   - Why does this exist? (vision, purpose, target user)
   - What patterns emerged? (architectural philosophy)
   - What decisions matter? (why X over Y)
   - What breaks often? (common gotchas)
   - What does "good" look like? (success metrics)

3. Create `CLAUDE.md` with structure:
```markdown
# [Project Name] - Project Context

## Vision & Purpose
[Why this exists, target user, value proposition]

## Architectural Philosophy
[Core patterns with rationale]

## Core Design Decisions
[Why X over Y, tradeoffs accepted]

## Critical Context
[Domain knowledge, key constraints]

## Development Principles
[How agents should help]

## Common Gotchas
[What breaks and why]

## Success Metrics
[What "good" looks like]
```

**Time**: 30-60 minutes. Write from memory, refine later.

---

### Phase 2: Map the Codebase (ARCHITECTURE.md)

**Goal**: Create greppable implementation reference.

**Process**:
1. List all endpoints/services/functions
2. For each, document with rigid subsections:
   - Purpose (1 sentence)
   - Primary Flow (numbered steps)
   - Input/Output (code blocks)
   - Called By / Calls
   - Error Handling (specific scenarios)
   - Notes (implementation details)

3. Create `ARCHITECTURE.md` with structure:
```markdown
# Architecture Documentation: [Project Name]

## [Feature Domain Name]

### Pattern Overview
[ASCII diagram of data flow]

## Endpoint: /endpoint_name
### Purpose
### Primary Flow
### Request Input
### Response Output
### Called By
### Calls
### Error Handling
### Notes

## Grep-Friendly Index
**Function Name**: location, purpose
```

**Tip**: Use `grep -r "def " .` or `rg "class "` to list functions/classes, then document incrementally.

**Time**: 2-4 hours for initial pass. Refine as you work.

---

### Phase 3: Document User Experience (README.md)

**Goal**: Teach external users how to use it.

**Process**:
1. Assume zero context - what would onboarding doc need?
2. Create `README.md` with structure:
```markdown
# [Project Name]

## Current Status & Features
[What works NOW]

## Setup Instructions
[Environment → Installation → Running]

## API/Usage Examples
[How to use it, organized by function]

## Testing
[How to validate it works]

## Troubleshooting
[Common problems, solutions]

## Additional Resources
[Links to CLAUDE.md, ARCHITECTURE.md, etc.]
```

**Time**: 1-2 hours. Focus on happy path first.

---

### Phase 4: Initialize History (CHANGELOG.md)

**Goal**: Start tracking changes going forward.

**Process**:
1. Create `CHANGELOG.md` with initial state:
```markdown
# Changelog

## [v1.0.0] - [DATE] - Initial Scalpel Structure

### Added
- CLAUDE.md: Project philosophy and intent
- ARCHITECTURE.md: Implementation reference
- README.md: User documentation
- CHANGELOG.md: Version tracking

### Changed
- [Migration from shears to scalpel]

### Notes
- Baseline for future updates
- All changes after this point tracked here

---

*This changelog follows [Keep a Changelog](https://keepachangelog.com/) principles.*
```

**Time**: 15 minutes.

---

## Post-Conversion Workflow

**From now on**:
1. **Before changing code**: Grep ARCHITECTURE.md to understand current implementation
2. **After changing code**: Update ARCHITECTURE.md → CHANGELOG.md → README.md (if user-facing)
3. **When adding agents**: Document in `/.claude/agents/` and reference in CLAUDE.md if needed
4. **When planning work**: Create `/plans/plan_N_description.md` before executing

**Agent Workflow**:
1. Captain (you) describes task
2. Lieutenant (REPL agent) proposes plan in `/plans/`
3. Captain approves/adjusts
4. Lieutenant executes or delegates to lackeys
5. Lieutenant updates docs (ARCHITECTURE → CHANGELOG → README)
6. Captain reviews

---

## Common Migration Pitfalls

**Pitfall 1: Over-documenting**
- **Symptom**: Spending more time on docs than code
- **Fix**: Document incrementally. Cover critical paths first, fill gaps as you work.

**Pitfall 2: Stale docs**
- **Symptom**: ARCHITECTURE.md doesn't match reality
- **Fix**: Update docs in same commit as code changes. Use CHANGELOG.md to track updates.

**Pitfall 3: Verbose ARCHITECTURE.md**
- **Symptom**: 10,000-line architecture doc, agents can't grep efficiently
- **Fix**: Rigid subsections, grep-friendly index. If section > 200 lines, split into subsections.

**Pitfall 4: Redundant info**
- **Symptom**: Same explanation in README, CLAUDE, and ARCHITECTURE
- **Fix**: Single responsibility. README = how to use, CLAUDE = why it exists, ARCHITECTURE = how it's built.

**Pitfall 5: Losing velocity**
- **Symptom**: Conversion takes weeks, project stalls
- **Fix**: Phase 1 + 2 in one day (80% solution). Refine incrementally.

---

## Quick-Start Migration (4 Hours)

**Hour 1: Intent Extraction**
- Write `/plans/conversion_notes.md` answering 5 questions
- Draft CLAUDE.md (rough, memory-based)

**Hour 2: Core Architecture**
- List 5-10 most critical functions/endpoints
- Document each with rigid subsections in ARCHITECTURE.md
- Add Grep-Friendly Index

**Hour 3: User Documentation**
- Draft README.md (setup + basic usage)
- Focus on happy path only

**Hour 4: Initialize & Test**
- Create CHANGELOG.md with baseline entry
- Test workflow: grep ARCHITECTURE → make change → update docs
- Commit everything

**After**: Refine incrementally as you work. Don't block on perfection.

---

## Template Checklist

Before conversion complete, verify you have:
- [ ] `CLAUDE.md` - Project philosophy (why, principles, gotchas)
- [ ] `ARCHITECTURE.md` - Implementation reference (greppable, rigid subsections)
- [ ] `README.md` - User manual (setup, usage, troubleshooting)
- [ ] `CHANGELOG.md` - Version history (initialized with v1.0.0)
- [ ] `/docs` directory (optional, for additional docs)
- [ ] `/plans` directory (for brainstorming)
- [ ] `/.claude/agents/` (if using agents)

Optional but recommended:
- [ ] Grep-Friendly Index in ARCHITECTURE.md
- [ ] ASCII diagrams for data flows
- [ ] Temperature rationale for AI calls
- [ ] Error handling documentation per endpoint

---

## Example: Before vs After

**Before (Shears Project)**:
```
my-project/
├── main.py (500 lines, no comments)
├── utils.py (200 lines, cryptic function names)
├── README.md (3 lines: "My cool project")
└── test.py (broken)
```

**After (Scalpel Project)**:
```
my-project/
├── main.py (500 lines, same code)
├── utils.py (200 lines, same code)
├── README.md (150 lines: setup, usage, examples)
├── CLAUDE.md (200 lines: philosophy, principles)
├── ARCHITECTURE.md (400 lines: endpoints, flows, greppable)
├── CHANGELOG.md (50 lines: version history)
├── /docs/ (devlogs, guides)
├── /plans/ (conversion notes, future plans)
└── test.py (fixed, documented)
```

**Key**: Code unchanged, but context captured. Agents can now grep instead of re-reading 500 lines.

---

## Token Efficiency Gains

**Before Conversion**:
- Agent reads 500-line main.py to understand 1 function: **~50k tokens**
- Repeated reads across conversation: **200k+ tokens**

**After Conversion**:
- Agent greps ARCHITECTURE.md for function: **~2k tokens**
- Reads 20-line subsection: **~3k tokens**
- **95% token reduction**

**Cost Savings**: $0.50/conversation (Sonnet) → $0.025/conversation

---

## Maintenance Cadence

**Every commit**:
- Update ARCHITECTURE.md if implementation changed
- Add entry to CHANGELOG.md

**Every sprint/feature**:
- Update README.md if user-facing changes
- Review CLAUDE.md - does philosophy still hold?

**Every quarter**:
- Audit docs for staleness
- Refactor ARCHITECTURE.md if > 5,000 lines
- Clean up `/plans` directory

---

*Created: 2026-01-19*
*For philosophy and principles, see SCALPEL_PHILOSOPHY.md*
