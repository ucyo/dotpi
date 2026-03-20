---
name: project-planner
description: "Plan and execute projects through structured spec-driven development. Use when the user wants to plan a project, scope features, create a specification, build a roadmap, or break work into milestones. Trigger on phrases like 'let's plan this out,' 'help me scope this,' 'break this into steps,' 'create a roadmap,' 'define the requirements,' 'let's spec this,' or 'I want to build X.' If spec.md or progress.md exists in the workspace, use this skill to continue the project."
---

# Project Planner

A structured workflow for planning and executing projects of any kind — code, documentation, infrastructure, design, or anything else. Agree on what to build through discussion, capture it in a spec, then execute milestone by milestone with review checkpoints.

This keeps you and the user aligned throughout. Without a spec, projects drift — requirements get forgotten, decisions get revisited, and work gets thrown away. The spec is the contract; progress tracking is the memory.

## Step 0: Assess Intent (Do This First, Every Time)

Before doing anything else, classify the user's intent into one of three modes. This determines the entire flow — get it right.

**Mode A — Full planning** (user says things like "let's plan this out," "help me scope this," "let's spec this," "create a roadmap"):
Follow the complete workflow below starting at Phase 1.

**Mode B — Fast builder** (user says things like "just build it," "don't overthink it," "let's go," "keep it simple," or provides a small/clear task with urgency):
The user does not want a planning session. Do NOT skip straight to building either. Instead, respond with something like:

"Happy to build this right away. One quick thought — even a minimal spec (just goals, milestones, and done-criteria) helps if you want to pick this up later or change direction. Takes 30 seconds to review. Want one, or should I just start building?"

Then follow the user's answer. If they want the minimal spec, generate a stripped-down `spec.md` (goals + milestones + acceptance criteria only — no discovery, no proposal, no design decisions). If they say skip it, build without the spec. Either way, respect their pace. If this is a non-interactive session (no way to ask and wait), mention the spec option briefly at the top of your response, then proceed to build.

This step exists because a user who says "just build it" and gets a discovery interview will lose trust in the skill. But silently skipping the spec means losing resumability and alignment for free. The one-sentence offer is the right balance.

**Mode C — Resumption** (spec.md and/or progress.md exist in workspace):
Skip to "Resuming a Project" below.

---

## The Workflow

```
Discovery → Proposal → Spec → [Execute → Verify → Review → Commit] per milestone
```

Two files drive everything:
- **spec.md** — What to build, why, and how (the plan)
- **progress.md** — Where things stand, what's been done, what's next (the memory)

For templates of both files and the review format, see `references/templates.md`.

---

## Phase 1: Discovery

Start a conversation to understand the project. Be an active participant, not a passive note-taker. Take your time here — a thorough discovery prevents wasted work downstream.

**How to run the discussion:**
- Ask questions one at a time — don't overwhelm with a list of ten things
- When the user describes something, suggest ideas and improvements
- Always offer multiple options (at least two) for any decision or approach — explain trade-offs briefly so the user can make an informed choice
- Dig into areas that feel vague — "what does success look like?" and "what are the constraints?" are always good questions
- Draw on relevant knowledge to suggest things the user might not have considered

**Areas to explore** (adapt to the project type):
- What is the project trying to achieve and why?
- Who is it for?
- What are the key features or deliverables?
- What are the constraints (time, technology, dependencies)?
- What does "done" look like?
- Are there existing patterns, tools, or conventions to follow?

**When to move on:**
When you feel you have enough context to write a meaningful proposal, say so: "I think I have a good understanding of what you're after. Shall I draft a proposal so we can check alignment before I write the full spec?" Let the user confirm or add more context.

---

## Phase 2: Proposal

Before investing in a full spec, write a short proposal to confirm alignment. This catches misunderstandings early and cheaply — a wrong direction caught here saves hours of wasted execution.

**The proposal includes:**

**Goals** — State your understanding of what the project aims to achieve and why it matters. This is the most important alignment check — if the goals are wrong, everything downstream is wrong.

**Core Approach** — How you plan to tackle it — the high-level strategy, not the details. Enough for the user to say "yes, that's the direction" or "no, you've missed the point."

**Key Constraints** — Anything that limits or shapes the solution — deadlines, technology choices, compatibility requirements, budget, team size.

