---
name: project-planner
description: "Plan and BUILD projects through structured spec-driven development with enforced interface contracts and integration tests. Generates SPEC.md, PROGRESS.md, CONTRACTS.md, and AGENTS.md (agent onboarding guide). Use when the user wants to plan a project, scope features, define new features, update the spec or roadmap, create a specification, break work into milestones, or add new capabilities to an existing project. Trigger on phrases like 'let's plan this out,' 'help me scope this,' 'let's define new features,' 'let's define features,' 'let's update the specs,' 'break this into steps,' 'create a roadmap,' 'define the requirements,' 'let's spec this,' 'I want to build X,' 'can we add a milestone for,' or any similar intent to define, plan, or extend a project's scope. If SPEC.md or PROGRESS.md exists in the workspace and the user wants to add, plan, or continue work, use this skill. THIS SKILL MUST PRODUCE WORKING CODE AND FILES, NOT JUST PLANS. All code follows thin-boundary architecture: strict interfaces and contracts at module boundaries, flexible implementation internals, mandatory integration tests."
---

# Project Planner

A structured workflow for planning AND BUILDING projects — code, documentation, infrastructure, design, or anything else. Agree on what to build through discussion, capture it in a spec, then build it sub-milestone by sub-milestone with review checkpoints after each one.

**The cardinal rule: this skill BUILDS things.** Planning exists to make building effective, not to replace it. After each sub-milestone is specified, you must write the actual code, create the actual files, and produce working output. If you finish a planning phase and haven't created any project files, something is wrong.

**The strictness rule: all code follows thin-boundary architecture.** Interfaces and data contracts are enforced at module boundaries. Implementation code inside those boundaries is flexible. Integration tests validate that boundaries hold. See "Thin-Boundary Architecture" below for language-specific guidance.

Four files drive everything:
- **SPEC.md** — What to build: overview, execution plan, milestones with sub-milestones and granular tasks
- **PROGRESS.md** — Where things stand: checklist of completed sub-milestones plus notes
- **CONTRACTS.md** — Boundary contracts: interface definitions, data schemas, and integration test requirements
- **AGENTS.md** — Agent onboarding guide: project overview, stack, setup commands, workflow rules, project structure, pointer to current state

For templates, see `references/templates.md`.

## Step 0: Assess Intent (Do This First, Every Time)

Classify the user's intent into one of three modes:

**Mode A — Full planning** (user says things like "let's plan this out," "help me scope this," "let's spec this," "create a roadmap"):
Follow the complete workflow starting at Phase 1.

**Mode B — Fast builder** (user says things like "just build it," "don't overthink it," "let's go," "keep it simple," or provides a small/clear task with urgency):
The user does not want a planning session. Briefly offer a minimal spec:

"Happy to build this right away. Even a minimal spec (overview + milestones + tasks + boundary contracts) helps if you want to pick this up later or change direction. Takes 30 seconds to review. Want one, or should I just start building?"

If they want the minimal spec, generate a stripped-down SPEC.md (overview table + execution plan + milestones with sub-milestones and tasks — no discovery, no proposal) and a minimal CONTRACTS.md. If they say skip it, build without the spec but still follow thin-boundary patterns in the code.

**Mode C — Resumption** (SPEC.md and/or PROGRESS.md exist in workspace):
Skip to "Resuming a Project" below.

---

## The Workflow

```
Discovery → Proposal → Spec + Contracts → [Build Sub-milestone → Test Boundaries → Review → Approve] per sub-milestone
```

---

## Phase 1: Discovery

Start a conversation to understand the project. Be an active participant, not a passive note-taker.

**How to run the discussion:**
- Ask questions one at a time — don't overwhelm with a list
- Suggest ideas and improvements as the user describes things
- Offer multiple options (at least two) for decisions — explain trade-offs briefly
- Dig into vague areas — "what does success look like?" and "what are the constraints?"

**Areas to explore** (adapt to the project type):
- What is the project trying to achieve and why?
- Who is it for?
- What are the key features or deliverables?
- What are the constraints (time, technology, dependencies)?
- What does "done" look like?
- Are there existing patterns, tools, or conventions to follow?
- What language/stack will this use? (needed for boundary contract decisions — see language table below)

**When to move on:**
When you have enough context to write a meaningful proposal, say so and let the user confirm.

---

## Phase 2: Proposal

A short alignment check before investing in the full spec.

**The proposal includes:**
- **Goals** — What the project aims to achieve and why
- **Core Approach** — High-level strategy, not details
- **Key Constraints** — Deadlines, technology choices, compatibility requirements
- **Boundary Strategy** — Which thin-boundary pattern fits this project (see language table)

