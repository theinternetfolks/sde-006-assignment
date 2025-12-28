# Phase 2: Forms

Implement form management and the schema system.

---

## Form Management APIs

All form management requires authentication and org membership.

### List Forms

**Requirements:**
- Form belongs to the organization from `X-Org-Id`

### Create Form

**Requirements:**
- Form belongs to the organization from `X-Org-Id`
- Slug must be unique within the organization

### Update Form

**Important behavior:**
- Forms can be edited **after responses exist**
- New submissions validate against the **latest** schema
- Old responses are NOT migrated or revalidated

### Delete Form

**Cascade deletion:**
- Remove form metadata from Postgres
- Drop the form's MongoDB collection (all responses)
- Handle the case where the collection doesn't exist

---

## Public Form Access

Forms are publicly accessible by slug. **No authentication required.**

**Notes:**
- Security by obscurity is intentional—slugs act as "secret" links
- Response must be sufficient for frontend to render the form
- Do NOT expose organization details or internal IDs unnecessarily

---

## Form Schema Design

You design the JSON structure. The example above is a suggestion—feel free to adapt it.

### Required Field Types

Your schema must support ALL of these field types:

| Type      | Description            | Example Properties             |
| --------- | ---------------------- | ------------------------------ |
| `text`    | Single-line text input | `placeholder`, `maxLength`     |
| `number`  | Numeric input          | `min`, `max`, `placeholder`    |
| `select`  | Dropdown/choice        | `options` (array of strings)   |
| `boolean` | Yes/no toggle          | `defaultValue`                 |
| `rating`  | Numeric scale          | `min`, `max` (e.g., 1-5 stars) |
| `email`   | Email with validation  | `placeholder`                  |
| `phone`   | Phone number           | `placeholder`                  |
| `url`     | URL with validation    | `placeholder`                  |

### Field Properties

Each field should support at minimum:
- `id` — Unique identifier within the form
- `type` — One of the 8 types above
- `label` — Display text for the field
- `required` — Boolean, whether field is mandatory

Additional properties vary by type (placeholder, options, min, max, etc.).
ß
## Schema Evolution

Forms can be edited after responses exist.

**Defined behavior:**
- New submissions validate against the **current** schema
- Historical responses retain their original data
- Fetching old responses may return fields that no longer exist in the schema
- Fetching old responses may be missing fields that were added later

**Example scenario:**
1. Form has fields: `name`, `email`, `rating`
2. User submits response with all three fields
3. Form schema updated: `rating` removed, `phone` added
4. New submissions must have `name`, `email`, `phone`
5. Old response still has `name`, `email`, `rating` (no `phone`)

Document how you handle this in your README.

---

## Checklist

Before moving to Phase 3, ensure:

- [ ] Form CRUD APIs working (create, read, update, delete, list)
- [ ] All APIs require authentication + org membership
- [ ] Schema stored as JSON in Postgres
- [ ] Public slug endpoint returns schema without auth
- [ ] All 8 field types representable in your schema design
- [ ] Form deletion drops MongoDB collection
- [ ] Slug uniqueness enforced within organization

---

## Design Decisions to Document

In your README, explain:
- Your schema JSON structure with examples
- How you handle slug uniqueness (global vs per-org)
- Your approach to schema evolution
- Any constraints or validation on the schema itself (e.g., max fields, valid field types)
