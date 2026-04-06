---
name: clawql
description: Call a remote ClawQL Streamable HTTP MCP server (search + execute) from the app via HTTP — same JSON-RPC tool results as native MCP, without embedding OpenAPI in chat. Use for GitHub, Jira, Cloudflare, GCP, Slack, Sentry, n8n, or custom OpenAPI specs the server was configured with.
metadata:
  require-secret: true
  require-secret-description: |
    Two values in this one field (Gallery supports a single secret):
    **Option A — two lines:** Line 1 = full MCP URL (e.g. `https://your-host/mcp`). Line 2 = bearer token (paste the raw token only; the script adds `Bearer `). Leave line 2 empty only if your endpoint needs no auth.
    **Option B — one JSON object:** `{"mcp_url":"https://your-host/mcp","token":"your-token"}` (also accepts `url` / `mcpUrl` for the URL key).
  homepage: https://github.com/danielsmithdevelopment/ClawQL
---

# ClawQL (HTTP → Streamable MCP)

[ClawQL](https://github.com/danielsmithdevelopment/ClawQL) exposes MCP tools **`search`** and **`execute`**. This skill talks to your **`clawql-mcp-http`** (or compatible) **Streamable HTTP** endpoint with `POST` + **SSE** responses — the same **`tools/call`** payloads a native MCP client would receive, not a separate REST API.

## When to use

- Discover API operations from natural language, then run one by `operationId`.
- The user’s ClawQL instance is reachable over HTTPS (or HTTP for local dev). **Your server must allow CORS** from the Gallery webview origin, or browser `fetch` will fail.

## Instructions for the agent

1. Ensure the **clawql** skill is enabled and the user has filled the **secret** (URL + token as described in metadata).
2. Call the **`run_js`** tool with **`index.html`** and a **JSON string** in **`data`**:

### `action`: `"search"`

```json
{
  "action": "search",
  "query": "natural language intent, e.g. list GKE clusters in a region",
  "limit": 5
}
```

- **`query`** (required): search intent.
- **`limit`** (optional): 1–50, default 5.

### `action`: `"execute"`

```json
{
  "action": "execute",
  "operationId": "exact id from search results",
  "args": {},
  "fields": ["field1", "field2"]
}
```

- **`operationId`** (required).
- **`args`** (required): parameter map for the operation (path/query/body as ClawQL expects).
- **`fields`** (optional): response field hints for leaner output when supported.

3. Parse the returned **`result`** string (JSON). It mirrors MCP **`CallToolResult`**: typically **`content`** (e.g. text parts with JSON bodies), and **`isError`** when the tool failed.
4. **`search` before `execute`**. Ask the user for missing IDs, paths, or auth-related details. Do not echo the secret or full token.

## Troubleshooting (user-visible)

- **CORS errors**: configure the reverse proxy / ClawQL host to allow the app’s origin (or `*` for testing only).
- **401/403**: check bearer token and gateway rules.
- **GraphQL / execute**: upstream ClawQL still needs its GraphQL proxy for some execute paths; see [ClawQL README](https://github.com/danielsmithdevelopment/ClawQL/blob/main/README.md).

## Security

- **`execute`** can perform real API calls. Confirm destructive operations with the user.
- Never put the URL or token in the **`data`** JSON; only in the app secret field.
