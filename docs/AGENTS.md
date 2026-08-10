# Agent roles

Two scheduled agents drive this repo. Their protocols live here — versioned
with the code — so each cron prompt stays a one-liner pointing at this file.
(A third, one-shot role — Bootstrap — turns the template into a project and
then deletes itself from this file.)

## Bootstrap (one-shot — delete this section when done)

Turns this template into a real project from the human's idea brief (the
paragraph in the invoking prompt). Do it all **in one PR to `main`** titled
`Bootstrap: <project name>`, so the human signs off on the framing by merging:

1. **Write [PRODUCT.md](PRODUCT.md) first** — one-liner, why now, MVP scope.
   Every other doc derives from it. The key move: find the **wedge** — the
   cheapest version that proves the core interaction/value, and push
   everything that needs accounts, backends, or policy to a later phase.
2. **Fill the remaining docs** ([ARCHITECTURE.md](ARCHITECTURE.md),
   [UX.md](UX.md), [PRIVACY.md](PRIVACY.md), [RISKS.md](RISKS.md),
   [ROADMAP.md](ROADMAP.md)): replace every `⟨placeholder⟩`, delete every
   `<!-- template: … -->` comment. A doc with nothing true to say for this
   idea gets **deleted, not stubbed** (a UX spec may become an API spec, a
   privacy doc may be one paragraph) — rename or drop docs to fit the idea
   and keep both docs indexes true.
3. **Fill [CLAUDE.md](../CLAUDE.md):** the header block and the firm calls.
   Propose firm calls decisively — scope, stack, posture, monetization —
   with one line of why each; merging the PR is the human's sign-off. Only
   file a blocked-by-human decision entry for a genuine coin-flip you cannot
   argue one way.
4. **Replace the root README** with the product's README: one-liner, why
   now, status, roadmap at a glance, tech stack, principles, docs table. No
   trace of the template may remain.
5. **Housekeeping in the same PR:** stamp `<owner>/<repo>` into the cron
   prompts below, verify the two pre-seeded entries in
   [blocked-by-human.md](../blocked-by-human.md) still match reality, and
   delete this Bootstrap section (memory stores current state — a
   bootstrapped project has no bootstrap protocol).
6. **End with a report:** PR link, the proposed firm calls, and what the
   human must do next (merge, then resolve blocked-by-human.md).

## Worker (a few times daily)

Follows the task loop in [CLAUDE.md](../CLAUDE.md). No extra rules.

**Cron prompt (fresh session per run):**

> Read CLAUDE.md in ⟨owner/repo⟩ and follow its task loop exactly.
> End with a short report: what shipped (PR link), what's blocked and on what.

## Reviewer-merger (daily)

The only path to `main`. Process every open PR, oldest first:

1. **Review the diff** for correctness, adherence to the firm calls in
   CLAUDE.md, and memory upkeep: roadmap box checked in the PR, docs edited
   in place where the change made them untrue, decisions documented at the
   right scope.
2. **Small issues** (bugs, missed doc updates, style) → push fixes to the PR
   branch yourself; don't bounce it back.
3. **Conflicts** → merge `main` into the branch and resolve.
4. **Good** → squash-merge and delete the branch. Squash keeps `main` at
   one commit per roadmap item, so `git log` reads as the roadmap's history.
5. **Firm-call violation or human-scale decision** (see escalation bar in
   CLAUDE.md) → do NOT merge. Leave a review comment; file an entry in
   blocked-by-human.md if the human must decide.
6. **Duplicate or superseded** → close with a one-line comment.

Never merge a PR that leaves the roadmap or docs untrue — that corrupts the
project memory every later agent reads.

**Cron prompt (fresh session per run):**

> You are the reviewer-merger for ⟨owner/repo⟩. Read docs/AGENTS.md
> and CLAUDE.md, then process all open PRs per the reviewer protocol.
> End with a report: merged / fixed / blocked / closed.
