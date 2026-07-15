# Endpoint: {{METHOD}} {{/path}}

**Endpoint ID:** EP-{{DOMAIN}}-{{seq}}
**Domain:** {{domain}}
**Workflow(s):** {{WF-DOMAIN-seq, ... or "Standalone"}}
**Risk tier:** {{Critical | High | Medium | Low}} — see RISK-{{DOMAIN}}-{{seq}}
**Deprecated:** {{Yes/No}}

---

## Description

{{What this endpoint does, in business terms.}}

## Authentication & Authorization

- **Auth required:** {{Yes/No}}
- **Scheme:** {{e.g., Bearer JWT}}
- **Required scopes/roles:** {{e.g., admin, self-only}}

## Request

**Path parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| {{id}} | {{string}} | {{Yes}} | {{description}} |

**Query parameters**

| Name | Type | Required | Description |
|---|---|---|---|
| {{param}} | {{type}} | {{Yes/No}} | {{description}} |

**Headers**

| Name | Required | Description |
|---|---|---|
| {{Authorization}} | {{Yes}} | {{Bearer token}} |

**Body schema**

```json
{
  "field": "type — description"
}
```

## Response

**Success**

| Status | Description | Body schema |
|---|---|---|
| {{200}} | {{OK}} | See below |

```json
{
  "field": "type — description"
}
```

**Error responses**

| Status | Condition | Body schema |
|---|---|---|
| {{400}} | {{Validation error}} | `{ "error": "string" }` |
| {{401}} | {{Missing/invalid auth}} | `{ "error": "string" }` |
| {{404}} | {{Resource not found}} | `{ "error": "string" }` |
| {{409}} | {{Conflict/duplicate}} | `{ "error": "string" }` |

## Business Rules & Constraints

- {{e.g., email must be unique}}
- {{e.g., amount must be > 0}}

## Dependencies

- **Upstream (must run before this):** {{EP-...}}
- **Downstream (depends on this):** {{EP-...}}

## Open Questions / Ambiguities

- {{Anything unclear from the spec that needs confirmation.}}
