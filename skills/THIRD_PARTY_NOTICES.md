# Third-Party Notices

Skills installed via `npx skills add` (see [`README.md`](./README.md)) are copies of skills
published by other projects, under the MIT License. This file collects the required
attribution for what's currently installed. `skills-lock.json` at the repo root is the
authoritative record of source + version; this file is the human-readable license summary.

## `.agents/skills/supabase/` (symlinked at `.claude/skills/supabase`)

- Source: https://github.com/supabase/agent-skills (`skills/supabase`)
- Copyright (c) 2026 Supabase
- License: MIT

## `.agents/skills/supabase-postgres-best-practices/` (symlinked at `.claude/skills/supabase-postgres-best-practices`)

- Source: https://github.com/supabase/agent-skills (`skills/supabase-postgres-best-practices`)
- Copyright (c) 2026 Supabase
- License: MIT

## `.agents/skills/zod/` (symlinked at `.claude/skills/zod`)

- Source: https://github.com/anivar/zod-skill
- Copyright (c) 2026 Anivar Aravind
- License: MIT
- Note: this repo keeps its skill files (`SKILL.md`, `rules/`, `references/`, `AGENTS.md`) at
  the repo root rather than under a `skills/<name>/` subfolder, so `npx skills add` only pulled
  `SKILL.md` — the `rules/`, `references/`, `AGENTS.md`, and `LICENSE` files were added manually
  to match upstream, since `SKILL.md` itself points at those paths.

## Not yet installed

`deploy-to-vercel`, `vercel-cli-with-tokens`, and `vercel-optimize` from
https://github.com/vercel-labs/agent-skills (Copyright (c) Vercel, Inc., MIT License) are not
currently installed. Add a row here when they are (`npx skills add vercel-labs/agent-skills
--skill <name>`).

`skills/vercel-optimize/SKILL.md` in this repo is a short original pointer written by us — not
a copy of upstream content — so it carries no separate attribution obligation.

---

MIT License text (applies to all installed skills above):

```
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
