# Spec-Driven Development

## What is Spec-Driven Development?

Spec-Driven Development (SDD) is a software development approach where a detailed specification document is written **before** any code is written.

The spec acts as the **single source of truth** for what the system should do. All development flows from it.

### Benefits

- Clear requirements before implementation
- Reduces rework and scope creep
- Easier to estimate effort
- Better communication between stakeholders
- Testable acceptance criteria

---

## What Should Be in a Spec Document

### 1. Problem Statement
What problem are we solving? For whom? Why does it matter?

**Example:**
> Users need to track daily expenses and view spending history. Currently, no system exists — users rely on manual spreadsheets.

### 2. Functional Requirements
What the system must do. Use numbered lists for clarity.

**Example:**
1. User can register with email and password.
2. User can log in with credentials.
3. User can add an expense with amount, category, and date.
4. User can view all expenses in a table.
5. User can edit or delete existing expenses.

### 3. API Contract
Define inputs, outputs, and data shapes.

**Example:**
```
POST /expenses/add
Request:
  - amount: float (required, > 0)
  - category: string (required, one of: food, transport, utilities, other)
  - date: ISO 8601 date (optional, defaults to today)
  - description: string (optional)

Response:
  201 Created
  { id: int, message: "Expense added" }

400 Bad Request
  { error: "Invalid amount" }
```

### 4. Constraints
Technical or business limitations.

**Example:**
- SQLite only — no external databases.
- No new pip packages without approval.
- Must work on Python 3.10+.
- Session-based auth only (no OAuth).

### 5. Edge Cases and Error Handling
What can go wrong? How do we handle it?

**Example:**
- Duplicate email during registration → 409 Conflict, "Email already registered"
- Invalid category → 400 Bad Request, "Invalid category"
- Expense not found → 404 Not Found
- Unauthenticated access → 302 Redirect to /login

### 6. Acceptance Criteria
How do we know it's done? Must be testable.

**Example:**
- [ ] User can register with valid email/password.
- [ ] Registration fails with duplicate email.
- [ ] Expense appears in list immediately after adding.
- [ ] Deleting expense removes it from database.
- [ ] All routes require authentication (except /register, /login).

---

## Workflow for Spec-Driven Development

```
Spec → Review → Design → Review → Tasks → Build → Validate
```

### 1. Spec
Write the specification document. Include all sections above.

### 2. Review
Get feedback on the spec. Check for:
- Missing requirements
- Unclear acceptance criteria
- Overlooked edge cases

### 3. Design
Plan the implementation:
- Which files will change?
- What new functions are needed?
- Database schema changes?

### 4. Review (Design)
Validate the design before coding:
- Does it match the spec?
- Any simpler approach?

### 5. Tasks
Break work into small, trackable tasks.

**Example:**
- [ ] Add `add_expense()` function to `database/db.py`
- [ ] Create `GET /expenses/add` route → render form
- [ ] Create `POST /expenses/add` route → process form
- [ ] Add flash message on success

### 6. Build
Implement tasks one at a time. Refer to spec for decisions.

### 7. Validate
Test against acceptance criteria:
- Manual testing
- Automated tests
- Edge case verification

---

## Using Specs with Claude Code

1. **Write spec first** — before `/loop` or implementation tasks.
2. **Pin spec to context** — reference `@notes/SPEC_DRIVEN_DEVELOPMENT.md` in prompts.
3. **Generate tasks from spec** — "Create tasks from this spec's acceptance criteria."
4. **Validate against spec** — "Check implementation against spec section 3 (API Contract)."

### Example Prompt

```
Build the expense add feature per @notes/SPEC_DRIVEN_DEVELOPMENT.md section 3.

Steps:
1. Add DB function in database/db.py
2. Create route in app.py
3. Create template in templates/

Validate against acceptance criteria before marking complete.
```

---

## Template: Quick Spec

```markdown
# Feature: [Name]

## Problem
[One paragraph]

## Requirements
1. ...
2. ...

## API
[Request/response shapes]

## Constraints
- ...

## Edge Cases
- ...

## Acceptance Criteria
- [ ] ...
- [ ] ...
```
