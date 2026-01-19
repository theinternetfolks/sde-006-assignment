# Assignment: Multi-Tenant Form Builder & Response Engine

## Overview

You are building the data and rendering backbone for a form product similar to Typeform. This system enables multiple organizations to create forms, collect responses, and retrieve submissions—all while maintaining strict tenant isolation.

**What you're building:**
- User authentication via Google OAuth2
- Organization management (users can create and manage orgs)
- Form schema management with custom field types
- Dynamic form rendering on the frontend
- Response collection and storage
- Multi-tenant data isolation

**What you're NOT building:**
- Analytics or reporting dashboards
- Drag-and-drop form builders
- Marketing pages
- Email/password authentication

---

## Phases

Work through these phases in order. Each builds on the previous.

| Phase                                               | Focus           | Description                                                |
| --------------------------------------------------- | --------------- | ---------------------------------------------------------- |
| [01 - Foundation](./01-foundation.md)               | Infrastructure  | Google OAuth, database setup, multi-tenancy, organizations |
| [02 - Forms](./02-forms.md)                         | Core CRUD       | Form creation, schema design, public access                |
| [03 - Responses](./03-responses.md)                 | Data Collection | Submission, validation, storage, retrieval                 |
| [04 - Frontend](./04-frontend.md)                   | UI              | Dynamic form rendering with React                          |
| [05 - Evaluation](./05-evaluation.md)               | Criteria        | How your submission will be assessed                       |
| [06 - Optional Admin UI](./06-optional-admin-ui.md) | Bonus           | Login UI, form builder, response viewer                    |

---

## Technical Stack

### Required

| Category           | Technology                            |
| ------------------ | ------------------------------------- |
| Runtime            | Bun                                   |
| Postgres ORM       | Drizzle or Prisma                     |
| MongoDB            | Native Driver (preferred) or Mongoose |
| Validation         | Zod                                   |
| Frontend           | React                                 |
| Auth               | Google OAuth2                         |
| Containerization   | Docker                                |
| Linting/Formatting | Biome                                 |

### Required Libraries

You must use these libraries from The Internet Folks:

- **@theinternetfolks/snowflake** — For generating unique identifiers at scale. Use this for all entity IDs (users, orgs, forms, responses).
- **@theinternetfolks/context** — For creating context that can reference data without prop drilling in Node-based environments. Use this for request-scoped data (current user, current org).

Install them:
```bash
bun add @theinternetfolks/snowflake @theinternetfolks/context
```

### Allowed
- Any React UI libraries
- Any additional Bun-compatible packages
- Yup for frontend validation (optional)
- Elysia (preferred), Express, Fastify, or other Node.js frameworks (use Bun.serve)

### Not Allowed
- GraphQL
- External form builder libraries
- Email/password or other auth methods (Google OAuth only)

---

## Agentic AI Coding Requirement

**Using agentic AI coding tools is mandatory for this assignment.**

### What We Mean by "Agentic"

We want you to use AI tools that can:
- Understand your codebase context
- Execute commands and see results
- Iterate autonomously on tasks
- Make multi-file changes coherently

### Approved Tools

Use one or more of:
- **Claude Code** (CLI-based agentic coding)
- **Cursor** (AI-first IDE with agentic features)
- **Antigravity** (agentic coding platform)
- **GitHub Copilot Workspace** (agentic mode)
- Or others (Amp CLI, Cline, etc.)

### NOT Approved

- ChatGPT (copy-paste workflow is not agentic)
- Simple autocomplete (Copilot suggestions alone don't count)
- Claude.ai web interface (not integrated with your codebase)

### The Goal

Write **more prompts/instructions** and **less manual code**. We want to see how effectively you can direct AI agents to build software.

### Document in Your README

- Which agentic tools you used
- Key prompts or task descriptions that worked well
- How the agent iterated on problems
- What you had to guide or correct
- Screenshots or logs showing agentic workflow (optional but appreciated)

---

## Submission

You can make your submission here on this form: https://forms.gle/yuGeBqWDJtjUwizt9

### Repository Setup

1. Create a **private** GitHub repository
2. Add **@theinternetfolksbot** as a collaborator with read access
3. Push your completed solution

### Deadline

The deadline will be communicated to you via email. Please remain dedicated during this timeline and avoid submitting last-minute work.

### Git History

We expect a **clean commit history** that tells the story of your implementation:
- Meaningful commit messages (not "fix", "wip", "asdf")
- Logical progression of features
- Each commit should represent a coherent change

Bad examples:
```
fix
wip
asdfasdf
final final v2
```

Good examples:
```
feat: implement Google OAuth flow with session management
feat: add form CRUD endpoints with Zod validation
fix: handle MongoDB connection timeout gracefully
refactor: extract tenant context middleware
```

---

## Running the Project

Your submission must be runnable with:

```bash
docker-compose up
```

This should start:
- The backend service
- The frontend (or serve it from the backend)
- Postgres
- MongoDB

Include a `.env.example` with all required environment variables documented, including:
- Google OAuth credentials (client ID, client secret)
- Database connection strings
- Any secrets or configuration

---

## Deliverables

1. **Backend service** — All APIs described in phases 1-3
2. **Frontend app** — Dynamic form renderer with Google login
3. **Database schema** — Drizzle or Prisma schema files
4. **Docker setup** — Dockerfile(s) and docker-compose.yml
5. **README** documenting:
   - How to run the project
   - How to set up Google OAuth credentials
   - Your form schema design and rationale
   - MongoDB collection/indexing strategy
   - Validation approach
   - Agentic AI tools used and key prompts
   - Known limitations and trade-offs

---

## Questions?

If any requirement is ambiguous, make a reasonable assumption and document it in your README.

### Contact Policy

**Email:** sameer.khan@theinternetfolks.com

**Use this email ONLY for:**
- Logical gaps in the assignment document
- Clarification on requirements that seem contradictory

**Do NOT contact for:**
- Technical issues with the starter repo, you can fix them
- System design discussions or confirmations
- "Is this approach okay?" questions
- Implementation guidance

We want to see how you think and make decisions independently. Your README should document your assumptions and rationale.
