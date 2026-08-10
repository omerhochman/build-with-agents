# ⟨Project name⟩ — Agent Instructions

<!-- template: 2–4 lines. The product one-liner, the core mechanic, and the
     sequencing thesis (what ships first and why). End with a pointer to
     docs/PRODUCT.md. If this file still contains ⟨placeholders⟩, the project
     is not bootstrapped — stop and follow docs/AGENTS.md § Bootstrap. -->

⟨One-liner and core mechanic.⟩ ⟨Sequencing thesis: what ships first and what
waits.⟩ Framing: [docs/PRODUCT.md](docs/PRODUCT.md).

## Task loop

1. Check [blocked-by-human.md](blocked-by-human.md): finish any entry that is
   now answered or satisfiable (e.g. the env var exists), deleting the entry
   in the same commit.
2. List open PRs — **an open PR is a claim on its roadmap item**; skip
   claimed items. Take the first unchecked, unblocked, unclaimed item in
   [docs/ROADMAP.md](docs/ROADMAP.md) (unless the user names a different task).
3. Do the work **on a fresh branch, and open a PR to `main`** titled with the
   roadmap item verbatim (that's what makes claims checkable). Never push to
   `main` directly — a daily reviewer-merger agent reviews and merges.
   If a step needs a human (secret, account action, pivotal decision): do
   everything up to that boundary — but never build past a decision fork —
   file the blocker (see below), mark the roadmap item
   `⛔ see blocked-by-human.md`, and take the next task. **Never end a run
   early because one task is blocked**; end only when nothing is workable,
   with a report of what's pending.
4. In the **same PR**: check the box, and update any doc this change made
   untrue (see memory rules below).

## Human-blocked work

[blocked-by-human.md](blocked-by-human.md) is the async agent↔human mailbox.
It holds **only open blockers** — the resolving agent deletes the entry.
Each entry: the roadmap item, what's already done, and the ask:

- **Secrets / account actions:** a precise, executable ask ("set
  `DEPLOY_TOKEN` in the host's project env") — never "configure X". Never ask
  for a secret in chat or a commit; code reads from env from day one.
- **Decisions:** a brief — options, one-line tradeoffs, and a recommendation.
  Never an open question that exports the analysis to the human. Often the
  cheapest ask is approval for a doc/firm-call amendment that unblocks the
  whole decision class, not just this instance.

**Escalation bar (reversibility × blast radius):** first try to disambiguate
from the firm calls and `docs/` — a question the docs already answer is
never a blocker. Then: decide-and-record anything reversible and local
(tuning, file structure, libs within the stack, implementing what a spec
already says) per the memory rules. Escalate what's hard to reverse or
shapes the product: interaction design the specs don't cover, money, privacy
posture, dependency lock-in — and anything contradicting a firm call (hard
stop, always). Genuinely unsure after checking the docs → escalate, with a
recommendation.

## Firm calls — do not re-litigate

<!-- template: written at bootstrap, signed off by the human merging the
     bootstrap PR. A firm call is a decision that would be expensive to
     reverse or that agents would otherwise reopen every run: scope cuts
     ("X waits for v1.1"), stack choices, privacy posture, monetization
     stance. One bold phrase + one line of why each. 5–10 entries; fold out
     entries that go stale rather than letting the list grow. The two
     pre-seeded calls below apply to every project — keep them. -->

- **Research before build.** Before implementing anything non-trivial,
  survey the current landscape and prefer the most modern, popular, actively
  maintained tool/library — or the established best practice — over
  hand-rolling.
- **Zero cost to the founder.** Anything adopted must be entirely free or
  have a freemium tier that covers our usage. A tool that would cost money
  is a blocked-by-human decision, never a default.
- ⟨**Scope call.** What v0 is and is not, and what waits for later.⟩
- ⟨**Stack call.** The chosen stack, and the rejected obvious alternative.⟩
- ⟨**Product-posture call.** e.g. privacy stance, offline stance.⟩
- ⟨**Monetization call.** e.g. what stays free, whether ads are acceptable.⟩

## Memory rules

Project memory is five tiers, one per kind of knowledge: truth lives in a
git-versioned tree co-located with code; constraints stay always-loaded and
tiny; conventions compile to machinery; history stays verbatim in git.

**Tier 0 — enforcement: lint, types, CI, hooks.** A convention's adult form;
prose is its larval stage. If a repo tool can enforce a constraint
deterministically, **the tool is the constraint**: add the check, then delete
the prose that stated it. Never keep a "don't use X" rule where you can
delete X or lint it away — negative rules keep X in attention.

**Tier 1 — this file, always loaded.** Identity, task loop, firm calls,
these rules, and a **complete pointer index** to everything below — a doc
not reachable from here effectively doesn't exist. Inclusion bar, all three:
deleting it would cause mistakes; it isn't derivable from the code (no
architecture overviews an agent can read from source); the lesson has come
up **twice** — record rules on the second occurrence, never the first. The
~100-line budget is a tripwire, not the mechanism; churn here means the
content belongs in a lower tier.

**Tier 2 — scoped truth: `docs/` and module memory.** Docs are **maps, not
manuals** — components, interfaces, invariants, and alternatives that
**failed and why** ("tried X, broke because Y" — the most valuable sentence
you can leave behind). Edited in place; a doc may only say one thing at a
time; docs scale with system complexity, never with project age. As code
lands, detail migrates down to the narrowest scope with all its readers:
line/function → why-comment at the site (bar: would a competent dev
plausibly "fix" this into a bug? readable from the code → no comment);
module → doc block atop the file; system → the relevant `docs/*.md`.
Shared reasoning: unify in code first (named constant / helper — one site
again), else write it once at the lowest common ancestor with one-line
pointers — **never paste the same explanation twice**; copies diverge
silently.

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
| [docs/ROADMAP.md](docs/ROADMAP.md) | Task queue — phased checkboxes |
| [docs/AGENTS.md](docs/AGENTS.md) | Worker & reviewer-merger protocols, cron prompts |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Stack, components, data model |
| [docs/UX.md](docs/UX.md) | Core interaction spec |
| [docs/PRIVACY.md](docs/PRIVACY.md) | Data lifecycle, guarantees |
| [docs/RISKS.md](docs/RISKS.md) | Monetization stance, key risks |
