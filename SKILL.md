---
name: rate-limit-this
description: Audit a JavaScript or TypeScript web app, especially Next.js, for abuse and rate-limit surfaces, ask the human only material policy questions, write a root RATE_LIMITS.md for review, and route an explicitly approved specification to a fresh implementation agent. Invoke manually to design fair usage policies, choose protections suited to the existing stack, or implement a reviewed rate-limit contract.
---

# Rate Limit This

Use two steps: first audit the app and agree on what should and should not be
rate limited, then implement the approved limits in a fresh agent context.
Write the agreed policy as a durable contract before changing code.

## Route the run

Read repository instructions such as `AGENTS.md` or `CLAUDE.md`, then read the
root `RATE_LIMITS.md` when it exists. Treat missing or unknown statuses as a
reason to stop and repair the contract, not permission to implement.

- No spec: begin the audit.
- `needs-spec-review`: present the draft and resolve review feedback. Do not
  implement.
- `approved`: proceed only when the current prompt explicitly identifies this
  context as the fresh implementation agent for that approved contract.
  Otherwise, route the contract to one; the planning agent must not implement.
- `needs-implementation-review`: present verification evidence and stop.
- `complete`: treat a new invocation as a request to audit or revise the limits;
  never silently reopen implementation.

## Step 1: audit and agree on the limits

### 1. Audit the application

Read [references/detection-playbook.md](references/detection-playbook.md).
Inspect the request and work-triggering surface, existing authentication,
external services, costly operations, persistence, deployment runtime, and
current limiters. Start with JavaScript/TypeScript and Next.js conventions, but
follow the repository's actual architecture.

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
trigger has been considered and recorded as **limit**, **no limit**, or
**defer** with a reason.

### 2. Interview the human

Ask one question at a time and wait for its answer. Ask only when the answer
could materially change protection, user experience, operational risk, or
verification. For each question:

1. Ask in plain product language.
2. Give one recommended default based on repository evidence and explain why in
   one sentence.
3. Mention alternatives only when their tradeoff is material.

Do not make the human choose an algorithm or provider when the desired behavior
and existing stack determine it. If no material gaps remain, draft the contract
without inventing questions.

Translate the answers into algorithms and infrastructure in the spec. Resolve,
when relevant:

- allowance and time window;
- whether short bursts are acceptable;
- identity: authenticated user, account, hashed API identity, IP, or an
  explicitly approved combination;
- paid, anonymous, admin, internal, and bring-your-own-key exemptions;
- global cost circuit breakers;
- what the user experiences when blocked;
- what happens when the limiter backend is unavailable;
- **Simple** or **Thorough** verification.

Finish the interview when every material decision is either answered or named
as an open question in the draft. Do not hide uncertainty behind a default.

Offer browser fingerprinting only after ordinary server-verified identities and
coarse platform controls are insufficient. Treat it as advanced opt-in, explain
its privacy and spoofing tradeoffs, and require explicit approval. Do not use it
as the sole protection. Never store a raw API key as an identifier.

### 3. Write the contract

Read [references/spec-template.md](references/spec-template.md). Create or
update one root-level `RATE_LIMITS.md`; do not create a competing JSON or YAML
spec. Give every limiter a stable ID. Explicitly resolve identity, algorithm,
allowance and burst, exemptions, backend, outage behavior, blocked response and
recovery, observability, and verification for each limiter.

Set:

```yaml
status: needs-spec-review
```

Present the draft, call out recommendations and open questions, and stop.
Application code, dependencies, schemas, external resources, credentials, and
deployment configuration must remain unchanged.

### 4. Pass the first gate

Only explicit human approval of the whole current contract can change the
status to `approved`. Before recording approval, require an empty **Open
questions** section. Record the approved revision, who approved, and when in
`RATE_LIMITS.md`. Increment the revision whenever a shared draft's policy
changes. Feedback, silence, approval of one limiter, or a request to continue
does not approve the whole contract.

## Step 2: implement the approved limits

### 5. Route the implementation

Read [references/provider-routing.md](references/provider-routing.md). Choose
one suitable primary enforcement backend or platform control based on the
existing stack. Verify current official provider documentation before changing
dependencies, schemas, or platform configuration.

Always hand implementation to a fresh subagent with a prompt that explicitly
identifies it as the implementation agent for the approved contract. Give it
the approved `RATE_LIMITS.md`, repository instructions, and codebase, not the
unresolved planning conversation. Require it to preserve the approved policy
and report any necessary deviation before acting. If the host cannot create
subagents, stop and ask the human to begin a fresh implementation task/session
with that same explicit role and the approved contract; never fall back to
implementation in the planning context.

Before implementing, the fresh agent must confirm that the status is
`approved`, `approved_revision` matches `revision`, approval metadata is
present, the review record covers that revision, and **Open questions** is
empty. Otherwise return to Step 1.

Creating an account, database, credential, OAuth grant, or deployment secret
requires action-time human confirmation even when the provider is approved in
the spec.

If repository reality makes an approved decision unsafe or impossible, stop
and reset the contract to `needs-spec-review`, clearing stale approval metadata;
do not silently substitute a different identity, limit, exemption, backend,
outage policy, or user behavior.

### 6. Verify proportionally

Use the verification depth approved in the spec.

- **Simple**: reuse existing checks and perform a focused smoke check showing
  allowed traffic, the blocked request, and the expected recovery/reset path.
  Add a small test only when the repository already has a suitable seam.
- **Thorough**: extend existing automated tests for identities, exemptions,
  headers, reset behavior, and backend failure modes, plus a safe provider smoke
  test when credentials exist.

When the contract says malformed requests do not consume capacity, validate the
required fields and types before admission. Successful JSON parsing alone does
not make a request valid. Test valid JSON with an invalid shape as well as JSON
parse failures.

Do not introduce a test framework solely for rate limiting. Document checks
that could not run rather than pretending they passed.

### 7. Pass the final gate

Update the spec to reflect the implementation and set:

```yaml
status: needs-implementation-review
```

Present the diff, verification evidence, deviations, and remaining operational
steps. Stop before merge, deployment, publication, or announcement unless the
human explicitly asks for that action after reviewing the output.

Only explicit final approval changes the status to `complete`.
