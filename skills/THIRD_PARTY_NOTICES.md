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

## `.agents/skills/deploy-to-vercel/` (symlinked at `.claude/skills/deploy-to-vercel`)

- Source: https://github.com/vercel-labs/agent-skills (`skills/deploy-to-vercel`)
- Copyright (c) Vercel, Inc.
- License: MIT
- Note: upstream also ships a redundant `Archive.zip` (the same `SKILL.md` +
  `resources/*.sh` re-packaged, with macOS `__MACOSX` cruft) alongside the unpacked files;
  we deleted it after install since it duplicated content already present unpacked.

## `.agents/skills/vercel-cli-with-tokens/` (symlinked at `.claude/skills/vercel-cli-with-tokens`)

- Source: https://github.com/vercel-labs/agent-skills (`skills/vercel-cli-with-tokens`)
- Copyright (c) Vercel, Inc.
- License: MIT

## `.agents/skills/vercel-composition-patterns/` (symlinked at `.claude/skills/vercel-composition-patterns`)

- Source: https://github.com/vercel-labs/agent-skills (`skills/composition-patterns`)
- Copyright (c) Vercel, Inc.
- License: MIT

## `.agents/skills/vercel-react-best-practices/` (symlinked at `.claude/skills/vercel-react-best-practices`)

- Source: https://github.com/vercel-labs/agent-skills (`skills/react-best-practices`)
- Copyright (c) Vercel, Inc.
- License: MIT

## Not yet installed

`vercel-optimize` from https://github.com/vercel-labs/agent-skills (Copyright (c) Vercel,
Inc., MIT License) is not currently installed — see [`README.md`](./README.md) for why and
the install command. Add a row here when it is.

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
