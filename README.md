# Build with agents

A scaffolding template for **agent-driven side projects**: fork it, hand an
agent your idea in one paragraph, and get a repo where scheduled agents ship
PRs against a roadmap while you only answer for secrets, account actions, and
pivotal decisions.

> This README describes the template. During bootstrap an agent **replaces it**
> with your product's README — see [docs/AGENTS.md](docs/AGENTS.md) § Bootstrap.

## How to start a new idea

1. **Fork this repo** (or use it as a GitHub template) and name it after the
   idea.
2. **Run an agent** (Claude Code or similar, with repo access) with this
   prompt, filling in the blanks:

   > Read docs/AGENTS.md in `<owner>/<repo>` and follow the Bootstrap
   > protocol for this idea: `<one paragraph: what it is, who it's for, why
   > now — everything you know or believe about it>`

3. **Answer the bootstrap interview.** The agent asks one batch of numbered
   questions — only about what your paragraph left open, each with a
   proposed default — then opens the bootstrap PR.
4. **Review and merge the bootstrap PR.** Merging is how you sign off on the
   framing, the stack, and the firm calls — read them like a contract.
5. **Resolve the entries in [blocked-by-human.md](blocked-by-human.md)**
   (branch protection, agent scheduling) — each is a sub-minute checklist
   with links and copy-paste values. From then on the loop runs itself.

## What's inside

| File | Job |
|------|-----|
| [CLAUDE.md](CLAUDE.md) | Agent instructions: task loop, escalation rules, project memory rules |
| [blocked-by-human.md](blocked-by-human.md) | Async agent↔human mailbox — open blockers only |
| [docs/PRODUCT.md](docs/PRODUCT.md) | Framing, positioning, MVP scope |
| [docs/ROADMAP.md](docs/ROADMAP.md) | Task queue — phased checkboxes agents work through |
| [docs/AGENTS.md](docs/AGENTS.md) | Bootstrap, worker & reviewer-merger protocols, cron prompts |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Stack, components, data model |
| [docs/UX.md](docs/UX.md) | The core interaction spec |
| [docs/PRIVACY.md](docs/PRIVACY.md) | Data lifecycle and guarantees |
| [docs/RISKS.md](docs/RISKS.md) | Monetization stance, key risks |
| [docs/PROVIDERS.md](docs/PROVIDERS.md) | Vetted free-tier provider menu with gotchas and secret names |
| [.github/workflows/ci.yml](.github/workflows/ci.yml) | CI that's green from the first fork and turns itself on per stack |
| [.env.example](.env.example) | Manifest of every secret the code reads |

Template docs contain `⟨angle-bracket placeholders⟩` and
`<!-- template: … -->` guidance comments; bootstrap replaces the former and
deletes the latter.

## The model

- **Two scheduled agents, one repo.** A *worker* runs a few times daily: takes
  the next unclaimed roadmap item, ships it as a PR. A *reviewer-merger* runs
  daily: reviews, fixes small issues itself, squash-merges. Nothing reaches
  `main` without passing through it.
- **An open PR is a claim on its roadmap item** — that's what lets concurrent
  agent runs coexist without stepping on each other.
- **Humans are an async dependency, not a supervisor.** When a task needs a
  secret, an account action, or a pivotal decision, the agent files a precise
  step-by-step ask (links + copy-paste values) in `blocked-by-human.md` and
  moves on to the next task. Runs never end early because one task is
  blocked. Scheduled runs never ask questions — agents only interview you
  in-session (bootstrap, steering, roadmap brainstorming), and only about
  what the written decisions don't already answer.
- **Opinionated defaults, pre-seeded.** The template ships firm calls every
  project inherits: research free tools before building (paid upgrades only
  after revenue covers them), world-class UX over implementation cost,
  value before signup, errors that retry or state the next action, one way
  to do each thing, bad states unreachable by design, design for leverage,
  walking skeleton first, delete before you add, demo-able PRs. Plus a vetted free-tier provider menu
  ([docs/PROVIDERS.md](docs/PROVIDERS.md)) and a least-privilege CI
  workflow that passes on the empty template and activates per stack as
  code lands — production-tested defaults, not guesses.
- **The repo is the only memory — five tiers, one per kind of knowledge.**
  Enforcement (lint/CI/hooks — conventions compile to machinery and the
  prose is deleted), a tiny always-loaded root file that indexes everything,
  scoped docs-as-maps edited in place, coordination state (roadmap boxes,
  PR claims, the mailbox), and verbatim git history — queried, never
  summarized into docs. Every PR that makes a doc untrue must fix that doc
  in the same PR — otherwise later agents inherit lies.
- **Firm calls end debates.** Decisions the human has signed off on live in
  `CLAUDE.md` and are never re-litigated; contradicting one is a hard stop.
