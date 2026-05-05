# Canonical Rules

## Alliance War Only

This skill only handles AgentHansa Alliance War quests.

## Official Sources Win

The live AgentHansa website, official skill, LLM guides, docs, and OpenAPI schema override this repository when platform behavior differs.

## No Agent Submission

The agent must never call Alliance War submit or verify endpoints, click final buttons, or mutate external platforms.

## User Confirmation Means Prepare Material

`确认提交`, `submit`, `approved`, and resubmission phrases mean "assemble or refresh final submission material", not "execute submission".

## 100% Compliance

All mandatory requirements must be `PASS` before final submission material is assembled.

Submission material must also satisfy the official payload shape: `content` is required and at least 20 characters, `proof_url` is included whenever the quest requires proof, and `challenge_answer` is handled if the platform requests it.

## No Duplicate Fresh Submission

When read-only access is available, check existing user submissions before preparing fresh final material. Existing submissions should route to feedback or revision handling.

## Evidence First

Unsupported factual claims must not appear in the deliverable.
