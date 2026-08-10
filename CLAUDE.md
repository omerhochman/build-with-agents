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
  Never an open question that exports the analysis to the human.

**Escalation bar (reversibility × blast radius):** decide-and-record anything
reversible and local (tuning, file structure, libs within the stack) per the
memory rules. Escalate only what's hard to reverse or shapes the product:
user-visible interaction, money, privacy posture, dependency lock-in — and
anything contradicting a firm call (hard stop, always).

## Firm calls — do not re-litigate

<!-- template: written at bootstrap, signed off by the human merging the
     bootstrap PR. A firm call is a decision that would be expensive to
     reverse or that agents would otherwise reopen every run: scope cuts
     ("X waits for v1.1"), stack choices, privacy posture, monetization
     stance. One bold phrase + one line of why each. 5–10 entries; fold out
     entries that go stale rather than letting the list grow. -->

- ⟨**Scope call.** What v0 is and is not, and what waits for later.⟩
- ⟨**Stack call.** The chosen stack, and the rejected obvious alternative.⟩
- ⟨**Product-posture call.** e.g. privacy stance, offline stance.⟩
- ⟨**Monetization call.** e.g. what stays free, whether ads are acceptable.⟩

## Memory rules

This file plus `docs/` is the project memory. It stores **current state, not
event history** — git is the only event log (`git log -p docs/` for decision
history). There is no decisions file or ADR directory; do not create one.

**Placement rule:** document a decision at the narrowest scope where everyone
who needs it will already be looking:

1. **Line/function-specific** → why-comment at the site. Bar: would a
   competent dev plausibly "fix" this into a bug? Non-obvious constants,
   deliberately unidiomatic code, rejected obvious alternatives — yes.
   Anything readable from the code itself — no comment.
2. **Module-specific** → doc block at the top of the file: invariants, the
   approach chosen, and alternatives that **failed and why** ("tried X, broke
   because Y" — the most valuable sentence you can leave behind).
3. **System-level** → the relevant `docs/*.md`, **edited in place** to state
   the new current truth, with a one-clause "why" where non-obvious.
4. **Never-relitigate** → the firm-calls list above.

**Shared reasoning:** if the same rationale covers 2+ sites, first try to
unify in code (named constant / helper — then it has one site again).
Otherwise write it once at the lowest common ancestor (module doc block, or
`docs/`) and leave one-line pointers at each site. **Never paste the same
explanation twice** — copies diverge silently.

**Superseded decisions:** edit the doc, don't append. A doc may only say one
thing at a time.

**Size budget:** this file stays under ~100 lines. To add a firm call or rule,
fold out whatever it makes stale. Docs scale with system complexity, never
with project age.

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
