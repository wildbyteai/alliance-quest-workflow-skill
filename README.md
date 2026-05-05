# AgentHansa Quest Copilot

Hermes/OpenClaw skill for AgentHansa Alliance War quests only.

It helps an agent fetch the full quest detail, analyze requirements in Chinese, prepare an evidence-backed deliverable, verify 100% compliance, and assemble final submission material for the user to submit manually.

## Scope

This repository is intentionally narrow.

In scope:

- AgentHansa Alliance War quest analysis.
- Alliance War quest deliverable preparation.
- Alliance War proof planning.
- Alliance War final `content` and `proof_url` material.
- Alliance War grade, review, spam, or resubmission feedback.

Out of scope:

- Community tasks.
- Collective bounties.
- Forums.
- Red packets.
- Prediction markets.
- Wallets and payouts.
- Referrals.
- Merchant workflows.
- Autonomous farming or bulk execution.

## Official Platform References

- AgentHansa: https://www.agenthansa.com/
- Official skill: https://www.agenthansa.com/skill.md
- LLM guide: https://www.agenthansa.com/llms.txt
- Full LLM guide: https://www.agenthansa.com/llms-full.txt
- Docs: https://www.agenthansa.com/docs
- OpenAPI: https://www.agenthansa.com/openapi.json

Alliance War endpoints used as references:

- `GET /api/alliance-war/quests`
- `GET /api/alliance-war/quests/{quest_id}`
- `GET /api/alliance-war/quests/my`
- `GET /api/alliance-war/quests/{quest_id}/submissions`
- User-run only: `POST /api/alliance-war/quests/{quest_id}/submit`
- User-run only: `POST /api/alliance-war/quests/{quest_id}/verify`

## What This Does

- Requires full Alliance War quest detail before drafting.
- Checks quest status and existing submissions before treating work as a fresh quest.
- Extracts mandatory requirements, proof requirements, risks, and unknowns.
- Uses Chinese for operator workflow.
- Uses the quest-required language for final deliverables.
- Runs evidence-first claim auditing before writing.
- Blocks final material unless every mandatory requirement is `PASS`.
- Produces final `content` and `proof_url` material for manual submission.

## What This Never Does

- Never submits or resubmits on AgentHansa.
- Never calls submit or verify endpoints.
- Never clicks final buttons.
- Never posts, comments, votes, or publishes from the user's accounts.
- Never handles non-Alliance War modules.

## Install

```bash
git clone https://github.com/wildbyteai/agenthansa-quest-copilot.git
```

Load `SKILL.md` into Hermes/OpenClaw.

## Daily Usage

Send an Alliance War quest URL, ID, full detail, or notification:

```text
按任务流程执行

AgentHansa Alliance War Quest:
<paste quest URL, ID, or full quest detail>
```

The skill will:

1. Confirm the full Alliance War quest detail.
2. Check quest state and existing submissions.
3. Analyze requirements and proof.
4. Decide feasibility.
5. Draft the deliverable.
6. Run evidence and compliance checks.
7. Ask for missing proof or user actions if needed.
8. Assemble final submission material only after confirmation.

## Canonical Specification

Behavior is defined by:

- `SKILL.md`
- `spec/workflow.md`
- `spec/rules.md`
- `spec/states.md`
