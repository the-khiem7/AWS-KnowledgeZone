# Kilo MCP Configuration Format

MCP servers are configured in `.kilo/kilo.json` under the `mcp` key.

## Local MCP (stdio subprocess)

```json
"<server-name>": {
  "type": "local",
  "command": ["uvx", "<package>"],
  "environment": {
    "KEY": "value"
  },
  "enabled": true,
  "timeout": 5000
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | `"local"` | yes | Must be `"local"` (not `"stdio"`) |
| `command` | `string[]` | yes | Command and arguments as a flat array |
| `cwd` | `string` | no | Working directory (relative paths resolve from workspace) |
| `environment` | `Record<string,string>` | no | Environment variables |
| `enabled` | `boolean` | no | Enable/disable on startup |
| `timeout` | `number` | no | Request timeout in ms (default: 5000) |

### Notes
- Use a single `command` array (do **not** split into separate `command` + `args` fields).
- Use `environment` (not `env`).

## Remote MCP (HTTP/SSE)

```json
"<server-name>": {
  "type": "remote",
  "url": "https://example.com/mcp",
  "headers": {
    "Authorization": "Bearer <token>"
  },
  "enabled": true,
  "timeout": 5000
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | `"remote"` | yes | Must be `"remote"` (not `"http"`) |
| `url` | `string` | yes | Server URL |
| `headers` | `Record<string,string>` | no | HTTP headers |
| `oauth` | `McpOAuthConfig \| false` | no | OAuth config, or `false` to disable auto-detection |
| `enabled` | `boolean` | no | Enable/disable on startup |
| `timeout` | `number` | no | Request timeout in ms (default: 5000) |

## Disabling an MCP server

```json
"<server-name>": {
  "enabled": false
}
```

Use a minimal object with only `enabled: false` — no other fields needed.
