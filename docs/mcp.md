# Shared MCP Server Config

[`.mcp.json`](../.mcp.json) at the repo root declares MCP servers that should be available
whenever this repo is open — in Claude Code directly, or wherever this repo is installed as a
plugin (`.mcp.json` at the plugin root is one of the recognized plugin component locations, same
as `skills/`, `agents/`, and `commands/`).

It currently ships empty (`{"mcpServers": {}}`) — add entries as you decide which MCP servers
belong here.

## When to add a server here vs. elsewhere

- **Here (`.mcp.json`, project-wide)**: a server you want available in every session opened on
  this repo and in every project that installs this as a plugin — e.g. something this library's
  skills/agents/commands are built to work with.
- **Scoped to one agent**: use the `mcpServers` frontmatter field on an individual file in
  [`agents/`](../agents) instead (see [`docs/agents.md`](./agents.md)) — the server connects
  only while that agent runs, and its tools never show up in the main conversation.
- **Personal, not shared**: use your own `~/.claude.json` or a project's `.mcp.json` in that
  project instead of adding it here, since anything in this file ships to everyone who installs
  this repo as a plugin.

## Format

```json
{
  "mcpServers": {
    "server-name": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@some-org/mcp-server"]
    }
  }
}
```

Supports `stdio`, `http`, `sse`, and `ws` server types — see the
[MCP configuration reference](https://code.claude.com/docs/en/mcp#installing-mcp-servers) for
the full schema, including how to reference secrets via environment variables instead of
hardcoding them here (this file is committed to a public-facing repo).

`${CLAUDE_PLUGIN_ROOT}` is available in command/arg values here and resolves correctly both when
this repo is opened directly and when it's installed elsewhere as a plugin — use it instead of a
relative path if a server needs to reference another file in this repo (e.g. a bundled script).
