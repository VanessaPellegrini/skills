# Correctness and security pass

Audit the changed behavior as if it were security-sensitive and depended on by
multiple packages. Nothing is a finding until it is traced to a credible impact.

## Inspect aggressively

- Incorrect state transitions, data loss, races, non-atomic related updates,
  stale reads, error swallowing, unsafe retries, and partial failure behavior.
- Authentication, authorization, tenant isolation, input validation, injection,
  secrets, unsafe deserialization, path handling, permission changes, and data
  exposure.
- Breaking API, schema, persistence, CLI, configuration, or runtime behavior,
  including compatibility with existing callers and stored data.
- Developer-experience regressions: changed environment variables or secret
  lookup, ports, networking, build/run prerequisites, scripts, local setup, and
  undocumented required manual steps.
- Feature-gate leaks through alternate routes, server/client disagreement,
  missing internal checks, caching, serialization, background jobs, or direct API
  access.
- Tests that pass while failing to exercise the changed contract, negative path,
  boundary condition, or rollback/recovery behavior.

Trace side effects across callers, consumers, migrations, jobs, APIs, and UI
boundaries. Check the implementation that could disprove a concern before
reporting it. Never write an unfinished finding such as “this is broken unless
the backend handles it” when the backend is available to inspect.

## Severity discipline

- `P0`: immediate catastrophic impact or active compromise.
- `P1`: high-probability security, data-loss, or core-functionality failure.
- `P2`: meaningful correctness, compatibility, or operational regression.
- `P3`: localized lower-impact defect with a concrete scenario.

Severity represents demonstrated impact and likelihood, not reviewer anxiety.
