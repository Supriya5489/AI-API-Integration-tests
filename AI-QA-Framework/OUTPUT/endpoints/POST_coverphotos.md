# Endpoint: POST /api/v1/CoverPhotos

**Endpoint ID:** EP-COVERPHOTO-002
**Domain:** COVERPHOTO
**Workflow(s):** WF-COVERPHOTO-001, WF-CATALOG-001
**Risk tier:** High — see RISK-COVERPHOTO-002
**Deprecated:** No

---

## Description

Creates a new cover photo record.

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
  "url": "string(uri), nullable"
}
```

Schema has `additionalProperties: false`.

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| 200 | Success | Single `CoverPhoto` object |

```json
{
  "id": "integer(int32)",
  "idBook": "integer(int32)",
  "url": "string(uri), nullable"
}
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | — | — |

## Business Rules & Constraints

- No documented validation that `idBook` refers to an existing `Book`, or that `url` is a well-formed URI.

## Dependencies

- **Upstream (must run before this):** EP-BOOK-002 (create book) if `idBook` is expected to reference a real book
- **Downstream (depends on this):** EP-COVERPHOTO-003 (get by book), EP-COVERPHOTO-004/005/006

## Open Questions / Ambiguities

- Whether malformed/non-URI `url` values are rejected is undocumented — a negative-test candidate for Phase 5.
