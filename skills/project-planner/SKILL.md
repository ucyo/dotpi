---
name: project-planner
description: "Plan and BUILD projects through structured spec-driven development. Use when the user wants to plan a project, scope features, create a specification, build a roadmap, or break work into milestones. Trigger on phrases like 'let's plan this out,' 'help me scope this,' 'break this into steps,' 'create a roadmap,' 'define the requirements,' 'let's spec this,' or 'I want to build X.' If SPEC.md or PROGRESS.md exists in the workspace, use this skill to continue the project. THIS SKILL MUST PRODUCE WORKING CODE AND FILES, NOT JUST PLANS."
---

# Project Planner

A structured workflow for planning AND BUILDING projects — code, documentation, infrastructure, design, or anything else. Agree on what to build through discussion, capture it in a spec, then build it sub-milestone by sub-milestone with review checkpoints after each one.

**The cardinal rule: this skill BUILDS things.** Planning exists to make building effective, not to replace it. After each sub-milestone is specified, you must write the actual code, create the actual files, and produce working output. If you finish a planning phase and haven't created any project files, something is wrong.

Two files drive everything:
- **SPEC.md** — What to build: overview, execution plan, milestones with sub-milestones and granular tasks
- **PROGRESS.md** — Where things stand: checklist of completed sub-milestones plus notes

For templates, see `references/templates.md`.

## Step 0: Assess Intent (Do This First, Every Time)

Classify the user's intent into one of three modes:

**Mode A — Full planning** (user says things like "let's plan this out," "help me scope this," "let's spec this," "create a roadmap"):
Follow the complete workflow starting at Phase 1.

**Mode B — Fast builder** (user says things like "just build it," "don't overthink it," "let's go," "keep it simple," or provides a small/clear task with urgency):
The user does not want a planning session. Briefly offer a minimal spec:

"Happy to build this right away. Even a minimal spec (overview + milestones + tasks) helps if you want to pick this up later or change direction. Takes 30 seconds to review. Want one, or should I just start building?"

If they want the minimal spec, generate a stripped-down SPEC.md (overview table + execution plan + milestones with sub-milestones and tasks — no discovery, no proposal). If they say skip it, build without the spec.

**Mode C — Resumption** (SPEC.md and/or PROGRESS.md exist in workspace):
Skip to "Resuming a Project" below.

---

## The Workflow

