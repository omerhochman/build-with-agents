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

3. **Review and merge the bootstrap PR.** Merging is how you sign off on the
   framing, the stack, and the firm calls — read them like a contract.
4. **Resolve the entries in [blocked-by-human.md](blocked-by-human.md)**
   (branch protection, agent scheduling). From then on the loop runs itself.

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
  ask in `blocked-by-human.md` and moves on to the next task. Runs never end
  early because one task is blocked.
- **The repo is the only memory — five tiers, one per kind of knowledge.**
  Enforcement (lint/CI/hooks — conventions compile to machinery and the
  prose is deleted), a tiny always-loaded root file that indexes everything,
  scoped docs-as-maps edited in place, coordination state (roadmap boxes,
  PR claims, the mailbox), and verbatim git history — queried, never
  summarized into docs. Every PR that makes a doc untrue must fix that doc
  in the same PR — otherwise later agents inherit lies.
- **Firm calls end debates.** Decisions the human has signed off on live in
  `CLAUDE.md` and are never re-litigated; contradicting one is a hard stop.
