# Provider routing

Choose infrastructure after the policy is approved. Confirm current official
documentation before implementation; package APIs and platform capabilities can
change.

## Selection order

1. Reuse a production-ready limiter already present in the application.
2. Prefer one backend for all compatible limits in the app.
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
- **Supabase, Neon, Prisma, or Postgres:** consider a database-backed design
  only when atomicity, cleanup/expiry, contention, latency, and serverless
  connection behavior are acceptable. The presence of an ORM alone is not
  enough.
- **No suitable backend:** recommend the smallest operational addition. Upstash
  is often appropriate for serverless JavaScript, but it is not mandatory.
- **Platform/WAF limit available:** use it for coarse global protection when it
  complements rather than replaces user/action policy.

## Guardrails

- Do not mix backends per limiter without a concrete operational reason.
- Do not trust user-supplied identity values without server-side verification.
- Do not use raw API keys, tokens, email addresses, or sensitive identifiers in
  logs or Redis keys; hash or map them when needed.
- Include a namespace per application and action.
- Define backend outage behavior explicitly.
- Keep rate limiting separate from billing entitlements unless the approved
  specification deliberately connects them.
