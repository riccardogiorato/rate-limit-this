---
name: rate-limit-this
description: Audit a web app, design human-approved rate limits, and implement the approved specification. Invoke manually when you want to decide where limits belong, define fair usage policies, choose an implementation suited to the existing stack, or add reviewed rate limiting to a JavaScript or TypeScript application.
---

# Rate Limit This

Use a human-first, two-gate workflow. Discover the app's likely limits before
asking questions. Write the policy as a durable contract before changing code.

## Route the run

Read repository instructions and `RATE_LIMITS.md` when it exists.

- No spec: begin the audit.
- `needs-spec-review`: present the draft and resolve review feedback. Do not
  implement.
- `approved`: begin implementation from the approved contract.
- `needs-implementation-review`: present verification evidence and stop.
- `complete`: treat a new invocation as a request to audit or revise the limits;
  never silently reopen implementation.

## Phase 1: design the limits

### 1. Audit the application

Read [references/detection-playbook.md](references/detection-playbook.md).
Inspect the whole request surface, existing authentication, external services,
costly operations, persistence, deployment runtime, and current limiters.

Identify candidate limits and explain each in product language:

- what resource or user experience it protects;
- who can trigger it;
- the likely cost or abuse mode;
- what identity and backend already exist;
- confidence and remaining policy questions.

Rank candidates. Do not recommend limits for harmless routes merely because
they are public.

Complete the audit only after every public write, expensive external call,
security-sensitive action, upload, generation workflow, and background-job
trigger has been considered.

### 2. Interview the human

Ask one question at a time and recommend an answer. Ask only about material
gaps the code cannot answer.

Use plain-language behavior questions. Translate the answers into algorithms
and infrastructure in the spec. Cover, when relevant:

- allowance and time window;
- whether short bursts are acceptable;
- identity: authenticated user, account, hashed API identity, IP, or an
  explicitly approved combination;
- paid, anonymous, admin, internal, and bring-your-own-key exemptions;
- global cost circuit breakers;
- what the user experiences when blocked;
- what happens when the limiter backend is unavailable;
- **Simple** or **Thorough** verification.

Offer browser fingerprinting only as an advanced opt-in. Explain its privacy
and spoofing tradeoffs, and do not use it as the sole protection by default.
Never store a raw API key as an identifier.

### 3. Write the contract

Read [references/spec-template.md](references/spec-template.md). Create or
update one root-level `RATE_LIMITS.md`; do not create a competing JSON or YAML
spec. Give every limiter a stable ID.

Set:

```yaml
status: needs-spec-review
```

Present the draft and stop. Application code, external resources, credentials,
and deployment configuration must remain unchanged.

### 4. Pass the first gate

Only explicit human approval can change the status to `approved`. Record the
approval in `RATE_LIMITS.md`. Feedback, silence, or approval of only one limiter
does not approve the whole contract.

## Phase 2: implement the approved contract

### 5. Route the implementation

Read [references/provider-routing.md](references/provider-routing.md). Reuse one
backend that fits the existing application when practical. Verify current
provider documentation before changing dependencies or schemas.

When subagents are available, hand implementation to a fresh subagent. Give it
the approved `RATE_LIMITS.md`, repository instructions, and codebase—not the
unresolved planning conversation. Require it to preserve the approved policy
and report any necessary deviation before acting.

Creating an account, database, credential, OAuth grant, or deployment secret
requires action-time human confirmation even when the provider is approved in
the spec.

### 6. Verify proportionally

Use the verification depth approved in the spec.

- **Simple**: reuse existing checks and perform a focused smoke check showing
  allowed traffic, the blocked request, and the expected recovery/reset path.
- **Thorough**: extend existing automated tests for identities, exemptions,
  headers, reset behavior, and backend failure modes, plus a safe provider smoke
  test when credentials exist.

Do not introduce a test framework solely for rate limiting. Document checks
that could not run rather than pretending they passed.

### 7. Pass the final gate

Update the spec to reflect the implementation and set:

```yaml
status: needs-implementation-review
```

Present the diff, verification evidence, deviations, and remaining operational
steps. Stop before merge, deployment, publication, or announcement unless the
human explicitly asks for that action.

Only explicit final approval changes the status to `complete`.