Keep it short — a few paragraphs, not a document. Present it conversationally and ask for explicit approval before proceeding to the full spec.

---

## Phase 3: Spec Generation

Once the proposal is approved, generate `spec.md`. This is the source of truth for the entire project. See `references/templates.md` for the full template.

The spec contains:
- **Goals** — Why the project exists and what success looks like
- **Approach** — The high-level strategy
- **Design Decisions** — What was chosen, what alternatives existed, and why (as a list, not a table — easier to maintain)
- **Milestones** — Each with a description, tasks, and acceptance criteria

**Writing good tasks:**
Tasks should be specific enough to execute without ambiguity, but not so granular that they prescribe exact filenames. Good: "Create authentication middleware that validates JWT tokens and extracts user roles." Bad: "Add auth." Also bad: "Write src/middleware/auth.ts line by line."

**Writing good acceptance criteria:**
Each criterion should be verifiable — you or the user can look at the result and definitively say yes or no. Good: "Login endpoint returns a token on valid credentials and 401 on invalid." Bad: "Authentication works properly." For non-code projects, lean on specific deliverables: "README covers installation, configuration, and usage with examples."

**After generating the spec:**
Present it to the user for review. Walk through each milestone and its acceptance criteria. Ask if anything needs adjustment — milestones reordered, tasks added or removed, criteria refined. Don't rush this — the spec is the foundation everything else builds on. Update based on feedback before proceeding.

Also create the initial `progress.md` (see template in `references/templates.md`).

---

## Phase 4: Execution

Work through milestones one at a time.

### Context Reset

Before starting any milestone (including the first one), re-read both `spec.md` and `progress.md` from disk. This is essential because:
- The spec may have been updated during a previous review
- In a new session, you have no memory of prior work
- Even within a session, conversation context degrades over time

Do this every time, without exception.

### Do the Work

Execute the tasks for the current milestone. Follow the spec, reference the design decisions, and stay within the scope of the milestone.

### Blocker Protocol

When you hit something that doesn't match the spec — a technical impossibility, a contradiction between requirements, unexpected complexity, or a decision that needs user input:

**For genuine blockers** — stop and raise the issue immediately:
1. Explain clearly what the problem is
2. Propose at least two options with trade-offs
3. Note the impact on the current milestone and any downstream effects
4. Wait for the user to decide before continuing

**For minor deviations** — small adjustments where the intent is clear but the specifics need tweaking: make a judgment call, note it in progress.md, and mention it during the milestone review. Save the user's attention for decisions that actually matter.

The distinction matters because stopping too often is annoying and breaks flow, but silently working around real problems leads to wasted effort. Use judgment: if you'd want to know about it as the project owner, stop and ask.

---

## Phase 5: Self-Verification

After completing a milestone's work, before presenting it to the user, check your work against the acceptance criteria. Take your time with this — thoroughness here saves review cycles.

Go through each criterion one by one:
- **Pass:** The work clearly satisfies this criterion
- **Partial:** The work addresses this but with caveats
- **Fail:** This criterion is not met

If anything is partial or failing, fix it before presenting. If it can't be fixed without user input, note it for the review.

This step exists because catching your own mistakes before the user sees them saves everyone time and builds trust in the process.

---

## Phase 6: Structured Review Presentation

Present the completed milestone using the review template from `references/templates.md`. The format covers: what was planned, what was delivered, acceptance criteria status (✅/⚠️), deviations, blockers encountered, and what's next.

This consistent format exists so the user always knows where to look. They can scan the acceptance criteria to see if things are on track, check deviations to understand what changed, and glance at what's next.

---

## Phase 7: User Decision

After presenting the review, wait for the user's response. Three paths:

**"Looks good, continue"** → Proceed to commit, then start the next milestone.

**Small feedback** (typos, minor adjustments, "can you also add X to this") → Revise the work in place. Re-verify against acceptance criteria. Re-present the updated review.

**Bigger shifts** ("this approach isn't working, we need to rethink authentication") → Update `spec.md` to reflect the new direction. This might affect the current milestone, future milestones, or both. Walk the user through the changes to the spec before revising the work. The spec is the source of truth — it should always reflect reality.

---

## Phase 8: Commit

Once the user approves a milestone, commit the work. One commit per milestone keeps the history clean and meaningful.

