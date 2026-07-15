# API Inventory — Book store

**Spec source:** `INPUT/swagger.json`
**Spec version:** OpenAPI 3.0.1 — "FakeRESTApi.Web V1", version `v1`
**Base URL(s):** `https://fakerestapi.azurewebsites.net` (no `servers` block in spec; base URL is the fetch origin)
**Global auth scheme:** None — confirmed unauthenticated for testing (2026-07-15). Spec defines no `securitySchemes`/`security` requirements.
**Inventoried on:** 2026-07-15
**Inventoried by:** agent

---

## Summary

| Metric | Count |
|---|---|
| Total endpoints | 27 |
| Domains/resource groups | 5 (Activity, Author, Book, CoverPhoto, User) |
| Endpoints requiring auth | 0 |
| State-changing endpoints (POST/PUT/PATCH/DELETE) | 15 |
| Deprecated endpoints | 0 |

---

## Endpoints by Domain

### Domain: ACTIVITY

| Endpoint ID | Method | Path | Summary | Auth Required | Endpoint Doc |
|---|---|---|---|---|---|
| EP-ACTIVITY-001 | GET | /api/v1/Activities | List all activities | No | [GET_activities.md](endpoints/GET_activities.md) |
| EP-ACTIVITY-002 | POST | /api/v1/Activities | Create a new activity | No | [POST_activities.md](endpoints/POST_activities.md) |
| EP-ACTIVITY-003 | GET | /api/v1/Activities/{id} | Get activity by ID | No | [GET_activities_id.md](endpoints/GET_activities_id.md) |
| EP-ACTIVITY-004 | PUT | /api/v1/Activities/{id} | Update activity by ID | No | [PUT_activities_id.md](endpoints/PUT_activities_id.md) |
| EP-ACTIVITY-005 | DELETE | /api/v1/Activities/{id} | Delete activity by ID | No | [DELETE_activities_id.md](endpoints/DELETE_activities_id.md) |

### Domain: AUTHOR

| Endpoint ID | Method | Path | Summary | Auth Required | Endpoint Doc |
|---|---|---|---|---|---|
| EP-AUTHOR-001 | GET | /api/v1/Authors | List all authors | No | [GET_authors.md](endpoints/GET_authors.md) |
| EP-AUTHOR-002 | POST | /api/v1/Authors | Create a new author | No | [POST_authors.md](endpoints/POST_authors.md) |
| EP-AUTHOR-003 | GET | /api/v1/Authors/authors/books/{idBook} | Get authors by book ID | No | [GET_authors_authors_books_idbook.md](endpoints/GET_authors_authors_books_idbook.md) |
| EP-AUTHOR-004 | GET | /api/v1/Authors/{id} | Get author by ID | No | [GET_authors_id.md](endpoints/GET_authors_id.md) |
| EP-AUTHOR-005 | PUT | /api/v1/Authors/{id} | Update author by ID | No | [PUT_authors_id.md](endpoints/PUT_authors_id.md) |
| EP-AUTHOR-006 | DELETE | /api/v1/Authors/{id} | Delete author by ID | No | [DELETE_authors_id.md](endpoints/DELETE_authors_id.md) |

### Domain: BOOK

| Endpoint ID | Method | Path | Summary | Auth Required | Endpoint Doc |
|---|---|---|---|---|---|
| EP-BOOK-001 | GET | /api/v1/Books | List all books | No | [GET_books.md](endpoints/GET_books.md) |
| EP-BOOK-002 | POST | /api/v1/Books | Create a new book | No | [POST_books.md](endpoints/POST_books.md) |
| EP-BOOK-003 | GET | /api/v1/Books/{id} | Get book by ID | No | [GET_books_id.md](endpoints/GET_books_id.md) |
| EP-BOOK-004 | PUT | /api/v1/Books/{id} | Update book by ID | No | [PUT_books_id.md](endpoints/PUT_books_id.md) |
| EP-BOOK-005 | DELETE | /api/v1/Books/{id} | Delete book by ID | No | [DELETE_books_id.md](endpoints/DELETE_books_id.md) |

