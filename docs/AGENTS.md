# Agent roles

One scheduled agent drives this repo — a single cron, twice daily, that
dispatches itself into worker or reviewer mode. Its protocol lives here —
versioned with the code — so the cron prompt stays a one-liner pointing at
this file. (A second, one-shot role — Bootstrap — turns the template into a
project and then deletes itself from this file.)

## Bootstrap (one-shot — delete this section when done)

Turns this template into a real project from the human's idea brief (the
paragraph in the invoking prompt). Do it all **in one PR to `main`** titled
`Bootstrap: <project name>`, so the human signs off on the framing by merging:

0. **Interview the human — gaps only.** Bootstrap is an interactive session:
   before writing anything, ask **one batch** of numbered questions covering
   only what the idea brief leaves open — typically audience, the wedge,
   platform/stack constraints, product posture (privacy, offline),
   monetization stance, and the project name. Give each question a proposed
   default so a one-word answer (or "all defaults") works. Never ask what
   the brief or the pre-seeded firm calls already answer. If genuinely
   running unattended, skip the interview and propose everything decisively
   — merging the PR is the sign-off either way.
1. **Write [PRODUCT.md](PRODUCT.md) first** — one-liner, why now, MVP scope.
   Every other doc derives from it. The key move: find the **wedge** — the
   cheapest version that proves the core interaction/value, and push
   everything that needs accounts, backends, or policy to a later phase.
   The wedge is the interaction **in the human's words**. If they named a
   map, v0 In is the map. Do not invent a "Now screen" / "hero card" and
   put the named surface in v1.1 Out.
2. **Write [END_GOAL.md](END_GOAL.md) second** — the **finished product
   across all phases** (never a v0 snapshot; no phase labels inside the
   flows) through the user's eyes: personas, one happy-path table, states,
   non-goals — per its own "How to write this" rules. Every roadmap item
   derives from a flow step here, and every later PR is judged against it;
   in the report, ask the human to read this one line by line before merging.
   Row 1 of the happy path **is** the named home surface.
3. **Fill the remaining docs** ([ARCHITECTURE.md](ARCHITECTURE.md),
   [UX.md](UX.md), [PRIVACY.md](PRIVACY.md), [RISKS.md](RISKS.md),
   [ROADMAP.md](ROADMAP.md)): pick the stack's providers from
   [PROVIDERS.md](PROVIDERS.md) first (record picks and rejected
   alternatives in ARCHITECTURE.md; list their secret names in
   `.env.example` and file the value asks in blocked-by-human.md);
   replace every `⟨placeholder⟩`, delete every
   `<!-- template: … -->` comment. A doc with nothing true to say for this
   idea gets **deleted, not stubbed** (a UX spec may become an API spec, a
   privacy doc may be one paragraph) — rename or drop docs to fit the idea
   and keep both docs indexes true. The first ROADMAP items after plumbing
   are that named surface, not a data-depth catalog.
4. **Fill [CLAUDE.md](../CLAUDE.md):** the header block and the firm calls.
   Propose firm calls decisively — scope, stack, posture, monetization —
   with one line of why each; merging the PR is the human's sign-off. Only
   file a blocked-by-human decision entry for a genuine coin-flip you cannot
   argue one way.
5. **Replace the root README** with the product's README: one-liner, why
   now, status, roadmap at a glance, tech stack, principles, docs table. No
   trace of the template may remain.
