<!-- Source: build-with-agents/docs/GUIDELINES.md. Swept copy — edit the template, re-sweep. Repos may append domain sections (e.g. UX) below. -->
# Guidelines

The engineering bar: worker mode builds to it, reviewer mode reviews against
it, it settles what the firm calls and other docs leave open. A bullet enters on
the 2nd occurrence of a problem and leaves once a lint/type/CI/hook enforces it.

## Engineering

- **One way per concept.** One endpoint, one verb, one call shape; a second
  way to do the same thing is a finding, not a feature.
- **One owning module per external system** (API, DB, service, upstream
  feed). A second call site to the same system is a finding.
- **Bad states unreachable, not caught.** Idempotent mutations, additive
  schema changes, typed boundaries, parameterized queries. Before a
  user-visible feature ships, list its edge cases (race, double-submit, stale
  cache, wrong tenant, huge input) and how the design makes each unreachable.
- **Untrusted boundaries.** Secrets from env from day one — never in code,
  commits, or chat. Validate (Zod/pydantic or equivalent) at every external
  boundary; clamp upstream data and stamp its freshness; handle absence
  explicitly — missing data is never "OK".
- **Pure core / thin glue / one consumer.** Domain logic is pure, unit-tested
  modules with no framework or IO imports; IO and platform APIs live in glue
  that degrades to a status, never a crash; exactly one consumer composes them.
- **Errors and retries.** Recoverable failures retry to success — never
  surface an error the code could have fixed. A surfaced error is one
  sentence: what happened and the single next action.
- **Dependencies are lock-in and must cost nothing.** Platform API, then
  stdlib, then a dependency — each new one needs a reason a reviewer would
  accept and a free tier our usage stays inside. Pin the latest stable major,
  verified against the live registry (`npm view`, PyPI), never from memory.
- **Design for leverage.** Every merged PR adds a capability or capacity to
  add capabilities; the 2nd/3rd instance of anything is a category to abstract.
- **Delete before you add.** First remove or simplify code and docs; then add.
- **Exports take minimal required params with sensible defaults;** a call
  site that needs a doc to be understood has failed developer experience.
- **Logs tell a story, not the novel.** One structured line per decision
  point — what was attempted, why it failed, the next action; successes
  silent (spans and metrics carry them); no per-iteration chatter; never a
  secret, PII, or unbounded payload. Levels honest: `error` = look now,
  `warn` = this week, `info` = notice, `debug` = off in production.
- **Comments say why, never what.** One sentence, only where the reason would
  surprise a reader who sees only the code (a workaround, a constraint, a
  rejected alternative) — never narration, never reflex JSDoc on internals.
- **Docs in sync.** A PR that makes a doc — README included — untrue fixes
  it in the same PR.

## Testing bar

- **Pure logic gets unit tests** — that is where silent wrongness hides.
- **Integrations get fixture tests:** recorded upstream payloads, including
  malformed and empty ones; CI never calls a live upstream.
- **UI glue is verified by build + typecheck + a walk,** not simulated
  gestures — until the walk graduates (below).
- **Every PR body carries a `Walked:` line** (the PR template): END_GOAL flow
  step, environment, command or steps, observed result. The reviewer re-runs
  it before merging; a walk that cannot be reproduced is a finding.
- **Walks graduate.** A walk performed twice becomes a script under
  `scripts/` or a Playwright/E2E spec and the `Walked` line becomes that
  command; a third occurrence goes into CI.
