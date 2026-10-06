# Structural quality pass

Audit maintainability with an ambitious bias toward deleting complexity. Search
for a “code-judo” restructuring that preserves behavior while making the design
dramatically smaller, more direct, and more inevitable.

## Review questions

- Can the state model or ownership boundary change so branches disappear?
- Did the diff add scattered special cases, nullable modes, flags, or unrelated
  conditionals to an already busy flow?
- Is logic in its canonical layer, using the existing canonical helper or model?
- Does an abstraction earn its indirection, or is it a thin wrapper, pass-through
  helper, generic mechanism, or one-implementation interface?
- Do `any`, `unknown`, casts, optionality, silent fallbacks, or ad-hoc object
  shapes hide an invariant that should be explicit?
- Did the change increase coupling, statefulness, sequential orchestration, or
  partial-update risk where a cleaner decomposition or atomic flow is evident?
- Does the PR push a file from below 1,000 lines to above 1,000 lines without a
  compelling structural reason?

## Flag as blockers when justified

- A plausible reframing would delete whole concepts, branches, layers, or modes,
  but the change preserves or redistributes the incidental complexity.
- Feature-specific logic leaks into a shared path or the wrong package.
- Spaghetti branching, casts, magic behavior, or bespoke helpers make the local
  architecture harder to reason about.
- A file crosses 1,000 lines because focused modules were not extracted.
- Related state can be left half-applied, or obviously independent work is made
  sequential in a way that complicates orchestration.

Prefer remedies that simplify the model: delete an indirection layer, change
ownership, make the boundary typed, collapse duplicate branches, isolate
orchestration, reuse the canonical utility, split a focused module, or make
related updates atomic. Do not settle for renaming or moving the same complexity.

Approval requires no clear structural regression and no high-confidence missed
opportunity for a materially simpler design. Working code alone is not enough.
