# Endpoint: GET /api/v1/Books/{id}

**Endpoint ID:** EP-BOOK-003
**Domain:** BOOK
**Workflow(s):** WF-BOOK-001, WF-CATALOG-001
**Risk tier:** Low — see RISK-BOOK-001
**Deprecated:** No

---

## Description

Returns a single book by its ID.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| id | integer (int32) | Yes | Book identifier |

**Query parameters**

_(none)_

**Headers**

_(none required)_

**Body schema**

_(none — GET request)_

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | Single `Book` object |

```json
{
  "id": "integer(int32)",
  "title": "string, nullable",
  "description": "string, nullable",
  "pageCount": "integer(int32)",
  "excerpt": "string, nullable",
  "publishDate": "string(date-time)"
}
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | Expected: 404 for unknown `id` | — |

## Business Rules & Constraints

- None documented beyond the `Book` schema.

## Dependencies

- **Upstream (must run before this):** EP-BOOK-002 (create)
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- Behavior for a non-existent `id` is undocumented.
