# Provider routing

Choose infrastructure after the policy is approved. Confirm current official
documentation before implementation; package APIs and platform capabilities can
change.

## Selection order

1. Reuse a production-ready limiter already present in the application.
2. Choose one primary backend for all compatible application limits.
3. Reuse the app's existing operational stack when it provides atomic counters,
   expiry, and acceptable serverless/runtime behavior.
4. Add new infrastructure only when the existing stack is a poor fit.

## Common routes

- **Upstash Redis already present:** prefer the supported Upstash rate-limit
  library and preserve existing connection/env conventions.
- **Redis already present:** use a maintained limiter compatible with that
  client and runtime; do not introduce a second Redis provider without reason.
- **Convex application:** prefer the maintained Convex rate-limiter component
  or a current documented Convex-native pattern.
- **Prisma, Neon, Postgres, or Supabase already operational:** consider a
  database-backed design only when an atomic server-side operation, cleanup or
  expiry, hot-key contention, latency, and serverless connection behavior are
  acceptable. The presence of an ORM alone is not enough, and a read-then-write
  counter is not atomic.
- **Platform-native protection already available:** use it as the primary
  control when the approved policy is coarse enough for its identity, window,
  exemption, outage, response, and observability capabilities. Use WAF or edge
  rules as defense in depth—not as a silent replacement—when the policy needs
  application-authenticated identity or action-specific recovery behavior.
- **No suitable backend:** recommend the smallest operational addition. Upstash
  is often appropriate for serverless JavaScript, but it is not mandatory.

Do not add both Redis and a database implementation for convenience. When a
platform-wide circuit breaker necessarily differs from the application
limiter, name that exception and its operational reason in the contract.

## Match behavior to mechanism

- Use a fixed window only when boundary bursts and simpler semantics are
  acceptable.
- Use a sliding window for a smoother allowance across time boundaries.
- Use a token bucket when the policy deliberately permits short bursts with a
  defined refill rate.
- Use concurrency limits or leases for long-running scarce work; request counts
  alone do not cap simultaneous jobs.
- Keep provider spend or capacity circuit breakers separate from per-user fair
  use, even if they share a backend.

## Guardrails

- Do not mix backends per limiter without a concrete operational reason.
- Do not trust user-supplied identity values without server-side verification.
- Do not use raw API keys, tokens, email addresses, or sensitive identifiers in
  logs or Redis keys; hash or map them when needed.
- Include a namespace per application and action.
- Define backend outage behavior explicitly.
- Verify runtime compatibility before choosing an SDK, especially for Next.js
  Edge versus Node.js execution.
- Ensure check-and-consume is atomic and expiration cannot leave unbounded keys
  or rows.
- Make retries and multi-step workflows consume exactly what the approved
  counting point says.
- Keep rate limiting separate from billing entitlements unless the approved
  specification deliberately connects them.
