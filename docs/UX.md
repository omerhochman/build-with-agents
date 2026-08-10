# ⟨Core interaction⟩ UX Spec

<!-- template: the spec for the ONE interaction the product is. Not a screen
     inventory — a behavior spec precise enough that an agent can implement
     "how it should feel" without asking. Rename this file if the core
     surface isn't visual UX (API spec, CLI spec…) and update both docs
     indexes. -->

## Default view

<!-- template: what the user sees by default, and the deliberate cut — the
     flashier alternative you rejected and why (permissions, battery,
     jank). Stating the rejection here stops agents from "adding it back". -->

⟨The default experience, and the rejected flashier alternative with why.⟩

## ⟨Primary state / mode⟩

<!-- template: one section per distinct interaction regime (far/near,
     empty/populated, offline/online). For each: behavior, feedback channels
     (visual, haptic, copy), and the labels/copy verbatim where the wording
     IS the feature. -->

- ⟨Behavior.⟩
- ⟨Feedback: visual / haptic / copy — quote key copy verbatim.⟩
- ⟨The "wow moment" to protect, if this state has one.⟩

## ⟨Degraded state⟩

<!-- template: what happens when inputs are bad (no signal, low accuracy,
     no permission). The firm shape: degrade gracefully, never hard-block
     the core interaction. -->

- ⟨Detection: what signal marks this state.⟩
- ⟨The non-blocking nudge shown to the user.⟩
- ⟨What must keep working regardless.⟩

## Error tolerance

<!-- template: quantify the real-world noise (sensor drift, GPS error,
     network latency) and list the mitigations that keep the magic alive
     despite it. This section is why the product survives contact with
     reality. -->

⟨Expected noise magnitude, and the mitigations: smoothing, generous success
thresholds, forgiving copy.⟩
