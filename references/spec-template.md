# `RATE_LIMITS.md` contract

Keep this document human-readable and implementation-ready. Do not duplicate it
into a separate configuration file.

```markdown
---
version: 1
status: needs-spec-review
verification: simple
approved_at: null
approved_by: null
---

# Rate limits

## Problem statement

Why this application needs limits, from the owner and user's perspective.

## Protected resources

The money, capacity, security boundary, or user experience being protected.

## Reviewed surfaces

| Surface | Risk | Decision | Reason |
| --- | --- | --- | --- |

## Approved limit matrix

| ID | Action | Actors | Identity | Allowance | Burst | Backend | On backend failure |
| --- | --- | --- | --- | --- | --- | --- | --- |

## Limiter details

### RL-01 — Descriptive name

- **User policy:** What users may do, in plain language.
- **Protected resource:** What this prevents from being exhausted or abused.
- **Scope:** The conceptual request or workflow boundary; avoid fragile code
  paths when domain language is clearer.
- **Identity:** User, account, hashed API identity, IP, or approved combination.
- **Exemptions/tiers:** Paid, admin, internal, BYOK, or none.
- **Algorithm:** Fixed window, sliding window, token bucket, concurrency, or
  another justified mechanism.
- **Backend:** Existing or approved infrastructure.
- **Key namespace:** Stable conceptual prefix; never raw credentials.
- **Blocked response:** Status, `Retry-After`, useful rate-limit headers,
  message, and UI recovery action.
- **Backend outage:** Fail open, fail closed, or named fallback.
- **Observability:** Minimal metrics/logs needed to operate the limit.
- **Verification:** Simple smoke behavior or thorough acceptance cases.

## Implementation decisions

Provider, module boundaries, runtime constraints, migrations, and operational
decisions. Avoid code snippets and fragile file lists.

## Verification decisions

The selected depth, test seam, commands/checks, and externally observable
behaviors that prove the policy.

## Acceptance criteria

- [ ] Every approved limiter is enforced at the intended boundary.
- [ ] Identity, tiers, and exemptions match the matrix.
- [ ] Blocked users receive the approved recovery information.
- [ ] Backend outage behavior matches the contract.
- [ ] Selected verification is complete or blockers are recorded.

## Out of scope

Controls intentionally deferred, including broader bot, fraud, WAF, or abuse
systems that are not required for the approved rate limits.

## Open questions

Questions must be empty before approval.

## Review record

- Spec approval: pending
- Implementation review: pending
```

Status transitions are strict:

`needs-spec-review` → `approved` → `needs-implementation-review` → `complete`.

Only explicit human decisions advance either review gate.
