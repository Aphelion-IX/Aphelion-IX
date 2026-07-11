---
name: vercel
description: Use when deploying or configuring a project on Vercel — setting up or debugging a deployment, managing environment variables/secrets, configuring vercel.json (rewrites, redirects, headers, functions), working with preview deployments, or diagnosing a build/runtime failure via deployment logs.
---

# Vercel

Guidance for deploying and operating a project on Vercel.

## When to use

- Setting up a new project for Vercel deployment or debugging why a deployment failed
- Managing environment variables across Production / Preview / Development
- Configuring `vercel.json` (rewrites, redirects, headers, cron jobs, function runtime/region)
- Investigating a build or runtime error via deployment logs
- Working with preview deployments tied to branches/PRs

## Before deploying

1. Check for an existing `vercel.json` and framework preset — most frameworks (Next.js, SvelteKit, Remix, etc.) are auto-detected and need no config; only add `vercel.json` for things the preset can't express (custom rewrites/redirects/headers, cron, non-default function settings).
2. Confirm the build command and output directory match what the project actually produces (`vercel build` locally reproduces the platform build if something is unclear).
3. Never commit secrets — use environment variables (`vercel env add`) instead of hardcoding them in `vercel.json` or source.

## Environment variables

- Vercel has three scopes: Production, Preview, Development. Set each explicitly — a var added only to Production will be undefined in preview deployments, a common source of "works on my branch, breaks in prod (or vice versa)" bugs.
- Client-exposed vars must be prefixed per-framework (`NEXT_PUBLIC_`, `VITE_`, `PUBLIC_`, etc.) — anything without the prefix stays server-only, which is usually what you want for API keys/secrets.
- Use `vercel env pull` to sync remote env vars into a local `.env.local` for local dev, rather than hand-copying values.

## Debugging a failed deployment

1. Read the build log first (`vercel logs <deployment-url>` or the dashboard) — most failures are build-time (missing env var, dependency, or type error), not platform issues.
2. For runtime errors on serverless/edge functions, check function logs specifically — build success doesn't guarantee the function runs correctly at request time.
3. Reproduce locally with `vercel dev` (or `vercel build`) before assuming it's a platform-specific problem.

## Preview deployments

- Every push to a non-production branch/PR gets its own preview URL with its own env var scope (Preview) — use this to validate changes before they hit Production, including env-var-dependent behavior.
- Preview deployments share the Preview-scoped env vars across all branches unless branch-specific overrides are configured — don't assume a preview URL has Production data/config.

## Notes

- `vercel.json` `functions` config (runtime, memory, region, maxDuration) only applies to serverless/edge functions matched by its glob — verify the glob actually matches the deployed function paths.
- Prefer the Vercel CLI (`vercel`, `vercel --prod`, `vercel rollback`) for scriptable/repeatable operations over manual dashboard clicks.
