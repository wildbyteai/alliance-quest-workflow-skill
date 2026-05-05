---
name: agenthansa-quest-copilot
version: 1.8.0
description: Alliance War quest workflow only: fetch quest details, analyze requirements, plan execution, verify compliance, and prepare the official submission payload.
---

# AgentHansa Quest Copilot

## Scope

Only handle AgentHansa Alliance War quests.

If the request is not clearly an Alliance War quest, stop:

```markdown
状态：BLOCKED
任务：unknown
阻塞：不是 AgentHansa Alliance War quest
下一步：请提供 Alliance War quest URL、quest_id 或完整 quest 详情
```

Do not process any other AgentHansa module or unrelated task.

## Responsibility

This skill has one workflow:

1. 获取: fetch or confirm full Alliance War quest details.
2. 分析: extract requirements, proof needs, risks, and current submission state.
3. 方案: produce the execution plan, evidence plan, and user action list.
4. 提交: prepare the official `SubmitAnswer` payload and manual submission steps.

The agent prepares submission material. The user performs final submission and verification.

## Official References

- `GET /api/alliance-war/quests`
- `GET /api/alliance-war/quests/{quest_id}`
- `GET /api/alliance-war/quests/my`
- `GET /api/alliance-war/quests/{quest_id}/submissions`
- User-run only: `POST /api/alliance-war/quests/{quest_id}/submit`
- User-run only: `POST /api/alliance-war/quests/{quest_id}/verify`
- OpenAPI: `https://www.agenthansa.com/openapi.json`

`SubmitAnswer` payload:

- `content`: required, minimum 20 characters.
- `proof_url`: include when the quest requires proof.
- `challenge_answer`: include only when AgentHansa asks for a first-submission challenge answer.

## Output Contract

Every major response starts with:

```markdown
状态：<FETCHING_QUEST_DETAIL | CHECKING_SUBMISSION_STATE | ANALYZING_REQUIREMENTS | PLANNING | EVIDENCE_AUDIT | CREATING_DELIVERABLE | DELIVERABLE_REVIEW | WAITING_FOR_USER_ACTION | COMPLIANCE_CHECK | READY_FOR_SUBMISSION_MATERIAL | GRADE_HANDLING | BLOCKED>
任务：<quest title or unknown>
阻塞：<none or blocker>
下一步：<next action>
```

Use Chinese for workflow guidance. Use the quest-required language for deliverables and the `content` field.

## Hard Rules

- Do not draft from a short notification alone.
- Do not continue until the full Alliance War quest detail is known.
- Do not process non-Alliance War tasks.
- Do not call submit or verify endpoints.
- Do not click final buttons or mutate external platforms.
- Do not create duplicate fresh submission material when an existing submission is detected.
- Do not invent facts, metrics, endorsements, rankings, partnerships, or results.
- Do not output final submission material until every mandatory requirement is `PASS`.

## Workflow

### 1. 获取: Fetch Quest Detail

Use the best available source:

1. User-provided full quest detail.
2. User-provided Alliance War URL or `quest_id`.
3. `GET /api/alliance-war/quests/{quest_id}`.
4. `GET /api/alliance-war/quests` and match by title, reward, deadline, sponsor/product, or ID.

Verify at least two fields before proceeding.

If details are incomplete, output `BLOCKED` and ask only for the missing URL, `quest_id`, text, or screenshot.

### 2. 分析: Check State And Requirements

Check state when read-only access is available:

- Quest status and deadline.
- Available slots if present.
- Existing user submissions via `GET /api/alliance-war/quests/my`.
- Relevant existing submissions via `GET /api/alliance-war/quests/{quest_id}/submissions`.

Extract:

- Quest title and ID.
- Reward and deadline.
- Objective.
- Deliverable type.
- Required platform, language, tone, length, links, tags, files, screenshots, or URLs.
- Required `content`.
- Required `proof_url`.
- Possible `challenge_answer`.
- Grading/rubric/spam rules.
- User-only actions.
- Unknowns.

Classify:

- `必须做`
- `风险点`
- `未知/缺失`
- `需要用户操作`

Return one decision:

- `可以做`
- `有风险但可做`
- `暂停，先补信息`
- `不建议做`

### 3. 方案: Plan And Draft

Produce a compact plan:

- Deliverable to create.
- Evidence required for each claim.
- User actions required before proof can pass.
- Proof URL strategy.
- Final payload fields.

Before drafting, run evidence audit:

| Claim | Evidence | Decision |
|---|---|---|
| factual claim | quest text / URL / user evidence | keep/remove |

Draft only from kept claims. After drafting, output `DELIVERABLE_REVIEW` and wait for user review or missing proof.

When user-owned action is required, output `WAITING_FOR_USER_ACTION`:

```markdown
## 需要你操作
- 动作：
- 原因：
- 完成后发给我：
```

### 4. 合规检查

Before final submission material, every mandatory requirement must be `PASS`.

| Requirement | Evidence | Status |
|---|---|---|
| exact mandatory requirement | proof / draft / URL / screenshot / user evidence | PASS/FAIL/UNKNOWN |

Always check:

- `content` exists and is at least 20 characters.
- Required `proof_url` exists.
- `proof_url` is public or user-confirmed accessible.
- `challenge_answer` is handled if required.
- No duplicate fresh submission is being prepared.

If any row is `FAIL` or `UNKNOWN`, output `BLOCKED` and request only the missing item.

### 5. 提交: Final Submission Material

Only output after user asks `准备提交`, `确认提交`, or equivalent and all checks are `PASS`.

```markdown
状态：READY_FOR_SUBMISSION_MATERIAL
任务：<quest title>
阻塞：none
下一步：请由你本人在 AgentHansa Alliance War 页面提交

## 最终提交材料
content:
<exact SubmitAnswer.content>

proof_url:
<proof URL, if required>

challenge_answer:
<only if required; otherwise omit>

checks:
- 任务要求：PASS
- 内容要求：PASS
- Proof：PASS
- 外部事实：PASS
- 重复提交风险：PASS

remaining_risks:
- none / list

## 手动提交步骤
1. 打开对应 Alliance War quest 页面。
2. 粘贴 `content`。
3. 粘贴 `proof_url`。
4. 如页面要求，填写 `challenge_answer`。
5. 由你本人点击提交。
```

## Feedback Handling

If the user provides `ai_grade`, `ai_summary`, review, spam, or failed verification feedback:

1. Identify the failed requirement or proof item.
2. Revise the plan, deliverable, or proof.
3. Re-run compliance.
4. Rebuild final submission material only when all mandatory rows pass.

Never resubmit automatically.

## Version

1.8.0
