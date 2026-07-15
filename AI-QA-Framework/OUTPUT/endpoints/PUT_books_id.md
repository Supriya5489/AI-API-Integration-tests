# Endpoint: PUT /api/v1/Books/{id}

**Endpoint ID:** EP-BOOK-004
**Domain:** BOOK
**Workflow(s):** WF-BOOK-001
**Risk tier:** High — see RISK-BOOK-002
**Deprecated:** No

---

## Description

Updates an existing book identified by `id`.

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
| 200 | Success | _(no response body schema documented — spec shows only `200`/"Success" with no content)_ |

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| _(not documented in spec — see Open Questions)_ | Expected: 404 for unknown `id`, 400 for malformed body | — |

## Business Rules & Constraints

- Path `id` vs. body `id` mismatch behavior undocumented.

## Dependencies

- **Upstream (must run before this):** EP-BOOK-002 (create) — resource must exist
- **Downstream (depends on this):** Pending — Phase 2 workflow mapping

## Open Questions / Ambiguities

- No documented response body — confirm actual live behavior empirically.
