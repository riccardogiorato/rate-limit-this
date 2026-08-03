# `RATE_LIMITS.md` contract

Keep this document human-readable and implementation-ready. Do not duplicate it
into a separate configuration file.

```markdown
---
version: 1
revision: 1
status: needs-spec-review
verification: simple
approved_revision: null
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

## Limit matrix

| ID | Action | Identity | Algorithm and limit | Exemptions | Enforcement | Outage | Blocked recovery | Observe / verify |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

## Limiter details

### RL-01: Descriptive name

- **User policy:** What users may do, in plain language.
- **Protected resource:** What this prevents from being exhausted or abused.
- **Scope:** The conceptual request or workflow boundary; avoid fragile code
  paths when domain language is clearer.
- **Counting point:** Which attempt, job start, successful completion, unit of
  cost, or concurrent lease consumes capacity. If invalid requests do not
  count, define the validation boundary and what makes an input valid.
- **Identity:** User, account, hashed API identity, IP, or approved combination.
- **Identity source:** The server-verified source and normalization/fallback
  rules, including trusted-proxy rules for IP identities.
- **Exemptions/tiers:** Paid, admin, internal, BYOK, or none.
- **Algorithm:** Fixed window, sliding window, token bucket, concurrency, or
  another justified mechanism.
- **Allowance and burst:** Exact count/cost, time or refill period, burst
  behavior, and any global circuit breaker.
- **Backend:** Existing or approved infrastructure.
- **Key namespace:** Stable conceptual prefix; never raw credentials.
- **Blocked response:** Whether to use `429`, the `Retry-After` semantics, useful
  rate-limit headers, API-safe error shape, message, and UI recovery action.
- **Backend outage:** Fail open, fail closed, or named fallback.
- **Observability:** Minimal metrics/logs and privacy-safe dimensions needed to
  operate the limit, plus alerting only when justified.
- **Verification:** Limiter-specific simple smoke behavior or thorough
  acceptance cases, including recovery and the approved outage mode.

## Implementation decisions

One selected primary provider/control, module boundaries, runtime constraints,
migrations, and operational decisions. Avoid code snippets and fragile file
lists. Name any justified exception to the one-backend rule.

## Verification decisions

The selected depth, test seam, commands/checks, and externally observable
behaviors that prove the policy.

## Acceptance criteria

- [ ] Every approved limiter is enforced at the intended boundary.
- [ ] Identity, algorithm, allowance, tiers, and exemptions match the contract.
- [ ] Blocked users receive the approved recovery information.
- [ ] Backend outage behavior matches the contract.
- [ ] Metrics and logs expose the approved signals without raw secrets or
      sensitive limiter keys.
- [ ] Selected verification is complete or blockers are recorded.

## Out of scope

Controls intentionally deferred, including broader bot, fraud, WAF, or abuse
systems that are not required for the approved rate limits.

## Open questions

Questions must be empty before approval.

## Review record

- Spec approval: pending for revision 1
- Implementation review: pending
```

The normal status transitions are strict:

`needs-spec-review` → `approved` → `needs-implementation-review` → `complete`.

Only explicit human decisions advance either review gate. A material contract
change after approval resets status to `needs-spec-review`, clears
`approved_revision`, `approved_at`, and `approved_by`, increments `revision`,
and records why another review is required.

At `needs-spec-review`, phrase all limits as proposals rather than implying they
are already active. At `approved`, preserve the approved policy verbatim unless
the review record documents a later human-approved revision.
