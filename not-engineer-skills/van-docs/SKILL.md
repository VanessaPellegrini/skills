---
name: van-docs
description: Decides whether a change needs documentation, which document type fits, where it lives according to the repository's convention, and the minimum format for each type; keeps existing docs from rotting. Use when creating or editing READMEs, runbooks, ADRs, standards, plans, changelogs, OpenAPI specs, or code comments; when adding or changing an HTTP endpoint; when a code change contradicts an existing doc; when the user asks "where do I document this" or "does this need documentation". NOT for external prose (blog posts, ebooks) or product specs.
---

# Documentation

## Overview

Code is the source of truth for the *what*. Document only what code cannot say: the *why*, the constraints, the decisions, and the contracts nobody can infer by reading the repo. Every doc is a maintenance promise: if nobody will update it, don't write it.

## When to Use

- A decision is expensive to reverse or shapes the design of several modules or teams.
- There is a non-obvious external constraint: compliance, vendor, cost, SLA.
- A new human or agent would need hours of archaeology to learn what two paragraphs explain.
- An operational procedure breaks production when done wrong.
- A code change leaves an existing doc out of date.
- A change adds, removes, or alters an HTTP endpoint.
- The user asks where or whether to document something.

**When NOT to use:**

- The code, tests, or types already say it.
- You would only add a copy of information that already lives in its canonical place.
- It is an expired plan: dead plans live in git history.
- It is "just in case": if you cannot name who reads it and when, don't write it.

## Core Process

### Step 1: Survey the repository convention

Look at what exists before writing anything. The repository convention overrides this skill.

1. Read the docs index if there is one: root `README.md`, `AGENTS.md`, `docs/README.md`, or a per-folder index.
2. List `docs/` and note: naming (kebab-case, numbering), location per type, recurring frontmatter or headings.
3. If the repo has a template for the type you are about to write, use it as is.
4. If numbering is sequential (ADR-12, doc-21), find the next free number across all open branches, not only main:

```bash
git fetch --all --prune
git branch -r | sed 's#origin/##' | xargs -I{} git ls-tree -r --name-only {} -- docs/ | sort -u | grep -E '^docs/.*/[0-9]+'
```

### Step 2: The ten-second test

Answer in writing, even if in one line:

- Who reads it?
- When?
- What do they do differently after reading it?

If any answer is "I don't know", don't write it. If there are two readers with different needs, that is two docs.

### Step 3: Pick the type

Each doc answers one need for one reader (Diátaxis: tutorial / how-to / reference / explanation), plus the records that are not meant for linear reading.

| Type | Answers | Content | Lifespan |
|---|---|---|---|
| README | "what is this and how do I start?" | what it is in 2 lines, run locally, where to read next | maintained |
| Tutorial | "how do I use it for the first time?" | from clone to first success, copy-paste | maintained |
| How-to / runbook | "how do I do X correctly?" | deploy, incident, migration | maintained |
| Reference | "what is the exact value of Y?" | config, domain glossary | maintained |
| API spec | "what does this endpoint accept and return?" | OpenAPI document | maintained |
| Explanation | "why is it like this?" | architecture, trade-offs | maintained |
| Standard | "which rule always applies to this surface?" | scope, required checks, exceptions | permanent |
| ADR | "what did we decide and why?" | one decision per file | permanent |
| Plan | "what are we going to do?" | tasks, order, exit criteria | temporary, with expiry date |
| Evidence | "what was verified and when?" | smoke test, acceptance result | temporary, archived |

If the repo defines its own types (for example a folder for standards, runbooks, or smoke-test evidence), those types and locations win.

### Step 4: Write with the minimum format

**README.** What it is in 2 lines, how to run it, where to read next. No project history.

**Runbook / how-to.** Expected outcome in the first line. Numbered, verifiable steps. How to confirm it worked. How to roll back if it failed. A runbook that has never been run end to end is fiction.

**Reference.** Tables or lists, zero narrative, one minimal example per entry.

