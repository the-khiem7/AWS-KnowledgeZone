# Kiro MCP Configuration

Kiro is an AI-powered conversational assistant from AWS. It uses VS Code-compatible MCP config format.

## Config file locations

- **Global**: `~/.kiro/settings/mcp.json`
- **Local (project)**: `.kiro/settings/mcp.json`

## Format

```json
{
    "mcpServers": {
        "<server-name>": {
            "command": "uvx",
            "args": ["<package>"],
            "env": {
                "KEY": "value"
            },
            "type": "stdio"
        }
    }
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `mcpServers` | `object` | yes | Top-level key (not `servers` or `mcp`) |
| `command` | `string` | yes | Command to run (flat string, not array) |
| `args` | `string[]` | no | Command arguments as array |
| `env` | `Record<string,string>` | no | Environment variables |
| `type` | `"stdio" \| "http"` | no | Server transport type |
| `url` | `string` | for http | URL for HTTP MCP servers |
| `disabled` | `string[]` | no | Tool patterns to disable for this server |
| `disabled` | `boolean` | no | Disable the entire server |

## Format mapping from VS Code

| Aspect | VS Code (.vscode/mcp.json) | Kiro (.kiro/settings/mcp.json) |
|--------|---------------------------|--------------------------------|
| Top-level key | `"servers"` | `"mcpServers"` |
| Command format | `"command": "uvx"` + `"args": [...]` | Same (identical) |
| Environment vars | `"env": {...}` | Same |
| Transport type | `"type": "stdio"` / `"type": "http"` | Same |
| Disabled | `"disabled": true/false` | `"disabled": true/false` or `"disabled": ["pattern_*"]` |
