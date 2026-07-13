# Detection playbook

Use this playbook during the audit. It is a search map, not a mandate to limit
every match.

## Candidate surfaces

- AI inference: chat, image, audio, video, embeddings, agents, and multi-step
  generation workflows.
- External APIs paid by the application: search, scraping, maps, OCR, email,
  SMS, storage transforms, and data enrichment.
- Authentication and account recovery: sign-in attempts, OTP delivery,
  invitations, password reset, and account creation.
- Public writes: uploads, comments, contact forms, reactions, webhooks, and
  mutations that create durable records.
- Bulk work: exports, imports, reports, PDF/media processing, fan-out jobs, and
  user-triggered background tasks.
- Capacity boundaries: anonymous traffic across the whole app, concurrency,
  provider quotas, and global spend protection.

## Existing signals

Start with `package.json`, lockfiles, framework/deployment configuration, and
environment-variable names. Search for route handlers, server actions, RPC
functions, webhook handlers, queues, cron triggers, SDK clients, billing checks,
auth/session access, IP helpers, Redis/Upstash, database clients, existing `429`
responses, and packages whose names contain `rate`, `limit`, `throttle`, or
`quota`.

For Next.js, inspect both App and Pages Router boundaries:

- `app/**/route.*`, `pages/api/**`, Server Actions, and RPC/GraphQL handlers;
- `middleware.*` or `proxy.*`, while remembering that broad edge middleware may
  not be the correct enforcement point for action-specific limits;
- auth callbacks, upload-signing routes, AI streams, and routes that enqueue or
  fan out background work;
- server-only helpers that call paid services, since several routes may share
  the real expensive boundary.

For other JavaScript/TypeScript stacks, inspect the equivalent Express, Hono,
Fastify, Nest, Remix, SvelteKit, Nuxt, Convex, or serverless function surfaces.

Map the trust levels already present: anonymous, authenticated, paid, admin,
internal, service-to-service, and bring-your-own-key.

Trace each candidate from the public entry point to the point where cost, side
effects, or scarce work begins. Prefer enforcing at that shared boundary so a
second route cannot bypass the policy. Note whether rejected attempts, started
jobs, successful completions, or concurrent work consume the allowance.

Treat identity evidence carefully:

- derive user, account, role, and BYOK state from server-verified data;
- accept IP forwarding headers only through the deployment's trusted proxy
  contract, and document IPv4/IPv6 normalization;
- distinguish signed provider webhooks and authenticated cron calls from
  anonymous public traffic;
- check provider retry behavior before limiting webhooks with `429`.

## Ranking

Rank a candidate higher when it combines several of these:

- direct monetary or scarce-compute cost;
- unauthenticated reach;
- easy automation or fan-out;
- security impact;
- durable writes or outbound messages;
- provider quotas shared across users;
- known abuse or reliability incidents.

Rank read-only, cached, inexpensive, authenticated operations lower unless they
threaten capacity or security.

Do not propose per-instance memory counters for horizontally scaled, serverless,
or edge deployments. Do not blanket-limit static assets, framework internals,
health checks, or CORS preflight requests without a demonstrated need.

## Audit output

For every candidate, report the protected resource, actor, entry and enforcement
boundary, counting point, existing identity, existing backend, recommendation,
confidence, and unresolved business decision. Also list reviewed surfaces that
do not need a limit so the audit is auditable without becoming alarmist.