### Domain: COVERPHOTO

| Endpoint ID | Method | Path | Summary | Auth Required | Endpoint Doc |
|---|---|---|---|---|---|
| EP-COVERPHOTO-001 | GET | /api/v1/CoverPhotos | List all cover photos | No | [GET_coverphotos.md](endpoints/GET_coverphotos.md) |
| EP-COVERPHOTO-002 | POST | /api/v1/CoverPhotos | Create a new cover photo | No | [POST_coverphotos.md](endpoints/POST_coverphotos.md) |
| EP-COVERPHOTO-003 | GET | /api/v1/CoverPhotos/books/covers/{idBook} | Get cover photos by book ID | No | [GET_coverphotos_books_covers_idbook.md](endpoints/GET_coverphotos_books_covers_idbook.md) |
| EP-COVERPHOTO-004 | GET | /api/v1/CoverPhotos/{id} | Get cover photo by ID | No | [GET_coverphotos_id.md](endpoints/GET_coverphotos_id.md) |
| EP-COVERPHOTO-005 | PUT | /api/v1/CoverPhotos/{id} | Update cover photo by ID | No | [PUT_coverphotos_id.md](endpoints/PUT_coverphotos_id.md) |
| EP-COVERPHOTO-006 | DELETE | /api/v1/CoverPhotos/{id} | Delete cover photo by ID | No | [DELETE_coverphotos_id.md](endpoints/DELETE_coverphotos_id.md) |

### Domain: USER

| Endpoint ID | Method | Path | Summary | Auth Required | Endpoint Doc |
|---|---|---|---|---|---|
| EP-USER-001 | GET | /api/v1/Users | List all users | No | [GET_users.md](endpoints/GET_users.md) |
| EP-USER-002 | POST | /api/v1/Users | Create a new user | No | [POST_users.md](endpoints/POST_users.md) |
| EP-USER-003 | GET | /api/v1/Users/{id} | Get user by ID | No | [GET_users_id.md](endpoints/GET_users_id.md) |
| EP-USER-004 | PUT | /api/v1/Users/{id} | Update user by ID | No | [PUT_users_id.md](endpoints/PUT_users_id.md) |
| EP-USER-005 | DELETE | /api/v1/Users/{id} | Delete user by ID | No | [DELETE_users_id.md](endpoints/DELETE_users_id.md) |

---

## Notes & Ambiguities

- **No documented error responses.** Every operation in the spec defines only a `200` response — there are no documented `400`/`401`/`404`/`409` schemas anywhere. Error-path test cases (Phase 5) will assume conventional REST behavior but this must be verified empirically against the live UAT instance, not assumed from the contract.
- **No auth scheme in contract**, consistent with the user-confirmed "unauthenticated for testing" decision (see `PROJECT_STATE.md`).
- **No query parameters, pagination, or filtering documented** on any `GET` list endpoint (`/Activities`, `/Authors`, `/Books`, `/CoverPhotos`, `/Users` all return the full unfiltered collection per the spec).
- **Foreign-key-like relationships are implicit, not enforced by the contract:** `Author.idBook` and `CoverPhoto.idBook` reference `Book.id`, but the spec does not document referential integrity behavior (e.g., what happens creating an Author with a non-existent `idBook`). Flagged for Phase 2/3.
- **Irregular path shapes:** `/Authors/authors/books/{idBook}` and `/CoverPhotos/books/covers/{idBook}` have unusual nested/duplicated path segments (not simple `/{resource}/{id}` shape) — verified as-is from the live spec, not a transcription error.
- **All schemas set `additionalProperties: false`** — request bodies with unknown fields should be rejected per contract; useful for negative test design in Phase 5.
- **`User.password` is a plain string field on the schema itself**, returned/accepted with no visible masking or hashing indication in the contract — worth a dedicated risk note in Phase 3 (Risk Assessment) even though this is a public demo API.

## Exclusions

| Path/Method | Reason for exclusion |
|---|---|
| _(none)_ | All 27 path+method combinations in the spec are included in this inventory. |