Keep it to a few paragraphs. Get explicit approval before proceeding.

---

## Phase 3: Spec + Contract Generation

Once the proposal is approved, generate `SPEC.md` and `CONTRACTS.md`. See `references/templates.md` for templates.

**The spec MUST contain:**

1. **Overview** — Brief description plus a summary table (project name, users, user goal, developer goal)

2. **Execution Plan** — A table showing all milestones at a glance (phase number, name, one-line description)

3. **Milestones with Sub-milestones** — Each milestone (M0, M1, M2...) is broken into sub-milestones (M0.1, M0.2, etc.). Each sub-milestone has:
   - A clear name describing what it delivers
   - Granular, file-level tasks as checkboxes (`- [ ]`)
   - Boundary tasks explicitly marked: `- [ ] 🔒 Define TaskService Protocol in contracts/task_service.py`
   - Integration test tasks explicitly marked: `- [ ] 🧪 Write integration test for TaskService.create in tests/integration/`
   - A `<!-- ⛔ STOP -->` comment at the end enforcing review before continuing

4. **Architecture Notes** — Inline blockquotes within milestones where non-obvious design decisions affect the work

**The contracts file MUST contain:**

1. **Boundary Interfaces** — Every module boundary expressed as a language-appropriate interface definition
2. **Data Schemas** — Every data type that crosses a module boundary, defined with the strictest available validation
3. **Integration Test Requirements** — What must pass before each sub-milestone is considered complete

**Writing good tasks:**
Tasks must be specific enough to execute without ambiguity. Name the files, modules, endpoints, and components. Good: "Create `backend/main.py` with FastAPI app implementing `AppService` Protocol" or "Add health check endpoint (`GET /health`) returning `HealthResponse` schema". Bad: "Set up backend." Also bad: "Write the server code."

**Writing good sub-milestones:**
Each sub-milestone must be a reviewable, completable unit of work. It should be small enough to build in one focused session and large enough to be meaningful. If a sub-milestone has more than ~7 tasks, consider splitting it.

**After generating the spec:**
Present it to the user. Walk through the execution plan, milestones, and boundary contracts. Ask if anything needs adjustment. Update based on feedback.

Also create the initial `PROGRESS.md` (see template in `references/templates.md`).

Also create `AGENTS.md` — the agent onboarding guide. This file is what any agent (including you in a future session) reads first to orient themselves. It must contain:

1. **What this project is** — 1–2 sentence description pulled from the proposal
2. **Stack** — technology table (language, framework, DB, test framework, etc.)
3. **Setup** — the exact commands to install dependencies, start the dev server, and run tests
4. **Before doing any work** — instruct agents to always read SPEC.md, CONTRACTS.md, and PROGRESS.md completely before starting or resuming work
5. **Workflow rules** — one sub-milestone at a time, three-gate validation, mandatory doc updates before every commit
6. **Project structure** — annotated directory tree matching the actual layout
7. **Current state** — a pointer to PROGRESS.md for the exact current sub-milestone

Keep it factual and imperative — it's a briefing document, not a narrative. An agent arriving in a fresh session with no memory should be able to read AGENTS.md and know exactly what the project is, how to run it, and what to do next.

---

## Thin-Boundary Architecture

**Core philosophy:** Strict at the edges, flexible in the middle. Define rigid interfaces and data contracts at every module boundary. Let implementation code inside those boundaries be straightforward and pragmatic. Validate boundaries with integration tests.

This keeps code generation focused — the builder knows exactly what shape code must conform to at the boundaries, and has freedom to solve problems creatively inside.

### Language-Specific Boundary Patterns

| Language | Boundary Interface | Data Contract | Internal Style | Test Framework |
|---|---|---|---|---|
| **Python** | `typing.Protocol` + `abc.ABC` | `pydantic.BaseModel` | Simple, skip `beartype` on internals | `pytest` + `hypothesis` |
| **TypeScript** | `interface` + strict mode | `zod` schemas | Standard TS, avoid `as any` | `vitest` or `jest` |
| **Rust** | `trait` definitions | Typed structs with `serde` | Clone-heavy, use `Arc`, skip complex lifetimes | `cargo test` |
| **Go** | Structural interfaces | Typed structs | Idiomatic Go, one way to do things | `go test` + `testify` |
| **Kotlin** | `sealed interface` | `data class` + validation | Standard Kotlin | `kotlin.test` + `kotest` |
| **Scala** | `sealed trait` | `case class` | Avoid implicits, stay simple | `scalatest` |

