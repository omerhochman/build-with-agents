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
- **Ask:**
  1. Open <https://github.com/⟨owner/repo⟩/settings/branches> and click
     **Add branch ruleset** (or **Add rule** on the classic page).
  2. Branch name pattern — paste: `main`
  3. Check **Require a pull request before merging**; set required
     approvals to `0` (the reviewer-merger agent is the approver; a
     required-review count would block its merges).
  4. Save.
- **Answer:** _(human writes here)_

## Schedule the worker and reviewer-merger agents
- **Type:** account action
- **Done so far:** both protocols and their one-line cron prompts are written
  in docs/AGENTS.md; nothing runs until you schedule them.
- **Ask:**
  1. Open your agent platform's scheduled/cron jobs page.
  2. Create a **worker** job, a few times daily, fresh session with access
     to this repo — paste its cron prompt verbatim from
     [docs/AGENTS.md](docs/AGENTS.md) § Worker.
  3. Create a **reviewer-merger** job, daily, fresh session with access to
     this repo — paste its cron prompt verbatim from
     [docs/AGENTS.md](docs/AGENTS.md) § Reviewer-merger.
- **Answer:** _(human writes here)_

<!-- Entry template — copy below this line. Asks are numbered steps the
     human executes in under a minute: a clickable full URL for every page
     to open, a copy-paste-ready block for every command, form field, or
     value (see CLAUDE.md § Human-blocked work).

## <roadmap item, verbatim>
- **Type:** secret | account action | decision
- **Done so far:** <state of the partial work, PR/commit refs>
- **Ask:** <numbered steps with links + copy-paste blocks, or decision
  brief with options, tradeoffs, and a recommendation>
- **Answer:** _(human writes here)_
-->
