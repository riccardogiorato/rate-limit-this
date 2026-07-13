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

Search for route handlers, server actions, RPC functions, webhook handlers,
queues, cron triggers, SDK clients, billing checks, auth/session access, IP
helpers, Redis/Upstash, database clients, existing `429` responses, and packages
whose names contain `rate`, `limit`, `throttle`, or `quota`.

Map the trust levels already present: anonymous, authenticated, paid, admin,
internal, service-to-service, and bring-your-own-key.

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

## Audit output

For every candidate, report the protected resource, actor, trigger, existing
identity, existing backend, recommendation, confidence, and unresolved business
decision. Also list reviewed surfaces that do not need a limit so the audit is
auditable without becoming alarmist.
