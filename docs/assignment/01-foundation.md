# Phase 1: Foundation

Set up authentication, database infrastructure, multi-tenancy model, and organization management.

---

## Authentication

Implement Google OAuth2 for user signup and signin.

**Requirements:**
- Users authenticate exclusively via Google OAuth2
- No email/password or other auth methods
- Store user profile from Google (email, name, profile picture)
- Issue session tokens or JWTs after successful OAuth flow

**Example flow:**
1. User clicks "Sign in with Google" on frontend
2. Redirected to Google consent screen
3. Google redirects back with auth code
4. Backend exchanges code for tokens
5. Backend creates/updates user record
6. Backend issues session to frontend

**Example user record:**
```json
{
  "id": "usr_abc123",
  "email": "priya.sharma@gmail.com",
  "name": "Priya Sharma",
  "picture": "https://lh3.googleusercontent.com/...",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

---

## Architecture Constraints

You must use a dual-database architecture. These constraints are non-negotiable.

### Postgres (Metadata Store)

All structural data lives in Postgres:
- Users
- Organizations
- Staff (which users belong to which orgs)
- Forms (name, slug, schema, timestamps, ownership)

Use **Drizzle** or **Prisma** as your ORM.

### MongoDB (Response Store)

All form submissions live in MongoDB with the following isolation model:

| Scope        | MongoDB Mapping |
| ------------ | --------------- |
| Organization | Database        |
| Form         | Collection      |
| Submission   | Document        |

This means:
- Each organization gets its own MongoDB database
- Each form within an organization gets its own collection
- Each response is a document in that collection

Use **MongoDB Native Driver** (preferred) or **Mongoose**.

**Example:**
- Organization "Acme Corp" (id: `org_xyz789`) → MongoDB database `org_xyz789`
- Form "Customer Feedback" (id: `form_456`) → Collection `form_456` in database `org_xyz789`
- Each submission → Document in that collection

**You decide:**
- Collection naming strategy
- Indexing approach (justify your decisions in README)
- Whether to provision databases/collections dynamically or assume they exist

---

## Multi-Tenancy

Users can create and belong to multiple organizations. Tenant context is determined by:

1. **Authenticated user** — From session/JWT
2. **Selected organization** — Via header `X-Org-Id`

**Hard requirements:**
- User must be authenticated for org-scoped operations
- User must be a member of the org specified in `X-Org-Id`
- All metadata queries must be scoped to the selected org
- MongoDB database selection must derive from the selected org
- Cross-tenant data access must be impossible by design

**Example request:**
```http
GET /api/forms
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
X-Org-Id: org_xyz789
```

The backend must verify:
1. Token is valid → User is "Priya Sharma" (usr_abc123)
2. User usr_abc123 is a member of org_xyz789
3. Query forms only from org_xyz789

---

## Organization Management

### Create Organization

Authenticated users can create new organizations.

**Example request:**
```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
Content-Type: application/json

{
  "name": "Acme Corp",
  "slug": "acme-corp"
}
```

**Example response:**
```json
{
  "id": "org_xyz789",
  "name": "Acme Corp",
  "slug": "acme-corp",
  "createdAt": "2024-01-15T10:35:00Z",
  "createdBy": "usr_abc123"
}
```

The creating user automatically becomes a member of the organization.

### List User's Organizations

Return all organizations the authenticated user belongs to.

**Example response:**
```json
{
  "organizations": [
    {
      "id": "org_xyz789",
      "name": "Acme Corp",
      "slug": "acme-corp",
      "role": "owner"
    },
    {
      "id": "org_def456",
      "name": "Beta Industries",
      "slug": "beta-industries",
      "role": "member"
    }
  ]
}
```

### Delete Organization

Only org owners can delete an organization.

**Cascade deletion:**
- Remove organization from Postgres
- Remove all forms belonging to the org
- Drop the organization's MongoDB database (all responses)
- Handle the case where the MongoDB database doesn't exist

---

## Required Libraries

You must use these libraries:

### @theinternetfolks/snowflake

Use for generating all entity IDs (users, organizations, forms, responses).

```typescript
import { Snowflake } from "@theinternetfolks/snowflake";

const userId = Snowflake.generate(); // "6917062538890022912"
```

### @theinternetfolks/context

Use for request-scoped data (current user, current organization) without prop drilling.

```typescript
import { Context } from "@theinternetfolks/context";

// Set context at the start of request
Context.set("user", currentUser);
Context.set("org", currentOrg);

// Access anywhere in the request lifecycle
const user = Context.get("user");
const org = Context.get("org");
```

This is particularly useful for:
- Middleware that sets the authenticated user
- Accessing tenant context in services without passing it through every function

## Checklist

Before moving to Phase 2, ensure:

- [ ] Google OAuth2 flow working (signup + signin)
- [ ] User records created/updated from Google profile
- [ ] Session/JWT issued after authentication
- [ ] Postgres schema defined (Drizzle or Prisma)
- [ ] MongoDB connection established with dynamic database selection
- [ ] Using @theinternetfolks/snowflake for ID generation
- [ ] Using @theinternetfolks/context for request-scoped data
- [ ] Biome configured for linting and formatting
- [ ] Organization CRUD working
- [ ] User-org membership tracked
- [ ] `X-Org-Id` header parsed and validated against user membership
- [ ] Deletion cascades to MongoDB database
- [ ] Docker setup for Postgres and MongoDB

---

## Design Decisions to Document

In your README, explain:
- How you implemented Google OAuth2 flow
- How you handle session management (cookies, JWTs, etc.)
- How you handle MongoDB database/collection provisioning
- Your approach to tenant isolation in queries
- Any middleware or patterns used for auth and tenant context
