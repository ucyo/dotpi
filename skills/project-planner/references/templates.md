# Templates

Reference templates for SPEC.md, PROGRESS.md, and CONTRACTS.md. Use these as starting points and adapt to the project. The key principles: specs must be granular enough to execute without ambiguity, sub-milestones enforce incremental review, boundary contracts are defined before implementation, and progress tracking stays lightweight.

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
| **Language/Stack**  | [primary language and key frameworks]             |
| **Boundary Strategy** | [e.g., Pydantic + Protocol + pytest]           |

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
- [ ] 🔒 Define boundary interface in `contracts/[module]_service.[ext]`
- [ ] 🔒 Define data schemas for [module] in `contracts/[module]_service.[ext]`
- [ ] 🧪 Write integration test in `tests/integration/test_[module].[ext]`
- [ ] Implement [specific task] in `src/[module]/impl.[ext]`
- [ ] Another specific task with enough detail to execute without guessing
<!-- ⛔ STOP: Complete this sub-milestone, run three-gate validation, then wait for user review before continuing -->

#### M0.2 - [Sub-milestone name]
- [ ] Task with concrete deliverable
- [ ] Another task
<!-- ⛔ STOP: Complete this sub-milestone, run three-gate validation, then wait for user review before continuing -->

---

### M1 - [Milestone Name]

> **Architecture Note:** [Include architecture decisions inline where they affect the milestone. E.g., "Sessions stored in JSONL format. No database needed."]

#### M1.1 - [Sub-milestone name]
- [ ] 🔒 Define boundary interface for [new module]
- [ ] 🧪 Write integration test for [new boundary]
- [ ] Granular implementation task
- [ ] Another task
<!-- ⛔ STOP: Complete this sub-milestone, run three-gate validation, then wait for user review before continuing -->

#### M1.2 - [Sub-milestone name]
- [ ] Task
- [ ] Task
<!-- ⛔ STOP: Complete this sub-milestone, run three-gate validation, then wait for user review before continuing -->

---
```

### Task markers

- `🔒` — Boundary contract task (interface or schema definition in `contracts/`)
- `🧪` — Integration test task (test that exercises a boundary in `tests/integration/`)
- Unmarked — Implementation or other task

Every sub-milestone that introduces a new module boundary should have at least one 🔒 task and one 🧪 task. Sub-milestones that only modify internals within an existing boundary may skip these.

### Guidelines for writing good sub-milestones

- **Break milestones into sub-milestones (M0.1, M0.2, etc.)** — each sub-milestone is a reviewable unit of work that can be completed and verified independently.
- **Tasks must be file-level specific** — name the files, modules, endpoints, and components being created or modified. Good: "Create `backend/session_parser.py` module implementing `SessionParser` Protocol". Bad: "Add session parsing."
- **Use checkbox format** — `- [ ]` for pending, `- [x]` for done. This makes progress scannable.
- **Add Architecture Notes** — when a milestone involves a non-obvious design choice, add a blockquote note at the top of the milestone explaining the decision so the builder doesn't have to look elsewhere.
- **Every sub-milestone ends with a STOP rule** — the `<!-- ⛔ STOP -->` comment enforces that the agent pauses for user review after each sub-milestone.
- **Each sub-milestone should be completable in one focused session** — if a sub-milestone feels too big, split it further.
- **Contracts before implementation** — 🔒 tasks should appear before implementation tasks in the task list. The build order is: contracts → tests → implementation.

## CONTRACTS.md Template

```markdown
# [Project Name] - Boundary Contracts

> This file defines all module boundary interfaces and data schemas.
> Implementation code in `src/` must conform to these contracts.
> Integration tests in `tests/integration/` validate conformance.

## Language & Tooling

| Aspect | Choice |
|---|---|
| **Language** | [e.g., Python 3.12] |
| **Interface enforcement** | [e.g., typing.Protocol + mypy --strict] |
| **Data validation** | [e.g., Pydantic v2] |
| **Test framework** | [e.g., pytest + hypothesis] |
| **Static checker** | [e.g., mypy --strict or pyright] |

## Module Boundaries

### [ModuleName] Service

**File:** `contracts/[module]_service.[ext]`

**Interface:**
```[language]
# Paste the actual interface/Protocol/trait definition here
```

**Data schemas:**
```[language]
# Paste the actual schema/model definitions here
```

**Integration test requirements:**
- [ ] Can create a [thing] with valid input
- [ ] Rejects invalid input with appropriate error
- [ ] Can retrieve a [thing] by ID
- [ ] Returns None/null for missing IDs
- [ ] List returns all [things] in expected order

---

### [AnotherModule] Service

**File:** `contracts/[another_module]_service.[ext]`

(Same structure as above)

---

## Cross-cutting Contracts

### Error Types
```[language]
# Shared error types used across module boundaries
```

