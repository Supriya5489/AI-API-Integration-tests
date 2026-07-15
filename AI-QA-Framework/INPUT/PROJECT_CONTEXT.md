# Project Context

## Project Name

AI Mentor Bot

## Project Type

REST API

## Primary Source

**Source type:** Swagger UI URL (live, not a static file)
**URL:** https://fakerestapi.azurewebsites.net/index.html

> Note: This is a Swagger **UI** page, not the raw spec document. The machine-readable spec is typically served separately, commonly at one of:
> - `https://your-company-url/swagger/v1/swagger.json`
> - `https://your-company-url/swagger/index.html/swagger.json`
> - Linked from the "Select Definition" dropdown or the top of the Swagger UI page
>
> Confirm the actual JSON/YAML endpoint and either:
> - Save it into `INPUT/` as `swagger.json`, `openapi.json`, or `swagger.yaml`, **or**
> - Fetch it directly from the URL above at the start of each testing run, if network access is available and the URL is reachable from the testing environment.
>
> Until a static spec file is present in `INPUT/`, treat this URL as the authoritative source and fetch it fresh — do not rely on a stale local copy.

## Environment

**UAT**

- Base URL: {{UAT base URL — confirm if different from the Swagger host above}}
- This is a shared test environment — do not assume data isolation; avoid destructive operations without confirming environment scope first (see [AGENT.md](../AGENT.md) constraints).

## Authentication

**Scheme:** Bearer Token

- Token acquisition method: {{e.g., login endpoint, manually issued token, SSO}}
- Token lifetime/expiry: {{if known}}
- Roles/scopes relevant to testing: {{e.g., admin, standard user}}
- Test credentials/tokens must be stored as placeholders only (`{{BEARER_TOKEN}}`) — never committed in plain text, per [STANDARDS.md §8](../STANDARDS.md#8-formatting-conventions).

## Notes

- The Swagger/OpenAPI specification is the primary source of truth for this project.
- Since the current source is a live URL rather than a committed file, the spec should be re-fetched at the start of each testing cycle to catch drift, and the fetched version/timestamp should be recorded in `OUTPUT/API_Inventory.md` for traceability.

---

**Context recorded on:** 2026-07-15
**Recorded by:** ksupriya@ariqt.com
