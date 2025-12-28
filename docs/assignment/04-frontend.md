# Phase 4: Public Form Frontend

Build a minimal React frontend for public form rendering and submission.

---

## Purpose

The frontend proves that:
- Your schema design can be rendered dynamically
- Your public APIs work correctly
- Your validation errors are actionable

**Styling does not matter.** Functional is sufficient.

---

## Scope

This phase covers **only the public-facing form experience**:
- Fetching a form by slug
- Rendering it dynamically
- Submitting responses
- Displaying validation errors

**NOT in this phase:**
- Google OAuth login UI
- Organization management UI
- Form creation/editing UI
- Response viewing UI

These are covered in [Phase 6 - Optional Admin UI](./06-optional-admin-ui.md).

---

## Public Form Page

Accessed via URL like `/forms/:slug` — **no authentication required**.

### Flow

1. Extract slug from URL
2. Fetch form schema from public API
3. Render form dynamically based on schema
4. User fills out form
5. Submit to public submission API
6. Show success message or validation errors

### States to Handle

| State | What to Show |
|-------|--------------|
| Loading | Loading spinner or skeleton |
| Form not found | "Form not found" message |
| Form loaded | Rendered form with submit button |
| Submitting | Disabled button, loading indicator |
| Success | "Thank you" confirmation message |
| Validation error | Errors mapped to specific fields |
| Network error | Generic error message with retry option |

---

## Dynamic Form Rendering

The core challenge: render ANY valid form schema without hardcoding.

### Field Type → Component Mapping

| Schema Type | Render As |
|-------------|-----------|
| `text` | `<input type="text">` |
| `number` | `<input type="number">` |
| `select` | `<select>` with `<option>`s |
| `boolean` | `<input type="checkbox">` |
| `rating` | Stars, number buttons, or `<input type="range">` |
| `email` | `<input type="email">` |
| `phone` | `<input type="tel">` |
| `url` | `<input type="url">` |

### Rendering Requirements

- Show field label
- Indicate required fields (e.g., with asterisk)
- Apply placeholder text if provided
- For `select`: render all options from schema
- For `rating`: respect min/max values
- For `number`: respect min/max if provided

---

## Handling Validation Errors

When backend returns errors, display them near the relevant field:

```
┌─────────────────────────────────────┐
│ Email *                             │
│ ┌─────────────────────────────────┐ │
│ │ not-valid-email                 │ │
│ └─────────────────────────────────┘ │
│ ⚠ Invalid email format              │
│                                     │
│ Rating *                            │
│ [1] [2] [3] [4] [5]                 │
│ ⚠ Required field is missing         │
└─────────────────────────────────────┘
```

### Error Mapping

Your frontend must:
1. Parse the structured error response from backend
2. Map each error to its corresponding field by `field` property
3. Display error message near the field
4. Show any general errors at the top of the form

---

## Success State

After successful submission:

```
┌─────────────────────────────────────┐
│                                     │
│     ✓ Thank you for your response!  │
│                                     │
│     Your feedback has been          │
│     submitted successfully.         │
│                                     │
└─────────────────────────────────────┘
```

Options:
- Show confirmation message
- Optionally allow "Submit another response"
- Do NOT redirect away immediately

---

## Not Required

- Google OAuth login UI (see Phase 6)
- Organization management UI (see Phase 6)
- Form creation/editing UI (see Phase 6)
- Response viewing UI (see Phase 6)
- Styling or visual polish
- State management libraries (useState is fine)
- Frontend-side validation (backend is source of truth)
- Mobile responsiveness
- Accessibility compliance

---

## Technical Notes

- Use React (included in starter setup)
- Bun serves the frontend via HTML imports
- Hot reload available with `bun --hot`

Example structure:
```
/index.html           → Entry point
/frontend.tsx         → React app
/components/
  /FormPage.tsx       → Public form page
  /FormRenderer.tsx   → Dynamic form rendering
  /FieldRenderer.tsx  → Individual field components
```

---

## Checklist

Before completing the core assignment:

- [ ] Public form page fetches schema by slug
- [ ] All 8 field types render appropriately
- [ ] Required fields are visually indicated
- [ ] Form submission sends data to API
- [ ] Validation errors display per-field
- [ ] Success message shown after submission
- [ ] Error states handled (form not found, network error)
- [ ] Works for ANY valid schema (not hardcoded)

---

## Design Decisions to Document

In your README, explain:
- How your frontend maps schema types to components
- How you handle unknown field types (if encountered)
- Any limitations in field type rendering