Read the `commit` skill if available for commit message formatting. If not, use a clear, descriptive commit message that references the milestone: e.g., `feat(auth): implement JWT authentication (milestone 2)`.

After committing, update `progress.md` — this is critical for resumability.

---

## Progress Tracking and Resumability

`progress.md` is the most important file for project continuity. It's not just a status tracker — it's a handoff document. When a new session starts, a fresh Claude instance reads this file and should be able to continue the project seamlessly.

Each milestone entry should capture: what was delivered, decisions made during execution, deviations from spec, and user feedback. For in-progress milestones: what's done so far, what's next, and any open questions. See `references/templates.md` for detailed examples.

This level of detail means a fresh Claude instance can pick up mid-milestone without re-asking questions or making different choices.

---

## Resuming a Project

When you detect an existing `spec.md` and `progress.md` in the workspace:

1. Read both files completely
2. Identify where the project left off
3. Summarize the current state to the user: "It looks like you're working on [project]. Milestone N is [status]. Want me to continue from here?"
4. If continuing, follow the normal execution flow starting from the context reset

If the user wants to change direction, go back to the discovery phase — but keep the existing spec as a starting point rather than starting from scratch.

---

## Example Walkthrough

Here's a condensed example of the full flow for a documentation project:

**Discovery:**
> User: "I want to create developer docs for our API."
> Claude: "Who's the primary audience — external developers integrating with your API, or internal team members?"
> User: "External developers."
> Claude: "Got it. Two options for structure: (A) endpoint-first reference docs, or (B) task-based guides that teach common workflows. Which resonates more?"
> User: "A mix — reference docs with a getting-started guide."

**Proposal:**
> **Goals:** Create API documentation that enables external developers to integrate successfully without support assistance.
> **Approach:** Getting-started guide + full endpoint reference + code examples in Python and JavaScript.
> **Constraints:** Must cover v2 API only. Ship within 2 weeks.

User approves. →

**Spec:** Claude generates spec.md with 3 milestones:
1. Getting Started guide (acceptance: covers auth, first API call, error handling)
2. Endpoint Reference (acceptance: all 15 endpoints documented with request/response examples)
3. Code Examples (acceptance: working examples in Python and JS for top 5 use cases)

**Execution of Milestone 1:**
- Claude re-reads spec.md and progress.md
- Writes the getting-started guide
- Hits a blocker: auth flow uses OAuth but spec didn't mention refresh tokens → raises it, user decides to include refresh token docs
- Self-verifies against acceptance criteria: ✅ auth, ✅ first call, ✅ error handling
- Presents structured review
- User approves → commit → progress.md updated

**Milestone 2 begins** with a context reset, and the cycle continues.

---

## Adapting to Project Type

This workflow is designed to be broad. The structure stays the same; the content adapts.

**Code projects:** Tasks reference components, endpoints, modules. Acceptance criteria are often testable. Commits contain code changes.

**Documentation projects:** Tasks reference sections, chapters, topics. Acceptance criteria focus on coverage and clarity.

**Infrastructure projects:** Tasks reference services, configurations, environments. Acceptance criteria focus on operational requirements.

**Design projects:** Tasks reference deliverables — wireframes, mockups, design systems. Acceptance criteria focus on completeness and consistency.

Don't force code-specific language onto non-code projects.

---

## Troubleshooting

**spec.md exists but progress.md is missing:**
The project was likely started but progress wasn't tracked. Read spec.md, assess the current state of the workspace (check what files/work already exist), and reconstruct a progress.md based on what you find. Confirm with the user before continuing.

**User returns mid-milestone in a new session:**
Read both files. The in-progress entry in progress.md should tell you exactly where things stand — what's done, what's next, and any open questions. Summarize this to the user and pick up where the previous session left off.

**Spec feels stale or out of sync with reality:**
If progress.md shows repeated deviations from the spec, the spec has drifted. Propose a spec refresh to the user — review remaining milestones and update them to reflect current reality. The spec should always be trustworthy.

**Project scope has grown significantly:**
If new milestones keep getting added during reviews, pause and have a scoping conversation. Help the user distinguish between "must have for this project" and "nice to have for a future project." Update the spec to reflect the agreed scope.

**User wants to skip the planning phases:**
See "Step 0: Assess Intent" at the top of this skill.
