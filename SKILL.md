---
name: agenthansa-quest-copilot
description: AgentHansa Alliance War quest workflow only. Use only when explicitly selected by agenthansa-router-copilot or the user for an Alliance War quest, New Quest, USDC Pool quest, quest_id, or /api/alliance-war task. Fetch the latest full quest detail, analyze requirements and existing state, create the deliverable and proof plan step by step, run looped truth/logic/requirement self-check, and wait for user confirmation before any submission.
---

# AgentHansa Quest Copilot

## Scope

Only handle AgentHansa Alliance War quests.

Do not handle help/pro bono requests, Active Tasks, Reddit/X engagement tasks, generic social posts, or unrelated work. If the input is not clearly an Alliance War quest, stop with:

```markdown
Status: BLOCKED
Task: unknown
Blocker: not an AgentHansa Alliance War quest
Next: use agenthansa-router-copilot or provide the Alliance War quest URL, quest_id, full text, or screenshot
```

## Responsibility

Run one workflow:

1. Fetch the latest full Alliance War quest detail from the brief, URL, ID, or official read-only endpoint.
2. Check quest status, deadline, slots when available, and existing submission risk.
3. Extract every explicit deliverable, content, proof, platform, format, language, and quality requirement.
4. Create the execution plan, evidence plan, deliverable, and submission material for user review.
5. Pause for user-owned external actions, account actions, publication, screenshots, or proof URLs.
6. Run looped self-check until truthfulness, logic, proof, and every requirement are `PASS`, or block on user evidence/action.
7. Wait for user confirmation before any final submission.

The agent prepares work and payload material. The user performs final submission and verification.

## Official References

- `GET /api/alliance-war/quests`
- `GET /api/alliance-war/quests/{quest_id}`
- `GET /api/alliance-war/quests/my`
- `GET /api/alliance-war/quests/{quest_id}/submissions`
- User-confirmed final submission only: `POST /api/alliance-war/quests/{quest_id}/submit`
- User-confirmed final verification only: `POST /api/alliance-war/quests/{quest_id}/verify`
- OpenAPI: `https://www.agenthansa.com/openapi.json`

`SubmitAnswer` payload:

- `content`: required string, minimum 20 characters.
- `proof_url`: optional string or null; required when the quest asks for proof.
- `challenge_answer`: optional string or null; include only when AgentHansa asks for the first-submission challenge answer.

## Output Contract

Every major response starts with:

```markdown
Status: <FETCHING_QUEST_DETAIL | CHECKING_SUBMISSION_STATE | ANALYZING_REQUIREMENTS | PLANNING | EVIDENCE_AUDIT | CREATING_DELIVERABLE | DELIVERABLE_REVIEW | WAITING_FOR_USER_ACTION | COMPLIANCE_CHECK | WAITING_FOR_USER_CONFIRMATION | GRADE_HANDLING | BLOCKED>
Task: <quest title or unknown>
Blocker: <none or blocker>
Next: <next action>
```

Use the user's language for workflow guidance. Use the quest-required language for deliverables and the `content` field.

## Hard Rules

- Do not execute from a short notification alone.
- Do not continue until the latest full Alliance War quest detail is known.
- Do not process non-Alliance-War tasks.
- Do not call submit or verify endpoints unless the user explicitly confirms after all checks pass.
- Do not click final buttons or mutate external platforms.
- Do not create duplicate fresh submission material when an existing submission is detected.
- Do not invent facts, metrics, endorsements, rankings, partnerships, proof, URLs, screenshots, or results.
- Do not output final submission material until truthfulness, logic, proof readiness, and every mandatory requirement are `PASS`.
- Do not continue a fresh quest when the latest status, deadline, or slots make valid submission impossible.

## Workflow

### 1. Fetch Latest Full Quest Detail

Start from the user's brief, but never execute from the brief alone. Use the best available source:

1. User-provided full quest detail.
2. User-provided Alliance War URL or `quest_id`.
3. `GET /api/alliance-war/quests/{quest_id}`.
4. `GET /api/alliance-war/quests` and match by title, reward, deadline, sponsor/product, or ID.

Verify at least two stable fields before proceeding: title, quest ID, reward, deadline, sponsor/product, platform, or status.

If details are incomplete, ask only for the missing URL, `quest_id`, full text, or screenshot.

### 2. Check State And Requirements

Check state when read-only access is available:

- Quest status and deadline.
- Available slots if present.
- Existing user submissions via `GET /api/alliance-war/quests/my` if available.
- Relevant existing submissions via `GET /api/alliance-war/quests/{quest_id}/submissions`.

If submission state cannot be checked, ask the user whether they already submitted this quest. Do not assume no existing submission.

If the latest status is not submit-ready, the deadline has passed, or slots are unavailable, output `BLOCKED` unless the user is handling an existing submission, revision, or feedback.

Extract:

- Quest title and ID.
- Reward and deadline.
- Objective and deliverable type.
- Required platform, language, tone, length, links, tags, files, screenshots, or URLs.
- Required `content`, `proof_url`, and possible `challenge_answer`.
- Grading/rubric/spam rules.
- User-only actions and unknowns.

Decision:

- `READY`: full detail and requirements are clear.
- `RISKY_BUT_FEASIBLE`: possible, but status, proof, external platform, or wording risk exists.
- `NEEDS_USER_INFO`: mandatory detail or user-owned evidence is missing.
- `DO_NOT_DO`: expired, unavailable, unsafe, duplicate-risk, or not Alliance War.

### 3. Plan, Evidence Audit, And Draft

Produce a compact plan:

- Deliverable to create.
- Evidence required for each factual claim.
- User actions required before proof can pass.
- Proof URL strategy.
- Final payload fields.

Before drafting, run an evidence audit:

| Claim | Evidence | Decision |
|---|---|---|
| factual claim | quest text / official source / user evidence | keep/remove |

Draft only from supported claims. Execute every agent-capable step safely: research, outlining, writing, formatting, source checks, proof document planning, and payload assembly. Do not stop at analysis if the next step is agent-capable.

When user-owned action is required, output `WAITING_FOR_USER_ACTION`:

```markdown
## User action needed
- Action:
- Reason:
- Send back after completion:
```

### 4. Looped Self-Check

Before final submission material, run this table:

| Check | Evidence | Status |
|---|---|---|
| Fresh quest detail | latest full quest detail was used | PASS/FAIL/UNKNOWN |
| Truthfulness | every factual claim is supported by quest text, official source, or user evidence | PASS/FAIL/UNKNOWN |
| Logic | structure and reasoning are coherent and non-contradictory | PASS/FAIL/UNKNOWN |
| Explicit requirements | every mandatory quest requirement is satisfied | PASS/FAIL/UNKNOWN |
| Content field | `content` exists and is at least 20 characters | PASS/FAIL/UNKNOWN |
| Proof | required `proof_url` exists and is public or user-confirmed accessible | PASS/FAIL/UNKNOWN |
| Challenge answer | `challenge_answer` is handled only if required | PASS/FAIL/UNKNOWN |
| Duplicate risk | no duplicate fresh submission is being prepared | PASS/FAIL/UNKNOWN |
| Submission readiness | latest status, slots, and deadline still allow submission | PASS/FAIL/UNKNOWN |

If any row is `FAIL` or `UNKNOWN` and agent-fixable, revise the analysis, plan, evidence audit, deliverable, or proof plan, then re-run the complete table.

Repeat until every row is `PASS` or the remaining blocker requires user evidence/action.

### 5. Final Submission Material

Only output after all checks are `PASS`. Stop at `WAITING_FOR_USER_CONFIRMATION` unless the user explicitly confirms submission.

```markdown
Status: WAITING_FOR_USER_CONFIRMATION
Task: <quest title>
Blocker: none
Next: review the final submission material; submit only after user confirmation

## Final submission material
quest_id:
<quest_id>

content:
<exact SubmitAnswer.content>

proof_url:
<proof URL, if required; otherwise omit>

challenge_answer:
<only if required; otherwise omit>

checks:
- Fresh quest detail: PASS
- Truthfulness: PASS
- Logic: PASS
- Explicit requirements: PASS
- Content field: PASS
- Proof: PASS
- Challenge answer: PASS
- Duplicate risk: PASS
- Submission readiness: PASS

remaining_risks:
- none / list
```

## Feedback Handling

If the user provides `ai_grade`, `ai_summary`, review, spam, failed verification, or revision feedback:

1. Identify the failed requirement or proof item.
2. Revise the plan, deliverable, or proof.
3. Re-run the looped self-check.
4. Rebuild final submission material only when all mandatory rows pass.

Never resubmit automatically.
