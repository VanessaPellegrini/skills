---
name: van-code-review
description: Audit a branch or PR with three coordinated passes for correctness and security, structural maintainability, and removable over-engineering, plus mechanical metric gates (complexity, coverage, dependency direction, mutant-killing tests). Use for van code review, deep review, thermo-nuclear review, or a rigorous review of changed code; do not use when the user asks to implement fixes instead of reviewing them.
---

# Van Code Review

Review the requested diff with fresh eyes. This skill combines the intent of
`thermo-nuclear-review`, `thermo-nuclear-code-quality-review`, and
`ponytail-review` into one evidence-led review. Review only; do not modify the
code unless the user separately asks for fixes.

## Establish the review target

1. Read repository instructions and required domain context.
2. Resolve the exact branch, PR, commit, or merge-base. If none is supplied,
   infer the safest comparison from the checked-out repository and state it.
3. Inspect the complete diff and relevant call sites, tests, configuration,
   schemas, and callers. Trace cross-module effects before reporting anything.
4. Keep findings scoped to code added or modified by the target change. Existing
   code may be context or evidence, but do not report unrelated pre-existing
   defects.

## Run all three passes

Run these passes independently before combining results:

1. Read [correctness-and-security.md](references/correctness-and-security.md)
   and audit for bugs, security flaws, breaking behavior, developer-experience
   regressions, and feature-gate leaks.
2. Read [structural-quality.md](references/structural-quality.md) and audit for
   maintainability regressions, poor boundaries, spaghetti growth, and missed
   opportunities for dramatic structural simplification.
3. Read [over-engineering.md](references/over-engineering.md) and identify code
   or dependencies that can be deleted, replaced with native facilities, or
   postponed under YAGNI.

Do not let one pass dilute another. A compact implementation can still be
incorrect; a correct implementation can still create unacceptable structural
debt; a clean abstraction can still be speculative.

## Apply mechanical gates

Prefer tool output to prose judgment. Run what the repo has; state the gap when a tool is missing:

- Complexity: run the available lint/complexity tool on the diff. Flag functions
  with high cyclomatic complexity or CRAP >= 10 (complexity weighted by
  uncovered paths).
- Coverage: run coverage on the diff when possible. Flag changed behavior that
  no test asserts.
- Dependencies: flag new upward or cyclic dependencies (unstable modules must
  depend on stable ones).
- Cohesion: flag changes that scatter one concern across unrelated modules
  (Common Closure violation).
- Tests: if a test asserts little or looks agent-generated, mutate one condition
  in the code under test and confirm some test fails. If none fails, report the
  suite as ineffective.

Calibrate depth: low-risk diffs get a spot check; security, money, and

data-integrity paths get every gate. For agent-generated diffs, add a reviewer
using a different model and treat generated output as unverified until the
gates pass.

## Evidence and confidence

- Reproduce or trace each candidate finding end to end. Inspect available code
  rather than leaving conditional research unfinished.
- Run focused tests or static checks when they materially confirm a finding.
- Distinguish demonstrated impact from inference. Do not inflate severity.
- Do not report intended, well-constrained breakage unless its implications are
  likely misunderstood, unsafe, or broader than intended.
- If medium-or-higher findings remain and a PR/MR exists, inspect its discussion
  only after completing the independent audit. Evaluate relevant reviewer or bot
  findings and identify those sources when incorporating them.

## Output

Lead with actionable findings ordered by severity and confidence. For each
finding include:

- priority (`P0` critical through `P3` low)
- tight file and line location
- the concrete failure or structural problem
- the triggering scenario and impact
- the smallest credible remedy or cleaner design direction

Keep over-engineering-only findings terse and tag them as `delete`, `stdlib`,
`native`, `yagni`, or `shrink`. End that subsection with an honest estimate:
`net: -<N> lines possible.` Omit speculative line savings.

After findings, add a short residual-risk or verification note only when useful.
Do not bury findings in a general summary. Do not flood the review with cosmetic
nits while higher-value problems exist.

If there are no actionable findings, say so clearly. If the diff is also already
lean, add `Lean already. Ship.`
