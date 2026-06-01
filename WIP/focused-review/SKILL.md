---
name: focused-review
description: Focused code review skill with multiple scope modes. Use when the user wants to review, audit, or assess code quality of a project — whether for a specific feature, user flow, hotspot, large file, architectural layer, or type contracts. Triggers on "review", "audit", "code review", "asses quality", "find problems", "check maintainability". NEVER review the entire project unless explicitly requested.
---

# Focused Review

Never review the entire project unless explicitly requested. Always select a scope mode first.

## Mode Selection

Before running any review, ask the user which mode they want (or recommend one based on context).

If no mode is specified, recommend modes in this order:

1. **Hotspot** — best default for unknown projects
2. **Feature** — when a specific feature is mentioned
3. **User Flow** — when a business flow is mentioned
4. **Large File** — when oversized files are obvious
5. **Layer** — when an architectural concern is mentioned

---

## Mode 1 — Feature Review

Use when auditing one product feature.

**Scope:** Only files directly related to the selected feature:
components, pages/routes, hooks, stores, services, use cases, validators, types, API clients, tests, related helpers.

**Prompt to agent:**

> Review only this feature: `[FEATURE_NAME]`
>
> Do not review the whole project. Focus only on implementation quality, boundaries, abstractions, branching complexity, file size, type contracts, and maintainability of this feature.

---

## Mode 2 — User Flow Review

Use when auditing a complete business flow from entry to outcome.

**Scope:** Only files involved in the selected flow.

**Focus areas:** flow clarity, orchestration, state transitions, duplicated decisions, misplaced business logic, side effects, error handling, type contracts, unnecessary branching.

**Prompt to agent:**

> Review only this user flow: `[USER_FLOW]`
>
> Start from the user entry point and follow the code path until the flow completes. Do not audit unrelated modules.

---

## Mode 3 — Hotspot Review

Use when the user does not know where to start. Best default for unknown projects.

**Process:**

1. Shallow scan of the project
2. Identify highest-risk areas:
   - large files
   - complex conditionals
   - duplicated logic
   - unclear ownership
   - mixed responsibilities
   - weak types
   - excessive side effects
   - modules that import too much
   - files likely to change often
3. Select only the top 3 hotspots
4. Deep review only on those 3

**Prompt to agent:**

> Do a shallow scan first. Identify the top 3 maintainability hotspots. Then perform a deep review only on those hotspots. Do not review the full project.

---

## Mode 4 — Large File Review

Use when the project has obvious oversized files.

**Scope:** Top 5 largest or most complex files.

**For each file, explain:**

- why it is risky
- what responsibilities are mixed
- what should be extracted
- what abstractions should disappear
- how to decompose it safely

**Prompt to agent:**

> Find the largest or most complex files in the project. Review only the top 5 files that appear most dangerous for maintainability.

---

## Mode 5 — Layer Review

Use when reviewing only one architectural layer.

**Available layers:**

UI/components · routing/pages · hooks/state · services/use cases · domain models · API clients · database/repositories · DTOs/types/contracts · tests · infrastructure/config

**Prompt to agent:**

> Review only this layer: `[LAYER_NAME]`
>
> Do not review the full project. Focus on whether this layer owns the right responsibilities and whether logic is leaking across boundaries.

---

## Mode 6 — Contract and Type Review

Use when the project feels fragile because of loose data shapes.

**Scope:** Type boundaries, DTOs, API contracts, request/response models, validators, optional fields, casts, fallbacks, shared types.

**Prompt to agent:**

> Review only the type boundaries, DTOs, API contracts, request/response models, validators, optional fields, casts, fallbacks, and shared types. Do not review general code style. Focus on whether the project has explicit, maintainable contracts.

---

## Output Format

For every finding, report:

| Field | Description |
|-------|-------------|
| **SEVERITY** | BLOCKER · MAJOR · MINOR · INFO |
| **LOCATION** | File path + line/range |
| **DESCRIPTION** | What's wrong and why it matters |
| **REMEDY** | Specific fix action |

End with a verdict per reviewed item:

- **APPROVE** — No blockers or majors
- **REQUEST_CHANGES** — Blockers or majors found
- **NEEDS_REWORK** — Fundamental issues requiring restructure