### What Goes at a Boundary

A boundary exists at every point where one module talks to another. Concretely:
- Function signatures exposed to other modules
- Data types passed between modules (request/response shapes)
- Error types returned across module boundaries
- Configuration shapes consumed by modules
- Event/message types in pub/sub or event-driven systems

### What Stays Internal

Everything else. Local helper functions, intermediate data transformations, algorithm implementation details, internal state management. These don't need strict typing or contract validation — they need to be correct, readable, and testable.

### The Three-Gate Validation

Every sub-milestone must pass three gates before review:

1. **Static check** — Type checker passes with no errors (mypy --strict, tsc --strict, cargo check, etc.)
2. **Contract check** — All boundary data validates correctly (Pydantic models parse, Zod schemas validate, struct serialization round-trips)
3. **Integration test** — Tests that exercise the boundary from the outside pass (call the interface, check the output matches the contract)

If any gate fails, fix it before presenting the review. Note failures and resolutions in PROGRESS.md.

---

## Phase 4: Build

**This is where you write actual code and create actual files.** Work through sub-milestones one at a time.

### Context Reset

Before starting any sub-milestone, re-read `AGENTS.md`, `SPEC.md`, `PROGRESS.md`, and `CONTRACTS.md` from disk. Do this every time, without exception — the spec may have been updated, and in a new session you have no memory of prior work.

### Build the Sub-milestone

Execute the tasks for the current sub-milestone. **This means writing real code, creating real files, running real commands.** Follow the spec, reference the architecture notes and boundary contracts, and stay within the scope of the sub-milestone.

**Build order within each sub-milestone:**
1. Write boundary contracts first (interfaces, schemas) if not yet created
2. Write integration tests against those contracts (they will fail initially — this is expected)
3. Write the implementation code
4. Run the three-gate validation (static check → contract check → integration test)
5. Fix until all gates pass

After completing the tasks:
1. Mark the tasks as done in `SPEC.md` (`- [x]`)
2. Run the three-gate validation — all must pass
3. Update `PROGRESS.md` — mark the sub-milestone completed, add notes about decisions, deviations, and gate results

### Present the Review

Present the completed sub-milestone using the review template from `references/templates.md`:
- What was delivered (files created, endpoints added, etc.)
- Three-gate validation results
- Any deviations from spec or contracts
- What the next sub-milestone is

### ⛔ STOP and Wait

**After presenting the review, STOP.** Do not continue to the next sub-milestone. Wait for the user to review and approve. Three possible responses:

- **"Looks good, continue"** → Start the next sub-milestone (with a context reset)
- **Small feedback** → Revise, re-verify, re-present
- **Bigger shift** → Update SPEC.md and CONTRACTS.md to reflect the change, then revise

### Blocker Protocol

When you hit something that doesn't match the spec:

**For genuine blockers** — stop and raise the issue:
1. Explain what the problem is
2. Propose at least two options with trade-offs
3. Wait for the user to decide

**For minor deviations** — make a judgment call, note it in PROGRESS.md, mention it in the review.

**For contract changes** — if a boundary interface or data schema needs to change:
1. Update CONTRACTS.md first
2. Explain the change and why
3. Update any existing integration tests
4. Then update the implementation

---

## Phase 5: Commit

Once the user approves a sub-milestone (or a group of sub-milestones that form a logical unit), commit the work.

Read the `commit` skill if available for commit message formatting. If not, use a clear message referencing the sub-milestone: e.g., `feat(auth): implement JWT authentication (M2.3)`.

---

## Resuming a Project

When you detect an existing `SPEC.md` and `PROGRESS.md` in the workspace:

1. Read all project files completely (`AGENTS.md` if it exists, `SPEC.md`, `PROGRESS.md`, `CONTRACTS.md` if it exists)
2. Identify where the project left off (check the "Current" section in PROGRESS.md)
3. Summarize the current state to the user: "It looks like you're working on [project]. Sub-milestone M[X].[Y] is next. Want me to continue from here?"
4. If continuing, follow Phase 4 starting with a context reset

If `AGENTS.md` is missing from an existing project, generate it now by reading the other files and synthesising the setup, stack, and workflow information from them.

If the user wants to change direction, go back to discovery — but keep the existing spec as a starting point.

---

## Project Structure Convention

When creating projects, follow this directory layout to separate boundary contracts from implementation:

