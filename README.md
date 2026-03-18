# Council v2 — Multi-Model Review System for OpenClaw

A hardened, multi-model council review skill for [OpenClaw](https://github.com/openclaw/openclaw). 

Spawns 3-5 independent AI reviewers across different providers, collects structured JSON verdicts, and applies **mechanical synthesis** — the vote count is the verdict, not the orchestrator's opinion.

## Features

- **Two tiers:** Standard (3 reviewers) and Full (5 reviewers)
- **5 providers, 5 training biases:** Anthropic, OpenAI, xAI, DeepSeek, Google
- **Mechanical synthesis:** Majority vote decides. Critical findings auto-block. Synthesizer cannot override.
- **Anti-consensus check:** Unanimous votes are flagged — agreement is hypothesis, not proof.
- **Minority reports preserved:** Strongest dissent is always surfaced.
- **Monthly retro:** Track past decisions against outcomes for continuous calibration.

## Install

```bash
clawhub install council-v2
```

Or manually:
```bash
git clone https://github.com/donzurbrick/council-v2.git ~/.openclaw/workspace/skills/council-v2
```

## Quick Start

```bash
# Code review
bash scripts/council.sh review code src/auth.py

# Plan review
bash scripts/council.sh review plan proposal.md

# Architecture review
bash scripts/council.sh review architecture design.md

# Decision review
bash scripts/council.sh review decision options.md
```

## Council Tiers

### Standard (3 reviewers)
| Role | Default Model | Provider |
|------|--------------|----------|
| Architecture Synthesizer | Opus | Anthropic |
| Adversarial Critic | GPT-5.4 | OpenAI |
| Security & Risk | Grok 4 | xAI |

### Full (5 reviewers)
| Role | Default Model | Provider |
|------|--------------|----------|
| Architecture Synthesizer | Opus | Anthropic |
| Adversarial Critic | GPT-5.4 | OpenAI |
| Security & Risk | Grok 4 | xAI |
| First Principles | DeepSeek R1 | DeepSeek |
| Structural Verifier | Gemini 3.1 Pro | Google |

## Synthesis Rules (v2 Protocol)

1. **Majority vote decides** — synthesizer narrates but cannot override the count
2. **Critical finding = auto-block** — any critical severity blocks approval
3. **Splits default conservative** — ties go to reject/modify
4. **Anti-consensus check** — unanimous votes require strongest counterargument
5. **Raw outputs to operator** — Full Council shows raw verdicts
6. **Synthesizer doesn't vote on Full** — prevents dual-role bias
7. **Minority reports preserved** — dissent is always surfaced
8. **"Reasons I disagree" required** — if synthesizer differs from majority

## Origin

Built by [Don Zurbrick](https://github.com/zurbrick) — battle-tested across plugin evaluations, security audits, architecture reviews, and one memorable session where the council reviewed itself and called out its own biases.

## License

MIT
