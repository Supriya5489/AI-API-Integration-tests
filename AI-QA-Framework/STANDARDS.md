# STANDARDS.md — Documentation, Naming & Quality Conventions

This document defines the conventions all `OUTPUT/` artifacts must follow so that inventory, workflows, risk, coverage, test cases, automation, and reports stay consistent and cross-referenceable. Every template in `TEMPLATES/` must comply with this file.

---

## 1. File & Folder Naming

- Use `PascalCase` with underscores for template-derived docs: `API_Inventory.md`, `Test_Case.md`, `Risk_Assessment.md`.
- Per-endpoint files live under `OUTPUT/endpoints/` and are named `<METHOD>_<resource-path>.md`, e.g. `POST_users.md`, `GET_orders_id.md` (path params written without braces, joined with underscores).
- Per-workflow files live under `OUTPUT/workflows/` and are named `<verb>-<noun>-workflow.md` in kebab-case, e.g. `user-signup-workflow.md`.
- One file per entity — do not merge multiple endpoints or workflows into a single doc.

## 2. ID Conventions

Every identifiable unit gets a stable, human-readable ID used consistently across all documents for traceability:

| Entity | ID format | Example |
|---|---|---|
| Endpoint | `EP-<domain>-<seq>` | `EP-USER-001` |
| Workflow | `WF-<domain>-<seq>` | `WF-CHECKOUT-001` |
| Risk item | `RISK-<domain>-<seq>` | `RISK-PAYMENT-003` |
| Coverage item | `COV-<domain>-<seq>` | `COV-AUTH-002` |
| Test case | `TC-<domain>-<seq>` | `TC-USER-014` |
| Defect | `DEF-<seq>` | `DEF-021` |

- IDs are assigned once and never reused, even if the item is later removed (mark as `Deprecated` instead).
- `<domain>` is the resource/business area (e.g., `USER`, `PAYMENT`, `AUTH`, `ORDER`), in uppercase.
- `<seq>` is zero-padded to 3 digits, incrementing per domain.

## 3. Priority Levels (for test cases & coverage items)

| Priority | Meaning |
|---|---|
| **P0 – Blocker** | Must pass before any release; core business path (e.g., login, payment, order creation) |
| **P1 – High** | Important functional path; failure significantly degrades the product |
| **P2 – Medium** | Secondary functionality; failure is inconvenient but has workarounds |
| **P3 – Low** | Edge case, cosmetic, or rarely-exercised path |

## 4. Severity Levels (for defects & risk findings)

| Severity | Meaning |
|---|---|
| **Critical** | Data loss/corruption, security breach, financial miscalculation, complete outage of a core flow |
| **High** | Major functionality broken, no workaround, affects many users/integrations |
| **Medium** | Functionality impaired but a workaround exists; limited scope |
| **Low** | Minor/cosmetic issue, inconsistent messaging, non-blocking |

Severity (impact if it fails) and Priority (order in which to address) are tracked separately — a Critical-severity defect in a rarely-used endpoint may still be scheduled below a High-severity defect in a P0 path.

## 5. Risk Tiers (for `Risk_Assessment.md`)

| Tier | Criteria |
|---|---|
| **Critical** | Handles auth/financial/PII data, or is state-changing with wide blast radius |
| **High** | State-changing (create/update/delete) or gated by auth, moderate blast radius |
| **Medium** | Read operations on sensitive data, or low-impact writes |
| **Low** | Public/read-only, non-sensitive data |

Risk tier determines minimum required coverage depth per `WORKFLOW.md` Phase 4.

## 6. Test Case Status Values

`Not Started` → `In Progress` → `Passed` / `Failed` / `Blocked` → `Deprecated`

- `Blocked` requires a linked reason (e.g., missing test data, environment down).
- A case is only `Passed` if executed against the current spec version; spec changes revert affected cases to `Not Started`.

## 7. Traceability Rules

- Every `Endpoint` must link to ≥1 `Workflow` (or be marked `Standalone`).
- Every `Workflow` must link to ≥1 `Risk` entry.
- Every `Risk` entry must link to ≥1 `Coverage` item, or state an explicit exclusion reason.
- Every `Coverage` item must link to ≥1 `Test Case`.
- Every `Test Case` must link to its `Automation Plan` classification.
- `Traceability.md` is the single source of truth for these links — other docs reference IDs, not duplicate content.
- No orphaned IDs: an ID referenced in one doc must exist and be resolvable in its origin doc.

## 8. Formatting Conventions

- All docs are Markdown, using `#`/`##`/`###` headers matching the structure defined in the corresponding template.
- Tables are used for enumerable data (endpoints, risk scores, test case lists); prose is reserved for rationale/context.
- Dates use `YYYY-MM-DD`. Never use relative dates ("yesterday", "next week") in committed artifacts.
- Status/priority/severity values must use the exact labels defined in Sections 3–6 above — no free-text synonyms (e.g., not "urgent" instead of `P0`).
- Placeholders for secrets/credentials use `{{VARIABLE_NAME}}` — never real values.
- Code/payload examples are fenced with the appropriate language tag (```json, ```yaml).

## 9. Review Checklist

Before any `OUTPUT/` artifact is considered complete, verify:

- [ ] File is named per Section 1 conventions.
- [ ] All entities have IDs per Section 2 format, and IDs are unique/not reused.
- [ ] Priority/severity/risk/status values use only the defined labels (Sections 3–6).
- [ ] All required traceability links are present (Section 7) or have an explicit, documented exclusion reason.
- [ ] Dates are absolute (`YYYY-MM-DD`); no relative time references.
- [ ] No real credentials, secrets, or production PII appear anywhere in the document.
- [ ] Tables used for enumerable data; no dense prose blocks that should be tabular.
- [ ] Content matches the current spec version in `INPUT/`; stale references are flagged or updated.
- [ ] Cross-references (endpoint ↔ workflow ↔ risk ↔ coverage ↔ test case) resolve to existing IDs in their source docs.
- [ ] Document reviewed against its template in `TEMPLATES/` for structural completeness.
