# Canonical Rules

- Alliance War quests only.
- Non-Alliance War requests return `BLOCKED`.
- Full quest detail is required before drafting.
- Latest status, deadline, slots, and existing submissions must be checked when possible.
- If `/quests/my` is unavailable, ask the user whether they already submitted.
- Claims require evidence.
- Final material requires truthfulness, logic, and all mandatory requirement rows to be `PASS`.
- Failed self-check rows must trigger 循环自检: revise and re-run self-check until every row is `PASS`, unless user evidence/action is required.
- Final material waits for user confirmation.
- The agent prepares `SubmitAnswer` material only; the user submits.
- The agent never calls submit or verify endpoints.
