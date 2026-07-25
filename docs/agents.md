# Personal Subagents Library

Custom [Claude Code subagents](https://code.claude.com/docs/en/sub-agents) — specialized
assistants with their own system prompt, tool restrictions, and model, that Claude delegates to
for a specific kind of task.

Like [`skills/`](../skills), the [`agents/`](../agents) directory serves two purposes:

1. **Self-loading for this repo.** `.claude/agents/` is a symlink to `agents/`, so any Claude
   Code session opened directly on this repo picks every agent up automatically.
2. **Installable elsewhere as a plugin.** Unlike skills, there's no standalone CLI that copies
   raw agent files between repos. Cross-repo reuse goes through the
   [plugin](https://code.claude.com/docs/en/plugins) this repo ships as — see the root
   [`README.md`](../README.md) for the install command. Once installed, these agents are
   available in other projects as `aphelion-ix:<name>`.

<!--
IMPORTANT: this documentation file intentionally lives in docs/, not in agents/ itself.
Claude Code's plugin loader treats every .md file directly inside a plugin's `agents/`
directory as an agent definition (unlike `skills/`, which only recognizes `<name>/SKILL.md`
subfolders and ignores loose top-level files). A prose file with no agent frontmatter dropped
straight into `agents/` risks showing up as a broken or bogus agent entry.
-->

## Layout

```
agents/
  <agent-name>.md   # flat file — YAML frontmatter + system prompt, no subfolder needed

.claude/agents   → symlink to ../agents
```

`agents/_template.md` contains a blank skeleton to copy when starting a new agent. It also
registers as an actual (if inert) agent named `template-agent` once loaded — matching how
`skills/_template/` behaves in the skills library — so give it a real `name`/`description`
before it ships to anyone, or delete it if that's undesirable in your workflow.

## Agent file format

```markdown
---
name: agent-name
description: When Claude should delegate to this agent — this is the only text Claude sees before deciding to invoke it, so front-load the trigger conditions.
tools: Read, Grep, Glob
model: sonnet
---

System prompt the agent runs under once invoked...
```

Guidelines:
- `name`: lowercase, hyphenated. Must be unique across this directory.
- `description`: written for Claude deciding *whether* to delegate — state the trigger, not just a summary.
- `tools`: restrict to what the agent actually needs (defaults to inheriting everything if omitted).
- `model`: pick a cheaper model (`haiku`) for narrow, high-volume tasks; leave as `sonnet`/`opus` for anything requiring judgment.

See the [full frontmatter reference](https://code.claude.com/docs/en/sub-agents#supported-frontmatter-fields) for every supported field (`disallowedTools`, `permissionMode`, `skills`, `mcpServers`, `memory`, `isolation`, etc.). Note that plugin-shipped agents don't support `hooks`, `mcpServers`, or `permissionMode` — those are stripped when this repo is installed as a plugin elsewhere, even though they still work for the standalone `.claude/agents/` symlink path in this repo itself.

## Adding a new agent

1. Copy `agents/_template.md` to `agents/<new-agent-name>.md`.
2. Fill in the frontmatter and system prompt.
3. Add a row to the index below.
4. Run `/reload-plugins` (or restart the session, if this is the first agent added since the
   session started) to pick it up.

## Agent index

| Agent | Description |
| --- | --- |
| [`docker-socket-proxy`](../agents/docker-socket-proxy.md) | Sets up and debugs a Docker socket proxy (Tecnativa's `docker-socket-proxy`) — least-privilege Docker API access for containers instead of bind-mounting the raw socket. |
