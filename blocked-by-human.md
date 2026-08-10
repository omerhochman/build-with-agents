# Blocked by human

Async mailbox between agents and the human. Agents add an entry when a task
needs a human; the human answers by editing the entry (fill in **Answer**, or
just do the thing); the resolving agent finishes the task and **deletes the
entry** in the same commit. Holds open blockers only — history is in git.
Protocol in [CLAUDE.md](CLAUDE.md) § Human-blocked work.

## Protect `main` (require PRs)
- **Type:** account action
- **Done so far:** task loop and reviewer protocol assume PRs are the only
  path to `main` (CLAUDE.md § Task loop, docs/AGENTS.md) — but nothing
  enforces it against a misbehaving agent.
- **Ask:** GitHub → Settings → Branches → add rule for `main`: enable
  "Require a pull request before merging" with **0 required approvals**
  (the reviewer-merger agent is the approver; a required-review count would
  block its merges).
- **Answer:** _(human writes here)_

## Schedule the worker and reviewer-merger agents
- **Type:** account action
- **Done so far:** both protocols and their one-line cron prompts are written
  in docs/AGENTS.md; nothing runs until you schedule them.
- **Ask:** in your agent platform's scheduled/cron jobs, create two recurring
  runs with the prompts from docs/AGENTS.md — the worker a few times daily,
  the reviewer-merger daily — each in a fresh session with access to this
  repo.
- **Answer:** _(human writes here)_

<!-- Entry template — copy below this line:

## <roadmap item, verbatim>
- **Type:** secret | account action | decision
- **Done so far:** <state of the partial work, PR/commit refs>
- **Ask:** <precise executable ask, or decision brief with options,
  tradeoffs, and a recommendation>
- **Answer:** _(human writes here)_
-->
