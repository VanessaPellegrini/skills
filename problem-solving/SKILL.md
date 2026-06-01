---
name: problem-solving
description: >
  Route problems to the right mental model. Classify -> select strategy -> produce artifact.
  Triggers on: problem, idea, vision, bug, incident, decision, architecture, refactor,
  migration, stuck, unsure, how to approach, RCA, spike, trade-off, ADR.
---

# Problem Solving Router

Classify -> pick method -> ask minimum questions -> produce artifact.

## Step 1 — Classify

| Signal | Category | Method | Artifact |
|--------|----------|--------|----------|
| "I have an idea but don't know where to start" | Fuzzy idea | Heuristic Resolution (Polya) | Problem Brief |
| "I want an app for X" | Unmodeled idea | Problem Modeling | Domain Model |
| "I don't know what the user needs" | User uncertainty | Design Thinking / Double Diamond | Problem Statement + Prototype |
| "I think this could work" | Product hypothesis | Lean / Build-Measure-Learn | Experiment Card |
| "Should I use X or Y?" | Technical decision | ADR + Trade-off Analysis | ADR |
| "This is too big" | Oversized problem | Divide and Conquer | Work Breakdown + Milestones |
| "I keep doing the same thing" | Repetitive work | Knowledge Reuse | Template / Checklist |
| "It broke again" | Bug or incident | Debugging + RCA | Bug/RCA Report |
| "Is it working? How do I know?" | Operational uncertainty | Observability / SRE | Observability Plan |
| "This code is a mess but it works" | Legacy / tech debt | Strangler Fig | Migration Plan |
| "Is this technically possible?" | Technical unknown | Technical Spike | Spike Report |
| "Does this feel right?" | UX risk | Rapid Prototyping | Clickable Prototype |
| "Too many combinations" | Hard optimization | Heuristics / Approximation | Heuristic Strategy |
| "Never seen this before" | Unknown problem | Problem Reduction | Problem Mapping |

Multiple signals match -> pick **highest-risk** first. Re-enter for secondary concerns.

## Step 2 — Apply Method

For each method, ask its guiding questions then produce the artifact. The model already knows these methods — no need to explain them.

**Guiding questions per method:**

- **Polya:** What to achieve? Known/unknown? Constraints? First step?
- **Problem Modeling:** Inputs? Outputs? Entities? Core operation? Classic problem it resembles?
- **Design Thinking:** Who has this problem? How solve today? Real pain? Minimal solution?
- **Lean:** Hypothesis? Smallest experiment? Validating metric?
- **ADR:** Decision context? Options (2-5)? Trade-offs each? Reversal trigger?
- **Divide & Conquer:** Independent parts? Which unblocks others? Parallelizable?
- **Knowledge Reuse:** Solved before? Pattern repeats? Template material?
- **Debugging + RCA:** Expected vs actual? What changed? Working/broken boundary? Reproduction steps? (5 Whys for simple, Fishbone for multiple causes — never blame people)
- **Observability:** Health signal? Key metric? Useful alert?
- **Strangler Fig:** What to isolate first? Compatibility contract? Rollback strategy?
- **Spike:** Risk? Minimal proof? Time budget? Go/no-go criteria?
- **Prototyping:** Must-feel-real part? Can-be-fake part?
- **Heuristics:** Optimal or good-enough? Relaxable constraint?
- **Problem Reduction:** Map to known category (search? sorting? scheduling? matching? state machine?)

## Step 3 — Output Template

```
## Classification: [category]
## Method: [method]
## Why: [1-2 sentences]
## Key Questions: [3-5 max]
## Artifact: [concrete output]
## Next Step: [one actionable step]
## Risks: [1-3]
```

## Step 4 — Re-assess if Stuck

1. Re-classify — maybe wrong problem type
2. Compound problem? Apply methods in sequence
3. Fallback: Polya (most general)

## Common Sequences

| Situation | Sequence |
|-----------|----------|
| New product | Fuzzy Idea -> Problem Modeling -> Design Thinking -> Lean |
| Technical migration | ADR -> Divide & Conquer -> Strangler Fig -> Observability |
| Recurring bug | Debugging -> RCA -> Knowledge Reuse |
| Large vision | Fuzzy Idea -> Divide & Conquer -> [per-part method] |
| Legacy system | ADR -> Strangler Fig -> Observability -> Refactor |

## ADR Template

```markdown
# ADR-NNN: Title

## Status: [Proposed | Accepted | Deprecated | Superseded by ADR-XXX]
## Context: What motivates this decision?
## Options: [2-5 with pros/cons each]
## Decision: What and why?
## Consequences: Easier or harder after?
## Reversal criteria: What triggers reconsideration?
```

## Rules

- Classify before implementing
- Always produce concrete artifact
- Uncertain between two methods -> explain both, let user choose
- Match user's language; technical terms in English
- RCA: never blame people, find systemic causes
