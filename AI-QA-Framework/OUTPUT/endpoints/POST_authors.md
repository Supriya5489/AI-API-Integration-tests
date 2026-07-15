# Endpoint: POST /api/v1/Authors

**Endpoint ID:** EP-AUTHOR-002
**Domain:** AUTHOR
**Workflow(s):** WF-AUTHOR-001, WF-CATALOG-001
**Risk tier:** High — see RISK-AUTHOR-002
**Deprecated:** No

---

## Description

Creates a new author.

## Authentication & Authorization

- **Auth required:** No
- **Scheme:** None — confirmed unauthenticated for testing (see `PROJECT_STATE.md`)
- **Required scopes/roles:** N/A

## Request

**Path parameters**

_(none)_

**Query parameters**

_(none)_

**Headers**

- `Content-Type: application/json` (or `text/json`, `application/*+json`)

**Body schema**

```json
{
  "id": "integer(int32)",
  "idBook": "integer(int32)",
  "firstName": "string, nullable",
  "lastName": "string, nullable"
}
```

Schema has `additionalProperties: false`.

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | Single `Author` object |

```json
{
  "id": "integer(int32)",
  "idBook": "integer(int32)",
  "firstName": "string, nullable",
  "lastName": "string, nullable"
}
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | — | — |

## Business Rules & Constraints

- No documented validation that `idBook` refers to an existing `Book`.

## Dependencies

- **Upstream (must run before this):** EP-BOOK-002 (create book) if `idBook` is expected to reference a real book — to be confirmed in Phase 2
- **Downstream (depends on this):** EP-AUTHOR-003 (get authors by book), EP-AUTHOR-004/005/006

## Open Questions / Ambiguities

- Whether the API enforces `idBook` referential integrity (rejects unknown book IDs) is undocumented — a key negative-test candidate for Phase 5.