```
project-root/
├── AGENTS.md           # Agent onboarding guide (what, stack, setup, workflow)
├── SPEC.md
├── PROGRESS.md
├── CONTRACTS.md
├── contracts/          # Boundary interfaces + data schemas
│   ├── __init__.py     # (or index.ts, mod.rs, etc.)
│   └── [module]_service.[ext]
├── src/                # Implementation code
│   ├── __init__.py
│   └── [module]/
│       └── impl.[ext]
├── tests/
│   ├── integration/    # Tests that exercise boundaries
│   │   └── test_[module].[ext]
│   └── unit/           # Internal logic tests (optional)
│       └── test_[module]_internals.[ext]
└── ...
```

The `contracts/` directory is the source of truth for all module boundaries. Implementation code in `src/` must conform to these contracts. Integration tests in `tests/integration/` validate that conformance.

---

## Example Walkthrough

Condensed example of the full flow:

**Discovery:**
> User: "I want to build a web portal for our team."
> Claude: "Who's the primary audience?"
> User: "Internal researchers, about 10 people."
> Claude: "Got it. Two options for the stack: (A) React SPA + REST API, or (B) server-rendered HTML with FastAPI. Which fits better?"
> User: "B — keep it simple."
> Claude: "For the backend, I'd define boundary contracts with Pydantic models and Protocol interfaces. Integration tests with pytest. Sound good?"

**Proposal:**
> **Goals:** Web portal for researchers to interact with the system without CLI knowledge.
> **Approach:** FastAPI backend, vanilla HTML/CSS/JS frontend, WebSocket for real-time.
> **Constraints:** Single instance per laptop, no external dependencies.
> **Boundary Strategy:** Python with Pydantic schemas at API boundaries, Protocol interfaces for services, pytest integration tests.

User approves. →

**Spec:** Claude generates SPEC.md with overview table, execution plan, and milestones broken into sub-milestones:
- M0: Project Setup (M0.1 folder structure + contracts, M0.2 dependencies, M0.3 basic app, M0.4 service integration)
- M1: Basic UI (M1.1 HTML/CSS, M1.2 WebSocket, M1.3 message routing, M1.4 rendering)
- M2: Persistence (M2.1 parser, M2.2 list API, M2.3 sidebar, M2.4 view past items)

**Contracts:** Claude generates CONTRACTS.md defining service interfaces and data schemas.

**Building M0.1:**
- Claude creates project folders, contracts/, README, .gitignore
- Writes boundary interfaces in contracts/
- Writes initial integration test stubs
- Runs three-gate validation
- Marks tasks `[x]` in SPEC.md, updates PROGRESS.md
- Presents review with gate results
- ⛔ Stops and waits for user

---

## Adapting to Project Type

The structure stays the same; the content adapts.

**Code projects:** Tasks reference files, endpoints, modules. Contracts define API boundaries. Sub-milestones are buildable units.
**Documentation projects:** Tasks reference sections, chapters. Contracts define structure/schema for content. Sub-milestones are reviewable drafts.
**Infrastructure projects:** Tasks reference services, configs, environments. Contracts define IaC interfaces.
**Design projects:** Tasks reference deliverables — wireframes, mockups, systems. Contracts define component APIs.

Don't force code-specific language onto non-code projects. For non-code projects, CONTRACTS.md may be lighter or unnecessary — use judgment.

---

## Troubleshooting

**SPEC.md exists but PROGRESS.md is missing:**
Read SPEC.md, assess the workspace state, reconstruct PROGRESS.md from what exists. Confirm with the user.

**CONTRACTS.md is missing on resume:**
Check if contracts/ directory exists in the codebase. If boundary definitions exist in code but not in CONTRACTS.md, generate CONTRACTS.md from the existing code. If no boundary patterns exist, propose adding them.

**User returns mid-sub-milestone in a new session:**
Read all files. PROGRESS.md should tell you what's done and what's next. Summarize and pick up.

**Spec feels stale:**
If PROGRESS.md shows repeated deviations, propose a spec refresh — review remaining milestones and update them. Also review CONTRACTS.md for contract drift.

**Scope creep:**
If new milestones keep getting added, pause and have a scoping conversation. Distinguish "must have" from "future project."

**Three-gate validation fails repeatedly:**
If the same gate fails more than twice in a row, stop and reassess. The contract may be wrong, not the implementation. Propose a contract change to the user.

**AGENTS.md is missing on resume:**
Generate it from the existing SPEC.md, PROGRESS.md, and CONTRACTS.md. Synthesise the project description, stack, setup commands, and workflow rules. Confirm with the user before writing.
