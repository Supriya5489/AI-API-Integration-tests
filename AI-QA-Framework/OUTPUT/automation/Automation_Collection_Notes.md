# Automation Collection Notes — AI Mentor Bot

**Built on:** 2026-07-15
**Built by:** agent
**Tool:** Postman/Newman (per explicit user direction; supersedes the pytest recommendation in `OUTPUT/Automation_Plan.md`'s original Tooling section)

---

## Artifacts

| File | Purpose |
|---|---|
| `AI_Mentor_Bot.postman_collection.json` | Collection covering all 35 "Automate Now" test cases (67 requests total — several test cases expand into setup + action + verify steps for self-contained fixture provisioning) |
| `AI_Mentor_Bot_UAT.postman_environment.json` | Defines `baseUrl` = `https://fakerestapi.azurewebsites.net` |

## Structure

One folder per domain (ACTIVITY, AUTHOR, BOOK, COVERPHOTO, USER, CATALOG), matching `OUTPUT/Test_Case.md`'s grouping. Every folder is self-contained: it provisions any fixture data it needs (books, authors, etc.) via its own requests rather than depending on another folder having run first, so folders can be run independently or the whole collection can be run end-to-end. Fixture IDs are generated at runtime (`Date.now()` + random offset in a pre-request script) and chained via Postman collection variables.

## Test Case → Request Mapping

| Test Case ID | Request/Test Name | Notes |
|---|---|---|
| TC-ACTIVITY-003 | Create activity with valid payload | Provisions `activityId` used by the rest of the folder |
| TC-ACTIVITY-001 | List all activities returns full collection | |
| TC-ACTIVITY-002 | Get activity by ID returns matching record | Targets a seed id discovered from the list response (see Environment-Behavior Findings) |
| TC-ACTIVITY-006 | Update activity with valid payload | |
| TC-ACTIVITY-008 | Repeated identical update is idempotent (call 1 + call 2) | 2 requests, compares response bodies |
| TC-ACTIVITY-009 | Delete existing activity | |
| TC-AUTHOR-004 | Create author with valid payload | Preceded by a Setup request creating the required book fixture |
| TC-AUTHOR-001 | List all authors returns full collection | |
| TC-AUTHOR-002 | Get author by ID returns matching record | Targets a seed id |
| TC-AUTHOR-003 | Get authors by book ID returns matching authors | |
| TC-AUTHOR-007 | Update author with valid payload | |
| TC-AUTHOR-009 | Delete existing author | |
| TC-BOOK-003 | Create book with valid payload | |
| TC-BOOK-001 | List all books returns full collection | |
| TC-BOOK-002 | Get book by ID returns matching record | Targets a seed id |
| TC-BOOK-005 | Update book with valid payload | |
| TC-BOOK-007 | Delete book with no dependent records | Preceded by a Setup request creating a dedicated throwaway book |
| TC-BOOK-010 | Repeated delete of the same book (call 1 + call 2) | 2 requests; own throwaway book fixture |
| TC-BOOK-011 | Delete book with an associated author (orphaning check) | 2 requests (delete + verify) + 3 Setup requests (book, author, pre-delete baseline) |
| TC-BOOK-012 | Delete book with an associated cover photo (orphaning check) | Mirrors TC-BOOK-011 for CoverPhotos |
| TC-COVERPHOTO-004 | Create cover photo with valid payload | Preceded by a Setup request creating the required book fixture |
| TC-COVERPHOTO-001 | List all cover photos returns full collection | |
| TC-COVERPHOTO-002 | Get cover photo by ID returns matching record | Targets a seed id |
| TC-COVERPHOTO-003 | Get cover photos by book ID returns matching photos | |
| TC-COVERPHOTO-007 | Update cover photo with valid payload | |
| TC-COVERPHOTO-009 | Delete existing cover photo | |
| TC-USER-005 | Create user with valid payload | Fake credentials only |
| TC-USER-001 | List all users returns full collection | |
| TC-USER-002 | Get user by ID returns matching record | Targets a seed id |
| TC-USER-008 | Update user with valid payload | |
| TC-USER-010 | Repeated identical update is idempotent (call 1 + call 2) | 2 requests, compares response bodies |
| TC-USER-011 | Delete existing user + repeat delete (error handling check) | 2 requests |
| TC-CATALOG-001 | Full book publishing happy path (steps 1/6–6/6) | 6 requests: create book/author/cover photo, then 3 retrieval steps (see Environment-Behavior Findings — steps 4–6 log actual behavior rather than asserting persistence) |
| TC-CATALOG-003 | Deleting a book mid-workflow orphans its author and cover photo (steps 1/6–6/6) | 6 requests: create book/author/cover photo, delete book, verify both lookups (logged, not asserted — see below) |
| TC-CATALOG-006 | Multiple authors and cover photos correctly associate with one book (steps 1/7–7/7) | 7 requests: create book + 2 authors + 2 cover photos, then verify both lookups (logged, not asserted) |

## Execution Record

| Date | Run By | Result | Notes |
|---|---|---|---|
| 2026-07-15 | agent (via `newman run`) | 67/67 requests, 93/93 assertions passing | First run (pre-fix) surfaced 13 failing assertions that assumed persistence; assertions were redesigned around the statelessness finding below, then re-run clean. |

## Environment-Behavior Findings

**The live UAT API is stateless.** `POST`/`PUT`/`DELETE` are validated and echoed with `200` but never actually persisted; `GET` always serves a fixed, unchanging seed dataset (~ids 1–30 per resource) regardless of any writes. Confirmed via direct `curl` probes: a freshly created record 404s on immediate `GET` by its own id, and a "deleted" pre-seeded record still returns `200` on `GET` immediately after.

Consequences applied in this collection:
- "Get by id" checks (TC-ACTIVITY-002, TC-AUTHOR-002, TC-BOOK-002, TC-COVERPHOTO-002, TC-USER-002) discover a real seed id from the preceding list response rather than using a freshly created id.
- TC-CATALOG-001's and TC-CATALOG-006's cross-resource lookup steps log the actual response (which will not contain the just-created associations) instead of asserting inclusion — asserting persistence here would fail by design, not because of a defect.
- TC-BOOK-011/012 and TC-CATALOG-003's post-delete "orphaning" verification steps similarly log actual behavior rather than asserting a specific outcome.

Full amendment notes are in `PROJECT_STATE.md`, `OUTPUT/Risk_Assessment.md` (RISK-BOOK-003, RISK-CATALOG-001), and `OUTPUT/Test_Case.md` (see the 2026-07-15 amendment note above the ACTIVITY Domain table).

## How to Run

```
cd OUTPUT/automation
newman run AI_Mentor_Bot.postman_collection.json -e AI_Mentor_Bot_UAT.postman_environment.json
```

Requires Node.js + `npm install -g newman` (or run via the Postman desktop app's Collection Runner).

## Known Limitations / Excluded Cases

None — all 35 "Automate Now" test cases from `OUTPUT/Automation_Plan.md` have a corresponding request in this collection. The 27 "Automate Later" test cases (Negative/Boundary/Security types) are intentionally out of scope here per the Automation Plan's classification policy.
