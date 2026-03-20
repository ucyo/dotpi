# Templates

Reference templates for SPEC.md and PROGRESS.md. Use these as starting points and adapt to the project. The key principles: specs must be granular enough to execute without ambiguity, sub-milestones enforce incremental review, and progress tracking stays lightweight.

## SPEC.md Template

```markdown
# [Project Name] - Specification

## Overview

Brief description of what the project is and who it serves.

| Aspect             | Description                                       |
| ------------------ | ------------------------------------------------- |
| **Project Name**   | [name]                                            |
| **Users**          | [who uses it, scale]                              |
| **User Goal**      | [what users want to accomplish]                   |
| **Developer Goal** | [what the developer/team wants to achieve]        |

## Execution Plan

| Phase | Milestone                 | Description                                       |
| ----- | ------------------------- | ------------------------------------------------- |
| M0    | [Name]                    | [Brief description]                               |
| M1    | [Name]                    | [Brief description]                               |
| M2    | [Name]                    | [Brief description]                               |
| M3    | [Name]                    | [Brief description]                               |

## Milestones

> **⛔ STOP RULE:** Complete ONE sub-milestone, then STOP and wait for user review. Do not continue to the next sub-milestone without approval.

### M0 - [Milestone Name]

#### M0.1 - [Sub-milestone name]
- [ ] Specific, file-level task (e.g., "Create `backend/main.py` with FastAPI app")
- [ ] Another specific task with enough detail to execute without guessing
- [ ] Include file paths, module names, endpoint paths where relevant
<!-- ⛔ STOP: Complete this sub-milestone, run tests, then wait for user review before continuing -->

#### M0.2 - [Sub-milestone name]
- [ ] Task with concrete deliverable
- [ ] Another task
<!-- ⛔ STOP: Complete this sub-milestone, run tests, then wait for user review before continuing -->

---

### M1 - [Milestone Name]

> **Architecture Note:** [Include architecture decisions inline where they affect the milestone. E.g., "Sessions stored in JSONL format. No database needed."]

#### M1.1 - [Sub-milestone name]
- [ ] Granular task
- [ ] Another task
<!-- ⛔ STOP: Complete this sub-milestone, run tests, then wait for user review before continuing -->

#### M1.2 - [Sub-milestone name]
- [ ] Task
- [ ] Task
<!-- ⛔ STOP: Complete this sub-milestone, run tests, then wait for user review before continuing -->

---
```

### Guidelines for writing good sub-milestones

- **Break milestones into sub-milestones (M0.1, M0.2, etc.)** — each sub-milestone is a reviewable unit of work that can be completed and verified independently.
- **Tasks must be file-level specific** — name the files, modules, endpoints, and components being created or modified. Good: "Create `backend/session_parser.py` module". Bad: "Add session parsing."
- **Use checkbox format** — `- [ ]` for pending, `- [x]` for done. This makes progress scannable.
- **Add Architecture Notes** — when a milestone involves a non-obvious design choice, add a blockquote note at the top of the milestone explaining the decision so the builder doesn't have to look elsewhere.
- **Every sub-milestone ends with a STOP rule** — the `<!-- ⛔ STOP -->` comment enforces that the agent pauses for user review after each sub-milestone.
- **Each sub-milestone should be completable in one focused session** — if a sub-milestone feels too big, split it further.

## PROGRESS.md Template — Initial State

```markdown
# Progress

## Current: M0 - [Milestone Name]

(No sub-milestones completed yet)

## Notes
- [Key architecture decisions, dependencies, or context worth remembering]
```

## PROGRESS.md — With Progress

```markdown
# Progress

## Completed: M0 - Project Setup

- [x] M0.1: completed - project structure initialized
- [x] M0.2: completed - Python environment set up
- [x] M0.3: completed - process management setup
- [x] M0.4: completed - basic app running
- [x] M0.5: completed - external service communication verified

## Completed: M1 - Basic Chat

- [x] M1.1: completed - Frontend HTML/CSS structure
- [x] M1.2: completed - WebSocket connection
- [x] M1.3: completed - Backend message routing

## Current: M2 - Session Persistence

- [x] M2.1: completed - Session file parser (backend/session_parser.py)
- [x] M2.2: completed - Session list API (GET /api/sessions)
- [ ] M2.3: not started - Sidebar session list

## Notes
- Architecture changed: using JSONL sessions as single source of truth instead of database
- Dependencies: fastapi, uvicorn, websockets, pydantic-settings
- Dev deps: pytest, pytest-asyncio, httpx, ruff
- WebSocket handles streaming with reconnection logic
```

### Guidelines for progress tracking

- **Keep it lightweight** — one line per sub-milestone with status and brief description. No verbose prose.
- **Group by milestone** — use "Completed:", "Current:", and optionally "Upcoming:" sections.
- **Notes section at the bottom** — capture architecture decisions, dependency lists, deviations, and important context. This is the handoff document for resuming in a new session.
- **Update after each sub-milestone** — mark the sub-milestone done and note any decisions or deviations in the Notes section.

## Structured Review Template

Use this when presenting a completed sub-milestone to the user for review:

```markdown
### M[X].[Y] Review: [Sub-milestone Name]

**Delivered:**
- What was actually built/created
- Each file or component
- ...

**Verification:**
- ✅ Thing that works
- ✅ Another thing verified
- ⚠️ Thing with caveats (explain)

**Deviations from spec:** None / [describe if any]

**Next up:** M[X].[Y+1] - [Next sub-milestone name]
```
