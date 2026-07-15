# Endpoint: POST /api/v1/Books

**Endpoint ID:** EP-BOOK-002
**Domain:** BOOK
**Workflow(s):** WF-BOOK-001, WF-CATALOG-001
**Risk tier:** High — see RISK-BOOK-002
**Deprecated:** No

---

## Description

Creates a new book.

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
  "title": "string, nullable",
  "description": "string, nullable",
  "pageCount": "integer(int32)",
  "excerpt": "string, nullable",
  "publishDate": "string(date-time)"
}
```

Schema has `additionalProperties: false`.

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | _(no response body schema documented for this operation — spec shows only `200`/"Success" with no content)_ |

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | — | — |

## Business Rules & Constraints

- No documented uniqueness/validation constraints (e.g., `pageCount` > 0).

## Dependencies

- **Upstream (must run before this):** None
- **Downstream (depends on this):** EP-AUTHOR-002/003 (authors reference `idBook`), EP-COVERPHOTO-002/003 (cover photos reference `idBook`)

## Open Questions / Ambiguities

- Unlike other domains' POST operations, this endpoint's `200` response has no documented content schema in the spec — worth confirming the actual live response shape empirically (does it echo the created `Book`?).
