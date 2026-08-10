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

**Cron prompt (fresh session per run) — copy verbatim, fill the repo:**

```
Read CLAUDE.md in ⟨owner/repo⟩ and follow its task loop exactly.
End with a short report: what shipped (PR link), what's blocked and on what.
```

## Reviewer-merger (daily)

The only path to `main` — and it **is authorized to merge**; that is the job.
Process every open non-draft PR, oldest first. Where the platform supports
it, review each PR in its own sub-agent so verdicts stay independent.

**Review criteria**, in priority order:

1. **User experience above all:** minimum user actions for maximum value;
   minimize user-regretted seconds — interruptions, spam, waiting, dead ends.
2. **Security:** injection (SQL and otherwise), authz on every surface,
   secrets only ever read from env.
3. **Correctness & robustness:** edge cases and failure paths, not just the
   happy path.
4. **Firm calls & memory upkeep:** no firm-call violations; roadmap box
   checked in the PR; docs — **including the root README** — edited in place
   wherever the change made them untrue; decisions documented at the right
   tier (CLAUDE.md § Memory rules).
5. **Readability, consistency, reusability, scalability, developer
   experience.**
6. **Observability, non-spammy:** logs/metrics that answer a real question;
   no noise.
7. **Comments judicious:** one-sentence why-comments only where the code
   can't say it (per the memory rules' bar); never narration.

**Per-PR loop:**

1. **Review the diff** against the criteria above.
2. **Conflicts** → merge `main` into the branch and resolve carefully.
3. **Fixable findings** (bugs, missed doc/README updates, style) → push
   fixes to the PR branch yourself; don't bounce it back.
4. **If you pushed any fixes** → a fresh pass (fresh sub-agent where
   possible) re-reviews the whole PR rigorously. Repeat until a review
   finds zero fixable issues.
5. **Clean review + green CI** → confirm nothing in the PR silently decided
   a human-scale question (escalation bar in CLAUDE.md) — file any such
   entry in [blocked-by-human.md](../blocked-by-human.md) — then
   squash-merge and delete the branch. Squash keeps `main` at one commit
   per roadmap item, so `git log` reads as the roadmap's history.
6. **Firm-call violation or human-scale decision** → do NOT merge. Leave a
   review comment; file an entry in blocked-by-human.md if the human must
   decide.
7. **Duplicate or superseded** → close with a one-line comment.

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

**Bot PRs are in scope:** also pick up PRs from automation accounts
(`github-actions`, Dependabot and the like) bumping package versions or
fixing vulnerabilities — just make sure everything still builds and CI is
green before merging. Version bumps within the existing stack are below the
escalation bar: decide them here, never file them in blocked-by-human.md.

**Cron prompt (fresh session per run) — copy verbatim, fill the repo:**

```
You are the reviewer-merger for ⟨owner/repo⟩. Read docs/AGENTS.md
and CLAUDE.md, then process all open PRs per the reviewer protocol.
End with a report: merged / fixed / blocked / closed.
```
