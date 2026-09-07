# ⟨Project name⟩ — Agent Instructions

<!-- template: 2–4 lines. The product one-liner, the core mechanic, and the
     sequencing thesis (what ships first and why). End with the pointers to
     docs/PRODUCT.md and docs/END_GOAL.md. If this file still contains
     ⟨placeholders⟩, the project is not bootstrapped — stop and follow
     docs/AGENTS.md § Bootstrap. -->

⟨One-liner and core mechanic.⟩ ⟨Sequencing thesis: what ships first and what
waits.⟩ Framing: [docs/PRODUCT.md](docs/PRODUCT.md). The finished product,
flow by flow: [docs/END_GOAL.md](docs/END_GOAL.md).

## Task loop

1. Check [blocked-by-human.md](blocked-by-human.md): finish any entry that is
   now answered or satisfiable (e.g. the env var exists), deleting the entry
   in the same commit.
2. Read [docs/END_GOAL.md](docs/END_GOAL.md); where the roadmap contradicts
   it, fix the roadmap. Then list open PRs — **an open PR is a claim on its
   roadmap item; a draft PR is parked, never a claim**; skip claimed items.
   Take the first unchecked, unblocked, unclaimed item in
   [docs/ROADMAP.md](docs/ROADMAP.md) (unless the user names a task).
3. Do the work **on a fresh branch, and open a PR to `main`** titled with the
   roadmap item verbatim, body per the PR template. Never push to `main`
   directly — the scheduled run's reviewer mode merges.
   If a step needs a human (secret, account action, pivotal decision): do
   everything up to that boundary — but never build past a decision fork —
   file the blocker (see below), mark the roadmap item
   `⛔ see blocked-by-human.md`, and take the next task. **Never end a run
   early because one task is blocked**; end only when nothing is workable.
4. In the **same PR**: check the box, and update any doc this change made
   untrue (see memory rules below).

## Human-blocked work

[blocked-by-human.md](blocked-by-human.md) holds **only open blockers**. Each
entry: the roadmap item, what's already done, and the ask:

- **Secrets / account actions:** numbered steps the human can execute in
  under a minute — a clickable link (full URL) for every page to open, and
  a copy-paste-ready block for every command, form field, or value — never
  "configure X".
- **Decisions:** a brief — options, one-line tradeoffs, and a recommendation.
  Never an open question that exports the analysis to the human. Often the
  cheapest ask is approval for a doc/firm-call amendment that unblocks the
  whole decision class, not just this instance.

**Escalation bar (reversibility × blast radius):** a question the firm calls
or `docs/` already answer is never a blocker. Decide-and-record anything
reversible and local (tuning, file structure, libs within the stack,
implementing what a spec already says) per the memory rules. Escalate what's
hard to reverse or shapes the product: interaction design the specs don't
cover, money, privacy posture, dependency lock-in — and anything
contradicting a firm call (hard stop, always). Genuinely unsure → escalate.

**Questions are for humans-in-session only** (bootstrap, steering, roadmap
brainstorming); a scheduled run never asks mid-run.

## Firm calls — do not re-litigate

<!-- template: written at bootstrap, signed off by the human merging the
     bootstrap PR. A firm call is a decision that would be expensive to
     reverse or that agents would otherwise reopen every run: scope cuts
     ("X waits for v1.1"), stack choices, privacy posture, monetization
     stance. One bold phrase + one line of why each. 5–10 entries; fold out
     entries that go stale rather than letting the list grow. The pre-seeded
     calls below apply to every project — keep them. -->

- **Research before build.** Before implementing anything non-trivial,
  survey the current landscape and prefer the most modern, popular, actively
  maintained tool/library — or the established best practice — over
  hand-rolling. Hard-pass on any candidate that is pre-1.0/RC on the
  critical path, hasn't released in over a year, or drags in a heavy
  peer-dep tree. DIY still starts with 10 minutes reading how the
  canonical implementations do it.
- **Zero cost to the founder.** Anything adopted must be entirely free or
  have a freemium tier that covers our usage. A tool that would cost money
  is a blocked-by-human decision, never a default — and paid upgrades only
  become eligible once the product earns revenue that covers them. Pick
  providers from the vetted menu in [docs/PROVIDERS.md](docs/PROVIDERS.md)
  first; secrets are read from env and manifested in `.env.example`.
- **World-class UX.** Smoothness of the core journey outranks
  implementation cost — cut scope, never UX. Show honest progress in
  user-meaningful units, never a fake spinner or invented percentage.
