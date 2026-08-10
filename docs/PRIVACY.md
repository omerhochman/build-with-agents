# Privacy Essentials

Privacy is a product feature, not a compliance checkbox.

<!-- template: one section per phase/mode, cheapest first. The strongest
     claims are structural — "collects nothing", "the data model doesn't
     support a history surface, by design" — write those in bold; they are
     marketing copy and engineering constraints at once. If this idea
     genuinely has no privacy surface, shrink this doc to one honest
     paragraph rather than padding it. -->

## ⟨Wedge mode⟩

- **⟨What it collects — ideally: nothing, no account required.⟩** ⟨Where to
  state this loudly (store listing, landing page).⟩
- ⟨What runs on-device vs. server, as a guarantee.⟩

## ⟨Account-bearing mode⟩

- **⟨Sharing/collection model: ephemeral? session-scoped? opt-in
  granularity?⟩**
- ⟨Visibility: how users see that sharing is live.⟩
- ⟨Control: per-relationship toggles, instant kill-switch.⟩
- ⟨Scope limits that dodge whole risk classes (e.g. foreground-only — no
  background collection, no battery drain, no store-policy risk).⟩

## Server-side guarantees

<!-- template: the enforceable versions of the promises above: TTLs and
     their mechanism, row-level access rules, and what the data model makes
     structurally impossible. -->

- ⟨Retention: what is deleted, after how long, by what mechanism.⟩
- ⟨Access: who can read each row, enforced at the database layer.⟩
- **⟨The surface that can never exist because the data model forbids it.⟩**