### Configuration
```[language]
# Configuration schema consumed by multiple modules
```

## Three-Gate Checklist

Before presenting any sub-milestone for review, all three gates must pass:

- [ ] **Static check:** `[command]` exits with 0 errors
- [ ] **Contract check:** All boundary data validates (schemas parse, round-trip correctly)
- [ ] **Integration test:** `[test command]` — all boundary tests pass
```

### Guidelines for writing good contracts

- **One file per module boundary** in `contracts/` — keep them focused and independent.
- **Interfaces define shape, not implementation** — a Protocol/interface/trait says what methods exist and what types they accept/return. It never dictates how they work internally.
- **Data schemas are the strictest layer** — use every validation feature available (min/max lengths, regex patterns, enums for constrained values, required vs optional fields).
- **Error types cross boundaries** — define a shared error enum/union so callers know what failures are possible.
- **Update CONTRACTS.md when contracts change** — this file is the source of truth. If the code diverges from CONTRACTS.md, the code is wrong.

## PROGRESS.md Template — Initial State

```markdown
# Progress

## Current: M0 - [Milestone Name]

(No sub-milestones completed yet)

## Notes
- Boundary strategy: [language + tools chosen]
- [Key architecture decisions, dependencies, or context worth remembering]
```

## PROGRESS.md — With Progress

```markdown
# Progress

## Completed: M0 - Project Setup

- [x] M0.1: completed - project structure + boundary contracts defined
  - Gates: ✅ static | ✅ contract | ✅ integration
- [x] M0.2: completed - Python environment set up
  - Gates: ✅ static | ✅ contract | ✅ integration
- [x] M0.3: completed - process management setup
  - Gates: ✅ static | ✅ contract | ⚠️ integration (1 skipped — no server yet)
- [x] M0.4: completed - basic app running
  - Gates: ✅ static | ✅ contract | ✅ integration
- [x] M0.5: completed - external service communication verified
  - Gates: ✅ static | ✅ contract | ✅ integration

## Completed: M1 - Basic Chat

- [x] M1.1: completed - Frontend HTML/CSS structure
  - Gates: ✅ static | ✅ contract | ✅ integration
- [x] M1.2: completed - WebSocket connection
  - Gates: ✅ static | ✅ contract | ✅ integration
- [x] M1.3: completed - Backend message routing
  - Gates: ✅ static | ✅ contract | ✅ integration

## Current: M2 - Session Persistence

- [x] M2.1: completed - Session file parser (contracts/session_service.py + src/session/impl.py)
  - Gates: ✅ static | ✅ contract | ✅ integration
- [x] M2.2: completed - Session list API (GET /api/sessions)
  - Gates: ✅ static | ✅ contract | ✅ integration
- [ ] M2.3: not started - Sidebar session list

## Contract Changes
- M1.2: Added `reconnect_delay` field to WebSocketConfig (default 3s)
- M2.1: Changed SessionParser.parse return type from list[Message] to Iterator[Message]

## Notes
- Boundary strategy: Python + Pydantic + Protocol + mypy --strict + pytest
- Architecture changed: using JSONL sessions as single source of truth instead of database
- Dependencies: fastapi, uvicorn, websockets, pydantic-settings
- Dev deps: pytest, pytest-asyncio, httpx, ruff, mypy
- WebSocket handles streaming with reconnection logic
```

### Guidelines for progress tracking

- **Keep it lightweight** — one line per sub-milestone with status and brief description. No verbose prose.
- **Include gate results** — show which of the three gates passed, failed, or were skipped for each sub-milestone.
- **Group by milestone** — use "Completed:", "Current:", and optionally "Upcoming:" sections.
- **Contract Changes section** — track when boundary contracts are modified after initial creation. This is the audit trail for contract drift.
- **Notes section at the bottom** — capture architecture decisions, dependency lists, deviations, and important context. This is the handoff document for resuming in a new session.
- **Update after each sub-milestone** — mark the sub-milestone done, record gate results, and note any decisions or deviations.

## Structured Review Template

Use this when presenting a completed sub-milestone to the user for review:

```markdown
### M[X].[Y] Review: [Sub-milestone Name]

**Delivered:**
- What was actually built/created
- Each file or component
- ...

**Boundary contracts:**
- 🔒 [New/updated contract]: `contracts/[file]` — [brief description]
- (or "No new contracts — working within existing boundaries")

**Three-gate validation:**
- ✅ Static check: `[command]` — [result summary]
- ✅ Contract check: [what was validated]
- ✅ Integration test: `[command]` — [X passed, Y skipped]
- (use ⚠️ for partial passes with explanation, ❌ for failures that were resolved)

**Deviations from spec:** None / [describe if any]

**Contract changes:** None / [describe if boundary definitions were modified]

**Next up:** M[X].[Y+1] - [Next sub-milestone name]
```
