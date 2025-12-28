# Phase 6: Optional Admin UI

This phase is **optional**. Complete it if you have time after finishing phases 1-4.

---

## Overview

Build an admin interface that allows authenticated users to:
1. Sign in with Google
2. Manage organizations
3. Create and edit forms visually
4. View and manage responses

This demonstrates full-stack capability and provides a complete user experience beyond the API.

---

## Google OAuth Login UI

### Login Page

- "Sign in with Google" button
- Clean, simple design
- Handle OAuth redirect flow

### OAuth Flow

1. User clicks "Sign in with Google"
2. Redirect to Google consent screen
3. Handle callback from Google
4. Store session token (localStorage, cookie, etc.)
5. Redirect to dashboard

### Session Management

- Persist authentication state across page refreshes
- Handle token expiration gracefully
- Provide logout functionality

---

## Dashboard

After login, show the user's organizations.

### Organization List

```
┌─────────────────────────────────────────────────────────┐
│ Your Organizations                    [+ New Organization]│
├─────────────────────────────────────────────────────────┤
│ ┌─────────────────────────────────────────────────────┐ │
│ │ Acme Corp                                    [Enter]│ │
│ │ 5 forms • 342 total responses                       │ │
│ └─────────────────────────────────────────────────────┘ │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ Beta Industries                              [Enter]│ │
│ │ 2 forms • 89 total responses                        │ │
│ └─────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

### Create Organization

Simple form:
- Organization name
- Slug (auto-generated from name, editable)

---

## Form Builder UI

### Forms List

For a selected organization, show all forms:

```
┌─────────────────────────────────────────────────────────┐
│ Acme Corp > Forms                         [+ New Form]  │
├─────────────────────────────────────────────────────────┤
│ Name                      │ Slug              │Responses│
├───────────────────────────┼───────────────────┼─────────┤
│ Customer Feedback Survey  │ customer-feedback │ 142     │
│ Event Registration        │ tech-meetup-jan   │ 89      │
│ Job Application           │ careers-2024      │ 23      │
└─────────────────────────────────────────────────────────┘
```

### Create/Edit Form Page

Allow users to build forms visually:

```
┌─────────────────────────────────────────────────────────┐
│ Create New Form                                         │
├─────────────────────────────────────────────────────────┤
│ Name: [Customer Feedback Survey          ]              │
│ Slug: [customer-feedback-survey          ] (auto)       │
├─────────────────────────────────────────────────────────┤
│ Fields:                                                 │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ ☰ 1. [text    ▼] Label: [Your Name    ] [Required ✓]│ │
│ │      Placeholder: [Enter your name]                 │ │
│ │                                          [🗑 Delete]│ │
│ └─────────────────────────────────────────────────────┘ │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ ☰ 2. [rating  ▼] Label: [Satisfaction ] [Required ✓]│ │
│ │      Min: [1]  Max: [5]                             │ │
│ │                                          [🗑 Delete]│ │
│ └─────────────────────────────────────────────────────┘ │
│                                                         │
│ [+ Add Field]                                           │
├─────────────────────────────────────────────────────────┤
│                              [Cancel]  [Create Form]    │
└─────────────────────────────────────────────────────────┘
```

### Field Editor

For each field, allow configuration of:
- Field type (dropdown: text, number, select, boolean, rating, email, phone, url)
- Label
- Required toggle
- Type-specific options:
  - `select` → Add/remove options
  - `rating` → Min and max values
  - `number` → Min and max values
  - `text` → Placeholder, max length

### Edit Form

Same as create, but:
- Pre-populated with existing form data
- Show warning if form has responses (schema changes may affect data interpretation)

---

## Response Viewer UI

### Response List Page

For a selected form, show paginated responses:

```
┌─────────────────────────────────────────────────────────┐
│ Responses: Customer Feedback Survey        [142 total]  │
├─────────────────────────────────────────────────────────┤
│ Submitted At       │ Name            │ Rating │ Actions │
├────────────────────┼─────────────────┼────────┼─────────┤
│ 2024-01-15 14:30   │ Ananya Krishnan │ ★★★★☆  │ [View]  │
│ 2024-01-15 13:15   │ Vikram Patel    │ ★★★★★  │ [View]  │
│ 2024-01-15 11:42   │ Meera Nair      │ ★★★☆☆  │ [View]  │
│ ...                │ ...             │ ...    │ ...     │
├─────────────────────────────────────────────────────────┤
│ [← Prev]  Page 1 of 8  [Next →]                         │
└─────────────────────────────────────────────────────────┘
```

### Response Detail View

Show full response data:

```
┌─────────────────────────────────────────────────────────┐
│ Response Details                               [← Back] │
├─────────────────────────────────────────────────────────┤
│ Submitted: 2024-01-15 14:30:00                          │
│ IP: 203.0.113.42                                        │
│ User Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X...)  │
├─────────────────────────────────────────────────────────┤
│ Your Name:        Ananya Krishnan                       │
│ Email:            ananya.k@gmail.com                    │
│ Satisfaction:     ★★★★☆ (4/5)                           │
│ Department:       Support                               │
│ Feedback:         Very helpful team, resolved my        │
│                   issue quickly!                        │
├─────────────────────────────────────────────────────────┤
│                                       [Delete Response] │
└─────────────────────────────────────────────────────────┘
```

---

## Bonus Features

If you're really going for it:

### Export Responses
- Export as CSV
- Export as JSON

### Response Filtering
- Filter by date range
- Search within response content

### Form Preview
- Preview form as it would appear to respondents
- Test submission without saving

### Form Analytics
- Response count over time
- Average rating (for rating fields)
- Most common select values

---

## Checklist

- [ ] Google OAuth login UI working
- [ ] Organization list and creation
- [ ] Form list for selected organization
- [ ] Form creation UI with field configuration
- [ ] Form editing UI
- [ ] Response list with pagination
- [ ] Response detail view
- [ ] Logout functionality
- [ ] All CRUD operations work through the UI

---

## Evaluation Note

This phase is **optional** and does not affect Pass/Good/Excellent tiers for the core assignment. However, completing it well demonstrates:

- Full-stack capability
- UI/UX thinking
- OAuth integration skills
- Ability to build complete features end-to-end
- Effective use of agentic AI for complex UI work

If you attempt this phase, document it in your README.
