# Templates

Reference templates for spec.md and progress.md. Use these as starting points and adapt to the project.

## spec.md Template

```markdown
# [Project Name] — Specification

## Goals
Why this project exists and what success looks like.

## Approach
The high-level strategy and key technical/design choices.

## Design Decisions
Decisions made during planning, what was chosen, and why.

### [Decision Name]
- **Options:** Option A, Option B, Option C
- **Choice:** Option B
- **Why:** Brief reasoning explaining the trade-off

### [Another Decision]
- **Options:** ...
- **Choice:** ...
- **Why:** ...

## Milestones

### Milestone 1: [Name]
**Description:** What this milestone delivers.

**Tasks:**
- [ ] Specific, actionable task closer to file-level detail
- [ ] Another task with enough context to execute without guessing
- [ ] ...

**Acceptance Criteria:**
- Concrete, verifiable condition that defines "done"
- Another condition
- ...

### Milestone 2: [Name]
**Description:** ...

**Tasks:**
- [ ] ...

**Acceptance Criteria:**
- ...
```

## progress.md Template — Initial State

```markdown
# [Project Name] — Progress

## Current Status
**Active Milestone:** Not started
**Overall:** 0 of N milestones complete

## Milestone Log

(Updated as milestones are completed)
```

## progress.md — Completed Milestone Entry

```markdown
## Milestone 2: Authentication — ✅ COMPLETE

### What was delivered
- JWT authentication middleware with role extraction
- Login/logout endpoints at /api/auth/login and /api/auth/logout
- Role-based access control (admin, user) on protected routes

### Decisions made during execution
- Used httpOnly cookies for token storage (avoids XSS, discussed with user)
- Token expiry set to 15min with refresh token pattern
- Chose jose library over jsonwebtoken (better TypeScript support)

### Deviations from spec
- Added refresh token endpoint (not in original spec) — needed for usable session length
- Updated in spec.md under design decisions

### Accepted by user
Date: 2026-03-19
Feedback: "Looks good, proceed"
```

## progress.md — In-Progress Milestone Entry

```markdown
## Milestone 3: User Profiles — 🔄 IN PROGRESS

### Completed so far
- Database schema for user profiles
- GET /api/users/:id endpoint

### Decisions made during execution
- Using a separate profiles table rather than extending the users table (normalization)

### Next up
- PUT /api/users/:id for profile updates
- Avatar upload with S3 storage

### Open questions
- Avatar size limits not specified in spec — need to ask user
```

## Structured Review Template

```markdown
### Milestone [N] Review: [Name]

**Planned:** Brief summary of what this milestone was supposed to deliver.

**Delivered:**
- What was actually built/created/completed
- Each major piece of work
- ...

**Acceptance Criteria:**
- ✅ Criterion that passed
- ✅ Another passing criterion
- ⚠️ Criterion with caveats (explain)

**Deviations:** Any differences from the spec and why they happened.
If none: "None — delivered as specified."

**Blockers Encountered:** Any issues that came up during execution.
If none: "None."

**Next Milestone:** Brief preview of what's coming next.
```
