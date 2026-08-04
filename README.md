# Rate Limit This

Install the skill:

```bash
npx skills add nutlope/rate-limit-this
```

![Rate Limit This cover](assets/cover.png)

Works with Codex, Claude Code, OpenCode, and any coding harness that supports
Agent Skills. It helps you decide where rate limits belong, then implements the
limits you approve.

## How it works

### 1. Invoke the skill

```text
# Codex
$rate-limit-this audit this app

# Claude Code
/rate-limit-this audit this app

# OpenCode and other coding harnesses
Use the rate-limit-this skill to audit this app.
```

The agent will:

- Look through routes, authentication, costly APIs, uploads, public writes,
  background jobs, and existing protections.
- Explain what should and should not be rate limited, and why.
- Ask one plain-language question at a time when the code cannot determine a
  product policy.
- Write the agreed plan to `RATE_LIMITS.md` for your approval.

No application code changes in this step.

### 2. Implement and review

After you approve the plan, the skill routes it to a fresh implementation
agent. It reuses your existing stack when possible and helps choose one suitable
option:

- Upstash or Redis
- An existing database, including Postgres, Neon, Supabase, or PlanetScale
- Convex
- Platform-native edge or WAF protections

The agent implements only the approved limits, verifies allowed, blocked,
recovery, and backend-outage behavior, then returns the changes and evidence
for your review.

That is the whole workflow: talk first, implement second.

## License

MIT. Use it, adapt it, ship it.

## Authors

- **Riccardo:** [GitHub](https://github.com/riccardogiorato) · [X/Twitter](https://x.com/riccardogiorato)
- **Hassan:** [GitHub](https://github.com/nutlope) · [X/Twitter](https://x.com/nutlope)
