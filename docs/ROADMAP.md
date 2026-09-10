# Roadmap

<!-- template: the agents' task queue — the worker takes the first unchecked,
     unblocked, unclaimed box, so ordering IS prioritization. Write items as
     shippable, PR-sized units (a checked box = a merged PR), concrete enough
     to start without asking questions: name the library, the API, the
     platform quirk in the item itself. Phases mirror PRODUCT.md § MVP scope;
     each item moves a named flow step in END_GOAL.md toward its written
     state — an item no flow step needs is scope creep.
     Blocked items get suffixed `⛔ see blocked-by-human.md` by the worker.

     The first items after plumbing are the USER-VISIBLE core interaction in
     its real shape (PRODUCT.md / END_GOAL.md / UX.md). Backend-only items
     (ingest, p95, keep-warm, extra factors, infra adoption) do not sit in
     front of that surface unless they unblock a number that surface shows.
     "Data before beauty" is omit-missing-factors, not "list of internals
     until every source is live". -->

Effort estimates assume **one developer, focused weeks**. One checkbox =
one merged PR. The queue living **in git** is the design; the checkbox
format is the swappable part — if parallel workers ever multiply, upgrade
to structured tasks with dependency edges in-repo, not to an external
tracker.

## v0 — ⟨wedge⟩ (⟨est.⟩)

⟨One line: what this phase proves.⟩

- [ ] ⟨first item — often permissions/inputs plumbing⟩
- [ ] ⟨core interaction, end to end, in its REAL SHAPE — if the product is
      a map, this is the map, not a list; if it is a camera, this is the
      camera, not a settings page⟩
- [ ] ⟨supporting states: errors, empty, edge — on that same surface⟩
- [ ] ⟨the shareable/viral surface⟩
- [ ] ⟨deploy: live at a URL / installable⟩

## v1.0 — ⟨next phase⟩ (⟨est.⟩)

- [ ] ⟨…⟩

## v1.1 — ⟨retention layer⟩ (⟨est.⟩)

- [ ] ⟨…⟩

## Sequencing rationale

<!-- template: one paragraph on why the phases are in this order — what the
     early phase de-risks or seeds for the later one. This is the paragraph
     that stops agents (and future you) from reordering the plan. -->

⟨Why this order: what v0 proves at near-zero cost, what building later
phases first would have front-loaded, what v0 seeds that later phases need.⟩
