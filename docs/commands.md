# Personal Slash Commands Library

Custom slash commands — explicit, on-demand instructions you invoke by name (`/command-name`)
rather than ones Claude decides to trigger itself. Modern Claude Code implements these as flat
Markdown skill files with `disable-model-invocation: true`, so they only ever run when you type
them.

Like [`skills/`](../skills) and [`agents/`](../agents), the [`commands/`](../commands) directory
serves two purposes:

1. **Self-loading for this repo.** `.claude/commands/` is a symlink to `commands/`, so any
   Claude Code session opened directly on this repo picks every command up automatically,
   invocable as `/command-name`.
2. **Installable elsewhere as a plugin.** See the root [`README.md`](../README.md) for the
   install command. Once installed elsewhere, these commands are namespaced as
   `/aphelion-ix:command-name`.

<!--
IMPORTANT: this documentation file intentionally lives in docs/, not in commands/ itself.
Claude Code's plugin loader treats every .md file directly inside a plugin's `commands/`
directory as a command definition — a prose doc file dropped straight into `commands/` risks
showing up as a broken or bogus command entry.
-->

## Layout

```
commands/
  <command-name>.md   # flat file — YAML frontmatter + instructions, no subfolder needed

.claude/commands   → symlink to ../commands
```

`commands/_template.md` contains a blank skeleton to copy when starting a new command. It also
registers as an actual (if inert) command once loaded, the same way `skills/_template/` behaves
in the skills library — give it a real `description` before it ships to anyone, or delete it if
that's undesirable in your workflow.

## Command file format

```markdown
---
description: What this command does — shown in command listings.
disable-model-invocation: true
---

Instructions Claude follows when this command runs. Use `$ARGUMENTS` to capture
whatever text follows the command name.
```

Guidelines:
- Keep `disable-model-invocation: true` unless you specifically want Claude to also be able to
  trigger this on its own judgement — that turns it into an ordinary auto-invoked skill instead
  of a pure command, at which point it probably belongs in `skills/` instead.
- Prefer `skills/` (folder-per-skill, with `references/`/`scripts/`/`assets/`) for anything that
  needs supporting files. Use flat `commands/*.md` for short, self-contained instructions only.

## Adding a new command

1. Copy `commands/_template.md` to `commands/<new-command-name>.md`.
2. Fill in the frontmatter and instructions.
3. Add a row to the index below.
4. Run `/reload-plugins` (or restart the session, if this is the first command added since the
   session started) to pick it up.

## Command index

| Command | Description |
| --- | --- |
| *(none yet — add one!)* | |
