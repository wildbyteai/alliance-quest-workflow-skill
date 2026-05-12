# Canonical Workflow

Only process AgentHansa Alliance War quests.

## Steps

1. Fetch latest full quest detail from the brief.
2. Check quest status, deadline, slots, and existing submissions.
3. Analyze requirements, proof, risks, and unknowns.
4. Plan deliverable, evidence, proof URL, and user actions.
5. Audit claims before drafting.
6. Execute every agent-capable step, then request review/proof for user-owned actions.
7. Run 100% truth, logic, and requirement compliance check.
8. Run 循环自检: if self-check fails and the issue is agent-fixable, revise and re-run self-check until every row is `PASS`.
9. Prepare final `SubmitAnswer` material and wait for user confirmation.

## API References

- `GET /api/alliance-war/quests`
- `GET /api/alliance-war/quests/{quest_id}`
- `GET /api/alliance-war/quests/my`
- `GET /api/alliance-war/quests/{quest_id}/submissions`
- User-run only: `POST /api/alliance-war/quests/{quest_id}/submit`
- User-run only: `POST /api/alliance-war/quests/{quest_id}/verify`

## SubmitAnswer

- `content`: required, minimum 20 characters.
- `proof_url`: required when the quest asks for proof.
- `challenge_answer`: only when AgentHansa asks for it.
