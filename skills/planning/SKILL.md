---
name: planning
description: "Plan software projects before writing code. Use this skill when the user wants to build a feature, implement a change, create a new module, refactor code, or work on any multi-step development task. Also use when the user asks to plan, design, spec out, break down, or think through a coding task. Triggers on requests involving building, implementing, creating, redesigning, or modifying software — even when the user doesn't explicitly ask for a plan."
---

# Planning Software Projects

Help turn ideas into structured, reviewable plans before any code is written. Explore the codebase, understand constraints, design an approach, and break work into milestones with concrete tasks.

**Core principle:** The human reviews and approves before code gets committed. No surprises.

Announce at start: "I'm using the planning skill to structure this work."

## When This Skill Activates

- User asks to build, implement, create, or add something
- User asks to refactor, redesign, or restructure code
- User says "plan", "spec", "break down", or "think through"
- Any coding request that touches multiple files or components

For single-line fixes, typos, or trivial changes — skip this skill and just do the work.

## The Three Files

Every plan produces three files in the project. Ask the user where they want them (suggest `docs/plans/` as default). All three are committed to git so they survive sessions and are reviewable by teammates.

### 1. Spec (`spec.md`)

The design document. What we're building and why.

```markdown
# [Feature Name] — Spec

## Goal
One paragraph: what does this achieve and why does it matter?

## Current State
What exists today. Relevant code, patterns, constraints discovered during exploration.

## Approach
The chosen technical approach. What was considered, why this was picked.

## Scope
### In scope
- Concrete deliverables

### Out of scope
- What we're explicitly not doing (and why)

## Architecture Decisions
Key decisions with brief rationale. Only include decisions where alternatives exist.

## Risks & Assumptions
What could go wrong. What we're assuming is true.

## Acceptance Criteria
How we know it's done. Concrete, verifiable conditions.
```

### 2. Tasks (`tasks.md`)

The work breakdown, grouped by milestones. Each milestone is a logical unit of work that can be reviewed and committed independently.

```markdown
# [Feature Name] — Tasks

## Milestone 1: [Name — what's deliverable after this milestone]

### Files
- Create: `path/to/new/file.ts`
- Modify: `path/to/existing/file.ts`
- Test: `tests/path/to/test.ts`

### Tasks
- [ ] 1.1 — [Concrete action, including what file and what change]
- [ ] 1.2 — [Next action]
- [ ] 1.3 — Write tests for [specific behavior]
- [ ] 1.4 — Run tests, verify passing
- [ ] 1.5 — **Review checkpoint** — present changes to user before commit

## Milestone 2: [Name]

### Files
- ...

### Tasks
- [ ] 2.1 — ...
- [ ] 2.2 — ...
- [ ] 2.3 — **Review checkpoint**

## Final: Verification
- [ ] Run full test suite
- [ ] Verify acceptance criteria from spec
- [ ] **Final review checkpoint** — present complete work to user
```

Rules for tasks:
- Each task is a single action (5–15 minutes of work)
- Every milestone ends with a **review checkpoint** — the user sees changes before commit
- Tasks include exact file paths
- Testing is inline (not a separate milestone), paired with the code it validates
- Milestones are ordered so each one produces working, testable software

### 3. Progress (`progress.md`)

Living document updated during execution. Tracks state, decisions, and blockers.

```markdown
# [Feature Name] — Progress

## Status: [planning | in-progress | blocked | complete]

## Current Milestone
Milestone 1: [Name]

## Completed
- [x] 1.1 — [What was done] — [timestamp or commit ref]
- [x] 1.2 — [What was done]

## In Progress
- [ ] 1.3 — [What's being worked on]

## Blocked
(Nothing currently)

## Decisions Made During Implementation
- [Decision]: [Rationale] — [date]

## Deviations From Plan
- [What changed vs. the original plan and why]

## Notes
- [Anything discovered during implementation that's worth recording]
```

## The Process

### Phase 1: Explore

Before asking questions, understand what exists:

1. Read relevant source files, tests, configs
2. Check recent git history for context on the area being changed
3. Identify patterns, conventions, dependencies
4. Note constraints the user may not have mentioned

Use subagents for exploration when the codebase is large — keeps context clean.

### Phase 2: Clarify

Ask questions to fill gaps. One question at a time.

- Prefer multiple-choice when possible ("Do you want A, B, or C?")
- Focus on: purpose, constraints, success criteria, edge cases
- If the scope is too large for a single plan, say so — suggest breaking into sub-projects
- Stop asking when you have enough to propose approaches (don't interrogate)

### Phase 3: Design

1. Propose 2–3 approaches with tradeoffs and your recommendation
2. After user picks (or suggests something else), present the design in sections
3. Get user approval on the approach before writing anything

### Phase 4: Write the Plan

Create the three files:

1. Write `spec.md` — present to user for review
2. Write `tasks.md` — present to user for review
3. Create `progress.md` with initial state
4. Commit all three files

### Phase 5: Execute

Follow the task list milestone by milestone:

1. Mark tasks in progress in `progress.md` as you work
2. Check off completed tasks in `tasks.md`
3. At every **review checkpoint**: stop, present what changed (files modified, summary of changes, test results), and wait for user approval before committing
4. Record any deviations or decisions in `progress.md`
5. If blocked, stop and ask — don't guess

### Review Checkpoints

At every review checkpoint (end of each milestone):

- Show which files changed and a brief summary of each change
- Show test results
- Ask: "Ready to commit?", "Show me the diff?", or "I want to adjust something"
- Only commit after user approval
- Update `progress.md` with commit reference

## Scaling by Complexity

The three files are always created, but their depth scales:

**Small changes** (add endpoint, new component, fix with multi-file impact):
- Spec: 10–20 lines. Brief goal, approach, acceptance criteria.
- Tasks: 1–2 milestones, 5–10 tasks total.
- Progress: Minimal, mostly just status tracking.

**Medium changes** (new feature, new module, significant refactor):
- Spec: Full sections. Architecture decisions, scope boundaries, risks.
- Tasks: 3–5 milestones, 15–30 tasks.
- Progress: Active tracking with decisions and deviations.

**Large changes** (new service, major redesign, multi-day work):
- Spec: Thorough. Multiple architecture decisions, risk analysis, interface definitions.
- Tasks: 5+ milestones, 30+ tasks. Consider splitting into sub-plans.
- Progress: Detailed log, may span multiple sessions.

## Working in Existing Codebases

- Explore current structure before proposing changes
- Follow established patterns — don't impose new conventions
- Focus the plan on what's changing (delta-oriented), not restating what exists
- If existing code in the area has problems that affect the work, include targeted improvements in the plan (the way a good developer improves code they touch)
- Don't propose unrelated refactoring

## Key Principles

- **Human reviews before commit** — the user is the final gatekeeper, always
- **One question at a time** — don't overwhelm during clarification
- **Propose alternatives** — always present options with tradeoffs, not just one path
- **Exact file paths** — never be vague about where changes go
- **Test alongside code** — testing is part of each milestone, not an afterthought
- **Plan is a living document** — update tasks and progress as reality diverges from the plan
- **Stop when blocked** — ask for help rather than guessing
- **YAGNI** — don't plan features that aren't needed yet
- **DRY** — if the plan has you writing the same thing twice, redesign
