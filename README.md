# Rate Limit This

A human-first rate-limit design skill for Codex and Claude Code, built for
JavaScript and TypeScript applications—especially Next.js.

It audits the real abuse and cost surfaces, asks only the policy questions that
code cannot answer, and writes one reviewable `RATE_LIMITS.md` before any
application code changes.

---

## Two review gates

| Stage | What the skill does | Where it stops |
| --- | --- | --- |
| Audit | Finds public writes, costly calls, auth flows, uploads, jobs, and existing protections. | Asks one material question at a time, with a recommended default. |
| Specification | Records every limiter's identity, algorithm, allowance, exemptions, backend, outage policy, blocked-user recovery, observability, and verification. | `status: needs-spec-review` |
| Implementation | After explicit approval, routes the contract to a fresh implementation agent and uses one backend suited to the existing stack. | `status: needs-implementation-review` |

Only explicit human approval advances either gate. A changed contract invalidates
stale approval.

---

## Stack-aware by default

The skill prefers the infrastructure the application already operates:

- Upstash or Redis
- Convex
- Prisma, Neon, or Postgres
- Supabase
- platform-native edge or WAF protections when they can enforce the approved
  policy

Browser fingerprinting is advanced opt-in only. The skill does not require
Python, a new test framework, or a second rate-limit backend for straightforward
projects.

---

## Install

```bash
npx skills add riccardogiorato/rate-limit-this
```

Re-run the command to update. Or copy `SKILL.md`, `references/`, and
`agents/openai.yaml` into:

- Claude Code: `~/.claude/skills/rate-limit-this/`
- Codex: `~/.agents/skills/rate-limit-this/`

Invoke it explicitly:

```text
# Codex
$rate-limit-this audit this app and draft RATE_LIMITS.md for my review

# Claude Code
/rate-limit-this audit this app and draft RATE_LIMITS.md for my review
```

---

## Output

The skill creates one root-level `RATE_LIMITS.md` as the durable contract. Its
normal lifecycle is:

```text
needs-spec-review → approved → needs-implementation-review → complete
```

The rule-set lives in [`SKILL.md`](SKILL.md). Detection, provider routing, and
the document contract live in [`references/`](references/).

---

## License

MIT. Use it, adapt it, ship it.