**API spec.** See [API documentation](#api-documentation).

**Standard.** Scope, required checks, exceptions, and escalation path. Catalog entry in the same change.

**ADR.** Context, Decision, Consequences, Alternatives considered, Status (`proposed` / `accepted` / `superseded by ADR-XXXX`). One decision, one file.

**Explanation / architecture.** Start from the current state, not the history. The text must survive without the diagram.

**Plan and evidence.** Creation date and expiry or review date in the header.

### Step 5: Link and close

1. Add the doc to the repo's single index in the same change. Past ~5 docs, a repo needs one index. Two indexes is how rot starts.
2. If the doc replaces another, the old one keeps a `superseded by` pointer to the new one. Never delete it: history lives in git, the pointer lives in the doc.
3. Declare the lifespan: temporary / maintained / permanent.

## Keeping docs current when code changes

If a code change contradicts a doc, fix the doc in the same PR. Code wins, but in that same PR.

If the fix does not fit in the PR, that same PR adds a note at the top of the doc:

```markdown
> Out of date since 2026-09-28. Behavior changed in #123; see issue #124 for the fix.
```

A doc marked as stale does not lie. A doc that looks valid and isn't does.

## API documentation

Every HTTP API is documented in OpenAPI, public or internal. Prose does not
replace the spec: a README table of endpoints drifts, a spec checked by a test
does not.

- One OpenAPI 3.1 document per service, versioned in the repo at the location
  the repo already uses. With none, use `openapi.yaml` at the service root.
- Every operation declares `operationId`, summary, parameters, request body,
  each response status with its schema, and the security scheme it requires.
  Error responses are documented, not only the 2xx.
- Generate the spec from the schemas the code already validates (Zod,
  TypeBox, class validators) when the stack allows it. A hand-written spec is
  acceptable only with a test that fails when routes and spec diverge.
- Adding, removing, or changing an endpoint updates the spec in the same PR.
  A removed endpoint is marked `deprecated: true` for a release before it
  disappears, when clients outside the repo consume it.
- Lint the spec in CI (Redocly, Spectral, or the repo's validator).
- Internal and admin endpoints are documented too. Mark them with a tag
  instead of leaving them out.
- A repo with endpoints and no spec is a gap to report, not a reason to skip
  the rule for the new endpoint: document the new endpoint and flag the rest.

## Code comments

This section extends the comment rules in `AGENTS.md`; if the repo has a different convention, the repo wins.

- If a comment explains the *what*, refactor instead of commenting: names, types, small functions.
- The why behind a design decision goes in the ADR, the PR, or the commit.
- The why behind a surprising line goes next to the line, in one sentence. A `sleep` for a vendor rate limit or a flag that dodges a tooling bug is read right there; nobody runs `git blame` for that.
- External workarounds (vendor bug, upstream issue): link plus date, so you know when it can be removed.
- Tooling directives (`@ts-expect-error`, `eslint-disable`) carry the reason on the same line.
- TODO only with a link to an issue. A TODO without an issue is a wish, not a task.
- Commented-out code gets deleted. Git has the history.

## Changelog

- Default: the git log is the changelog. With conventional commits it is greppable and generatable.
- `CHANGELOG.md` only when there are readers who will not open git: app releases, libraries consumed by others, a blog.
- Keep a Changelog format: `Added / Changed / Deprecated / Removed / Fixed / Security`, one entry per version.
- Language for the person who uses it, not the one who built it: "exports now fail closed" instead of "refactored the validation module".
- Written when the version is cut, never "I'll fill it in later". Without `CHANGELOG.md`, the release note goes in the annotated tag or the GitHub release.
- With generators (semantic-release, changesets), conventional commits stop being optional.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The code is self-documenting" | Code shows the what. It does not show the why, the rejected alternatives, or the external constraints. |
| "I'll document it once it stabilizes" | Writing the doc is the first test of the design. What cannot be explained in two paragraphs is not stable. |
| "I'll update the old doc later" | Later does not exist. A doc that contradicts the code lies from the merge onward. Fix it or mark it in the same PR. |
| "I'll leave this plan in case it's useful" | A plan without an expiry date is a doc that will lie. Archive it or delete it; git keeps it. |
| "I'll add a new index for this section" | Two indexes is where rot starts. Extend the existing index. |
| "I'll use the next number I see on main" | Another open branch may already have it. A duplicate number is confusion forever. |
| "It's an internal endpoint, nobody else calls it" | Agents, the frontend, and your future self call it. Undocumented internal APIs are the ones that break silently. |
| "The route handler is the documentation" | The handler shows one branch at a time. The spec shows the whole contract, including errors and auth, in one place. |
| "I'll explain it in a long comment" | If it explains the what, it is a pending refactor. If it explains a decision, it is an ADR or a PR. |

## Red Flags

- A new doc with no entry in the repo index.
- Two docs explaining the same thing with different wording.
- A plan without an expiry date.
- A superseded ADR without a `superseded by` pointer.
- A runbook without a verification step or a rollback step.
- An architecture doc that opens with the project's history.
- Comments that paraphrase the next line.
- TODOs without an issue, commented-out code.
- A PR that changes behavior and touches no doc that described it.
- An endpoint added or changed without an OpenAPI change in the same PR.
- An OpenAPI spec with no CI lint or no test tying it to the routes.
- Only 2xx responses documented.

## Verification

Before closing the change:

- [ ] The repo convention was surveyed and the doc follows it (location, naming, numbering, template).
- [ ] The ten-second test has three concrete answers.
- [ ] The doc is in the repo's single index, in the same change.
- [ ] Lifespan declared; if temporary, it has an expiry date.
- [ ] No existing doc contradicts the code after the change, or it is marked stale with an issue.
- [ ] Superseded docs point to their replacement.
- [ ] Every added or changed endpoint is in the OpenAPI spec, with errors and auth, and the spec lint passes.
- [ ] No new TODO without an issue, no commented-out code block.
