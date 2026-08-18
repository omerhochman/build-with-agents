# Vetted free-tier providers

The default menu for the **zero-cost firm call** — every entry has a
no-card free tier proven in production by a sibling project. Bootstrap
picks from here first (recording picks + rejected alternatives in
[ARCHITECTURE.md](ARCHITECTURE.md)); going off-menu requires the usual
research-before-build pass. Secret names are the convention — code reads
them from env, `.env.example` manifests them, actual values arrive via
[blocked-by-human.md](../blocked-by-human.md).

| Need | Default | Free-tier gotcha to design around | Secrets |
|------|---------|-----------------------------------|---------|
| Hosting / compute | [Cloudflare Workers + Pages](https://developers.cloudflare.com/workers/) | 3 MiB compressed bundle cap on free — check every heavy dep against it | `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID` |
| Static / frontend alt | [Vercel](https://vercel.com/pricing) or [GitHub Pages](https://pages.github.com/) | Vercel free is non-commercial-ish Hobby; Pages is public-repo-or-paid | `VERCEL_TOKEN` |
| Postgres | [Neon](https://neon.tech/pricing) | 10-branch cap per project — ephemeral CI branches must sweep + expire | `DATABASE_URL`, `NEON_API_KEY`, `NEON_PROJECT_ID` |
| DB + auth + storage in one | [Supabase](https://supabase.com/pricing) | free projects pause after 1 week idle — fine pre-launch, plan a keepalive | `SUPABASE_URL`, `SUPABASE_ANON_KEY` |
| Auth (self-hosted) | [Better Auth](https://better-auth.com) | library, not a service — you own the session table | — |
| LLM | [Gemini free tier](https://ai.google.dev/pricing), [Groq](https://groq.com/pricing), [OpenRouter :free models](https://openrouter.ai/models?q=free) | the key's project must have **no billing account linked** — a billed project bills even free-model calls and suspends on non-payment; route through a provider-agnostic call site so the provider is swappable | `GEMINI_API_KEY`, `GROQ_API_KEY`, `OPENROUTER_API_KEY` |
| Transactional email | [Resend](https://resend.com/pricing) | 100/day free; sending domain needs DNS records (human action) | `RESEND_API_KEY` |
| Payments | [Stripe](https://stripe.com/pricing) | no monthly fee, per-transaction only — safe to wire pre-revenue | `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET` |
| Pageview analytics | [Cloudflare Web Analytics](https://www.cloudflare.com/web-analytics/) | free, no SDK, no cookie banner — default for marketing pages | — |
| Product analytics | [PostHog](https://posthog.com/pricing) | generous free events; keep the client SDK off marketing pages | `POSTHOG_API_KEY` |
| Bot / abuse protection | [Cloudflare Turnstile](https://developers.cloudflare.com/turnstile/) | free, CAPTCHA-less | `TURNSTILE_SECRET_KEY` |
| Error/trace backend | [Grafana Cloud free](https://grafana.com/pricing/) or [Sentry free](https://sentry.io/pricing/) | instrument via OpenTelemetry so the backend is swappable | `OTEL_EXPORTER_OTLP_ENDPOINT` |
| CI | GitHub Actions | free for public repos; 2,000 min/mo private | — |

Rows are maintained like any doc: a provider that burns us gets its
gotcha updated (or the row replaced) in the same PR that felt the burn.

## Preferred stack (TypeScript, production-proven)

The default stack for a web/API product, inherited from a sibling
production repo (nlqdb). Bootstrap starts here; deviating requires the
usual research-before-build pass and a recorded rejection in
ARCHITECTURE.md.

| Layer | Pick | Why / gotcha |
|-------|------|--------------|
| Runtime + workspace | [Bun](https://bun.sh) monorepo (`apps/*`, `packages/*`) | one lockfile, built-in test runner for unit tests |
| Language | TypeScript, strict | zod at every external boundary |
| Lint/format | [Biome](https://biomejs.dev) | one tool replaces eslint+prettier; `check`/`fix` scripts |
| Git hooks | [lefthook](https://github.com/evilmartians/lefthook) | run Biome pre-commit so CI rarely reds |
| API framework | [Hono](https://hono.dev) on Cloudflare Workers | 3 MiB free-tier bundle cap — audit heavy deps |
| DB access | [Kysely](https://kysely.dev) (+ `kysely-d1` on D1, or Neon serverless) | typed SQL, no ORM magic; one owning `packages/db` module |
| Auth | [Better Auth](https://better-auth.com) | library not service; you own the session table |
| Web frontend | [Astro](https://astro.build) + React islands | zero-JS static pages by default; islands only where interactive |
| MCP surface | [`@modelcontextprotocol/sdk`](https://github.com/modelcontextprotocol/typescript-sdk) | serve from the same Worker as the API — one deploy, one origin |
| Tests | vitest (unit/integration) + Playwright (e2e) | `@cloudflare/vitest-pool-workers` to test Workers for real |
| Releases | [changesets](https://github.com/changesets/changesets) | only if publishing packages; apps just deploy |

Rejected default: Next.js + Vercel + Neon + separate auth service — a
platform and account per layer, each one a new secret ask and failure
mode. Cloudflare-only keeps one deploy target and one secret pair.
