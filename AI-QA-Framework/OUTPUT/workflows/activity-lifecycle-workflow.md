# Business Workflow: Activity Lifecycle Management

**Workflow ID:** WF-ACTIVITY-001
**Domain:** ACTIVITY
**Owner/Stakeholder:** Not documented — inferred from API surface only
**Risk tier:** High — see RISK-ACTIVITY-002

---

## Description

A standalone task/to-do management process: a user creates an activity (with a title and due date), tracks it via list/detail views, marks it complete or edits it, and eventually removes it. This resource has no documented relationship to any other resource in the API (no `idBook`/`idUser` foreign key), so it is modeled as a self-contained lifecycle rather than part of a larger cross-domain process.

## Trigger

Client creates a new activity (e.g., a task the Book store needs to track).

## Steps

| Step | Endpoint ID | Method & Path | Description | Data Passed Forward |
|---|---|---|---|---|
| 1 | EP-ACTIVITY-002 | POST /api/v1/Activities | Create a new activity | `id` |
| 2 | EP-ACTIVITY-003 | GET /api/v1/Activities/{id} | Retrieve the created activity to confirm state | `id`, current `completed`/`title`/`dueDate` |
| 3 | EP-ACTIVITY-004 | PUT /api/v1/Activities/{id} | Update the activity (e.g., mark `completed: true`) | `id` |
| 4 | EP-ACTIVITY-005 | DELETE /api/v1/Activities/{id} | Remove the activity once no longer needed | — |

`EP-ACTIVITY-001` (list all) supports this workflow as a read-only entry point but is not a required step in any single instance's lifecycle.

## State Dependencies

- Step 2–4 all require the `id` returned/used in Step 1 to reference an activity that still exists.
- Step 3 (update) is expected to be re-runnable independent of `completed` state (no documented state machine restricting valid transitions).

## Success Criteria

An activity can be created, retrieved with matching data, updated, and deleted, with each read reflecting the latest write — end-to-end CRUD consistency for a single resource.

## Failure & Rollback Scenarios

| Scenario | Expected Behavior |
|---|---|
| Step 3 (update) targets an `id` that was already deleted | Undocumented — spec shows only `200`; expected 404 needs empirical verification (see EP-ACTIVITY-004 Open Questions) |
| Step 1 (create) is retried after a network timeout | No idempotency key documented — risk of duplicate activities; flag for Phase 3 |

## Related Workflows

- None — this resource has no documented cross-domain relationship.

## Open Questions / Ambiguities

- No documented business meaning for `Activity` beyond its schema (e.g., is it tied to a specific user or book? Nothing in the schema suggests so) — confirm with stakeholder if this is truly a standalone entity or an undocumented relation exists.
