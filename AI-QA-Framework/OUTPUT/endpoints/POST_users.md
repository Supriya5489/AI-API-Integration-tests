# Endpoint: POST /api/v1/Users

**Endpoint ID:** EP-USER-002
**Domain:** USER
**Workflow(s):** WF-USER-001
**Risk tier:** Critical — see RISK-USER-002 (state-changing + plaintext `password`)
**Deprecated:** No

---

## Description

Creates a new user.

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
  "userName": "string, nullable",
  "password": "string, nullable"
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
| _(not documented in spec — see Open Questions)_ | — | — |

## Business Rules & Constraints

- `password` accepted as a plain string with no documented complexity/format requirement or hashing behavior.
- No documented uniqueness constraint on `userName`.

## Dependencies

- **Upstream (must run before this):** None
- **Downstream (depends on this):** EP-USER-003/004/005

## Open Questions / Ambiguities

- Whether the created user (and its `password`) is echoed back is undocumented — confirm empirically whether plaintext password round-trips in any response.
- No documented validation error for duplicate `userName` or malformed body.
