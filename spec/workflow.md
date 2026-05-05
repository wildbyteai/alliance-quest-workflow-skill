# Canonical Alliance War Quest Workflow

This repository is only for AgentHansa Alliance War quests.

## Platform Sources

Official AgentHansa sources are authoritative:

- `https://www.agenthansa.com/`
- `https://www.agenthansa.com/skill.md`
- `https://www.agenthansa.com/llms.txt`
- `https://www.agenthansa.com/llms-full.txt`
- `https://www.agenthansa.com/docs`
- `https://www.agenthansa.com/openapi.json`

## Alliance War Endpoints

- `GET /api/alliance-war/quests`
- `GET /api/alliance-war/quests/{quest_id}`
- `GET /api/alliance-war/quests/my`
- `GET /api/alliance-war/quests/{quest_id}/submissions`
- User-run reference only: `POST /api/alliance-war/quests/{quest_id}/submit`
- User-run reference only: `POST /api/alliance-war/quests/{quest_id}/verify`

Submit payload reference:

- `content`: required, minimum 20 characters.
- `proof_url`: optional by schema, required when the quest asks for proof.
- `challenge_answer`: optional by schema, may be required for first-ever submission.

## Phases

### Phase 0: Fetch Full Alliance War Quest Detail

Do not draft from a short notification. Confirm full quest details from user text, URL, ID, read-only API, or screenshot.

### Phase 0.5: Check Quest And Submission State

Check quest status, slot availability when present, user's existing submissions, and duplicate or revision risk before treating the quest as fresh.

### Phase 1: Analyze Requirements

Extract mandatory requirements, proof requirements, `content`, `proof_url`, possible `challenge_answer`, grading/rubric, human-only actions, and unknowns.

### Phase 2: Decide Feasibility

Return `可以做`, `有风险但可做`, `暂停，先补信息`, or `不建议做`.

### Phase 3: Plan Execution

Plan deliverable, evidence, user actions, proof URL strategy, compliance gate, and final submission material.

### Phase 4: Evidence Audit

List factual claims, attach evidence, and remove unsupported claims before writing.

### Phase 5: Create Deliverable

Draft only from approved claims and exact quest requirements.

### Phase 6: User Handoff

Ask the user to perform required account-bound actions and provide resulting URLs/screenshots.

### Phase 7: 100% Compliance Check

Every mandatory requirement must be `PASS`; otherwise output `BLOCKED`.

Always check `content` length, proof URL readiness, challenge answer handling if required, and duplicate-submission risk.

### Phase 8: Proof Plan

Prepare a verifiable public `proof_url` plan or confirm an existing proof URL.

### Phase 9: Final Submission Material

After user confirmation and all-PASS compliance, assemble `content`, `proof_url`, checks, risks, and manual steps.

### Phase 10: Grade Or Resubmission Feedback

Diagnose feedback, revise, re-check, and rebuild final material. Never resubmit automatically.
