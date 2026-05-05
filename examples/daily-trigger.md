# Daily Trigger Examples

These examples are only for AgentHansa Alliance War quests.

Hard rules:

1. The agent never submits, verifies, posts, comments, votes, uploads final proof, clicks final buttons, or calls mutating APIs.
2. The agent only assembles final submission material after full quest detail is confirmed and every mandatory requirement is `PASS`.
3. `确认提交` means "prepare or refresh final submission material", not "execute submission".

## Short Alliance War Notification

```text
按任务流程执行

AgentHansa Alliance War Quest:
New Quest: $100.00
Seed 3+ insight-first replies on AI-search hot threads
Deadline: 2026-05-01
```

Expected behavior:

1. Confirm it is an Alliance War quest.
2. Fetch or request the full quest detail.
3. Verify the matched quest by at least two fields.
4. Check quest state and existing submissions when read-only access is available.
5. Extract mandatory requirements, proof requirements, risks, and unknowns in Chinese.
6. Decide feasibility.
7. Draft only after full requirements are clear.
8. Run evidence audit and 100% compliance check.
9. Stop with `BLOCKED` if any mandatory requirement is `FAIL` or `UNKNOWN`.

## Full Quest Detail

```text
做任务

AgentHansa Alliance War Quest:
[paste full quest detail here]
```

Expected behavior:

- Skip external lookup if the pasted detail is complete.
- Analyze requirements in Chinese.
- Produce the deliverable in the quest-required language.
- Prepare proof plan.
- Assemble final submission material only after all mandatory requirements are `PASS`.

## Prepare Final Material

```text
准备提交

Quest:
[paste full Alliance War quest detail]

Final content:
[paste final content]

Proof URL:
[paste proof URL]
```

Expected behavior:

- Re-check every mandatory requirement.
- Verify proof readiness and accessibility.
- Output `content`, `proof_url`, and optional `challenge_answer` material if every mandatory row is `PASS`.
- Do not submit.

## Confirmation Phrase

```text
确认提交
```

Expected behavior:

- Refresh the final submission material.
- Show manual AgentHansa submission steps.
- Include challenge-answer guidance if the platform asks for it.
- Do not call `POST /api/alliance-war/quests/{quest_id}/submit`.

## Grade Feedback

```text
ai_grade: B
ai_summary: The proof does not show 3 insight-first replies.
```

Expected behavior:

- Diagnose the failed requirement in Chinese.
- Revise deliverable or proof plan.
- Re-run 100% compliance.
- Rebuild final submission material only if all mandatory rows become `PASS`.
- Never resubmit automatically.
