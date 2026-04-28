# SKILL: Perfex CRM n8n Node Operations

## DESCRIPTION
Use the Webo MCP - Perfex CRM node in n8n correctly and safely for Perfex CRM REST API actions.

## INTENT
N8N, PERFEX, API, CRM, AUTOMATION

## RULES

### 1. Scope
- Only use this skill for Perfex CRM API tasks in n8n.
- Target node: Webo MCP - Perfex CRM (`perfexCrm`).
- Do not use this skill for WordPress, scraping, or unrelated APIs.

### 2. Resource Selection
- Use `Clients -> Get All` only for quick `GET /api/clients` (Themesic style).
- Use `Custom Request` for all other endpoints.

### 3. Method and Path Discipline
- Always set both `HTTP Method` and `Path` explicitly in `Custom Request`.
- Build URL from credential `API URL` + `Path`.
- Respect module path style:
  - Themesic style often uses `/api/...`
  - Other modules may use `/api/v1/...`
- Never invent unsupported endpoints.

### 4. Query Parameters
- `Query Parameters` must be a JSON object string (example: `{}` or `{"page":"1"}`).
- In AI Tool mode, query may be hidden from schema; if filtering is required, prefer full canvas node config.

### 5. Request Body Rules
- Enable `Send Body` only for methods that need payload (`POST`, `PUT`, `PATCH`).
- `Body` must be exactly one JSON object string.
- Use only field names from endpoint documentation.
- Never guess property names.
- If required fields are missing, ask the user before calling the node.
- Empty-body intent:
  - Keep default placeholder `{"__n8nToolPlaceholder":true}` when no payload is required.

### 6. Authentication Rules
- Credentials supported:
  - Bearer Token (`Authorization: Bearer <token>`)
  - X-API-KEY (`X-API-KEY: <key>`)
- If auth fails, verify API URL, token/key, and API module availability.

### 7. Error Handling
- `401/403`: likely invalid token/key or permission issue.
- `404` with HTML page: likely wrong base URL or wrong endpoint path, not a node bug.
- Re-check endpoint path against the active Perfex API documentation.

### 8. Non-Hallucination Policy
- Do not claim records were created/updated/deleted unless node execution confirms it.
- Do not fabricate CRM data.
- If API contract is unclear, ask for endpoint docs or sample payload first.

## WORKFLOW
1. Identify user intent (list, create, update, delete, custom action).
2. Choose `Clients` shortcut or `Custom Request`.
3. Confirm method + exact path from docs.
4. Collect required fields for payload (if any).
5. Build query/body JSON strings correctly.
6. Execute node.
7. Return concise result summary based on real response.

## OUTPUT FORMAT
- Action: method + path
- Inputs used: key query/body fields
- Result: success/failure + key response facts
- Next step: optional follow-up action
