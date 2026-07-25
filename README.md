## Hi there 👋

<!--
**Aphelion-IX/Aphelion-IX** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

### 🛠️ Skills & Agents Library

This repo also hosts a personal library of reusable [Claude Code](https://code.claude.com/docs/en/overview) extensions:

| Component | Directory | Docs |
| --- | --- | --- |
| Skills | [`skills/`](./skills) | [`skills/README.md`](./skills/README.md) |
| Subagents | [`agents/`](./agents) | [`docs/agents.md`](./docs/agents.md) |
| Slash commands | [`commands/`](./commands) | [`docs/commands.md`](./docs/commands.md) |
| Shared MCP servers | [`.mcp.json`](./.mcp.json) | [`docs/mcp.md`](./docs/mcp.md) |

**Two ways to use it:**

1. **Open this repo directly.** `.claude/skills`, `.claude/agents`, `.claude/commands`, and
   `.mcp.json` all load automatically — no install step. `.agents/skills` mirrors the same
   skills for tools that read that convention.
2. **Install it in another project as a plugin.** Everything above — skills, agents, commands,
   and MCP config — is also packaged as a single self-referencing plugin (see
   [`.claude-plugin/`](./.claude-plugin)):

   ```
   /plugin marketplace add Aphelion-IX/Aphelion-IX
   /plugin install aphelion-ix@aphelion-ix
   ```

   Installed this way, components are namespaced under `aphelion-ix:` (e.g.
   `/aphelion-ix:command-name`, `@agent-aphelion-ix:agent-name`).

   Skills specifically can also be pulled in individually without the plugin system, via
   `npx skills add Aphelion-IX/Aphelion-IX --skill <skill-name>` — see
   [`skills/README.md`](./skills/README.md) for why that CLI exists alongside the plugin path.
