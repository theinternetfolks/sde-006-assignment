# Phase 3: Responses

Implement response submission, validation, storage, and retrieval.

---

## Response Submission

Public endpoint—**no authentication required**.

### Design Challenge: Slug → Organization Resolution

Since the submission endpoint is public (no `X-Org-Id` header), you must resolve which organization owns the form using only the slug.

**The challenge:**
- Slugs are unique within an organization, but two different orgs could have forms with the same slug
- You need to determine which MongoDB database to write to
- This lookup should be efficient (responses may have high throughput)

**Your options include:**
- Make slugs globally unique (simplest, but limits flexibility)
- Store the organization ID in the form record and look it up by slug
- Use a composite identifier in the URL

Document your approach in your README.


### Validation Requirements

**Use Zod for all backend validation.**

| Scenario                    | Behavior             | Example                                       |
| --------------------------- | -------------------- | --------------------------------------------- |
| Required field missing      | Reject with error    | `email` not provided                          |
| Field has wrong type        | Reject with error    | `rating: "five"` instead of `5`               |
| Extra fields in payload     | Ignore (don't store) | `unknownField: "value"`                       |
| email format invalid        | Reject with error    | `email: "not-an-email"`                       |
| phone format invalid        | Reject with error    | `phone: "abc"`                                |
| url format invalid          | Reject with error    | `url: "not-a-url"`                            |
| select value not in options | Reject with error    | `department: "Marketing"` when not in options |
| rating outside range        | Reject with error    | `rating: 10` when max is 5                    |

Validate against the **current** schema, not any historical version.

### Error Response Format

Errors must be structured and actionable.

**Example error response:**
```json
{
  "status": false,
  "content": {
    "title": "Invalid Input",
    "description": "The data you provided for the collection import is invalid.",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format",
        "code": "invalid_format"
      },
      {
        "field": "rating",
        "message": "Rating must be between 1 and 5",
        "code": "out_of_range"
      },
      {
        "field": "department",
        "message": "Required field is missing",
        "code": "required"
      }
    ]
  }
}
```

**Requirements:**
- **Structured** — Object format, not just strings
- **Predictable** — Same format for all validation errors
- **Actionable** — Frontend can map errors to specific fields


---

## Response Retrieval

Requires authentication and org membership.

### Access Control

- The requesting user must be authenticated
- The user must be a member of the organization that owns the form
- If the form doesn't exist or user lacks access → return 404 (don't leak existence)

---

## MongoDB Strategy

### Collection Naming

Each form has its own collection within the organization's database.

**Example:**
- Organization: `org_xyz789` → Database: `org_xyz789`
- Form: `form_abc123` → Collection: `form_abc123`
- Form: `form_def456` → Collection: `form_def456`

You should choose a different naming convention—document your choice.

### Indexing Strategy

Consider indexes for:
- `metadata.submitted_at` — For ordering and pagination
- Compound indexes if supporting filtering

**Example index:**
```javascript
// For pagination queries ordered by submission time
db.form_abc123.createIndex({ "metadata.submitted_at": -1 })
```

**Justify your indexing decisions in README:**
- What queries does each index support?
- Trade-offs (write performance vs read performance)

---

## Edge Cases

Handle these scenarios:

| Scenario                    | Expected Behavior                  |
| --------------------------- | ---------------------------------- |
| Form slug doesn't exist     | Return 404 with message            |
| Form was deleted            | Return 404 (collection dropped)    |
| No responses yet            | Return empty array with pagination |
| Very large response payload | Consider size limits               |
| Concurrent submissions      | Should not lose data               |

**Example 404 response:**
```json
{
  "status": false,
  "content": {
    "title": "Form not found",
    "description": "No form exists with slug 'invalid-slug'."
  }
}
```

---

## Checklist

Before moving to Phase 4, ensure:

- [ ] Public submission endpoint validates against current schema
- [ ] All validation errors are structured and field-specific
- [ ] Extra fields are ignored (not stored)
- [ ] Responses stored in MongoDB with metadata (IP, user agent, timestamp)
- [ ] Response retrieval requires auth + org membership
- [ ] Pagination works correctly
- [ ] Proper error responses for missing forms

---

## Design Decisions to Document

In your README, explain:
- Your validation error format
- MongoDB collection naming strategy
- Indexing strategy and rationale
- How you resolve slug → organization for public submissions
- How you extract IP address and user agent from requests
