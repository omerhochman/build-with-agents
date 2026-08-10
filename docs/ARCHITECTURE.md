# Architecture

<!-- template: current technical truth — stack, components, data model —
     organized by phase so v0 stays visibly minimal. State choices as
     decisions with the one-line why, and name the rejected obvious
     alternative where an agent might otherwise "upgrade" to it. Sections
     below are the common shape; rename/drop to fit the idea. -->

## v0 — ⟨wedge stack⟩

<!-- template: the minimal stack: language/framework, key libraries (as
     links), hosting. Note constraints that shaped choices (platform quirks,
     API availability, pricing). -->

- ⟨Framework/runtime and why it's the cheapest adequate choice.⟩
- ⟨Key library (link) and what it's responsible for.⟩
- ⟨Hosting/deploy target and any hard requirements (HTTPS, regions…).⟩

## ⟨v1.0+ client / app⟩

<!-- template: repeat the firm stack call here with its full justification —
     this is where an agent tempted to rewrite it will look. -->

**Firm call: ⟨chosen stack⟩ over ⟨rejected alternative⟩.** ⟨One or two
sentences of why: shared logic, solo-dev capacity, performance evidence.⟩

## ⟨Core subsystem⟩

<!-- template: one section per load-bearing subsystem (sensors, sync engine,
     ML pipeline, payments…). For each: the chosen approach, availability/
     platform tiers if relevant, and the traps ("never use raw X"). -->

- ⟨Approach, per platform if they differ.⟩
- ⟨The trap a competent dev would fall into, and what to do instead.⟩

## Backend (⟨phase⟩) — ⟨provider⟩

<!-- template: only if/when a phase needs one. If the wedge is backend-free,
     say so here as a guarantee ("no backend for v0, ever"), not a gap. -->

⟨Auth model, data store, realtime/queue choices.⟩

### Schema sketch

```sql
⟨tables with inline comments explaining the non-obvious columns —
 especially the ones that encode product posture (TTLs, per-direction
 flags, session scoping)⟩
```

### Data lifecycle & access

- ⟨Access control: who can read what, enforced where.⟩
- ⟨Retention: TTLs, expiry, deletion mechanics.⟩
