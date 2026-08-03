# Rate Limit This

Install the skill:

```bash
npx skills add riccardogiorato/rate-limit-this
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

The agent audits the app, understands its structure and existing
infrastructure, and discusses what should and should not be rate limited. It
then writes the agreed plan to `RATE_LIMITS.md` for your approval. No
application code changes in this step.

### 2. Implement the approved limits

After you approve the plan, the skill routes it to a fresh implementation
agent. The agent reuses your existing stack when possible, implements the
limits, verifies the blocked and recovery paths, and returns the changes for
review.

That is the whole workflow: talk first, implement second.

## License

MIT. Use it, adapt it, ship it.

## Authors

- **Riccardo:** [GitHub](https://github.com/riccardogiorato) · [X/Twitter](https://x.com/riccardogiorato)
- **Hassan:** [GitHub](https://github.com/nutlope) · [X/Twitter](https://x.com/nutlope)
