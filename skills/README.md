# Personal Claude Skills Library

This directory is a personal library of reusable [Claude Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) — self-contained folders that give Claude Code (and other Claude Agent SDK apps) a specific, on-demand capability.

See also [`agents/`](../agents) and [`commands/`](../commands) for the rest of this repo's
Claude Code library (subagents and slash commands), and the root [`README.md`](../README.md)
for how everything here also installs together as a single plugin in other projects.

This directory serves **two** purposes at once:

1. **A shared skill source.** Because every skill's real files live in this plain, git-visible
   `skills/` folder (not a hidden dot-directory), any other repo can pull them in with:

   ```bash
   npx skills add Aphelion-IX/Aphelion-IX --skill <skill-name>   # one skill
   npx skills add Aphelion-IX/Aphelion-IX -s '*'                 # everything
   ```

   This is the actual fix for "skills installed here don't show up in my other repos" —
   project-scoped skill installs (what `npx skills add` does by default) only apply to the
   repo they're installed into. There's no cross-repo or cross-session persistence otherwise
   (see "Why not global/`-g`?" below). To use these skills somewhere else, run the command
   above **in that other repo**.

2. **A self-loading skill set for this repo.** `.agents/skills/<name>` and `.claude/skills/<name>`
   are symlinks into `skills/<name>/`, so Claude Code sessions opened directly on this repo
   pick everything up automatically too, with no extra install step.

## Layout

```
skills/
  <skill-name>/
    SKILL.md        # required — frontmatter + instructions
    references/     # optional — longer docs the agent reads on demand
    scripts/         # optional — helper scripts the skill can run
    assets/          # optional — templates, fixtures, etc. the skill uses or produces

.agents/skills/<skill-name>   → symlink to ../../skills/<skill-name>
.claude/skills/<skill-name>   → symlink to ../../skills/<skill-name>
```

`skills/_template/` contains a blank skeleton to copy when starting a new skill.

**Important:** keep skill content under the plain `skills/` folder, not under `.agents/` or
`.claude/` directly. Those two are dot-prefixed, and the `skills` CLI's own repo scanner
(what runs when another repo does `npx skills add Aphelion-IX/Aphelion-IX`) skips
dot-directories when looking for `SKILL.md` files — confirmed by testing it against this repo
before this layout existed; only skills already sitting in the visible `skills/` folder showed
up. Anything installed straight into `.agents/skills/<name>` (the tool's own default output
location) needs to be moved into `skills/<name>/` and re-symlinked, the same way we migrated
the first seven skills here.

## Why not global (`-g`) install instead?

`npx skills add <repo> -g` installs to the machine's home directory (`~/.agents`,
`~/.claude`) so it's available to every project on that machine, without needing to touch each
repo. That works well on a persistent local machine. It does **not** work in Claude Code's
remote cloud sessions (like the one used to build this repo): each session runs in an
isolated, ephemeral container that's reclaimed after inactivity or when the session ends, so a
global install there is gone as soon as the container is. Project-scoped install into a
repo you actually commit and push is the only thing that survives across sessions in that
environment — which is exactly what installing from this shared repo (option 1 above) gives
you, regardless of which kind of environment you're running in.

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

## Adding a new skill authored here

1. Copy `skills/_template` to `skills/<new-skill-name>`.
2. Fill in `SKILL.md` — name, description (with clear trigger conditions), and instructions.
3. Add any `references/`, `scripts/`, or `assets/` the skill needs.
4. Add a row to the skill index below.
5. Symlink it in: `ln -s ../../skills/<name> .agents/skills/<name> && ln -s ../../skills/<name> .claude/skills/<name>`.

## Adding a third-party skill

```bash
npx skills add <owner>/<repo> --skill <skill-name>
```

This installs to `.agents/skills/<name>` (real files) with `.claude/skills/<name>` symlinked to
it, and records source + a content hash in `skills-lock.json` at the repo root. **Then move it
into the shared layout** so it's discoverable from other repos too:

```bash
name=<skill-name>
mv ".agents/skills/$name" "skills/$name"
rm ".claude/skills/$name"
ln -s "../../skills/$name" ".agents/skills/$name"
ln -s "../../skills/$name" ".claude/skills/$name"
```

Note: this tool's `--skill a,b,c` comma-separated form has silently matched nothing for us
every time; use a separate `-s <name>` per skill instead, or `-s '*'` for everything in a repo.

See [`THIRD_PARTY_NOTICES.md`](./THIRD_PARTY_NOTICES.md) for attribution/licensing of what's
currently installed this way.

## Skill index

| Skill | Source | Description |
| --- | --- | --- |
| `supabase` | `supabase/agent-skills` | Schema/migrations, RLS security traps, CLI/MCP troubleshooting, docs lookup discipline. |
| `supabase-postgres-best-practices` | `supabase/agent-skills` | Postgres performance/schema/locking/security best practices. |
| `zod` | `anivar/zod-skill` | Zod v4 API guidance — parsing, schema design, refinements/transforms, error handling, v3→v4 migration. Repo keeps skill files at its root rather than under `skills/<name>/`, so `rules/`, `references/`, `AGENTS.md`, and `LICENSE` were added by hand after install to match upstream — see `THIRD_PARTY_NOTICES.md`. |
| `deploy-to-vercel` | `vercel-labs/agent-skills` | Deployment decision tree (git push vs. CLI vs. link-first vs. no-auth fallback); always previews unless production is explicit. |
| `vercel-cli-with-tokens` | `vercel-labs/agent-skills` | Deploying/managing a Vercel project via CLI with token-based auth instead of `vercel login`. |
| `vercel-react-best-practices` | `vercel-labs/agent-skills` | 70+ React/Next.js performance rules (rendering, re-renders, async/data fetching, bundling, server components) from Vercel Engineering. |
| `vercel-composition-patterns` | `vercel-labs/agent-skills` | React composition strategies — compound components, avoiding boolean-prop proliferation, children over render props, React 19 API changes (no `forwardRef`). |
| [`vercel-optimize`](./vercel-optimize) | *(authored here, pointer only)* | Explains why Vercel's cost/performance audit skill isn't vendored here (needs Vercel CLI v53+ and paid Observability Plus) and gives the real install command. |

Not yet installed:

```bash
npx skills add vercel-labs/agent-skills -s vercel-optimize
```
