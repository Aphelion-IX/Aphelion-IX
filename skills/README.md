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

## Skill index

| Skill | Description |
| --- | --- |
| [`supabase`](./supabase) | Guidance for working with a Supabase project — schema/migrations, debugging via logs/advisors, RLS, and client integration. |
| [`vercel`](./vercel) | Guidance for deploying and operating a project on Vercel — config, environment variables, preview deployments, and debugging failed builds. |
