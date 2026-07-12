# Personal Claude Skills Library

This directory is a personal library of reusable [Claude Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) — self-contained folders that give Claude Code (and other Claude Agent SDK apps) a specific, on-demand capability.

## Layout

Each skill lives in its own folder under `skills/`:

```
skills/
  <skill-name>/
    SKILL.md        # required — frontmatter + instructions
    references/     # optional — longer docs the agent reads on demand
    scripts/         # optional — helper scripts the skill can run
    assets/          # optional — templates, fixtures, etc. the skill uses or produces
```

`skills/_template/` contains a blank skeleton to copy when starting a new skill.

## SKILL.md format

```markdown
---
name: skill-name
description: One or two sentences, third person, stating what the skill does and — critically — when to use it (the trigger). This is the only text an agent sees before deciding to load the skill, so front-load the triggering conditions.
---

# Skill Name

Instructions the agent follows once the skill is loaded...
```

Guidelines:
- `name`: lowercase, hyphenated, matches the folder name.
- `description`: written for an agent deciding *whether* to invoke this skill — be concrete about triggers ("use when X", "use before Y"), not just a summary of what it does.
- Keep `SKILL.md` itself short; push long reference material into `references/*.md` and link to it, so it's only loaded when actually needed.
- Scripts in `scripts/` should be runnable standalone (with documented usage in `SKILL.md`) rather than requiring the agent to read and reimplement logic.

## Adding a new skill

1. Copy `skills/_template` to `skills/<new-skill-name>`.
2. Fill in `SKILL.md` — name, description (with clear trigger conditions), and instructions.
3. Add any `references/`, `scripts/`, or `assets/` the skill needs.
4. Add a row to the skill index below.

## Third-party skills: installed, not vendored

Skills published by other projects (Supabase, Vercel, etc.) are installed with the
[`skills` CLI](https://github.com/vercel-labs/skills) rather than hand-copied into this
folder. That tool:

- Clones the source repo and writes the real skill files to `.agents/skills/<name>/`
  (the canonical, tool-managed copy) at the repo root.
- Symlinks `.claude/skills/<name>` → `.agents/skills/<name>` so Claude Code picks it up.
- Records what's installed, from where, and a content hash in `skills-lock.json` at the
  repo root — that file is the source of truth for what third-party skills exist and where
  they came from.

`skills/` (this folder) is reserved for skills we author ourselves. To pull in a third-party
skill:

```bash
npx skills add <owner>/<repo> --skill <skill-name>   # e.g. supabase/agent-skills --skill supabase
npx skills add <owner>/<repo> --skill '*'            # install every skill in the repo
```

See [`THIRD_PARTY_NOTICES.md`](./THIRD_PARTY_NOTICES.md) for attribution/licensing of what's
currently installed this way.

### Currently installed via `npx skills add`

| Skill | Source | Description |
| --- | --- | --- |
| `supabase` | `supabase/agent-skills` | Schema/migrations, RLS security traps, CLI/MCP troubleshooting, docs lookup discipline. |
| `supabase-postgres-best-practices` | `supabase/agent-skills` | Postgres performance/schema/locking/security best practices. |
| `zod` | `anivar/zod-skill` | Zod v4 API guidance — parsing, schema design, refinements/transforms, error handling, v3→v4 migration. Repo keeps skill files at its root rather than under `skills/<name>/`, so `rules/`, `references/`, `AGENTS.md`, and `LICENSE` were added by hand after install to match upstream — see `THIRD_PARTY_NOTICES.md`. |

Not yet installed, but recommended when Vercel deployment work comes up:

```bash
npx skills add vercel-labs/agent-skills --skill deploy-to-vercel,vercel-cli-with-tokens,vercel-optimize
```

## Skill index (authored here)

| Skill | Description |
| --- | --- |
| [`vercel-optimize`](./vercel-optimize) | *(pointer only)* Explains why Vercel's cost/performance audit skill isn't vendored here and gives the real install command. Superseded once the skill above is actually installed via `npx skills add`. |
