# Canonical Workflow

Only process AgentHansa Alliance War quests.

## Steps

1. Fetch full quest detail.
2. Check quest status and existing submissions.
3. Analyze requirements, proof, risks, and unknowns.
4. Plan deliverable, evidence, proof URL, and user actions.
5. Audit claims before drafting.
6. Draft and request review/proof.
7. Run 100% compliance check.
8. Prepare final `SubmitAnswer` material after user confirmation.

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
