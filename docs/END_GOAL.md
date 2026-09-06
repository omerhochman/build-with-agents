# End goal — ⟨project name⟩, finished

<!-- template: the FINISHED product — every phase the docs plan (v0, v1.x,
     v2…) — through the user's eyes, flow by flow. Never a v0 snapshot;
     sequencing stays in PRODUCT.md and ROADMAP.md, mechanics in UX.md. Fill
     every ⟨placeholder⟩, delete these comments; the two guidance sections
     stay — they govern every later amendment. -->

## How agents use this doc

- **Read it before taking a roadmap item and before judging a PR.** Which step
  of which flow does this move toward its written state — and which away?
- **This doc outranks the roadmap.** An item contradicting a flow here is a
  roadmap bug: fix the item in the same PR, never the flow. A flow needing a
  **firm-call** violation is a hard stop → blocked-by-human.md.
- **A gap here is a decision, not a license.** Interaction design this doc
  doesn't cover goes through the escalation bar (CLAUDE.md § Human-blocked
  work) as a proposed amendment — never invented in code.
- **It describes done, not the order to get there** — amended only by a PR
  whose stated purpose is the amendment, never as a side effect of shipping.

## How to write this

Write the app **as if every planned phase had already shipped**, watching one
person use it. A line passes if an agent can build to it without asking and a
reviewer can tell from the running app whether it is true.

- **The end state, not v0.** A feature any phase in PRODUCT.md/ROADMAP.md
  plans belongs here as a flow row or a state; one the docs never plan does
  not — never invent. **No phase labels inside the flows** ("in v0", "later",
  "phase 2"): the tables read as one shipped product. Absences here are what
  the finished product deliberately never has — not what v0 lacks.
- **Flows, not features, and no adjectives.** "Export to CSV" and "handles
  errors gracefully" are placeholders for facts you haven't decided; "she taps
  Export, the file name pre-filled from the list title" and "save retries
  three times over 10 s, then a one-line banner" are steps. Quote copy only
  where a reviewer checks it word for word (≤ 6 strings in the file); give a
  latency only where a doc fixes the number.
- **Stay short — ≤ 5 KB filled.** One flow table ≤ 14 rows, states ≤ 6, done
  ≤ 5 lines, non-goals ≤ 4. Mechanics needing more room belong in UX.md.

## Personas & entry points

<!-- ≤ 3 one-line bullets: who, the moment, and the exact entry point (link,
     store listing, icon, command) — the entry point is step 1 of the flow. -->

- **⟨Persona⟩** — ⟨who, in one clause⟩. Reaches for it when ⟨the moment⟩, via
  ⟨exact entry point⟩. Success: ⟨one observable outcome⟩.

## The happy path

<!-- ONE table, ≤ 14 rows, end to end: first open → the moment of value → the
     surfaces later phases add (retention layer, shared link, second device) →
     the return visit. One clause per cell; `—` where no latency is fixed. -->

**⟨Flow name⟩ — ⟨persona⟩, from ⟨entry point⟩ through ⟨the moment of value⟩
to the return visit.**

| # | Sees | Does | Latency |
|---|------|------|---------|
| 1 | ⟨what's on screen at the entry point⟩ | ⟨taps / types / waits⟩ | ⟨budget⟩ |
| 2 | ⟨…⟩ | ⟨…⟩ | — |
| n | ⟨the return visit: what brings them back, what's already there⟩ | ⟨…⟩ | — |

**Not in this flow:** ⟨≤ 2 lines — steps permanently absent (signup, config,
confirmation) and why.⟩

## Empty, error, and edge states

<!-- ≤ 6 rows: nothing yet, network down, permission denied, huge input,
     stale data. Recoverable failures retry to success before anything is
     shown; a surfaced error is one sentence — what happened + next action. -->

| State | Trigger | Sees | Can do |
|-------|---------|------|--------|
| Empty | ⟨first run, nothing yet⟩ | ⟨…⟩ | ⟨the one action⟩ |
| ⟨Error⟩ | ⟨…⟩ | `⟨what happened — the next action⟩` | ⟨…⟩ |
| ⟨Edge⟩ | ⟨…⟩ | ⟨…⟩ | ⟨…⟩ |

## Done looks like

<!-- ≤ 5 binary lines a stranger could verify against the live product in an
     afternoon, quality floor included: latency, platforms, offline. -->

- [ ] ⟨Every row above works end to end at the real URL / installed app,
      within its latency budgets, on ⟨platforms⟩.⟩
- [ ] ⟨A new user reaches the moment of value in ⟨n⟩ actions, no account.⟩
- [ ] ⟨Every state above is reachable and reads as written.⟩

## Non-goals

<!-- ≤ 4 one-line bullets: what the finished product never does, and why.
     Identity, not sequencing — PRODUCT.md's phase Out-lists carry that. -->

- **⟨Not this — ever.⟩** ⟨Why — what it would cost the core flow.⟩
- **⟨Not this, until ⟨condition⟩.⟩** ⟨Why.⟩
