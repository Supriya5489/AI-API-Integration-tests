# Business Workflow: User Account Lifecycle Management

**Workflow ID:** WF-USER-001
**Domain:** USER
**Owner/Stakeholder:** Not documented — inferred from API surface only
**Risk tier:** Critical — see RISK-USER-002 (state-changing + plaintext `password`)

---

## Description

Standalone account management process: a user account is created with a username/password, can be looked up individually or in bulk, updated, and removed. Unlike a typical auth-bearing API, this contract has **no login/token-issuance endpoint** — `Users` here is a plain data resource, not a session/identity provider, per the Phase 0 finding that no `securitySchemes` exist anywhere in the spec.

## Trigger

Client creates a new user account record.

## Steps

| Step | Endpoint ID | Method & Path | Description | Data Passed Forward |
|---|---|---|---|---|
| 1 | EP-USER-002 | POST /api/v1/Users | Create a new user | `id` |
| 2 | EP-USER-003 | GET /api/v1/Users/{id} | Retrieve the created user | `id` |
| 3 | EP-USER-004 | PUT /api/v1/Users/{id} | Update user details (e.g., change `userName`/`password`) | `id` |
| 4 | EP-USER-005 | DELETE /api/v1/Users/{id} | Remove the user account | — |

`EP-USER-001` (list all) supports this workflow as a read-only entry point.

## State Dependencies

- Steps 2–4 require the `id` from Step 1 to still reference an existing user.
- No documented dependency on any other resource (Books/Authors/Activities do not reference `User.id`).

## Success Criteria

A user account can be created, retrieved, updated, and deleted, with reads reflecting the latest writes.

## Failure & Rollback Scenarios

| Scenario | Expected Behavior |
|---|---|
| Step 3 sends a partial body (e.g., omits `password`) | Undocumented whether this clears or preserves the field — see EP-USER-004 Open Questions |
| Step 1 is retried after timeout | No idempotency key documented — risk of duplicate accounts with the same `userName` |

## Related Workflows

- None — no other domain references `User.id`.

## Open Questions / Ambiguities

- `password` is stored/returned as plaintext per the schema with no hashing or masking documented — this is a data-sensitivity concern to carry into Phase 3 Risk Assessment even for a test/demo API, since test design should default to secure-handling assumptions unless the contract proves otherwise.
- No documented relationship between `User` and any other resource (e.g., `Activity`, `Book`) despite the project being named "Book store" — confirm with stakeholder whether an undocumented ownership link is expected in a future spec revision.
