# End goal — ⟨project name⟩, finished

<!-- template: the finished product seen through the user's eyes, flow by
     flow. PRODUCT.md says why it should exist and what ships first; UX.md
     specs how the one core interaction behaves; ROADMAP.md is the queue.
     This doc is what all of them are FOR — the concrete end state every
     roadmap item and PR is judged against. Written at bootstrap from the
     idea brief; the human's merge is the sign-off, so read it line by line:
     of every doc here, this is the one most worth your own words. Fill
     every ⟨placeholder⟩ and delete the template comments; the two
     guidance sections below stay — they govern every later amendment. -->

## How agents use this doc

- **Read it before taking a roadmap item and before judging a PR.** The
  question is the same for both: which step of which flow below does this
  move closer to its written state — and does it move any other step away?
- **This doc outranks the roadmap.** A roadmap item that contradicts a flow
  here is a roadmap bug: fix the item in the same PR (rewrite, reorder, or
  delete it); never bend the flow to fit the item.
- **Firm calls outrank this doc.** A flow that needs a firm-call violation
  is a hard stop → blocked-by-human.md, with a recommendation.
- **A gap here is a decision, not a license.** Interaction design this doc
  doesn't cover goes through the escalation bar (CLAUDE.md § Human-blocked
  work): propose the missing step as an amendment to this doc; never invent
  it silently in code.
- **It describes done, not the order to get there.** Sequencing lives in
  PRODUCT.md and ROADMAP.md. This doc changes only by deliberate amendment
  (a steering session, or a PR whose stated purpose is the amendment),
  never as a side effect of shipping.

## How to write this

Write the app as if it shipped and you are watching one person use it. A
sentence passes if an agent can build to it without asking and a reviewer
can tell from the running app whether it's true.

- **Flows, not features.** "Export to CSV" is a feature. "She taps Export;
  a sheet slides up with the file name pre-filled from the list title, and
  the share sheet opens in under a second" is a flow step.
- **Ban the adjectives.** Intuitive, seamless, delightful, clean, fast,
  simple, easy, powerful — each is a placeholder for a fact you haven't
  decided yet. Replace it with the observable: what's on screen, what the
  user does, how long it takes, what the copy says verbatim.
- **Numbers over vibes.** "Fast" → "first paint under 300 ms; shows the
  last result instantly while refreshing". "Works offline" → "every read
  works with no network; writes queue behind a one-line banner: `Saved on
  this device — syncs when you're back online`."
- **Feeling, then its cause.** "Relief that it remembered" is only useful
  next to "because the field is pre-filled from last time".
- **Say what isn't there.** The button you refused to add, the setting that
  doesn't exist, the confirmation you removed. Absences are the part of a
  spec agents most often "helpfully" undo.

| Bad | Good |
|-----|------|
| Onboarding is smooth and intuitive. | First launch opens straight onto a live example already populated with sample data; the only affordance is the input, focused, keyboard up. No tour, no account, no permission prompt. |
| Fast search with great results. | Results update on every keystroke, first paint under 100 ms from the local index. A query with no match shows the three nearest terms as tappable chips — never an empty list. |
| Handles errors gracefully. | Save retries silently, three times over 10 s. Only then a one-line banner — `Couldn't save — check your connection. Retrying…` — and the draft is never lost. |
| Supports sharing. | Tapping Share copies a link and shows `Link copied` inline for 2 s. The recipient opens it to the exact same view, no account, in under 2 s on 3G. |

## Personas & entry points

<!-- template: 1–3 personas, one or two lines each: who they are, the moment
     they reach for this, and the exact entry point — a shared link, a
     store listing, a home-screen icon, a CLI command, a browser tab. The
     entry point IS step 1 of the flow; name it precisely. -->

- **⟨Persona⟩** — ⟨who, in one clause⟩. Reaches for it when ⟨the moment⟩,
  via ⟨exact entry point⟩. Success for them: ⟨one observable outcome⟩.
- **⟨Persona⟩** — ⟨…⟩

## The happy path

<!-- template: the flow the product IS — from entry point through the moment
     of value to the natural exit, one row per step. Per step: what's on
     screen (concretely, key copy verbatim), what the user does, the latency
     budget, what they feel and the concrete cause. 6–15 steps; a happy path
     that needs more is two flows. Where UX.md already specs a step's
     mechanics (thresholds, feedback channels), link to it — never restate. -->

**⟨Flow name⟩ — ⟨persona⟩, from ⟨entry point⟩ to ⟨the moment of value⟩.**

| # | Sees | Does | Latency | Feels — because |
|---|------|------|---------|-----------------|
| 1 | ⟨what's on screen, copy verbatim⟩ | ⟨taps / types / waits…⟩ | ⟨budget⟩ | ⟨feeling — its concrete cause⟩ |
| 2 | ⟨…⟩ | ⟨…⟩ | ⟨…⟩ | ⟨…⟩ |
| n | ⟨the moment of value, written so a reviewer recognises it in the app⟩ | ⟨…⟩ | ⟨…⟩ | ⟨…⟩ |

**Not in this flow:** ⟨the steps deliberately absent — signup, config,
confirmations, tours — and why.⟩

### ⟨Second flow, if the product has one⟩

<!-- template: same table — the return visit, the share-recipient's path,
     the retention layer's flow. Keep flows separate; never merge them into
     one epic. -->

## Key screens

<!-- template: one short paragraph per screen (view / command / response —
     whatever the surface is) that appears in the flows above: what's above
     the fold, the single primary action, what's deliberately not there.
     Not a component inventory. -->

- **⟨Screen⟩** — ⟨above the fold; the primary action; what's absent and why⟩.
- **⟨Screen⟩** — ⟨…⟩

## Empty, error, and edge states

<!-- template: every state that isn't the happy path. First run with
     nothing; network down; permission denied; huge input; stale data; two
     people editing at once… For each: the trigger, exactly what appears
     (copy verbatim), the one action offered, and what keeps working.
     Recoverable failures retry to success before anything is shown, and
     surfaced errors are one sentence — what happened + the next action
     (firm calls). -->

| State | Trigger | The user sees | Can do | Keeps working |
|-------|---------|---------------|--------|---------------|
| Empty | ⟨first run, nothing yet⟩ | ⟨…⟩ | ⟨the one action⟩ | — |
| ⟨Error⟩ | ⟨…⟩ | `⟨what happened — the next action⟩` | ⟨…⟩ | ⟨…⟩ |
| ⟨Edge⟩ | ⟨…⟩ | ⟨…⟩ | ⟨…⟩ | ⟨…⟩ |

## Done looks like

<!-- template: the whole product, finished — a checklist a stranger could
     verify against the live app in an afternoon. Every line observable and
     binary. Include the quality floor: latency budgets, platforms,
     accessibility, offline behavior, the promise the landing page makes. -->

- [ ] ⟨Every flow above works end to end at the real URL / installed app,
      within its latency budgets, on ⟨platforms⟩.⟩
- [ ] ⟨A new user reaches the moment of value in ⟨n⟩ actions and under
      ⟨t⟩ seconds, with no account.⟩
- [ ] ⟨Every state in the table above is reachable and reads as written.⟩
- [ ] ⟨…⟩

## Non-goals

<!-- template: what the finished product deliberately does not do — ever, or
     not until a named condition — one line each with the why. This list is
     what stops "while I'm here" scope creep. Distinct from PRODUCT.md's
     phase Out-lists: those are sequencing, these are identity. -->

- **⟨Not this.⟩** ⟨Why — what it would cost the core flow.⟩
- **⟨Not this, until ⟨condition⟩.⟩** ⟨Why.⟩
