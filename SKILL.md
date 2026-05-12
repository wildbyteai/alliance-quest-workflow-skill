---
name: agenthansa-quest-copilot
version: 1.8.1
description: Alliance War quest workflow only: fetch the latest full quest detail from a brief, execute the quest step by step, verify 100% truth/logic/requirement compliance, and wait for user confirmation before final submission.
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

1. 获取: use the brief to fetch the latest full Alliance War quest details.
2. 分析: extract requirements, proof needs, risks, status, slots, deadline, and submission state.
3. 方案: produce the execution plan, evidence plan, and user action list.
4. 执行: complete every agent-capable step and pause only for user-owned actions.
5. 自检: verify 100% truthfulness, 100% logical consistency, and 100% requirement compliance.
6. 确认: prepare the official `SubmitAnswer` payload and wait for user confirmation.

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
状态：<FETCHING_QUEST_DETAIL | CHECKING_SUBMISSION_STATE | ANALYZING_REQUIREMENTS | PLANNING | EVIDENCE_AUDIT | CREATING_DELIVERABLE | DELIVERABLE_REVIEW | WAITING_FOR_USER_ACTION | COMPLIANCE_CHECK | WAITING_FOR_USER_CONFIRMATION | GRADE_HANDLING | BLOCKED>
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
- Do not output final submission material until truthfulness, logic, and every mandatory requirement are `PASS`.
- Do not continue a fresh quest when the latest status, deadline, or slots make a valid submission impossible.
- 必须循环自检：如果自检质量不够，修正方案、证据、交付物或 proof 后重新自检。

## Workflow

### 1. 获取: Fetch Latest Full Quest Detail

Start from the user's brief notification, but never execute from the brief alone. Use the best available source:

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
- Existing user submissions via `GET /api/alliance-war/quests/my` if the endpoint works.
- Relevant existing submissions via `GET /api/alliance-war/quests/{quest_id}/submissions`.

If `/quests/my` fails or is unavailable, ask the user to confirm whether they already submitted this quest. Do not assume there is no existing submission.

If the latest status is not submit-ready, the deadline has passed, or slots are unavailable, output `BLOCKED` unless the user is explicitly handling an existing submission or revision.

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

Draft only from kept claims.

Execute every step the agent can complete safely: research, outlining, writing, formatting, source checks, proof document planning, and payload assembly. Do not stop at analysis if the next step is agent-capable.

After drafting, output `DELIVERABLE_REVIEW` and wait for user review or missing proof.

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

Always check these self-check rows:

- 真实性：every factual claim is supported by quest text, official source, or user evidence.
- 逻辑性：content structure and reasoning are coherent and non-contradictory.
- 任务要求：every mandatory quest requirement is satisfied.
- `content` exists and is at least 20 characters.
- Required `proof_url` exists.
- `proof_url` is public or user-confirmed accessible.
- `challenge_answer` is handled if required.
- No duplicate fresh submission is being prepared.
- Latest quest status, deadline, and slots still allow submission.

If any row is `FAIL` or `UNKNOWN`, output `BLOCKED` and request only the missing item. Never claim 100% truth, 100% logic, or 100% compliance without evidence.

## 循环自检

If all missing items are agent-fixable, do not block. Run a self-check loop:

1. Identify every failed or weak row.
2. Return to the required phase: analysis, plan, evidence audit, draft, or proof plan.
3. Revise the work.
4. Re-run the complete self-check table.
5. Repeat the loop until every row is `PASS`.

Only stop the loop when every row is `PASS` or the remaining blocker requires user evidence/action.

### 5. 确认: Final Submission Material

Only output after user asks `准备提交`, `确认提交`, or equivalent and all checks are `PASS`.

```markdown
状态：WAITING_FOR_USER_CONFIRMATION
任务：<quest title>
阻塞：none
下一步：请审核最终提交材料；确认无误后再由你本人提交

## 最终提交材料
content:
<exact SubmitAnswer.content>

proof_url:
<proof URL, if required>

challenge_answer:
<only if required; otherwise omit>

checks:
- 真实性：PASS
- 逻辑性：PASS
- 任务要求：PASS
- 内容要求：PASS
- Proof：PASS
- 外部事实：PASS
- 最新状态/slots/deadline：PASS
- 重复提交风险：PASS

remaining_risks:
- none / list

## 确认门
请先审核 `content`、`proof_url`、checks 和 remaining_risks。
只有你确认后，才进入手动提交。

## 手动提交步骤（由你本人执行）
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

1.8.1