6. **Housekeeping in the same PR:** stamp `<owner>/<repo>` into the cron
   prompt below **and into the links in
   [blocked-by-human.md](../blocked-by-human.md)**, verify the two
   pre-seeded entries there still match reality, adapt
   `.github/workflows/ci.yml` to the chosen stack (replace the prechecks
   the stack decision resolves; keep its least-privilege/self-skip
   properties per the file's header), set the stack's ecosystems in
   `.github/dependabot.yml` (default `bun`; swap or add per its header), and
   delete this Bootstrap section (memory stores current state — a
   bootstrapped project has no bootstrap protocol).
7. **End with a report:** PR link, the proposed firm calls, and what the
   human must do next (merge, then resolve blocked-by-human.md).

## Scheduled run (one cron, twice daily)

Runs unattended — never asks questions; anything needing a human goes to
[blocked-by-human.md](../blocked-by-human.md).

**Cron prompt (fresh session per run) — copy verbatim, fill the repo:**

```
Scheduled run for ⟨owner/repo⟩: follow docs/AGENTS.md § Scheduled run. End with a 5-line report.
```

**Dispatch — first action of every run.** List open PRs on this repo — bot
PRs (Dependabot, `github-actions`) count like any other: they are cheap to
process, and stale bumps accumulate risk.

- **0 open non-draft PRs → Worker mode.** Follow the task loop in
  [CLAUDE.md](../CLAUDE.md). Read [END_GOAL.md](END_GOAL.md) and only the
  docs the item touches.
- **≥ 1 open non-draft PR → Reviewer-fixer-merger mode** (below). Do not also
  take a new item — one mode per run keeps each run's context to one job.

**Draft = parked, not a claim.** Dispatch ignores drafts. Reviewer mode
converts a PR to draft when it will not merge it this run and cannot fix it
(firm-call violation, human-scale decision, fix larger than the PR, or
blocked on an external step) — with ONE comment: why, and the exact condition
that un-drafts it; a human-scale decision also gets its blocked-by-human.md
entry. Worker mode, before taking a fresh roadmap item, adopts a draft whose
un-draft condition is now met (merge `main` in, fix, mark ready for review) —
that counts as its item. Nothing else touches drafts. Duplicate or superseded
PRs are closed with a one-line comment, never drafted.

**Report (5 lines), either mode:** mode taken, what shipped or merged / fixed
/ drafted / closed, what is blocked and on what, then the current
blocked-by-human.md entries verbatim.

### Reviewer-fixer-merger mode

The only path to `main` — and it **is authorized to merge**; that is the job.
Process every open non-draft PR, oldest first. Where the platform supports
it, review each PR in its own sub-agent so verdicts stay independent.

**Review criteria**, in priority order:

1. **User experience above all**, measured against
   [END_GOAL.md](END_GOAL.md): the PR moves a flow step toward its written
   state and moves no other step away; minimum user actions for maximum
   value; minimize user-regretted seconds — interruptions, spam, waiting,
   dead ends. A roadmap item that contradicts END_GOAL.md is a roadmap bug
   to fix, never a reason to bend the flow. A PR that ships or preserves a
   debug list / dump of internals as the home surface, when END_GOAL names
   a different home, is a finding — not "the roadmap asked for a list".
2. **Security:** injection (SQL and otherwise), authz on every surface,
   secrets only ever read from env.
3. **Correctness & robustness:** edge cases and failure paths, not just the
   happy path.
4. **Firm calls & memory upkeep:** no firm-call violations; roadmap box
   checked in the PR; docs edited in place wherever the change made them
   untrue; decisions documented at the right tier (CLAUDE.md § Memory rules).
5. **Engineering bar:** every bullet of [GUIDELINES.md](GUIDELINES.md) is a
   review criterion.

**Per-PR loop:**

1. **Review the diff** against the criteria above.
2. **Conflicts** → merge `main` into the branch and resolve carefully.
3. **Fixable findings** (bugs, missed doc/README updates, style) → push
   fixes to the PR branch yourself; don't bounce it back.
4. **If you pushed any fixes** → a fresh pass (fresh sub-agent where
   possible) re-reviews the whole PR rigorously. Repeat until a review
   finds zero fixable issues.
5. **Re-run the PR's `Walked` line** in the stated environment; a `Walked`
   that cannot be reproduced is a finding, fixed like any other.
6. **Clean review + green CI + reproduced walk** → confirm nothing in the PR
   silently decided a human-scale question (escalation bar in CLAUDE.md) —
   file any such entry in [blocked-by-human.md](../blocked-by-human.md) —
   then squash-merge and delete the branch. Squash keeps `main` at one
   commit per roadmap item, so `git log` reads as the roadmap's history.
7. **Firm-call violation, human-scale decision, or a fix larger than the
   PR** → do NOT merge: convert the PR to draft per the draft rule above,
   and file the blocked-by-human.md entry if the human must decide.
8. **Duplicate, superseded, or un-mergeable** → close with a one-line
   comment saying why.

Never merge a PR that leaves the roadmap, docs, or README untrue — that
corrupts the project memory every later agent reads. And never a red CI.

**Memory janitor** — part of every run, not a separate pass:

- A PR adding a prose rule to CLAUDE.md must pass its inclusion bar
  (deletion test, non-derivable, second occurrence) — otherwise push the
  rule down a tier or drop it.
- A convention a repo tool could enforce gets **graduated**: add the
  lint/CI/hook on the PR branch and delete the prose stating it.
- Every doc a PR adds must be reachable by pointer from CLAUDE.md's index.
- Periodically (every few weeks of merges) prune: rules the toolchain now
  enforces, docs restating what the code says, entries that stopped earning
  their place.

**Bot PRs:** dependency bumps from automation accounts (Dependabot,
`github-actions`) merge on green CI, no human sign-off. No CI for the target
(e.g. a native app)? Reproduce install, typecheck, tests, and a bundle
locally and treat that as green. A bump that cannot install or build and
whose fix is an out-of-scope upgrade (e.g. a package the platform SDK pins)
→ close per step 8. Version bumps within the existing stack are below the
escalation bar: decide them here, never file them in blocked-by-human.md.

## Steering (on-demand, human present)

When the human invokes an agent in-session to steer direction, brainstorm
the roadmap, or amend firm calls: first read the firm calls and `docs/` —
then ask about only the genuine gaps, with a recommendation attached to
each question. Record the outcomes at the right memory tier (CLAUDE.md
§ Memory rules) in the same session; a steering conversation that changes
direction but leaves the docs unchanged never happened.
