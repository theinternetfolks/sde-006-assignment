# Phase 5: Evaluation Criteria

How your submission will be assessed.

You can make your submission here on this form: https://forms.gle/yuGeBqWDJtjUwizt9

---

## Tiers

### Pass

The basics work end-to-end.

- [ ] Google OAuth login functional
- [ ] Organizations can be created and listed
- [ ] Forms can be created, listed, and deleted
- [ ] Public form page renders from schema
- [ ] Responses submit and validate correctly
- [ ] Responses stored in MongoDB with metadata
- [ ] Response retrieval works with pagination
- [ ] Tenant isolation enforced (users only see their orgs' data)
- [ ] Uses required libraries (@theinternetfolks/snowflake, @theinternetfolks/context)
- [ ] Code formatted with Biome
- [ ] Project runs with `docker-compose up`

### Good

Everything in Pass, plus thoughtful implementation.

- [ ] Clean separation between metadata (Postgres) and responses (MongoDB)
- [ ] Schema design handles all 8 field types elegantly
- [ ] Structured, consistent error responses
- [ ] Sensible MongoDB indexing with justification
- [ ] Code is organized and readable
- [ ] Clean git history with meaningful commits
- [ ] README clearly explains design decisions
- [ ] Agentic AI workflow documented with examples

### Excellent

Everything in Good, plus senior-level execution.

- [ ] Robust edge case handling (deleted forms, concurrent submissions, invalid tokens)
- [ ] Schema design anticipates future field types without code changes
- [ ] Pagination is efficient and handles large datasets
- [ ] Deletion cascades are atomic or handle partial failures gracefully
- [ ] Clear separation of concerns (routes, services, data access)
- [ ] Effective use of agentic AI demonstrated in README
- [ ] Pragmatic code without over-engineering
- [ ] Optional admin UI completed (Phase 6)

---

## Bonus (Not Required)

These do not affect Pass/Good/Excellent tiers but demonstrate additional depth:

- Automated tests (unit and/or integration)
- Invite users to organizations
- Response filtering/search capabilities
- Form versioning or response schema snapshots
- Admin UI (see [Phase 6](./06-optional-admin-ui.md))

---

## Anti-Patterns to Avoid

These will negatively impact your assessment:

| Anti-Pattern                                   | Why It's a Problem               |
| ---------------------------------------------- | -------------------------------- |
| Single MongoDB collection for all responses    | Violates architecture constraint |
| Missing or superficial schema validation       | Core requirement not met         |
| Tenant ID leaking across queries               | Security failure                 |
| Users can access other orgs' data              | Authorization failure            |
| Over-engineered abstractions                   | Unnecessary complexity           |
| Brittle frontend that breaks on schema changes | Doesn't prove schema works       |
| No error handling for database operations      | Production-readiness concern     |
| Hardcoded configuration                        | DevOps/deployment concern        |
| Giant files with mixed concerns                | Code organization issue          |
| No agentic AI documentation                    | Requirement not followed         |
| Using ChatGPT instead of agentic tools         | Requirement not followed         |
| Messy git history ("fix", "wip", "asdf")       | Shows lack of discipline         |
| Not using required libraries                   | Requirement not followed         |

---

## What We're Looking For

### Strong Signals

- **Pragmatic decisions** — Solves the problem without gold-plating
- **Clear thinking** — README explains the "why" not just the "what"
- **Defensive code** — Handles errors, edge cases, and invalid input
- **Consistent patterns** — Similar problems solved similarly
- **Effective agentic AI usage** — Directed AI agents thoughtfully, not blindly

### We Value

- Working software over comprehensive documentation
- Readable code over clever code
- Explicit over implicit
- Simple solutions that can evolve over complex solutions that anticipate everything

---

## Git History Expectations

Your commit history should tell the story of your implementation:

**Good commits:**
```
feat: implement Google OAuth flow with session management
feat: add form CRUD endpoints with Zod validation
feat: create dynamic form renderer component
fix: handle MongoDB connection timeout gracefully
refactor: extract tenant context using @theinternetfolks/context
```

**Bad commits:**
```
fix
wip
asdfasdf
final final v2
changes
```

---

## README Expectations

Your README should cover:

### 1. Setup Instructions
- How to run with docker-compose
- How to configure Google OAuth credentials
- Environment variables needed
- Any additional setup steps

### 2. Agentic AI Workflow
- Which tools you used (Claude Code, Cursor, Antigravity, Copilot Workspace)
- Key prompts or task descriptions that worked well
- How the agent iterated on problems
- What you had to guide or correct
- Screenshots or logs (optional but appreciated)

### 3. Schema Design
- Your JSON schema structure
- Example of a complete form schema
- Rationale for the structure

### 4. MongoDB Strategy
- Collection naming convention
- Indexing strategy with justification
- How databases/collections are provisioned

### 5. Validation Approach
- Error response format
- How errors map to fields
- Example error responses

### 6. Trade-offs & Limitations
- What you chose not to do and why
- Known limitations
- What you'd do differently with more time

### 7. Assumptions
- Any ambiguities you resolved
- Decisions you made without clarification

---

## Questions?

**Email:** sameer.khan@theinternetfolks.com

Contact ONLY for logical gaps or contradictions in the assignment. Do NOT contact for system design discussions or "is this approach okay?" questions. Make decisions independently and document your reasoning in your README.