```
Discovery → Proposal → Spec → [Build Sub-milestone → Review → Approve] per sub-milestone
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

**When to move on:**
When you have enough context to write a meaningful proposal, say so and let the user confirm.

---

## Phase 2: Proposal

A short alignment check before investing in the full spec.

**The proposal includes:**
- **Goals** — What the project aims to achieve and why
- **Core Approach** — High-level strategy, not details
- **Key Constraints** — Deadlines, technology choices, compatibility requirements

Keep it to a few paragraphs. Get explicit approval before proceeding.

---

## Phase 3: Spec Generation

Once the proposal is approved, generate `SPEC.md`. See `references/templates.md` for the full template.

**The spec MUST contain:**

1. **Overview** — Brief description plus a summary table (project name, users, user goal, developer goal)

2. **Execution Plan** — A table showing all milestones at a glance (phase number, name, one-line description)

3. **Milestones with Sub-milestones** — Each milestone (M0, M1, M2...) is broken into sub-milestones (M0.1, M0.2, etc.). Each sub-milestone has:
   - A clear name describing what it delivers
   - Granular, file-level tasks as checkboxes (`- [ ]`)
   - A `<!-- ⛔ STOP -->` comment at the end enforcing review before continuing

4. **Architecture Notes** — Inline blockquotes within milestones where non-obvious design decisions affect the work

**Writing good tasks:**
Tasks must be specific enough to execute without ambiguity. Name the files, modules, endpoints, and components. Good: "Create `backend/main.py` with FastAPI app" or "Add health check endpoint (`GET /health`)". Bad: "Set up backend." Also bad: "Write the server code."

**Writing good sub-milestones:**
Each sub-milestone must be a reviewable, completable unit of work. It should be small enough to build in one focused session and large enough to be meaningful. If a sub-milestone has more than ~7 tasks, consider splitting it.

**After generating the spec:**
Present it to the user. Walk through the execution plan and milestones. Ask if anything needs adjustment. Update based on feedback.

Also create the initial `PROGRESS.md` (see template in `references/templates.md`).

---

## Phase 4: Build

**This is where you write actual code and create actual files.** Work through sub-milestones one at a time.

### Context Reset

Before starting any sub-milestone, re-read both `SPEC.md` and `PROGRESS.md` from disk. Do this every time, without exception — the spec may have been updated, and in a new session you have no memory of prior work.

### Build the Sub-milestone

Execute the tasks for the current sub-milestone. **This means writing real code, creating real files, running real commands.** Follow the spec, reference the architecture notes, and stay within the scope of the sub-milestone.

After completing the tasks:
1. Mark the tasks as done in `SPEC.md` (`- [x]`)
2. Verify the work — run tests, check the output, confirm things work
3. Update `PROGRESS.md` — mark the sub-milestone completed, add any notes about decisions or deviations

### Present the Review

Present the completed sub-milestone using the review template from `references/templates.md`:
- What was delivered (files created, endpoints added, etc.)
- Verification results
- Any deviations from spec
- What the next sub-milestone is

### ⛔ STOP and Wait

**After presenting the review, STOP.** Do not continue to the next sub-milestone. Wait for the user to review and approve. Three possible responses:

- **"Looks good, continue"** → Start the next sub-milestone (with a context reset)
- **Small feedback** → Revise, re-verify, re-present
- **Bigger shift** → Update SPEC.md to reflect the change, then revise

### Blocker Protocol

When you hit something that doesn't match the spec:

**For genuine blockers** — stop and raise the issue:
1. Explain what the problem is
2. Propose at least two options with trade-offs
3. Wait for the user to decide

**For minor deviations** — make a judgment call, note it in PROGRESS.md, mention it in the review.

---

## Phase 5: Commit

Once the user approves a sub-milestone (or a group of sub-milestones that form a logical unit), commit the work.

Read the `commit` skill if available for commit message formatting. If not, use a clear message referencing the sub-milestone: e.g., `feat(auth): implement JWT authentication (M2.3)`.

---

## Resuming a Project

When you detect an existing `SPEC.md` and `PROGRESS.md` in the workspace:

1. Read both files completely
2. Identify where the project left off (check the "Current" section in PROGRESS.md)
3. Summarize the current state to the user: "It looks like you're working on [project]. Sub-milestone M[X].[Y] is next. Want me to continue from here?"
4. If continuing, follow Phase 4 starting with a context reset

If the user wants to change direction, go back to discovery — but keep the existing spec as a starting point.

---

## Example Walkthrough

Condensed example of the full flow:

**Discovery:**
> User: "I want to build a web portal for our team."
> Claude: "Who's the primary audience?"
> User: "Internal researchers, about 10 people."
> Claude: "Got it. Two options for the stack: (A) React SPA + REST API, or (B) server-rendered HTML with FastAPI. Which fits better?"
> User: "B — keep it simple."

**Proposal:**
> **Goals:** Web portal for researchers to interact with the system without CLI knowledge.
> **Approach:** FastAPI backend, vanilla HTML/CSS/JS frontend, WebSocket for real-time.
> **Constraints:** Single instance per laptop, no external dependencies.

User approves. →

**Spec:** Claude generates SPEC.md with overview table, execution plan, and milestones broken into sub-milestones:
- M0: Project Setup (M0.1 folder structure, M0.2 dependencies, M0.3 basic app, M0.4 service integration)
- M1: Basic UI (M1.1 HTML/CSS, M1.2 WebSocket, M1.3 message routing, M1.4 rendering)
- M2: Persistence (M2.1 parser, M2.2 list API, M2.3 sidebar, M2.4 view past items)

**Building M0.1:**
- Claude creates project folders, README, .gitignore, initializes git
- Marks tasks `[x]` in SPEC.md, updates PROGRESS.md
- Presents review: "Created project structure with backend/, frontend/, README.md, .gitignore. Next up: M0.2 - Set up Python environment."
- ⛔ Stops and waits for user

**User:** "Looks good, continue" → Claude starts M0.2 with context reset, builds the next sub-milestone.

---

## Adapting to Project Type

The structure stays the same; the content adapts.

**Code projects:** Tasks reference files, endpoints, modules. Sub-milestones are buildable units.
**Documentation projects:** Tasks reference sections, chapters. Sub-milestones are reviewable drafts.
**Infrastructure projects:** Tasks reference services, configs, environments.
**Design projects:** Tasks reference deliverables — wireframes, mockups, systems.

Don't force code-specific language onto non-code projects.

---

## Troubleshooting

**SPEC.md exists but PROGRESS.md is missing:**
Read SPEC.md, assess the workspace state, reconstruct PROGRESS.md from what exists. Confirm with the user.

**User returns mid-sub-milestone in a new session:**
Read both files. PROGRESS.md should tell you what's done and what's next. Summarize and pick up.

**Spec feels stale:**
If PROGRESS.md shows repeated deviations, propose a spec refresh — review remaining milestones and update them.

**Scope creep:**
If new milestones keep getting added, pause and have a scoping conversation. Distinguish "must have" from "future project."