- **Value before signup.** No login wall and no configuration before the
  user's first taste of value; ask for the least permission at the moment
  it becomes necessary, never up front.
- **Engineering bar — binding.** [docs/GUIDELINES.md](docs/GUIDELINES.md):
  one way per concept, bad states unreachable, leverage, delete-before-add,
  retries — reviewed against, not re-litigated.
- **Walking skeleton first.** Each phase starts with the thinnest
  end-to-end slice live at a real URL (or installable); everything after
  iterates on a live thing.
- **Demo-able PRs.** Every PR changes what a user can see or do, or adds
  leverage — and its `Walked:` line (PR template) proves it in under a minute.
- ⟨**Scope call.** What v0 is and is not, and what waits for later.⟩
- ⟨**Stack call.** The chosen stack, and the rejected obvious alternative.⟩
- ⟨**Product-posture call.** e.g. privacy stance, offline stance.⟩
- ⟨**Monetization call.** e.g. what stays free, whether ads are acceptable.⟩

## Memory rules

Project memory is five tiers, one per kind of knowledge.

**Tier 0 — enforcement: lint, types, CI, hooks.** If a repo tool can enforce
a constraint deterministically, **the tool is the constraint**: add the
check, then delete the prose that stated it. Never keep a "don't use X" rule
where you can delete X or lint it away — negative rules keep X in attention.

**Tier 1 — this file, always loaded.** Identity, task loop, firm calls,
these rules, and a **complete pointer index** to everything below — a doc
not reachable from here effectively doesn't exist. Inclusion bar, all three:
deleting it would cause mistakes; it isn't derivable from the code (no
architecture overviews an agent can read from source); the lesson has come
up **twice**. The ~100-line budget is a tripwire, not the mechanism; churn
here means the content belongs in a lower tier.

**Tier 2 — scoped truth: `docs/` and module memory.** Docs are **maps, not
manuals** — components, interfaces, invariants, and alternatives that
**failed and why** ("tried X, broke because Y"). Edited in place; a doc may
only say one thing at a time; docs scale with system complexity, never with
project age. As code lands, detail migrates down to the narrowest scope with
all its readers: line/function → why-comment at the site (bar:
docs/GUIDELINES.md § Comments); module → doc block atop the file; system →
the relevant `docs/*.md`.
Shared reasoning: unify in code first (named constant / helper), else write
it once at the lowest common ancestor with one-line pointers — **never paste
the same explanation twice**; copies diverge silently.

**Tier 3 — coordination state.** [docs/ROADMAP.md](docs/ROADMAP.md)
checkboxes are the queue, an open PR is the claim, the box checked in that
same PR is the completion record, blocked-by-human.md is the mailbox. All in
git — no external tracker.

**Tier 4 — history: git and PR threads, verbatim.** The only event log
(`git log -p docs/` for decision history) — queried, never bulk-loaded, and
**never summarized into memory files**. There is no decisions file or ADR
directory; do not create one. Any search index over the repo must be derived
and rebuildable — truth never moves into it.

## Docs index

| Doc | Contents |
|-----|----------|
| [docs/PRODUCT.md](docs/PRODUCT.md) | Framing, positioning, MVP scope |
| [docs/END_GOAL.md](docs/END_GOAL.md) | The finished product, flow by flow — north star for every item and PR |
| [docs/ROADMAP.md](docs/ROADMAP.md) | Task queue — phased checkboxes |
| [docs/AGENTS.md](docs/AGENTS.md) | Bootstrap + the scheduled run: dispatch, worker / reviewer-fixer-merger modes, cron prompt |
| [docs/GUIDELINES.md](docs/GUIDELINES.md) | Engineering + testing bar — swept from the template; the `Walked:` rule |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Stack, components, data model |
| [docs/UX.md](docs/UX.md) | Core interaction spec |
| [docs/PRIVACY.md](docs/PRIVACY.md) | Data lifecycle, guarantees |
| [docs/RISKS.md](docs/RISKS.md) | Monetization stance, key risks |
| [docs/PROVIDERS.md](docs/PROVIDERS.md) | Vetted free-tier provider menu, gotchas, secret names |
| [.github/workflows/ci.yml](.github/workflows/ci.yml) | Tier-0 backstop — self-skipping lint/typecheck/test per stack |
| [.github/pull_request_template.md](.github/pull_request_template.md) | PR body: roadmap item, `Walked:` line, blockers filed |
| [.github/dependabot.yml](.github/dependabot.yml) | Weekly grouped minor+patch bumps (bun, actions); majors separate |
