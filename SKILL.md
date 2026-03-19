---
name: council-v2
description: >
  Publishable multi-model council review system for code review, plan review,
  architecture review, decision review, structured critique, adversarial review,
  second opinion, pre-flight review, final review before merge, QA council,
  sanity check with multiple models, and review-before-shipping workflows.
version: 2.0.0
---

# Council v2

A hardened OpenClaw skill for multi-model council reviews.
It dispatches independent reviewers, collects structured JSON, and applies a
**mechanical synthesis protocol** so the final verdict is driven by votes and
critical findings — not orchestrator vibes.

Primary entrypoint: `bash skills/council-v2/scripts/council.sh review <type> [file]`

## When to Use

Use when a single model reviewing its own work is not enough:
- Code review before merge or deployment
- Plan review before committing resources
- Architecture review for important technical decisions
- Decision review when multiple plausible options exist
- Security-sensitive or irreversible choices
- Pre-flight review, adversarial critique, or second-opinion work

## When Not to Use

Do **not** use for:
- One-line fixes or trivial edits
- Low-stakes decisions where overhead exceeds risk
- Purely factual lookups with no judgment call
- Work already reviewed recently with no material change

## Council Shape

Two tiers are supported:
- **Standard** — 3 reviewers for routine code, plan, and decision reviews
- **Full** — 5 reviewers for high-stakes, security-sensitive, or irreversible choices

Detailed role composition, mandatory Full Council triggers, and synthesis rules live in:
- `references/review-types.md`
- `references/synthesis-rules.md`
- workspace policy in `AGENTS.md`

Use this skill for the runtime workflow. Use the references and workspace policy for the full mechanics.

## Review Types

| Type | Typical use |
|------|-------------|
| `code` | Source files, scripts, patches, PR diffs |
| `plan` | Proposals, project plans, rollout plans |
| `architecture` | Systems design, infra decisions, workflows |
| `decision` | A/B/C choices with tradeoffs |

Definitions: `references/review-types.md`

## Quick Start

```bash
# Standard code review
bash skills/council-v2/scripts/council.sh review code src/auth.py

# Force full plan review
bash skills/council-v2/scripts/council.sh review plan proposal.md --tier full

# Architecture review from stdin
cat design.md | bash skills/council-v2/scripts/council.sh review architecture --tier full

# Decision review with options
bash skills/council-v2/scripts/council.sh review decision options.md --options "SQLite,Postgres,Cloud SQL"

# Emit orchestration plan as JSON
bash skills/council-v2/scripts/council.sh review code src/auth.py --format json
```

## How It Works

1. Loads content from file or stdin
2. Selects Standard or Full tier
3. Builds reviewer prompts from `references/role-prompts.md`
4. Emits an orchestration plan suitable for `sessions_spawn`
5. Collects reviewer JSON outputs
6. Runs `python3 scripts/synthesize.py ...`
7. Optionally runs a monthly retrospective via `scripts/retro.sh`

## Notes

- Requires `bash`, `python3`, and OpenClaw reviewer dispatch capability
- Model aliases can be overridden in `skills/council-v2/scripts/council.sh`
- Retro workflow details live in `scripts/retro.sh`

## References

- `references/review-types.md`
- `references/role-prompts.md`
- `references/schema.md`
- `references/synthesis-rules.md`
