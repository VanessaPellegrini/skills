# Over-engineering pass

Hunt only for unnecessary complexity. The desired result is a shorter diff with
fewer concepts and dependencies, without weakening required behavior, safety, or
tests.

Use these tags:

- `delete`: dead code, unused flexibility, or speculative behavior; nothing
  replaces it.
- `stdlib`: hand-rolled behavior replaced by a named standard-library facility.
- `native`: a dependency or helper replaced by a named platform feature.
- `yagni`: abstraction with one implementation, unused configuration, or a layer
  with one caller; inline or defer it until the second real need.
- `shrink`: identical behavior expressed directly with fewer lines; show the
  concise direction.

For each item, use a tight location and state what to cut and what replaces it.
Do not route correctness, security, or performance findings into this pass; they
belong in the main review. A focused smoke test or assertion is minimum evidence,
not bloat, and must not be flagged for deletion.

Estimate net removable lines only when the diff supports a credible number. If
nothing should be cut, report `Lean already. Ship.`
