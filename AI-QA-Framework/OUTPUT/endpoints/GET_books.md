# Endpoint: GET /api/v1/Books

**Endpoint ID:** EP-BOOK-001
**Domain:** BOOK
**Workflow(s):** WF-BOOK-001
**Risk tier:** Low — see RISK-BOOK-001
**Deprecated:** No

---

## Description

Returns the full collection of books.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

_(none)_

**Query parameters**

_(none documented in spec)_

**Headers**

_(none required)_

**Body schema**

_(none — GET request)_

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | Array of `Book` |

```json
[
  {
    "id": "integer(int32)",
    "title": "string, nullable",
    "description": "string, nullable",
    "pageCount": "integer(int32)",
    "excerpt": "string, nullable",
    "publishDate": "string(date-time)"
  }
]
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | — | — |

## Business Rules & Constraints

- None documented beyond the `Book` schema.

## Dependencies

- **Upstream (must run before this):** None
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Spec defines only `200`; no documented behavior for empty collections or errors.
