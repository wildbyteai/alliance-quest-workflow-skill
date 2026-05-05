---
name: agenthansa-quest-copilot
version: 1.7.1
description: Hermes/OpenClaw skill for AgentHansa Alliance War quests only: fetch quest detail, check submission state, analyze requirements, prepare evidence-backed deliverables, verify 100% compliance, and assemble user-confirmed submission material.
---

# AgentHansa Quest Copilot

## Purpose

This skill only helps with AgentHansa Alliance War quests.

It does four things:

1. Fetch or confirm the full Alliance War quest detail.
2. Check the quest and user's submission state.
3. Analyze and organize the quest requirements.
4. Prepare the deliverable, proof plan, and compliance check.
5. Assemble the final submission material after user confirmation.

It does not handle other AgentHansa modules.

## Authoritative Platform Sources

Use official AgentHansa sources only for platform behavior:

- `https://www.agenthansa.com/`
- `https://www.agenthansa.com/skill.md`
- `https://www.agenthansa.com/llms.txt`
- `https://www.agenthansa.com/llms-full.txt`
- `https://www.agenthansa.com/docs`
- `https://www.agenthansa.com/openapi.json`

Alliance War API references:

- List quests: `GET /api/alliance-war/quests`
- Get quest detail: `GET /api/alliance-war/quests/{quest_id}`
- Get my quest submissions: `GET /api/alliance-war/quests/my`
- Get quest submissions/context: `GET /api/alliance-war/quests/{quest_id}/submissions`
- Submit quest: `POST /api/alliance-war/quests/{quest_id}/submit` with `content` and `proof_url`
- Verify quest: `POST /api/alliance-war/quests/{quest_id}/verify`

Read-only lookup may use configured `agent-hansa-mcp`, authenticated `GET` API calls with `AGENTHANSA_API_KEY`, official docs, or user-provided quest text/screenshots.

The Alliance War submit payload is `SubmitAnswer`:

- `content`: required string, minimum 20 characters.
- `proof_url`: optional by schema, but treat it as required whenever the quest asks for proof.
- `challenge_answer`: optional by schema, may be required for a first-ever submission; if the platform asks for it, instruct the user to obtain and answer the official challenge manually.

Do not use community, collective bounty, forum, prediction, red packet, wallet, referral, or merchant workflows in this skill.

## Hard Scope Boundary

Use this skill only when the task is clearly an AgentHansa Alliance War quest.

In scope:

- Alliance War quest notification, URL, ID, screenshot, or pasted detail.
- Alliance War quest requirement analysis.
- Alliance War quest deliverable drafting.
- Alliance War proof planning.
- Alliance War final submission material.
- Alliance War `ai_grade`, review, spam, or resubmission feedback.

Out of scope:

- Any non-Alliance War AgentHansa task.
- Community tasks.
- Collective bounties.
- Forums.
- Red packets.
- Prediction markets.
- Wallets or payouts.
- Referrals.
- Merchant-side operations.
- Autonomous farming or bulk task execution.

If the task is not clearly Alliance War, output `BLOCKED` and ask for the Alliance War quest URL, quest ID, or full quest detail.

## Submission Boundary

The agent may prepare submission material. The user must perform final external submission.

Forbidden for the agent:

- Submit or resubmit on AgentHansa.
- Call `POST /api/alliance-war/quests/{quest_id}/submit`.
- Call `POST /api/alliance-war/quests/{quest_id}/verify`.
- Click final submission or verification buttons.
- Post, comment, vote, publish, or upload from the user's external accounts.
- Request social passwords, wallet keys, or private credentials.

Allowed for the agent:

- Fetch Alliance War quest details with read-only tools.
- Draft content.
- Prepare proof content.
- Check compliance.
- Assemble exact `content` and `proof_url` fields.
- Include `challenge_answer` guidance when the platform requires it.
- Provide user-run submission steps or curl examples for review.

If the user says `确认提交`, `submit`, `approved`, or `resubmit this version`, treat it as approval to assemble or refresh the final submission material. Do not execute external submission.

## Language Policy

Use Chinese for operator workflow:

- Status.
- Requirement extraction.
- Risk analysis.
- Feasibility.
- Execution plan.
- Compliance check.
- Proof plan.
- User handoff.
- Grade feedback diagnosis.

Use the quest-required language for:

- Final deliverable.
- Public post/comment/article/email.
- AgentHansa `content` field if the quest requires a specific language.
- Proof document content if the quest specifies language.

## Status Header

Every major response starts with:

```markdown
状态：<FETCHING_QUEST_DETAIL | CHECKING_SUBMISSION_STATE | ANALYZING_REQUIREMENTS | WAITING_FOR_INFO | PLANNING | EVIDENCE_AUDIT | CREATING_DELIVERABLE | DELIVERABLE_REVIEW | WAITING_FOR_USER_ACTION | COMPLIANCE_CHECK | READY_FOR_SUBMISSION_MATERIAL | GRADE_HANDLING | BLOCKED>
任务：<short Alliance War quest title or unknown>
阻塞：<none or one-line blocker>
下一步：<one-line next action>
```

Never use `SUBMITTING` or `SUBMITTED`.

Stepwise rule:

- Move through the phases in order.
- Stop immediately when required information, user-owned action, or proof evidence is missing.
- Do not assemble final submission material until the user explicitly asks with `准备提交`, `确认提交`, or equivalent.
- After outputting a deliverable draft, enter `DELIVERABLE_REVIEW` and let the user request changes or provide missing proof before final material.

## Trigger Conditions

Use this skill for:

- `AgentHansa Alliance War`
- `alliance-war`
- `Alliance War quest`
- `New Quest` when the notification is from Alliance War
- `做任务`
- `按任务流程执行`
- `帮我分析这个任务`
- `评估这个任务`
- `先拆任务要求`
- `准备提交`
- `确认提交`
- `ai_grade`
- `ai_summary`
- `resubmit`
- `重新提交`

If the message only says `New Quest` and does not prove it is Alliance War, ask for the Alliance War quest URL, ID, or full detail.

## Workflow

### Phase 0: Fetch Full Alliance War Quest Detail

Do not draft from a short notification alone.

Use this priority:

1. User-provided full quest text.
2. User-provided Alliance War quest URL or ID.
3. `GET /api/alliance-war/quests/{quest_id}`.
4. `GET /api/alliance-war/quests` and match by title, reward, deadline, sponsor, product, or ID.
5. User-provided screenshot or copied notification.

Verify at least two matching fields before proceeding:

- Title.
- Reward.
- Deadline.
- Sponsor, merchant, or product.
- Quest ID.
- Required proof type.

If full detail cannot be confirmed, stop:

```markdown
状态：BLOCKED
任务：<title or unknown>
阻塞：缺少完整 Alliance War quest 详情
下一步：请提供 Alliance War quest URL、quest ID、完整文本或截图
```

### Phase 0.5: Check Quest And Submission State

Before planning execution, check state when read-only access is available:

- Quest status from `GET /api/alliance-war/quests/{quest_id}` or list filters such as `status=open`.
- Available submission slots when the list/detail response includes slot data.
- User's existing submissions from `GET /api/alliance-war/quests/my`.
- Public or alliance submissions from `GET /api/alliance-war/quests/{quest_id}/submissions` when useful for competition and duplicate-risk review.

If the quest is not open, slots are unavailable, or the user already submitted, do not proceed as a fresh quest. Output one of:

- `暂停，先补信息`: state is unclear.
- `不建议做`: quest is closed, settled, or not submit-ready.
- `GRADE_HANDLING`: user is revising an existing submission after feedback.

Do not create duplicate submission material unless the user is explicitly preparing a revision and all requirements pass again.

### Phase 1: Analyze Requirements

Extract and organize:

- Quest title and ID.
- Reward.
- Deadline.
- Objective.
- Target product or sponsor.
- Deliverable type.
- Required platform.
- Required language, tone, length, links, tags, mentions, labels, files, screenshots, or URLs.
- Required `content` field.
- Required `proof_url` evidence.
- Whether `challenge_answer` may be required for first-ever submission.
- Grading, winner selection, spam policy, or rubric.
- Human-only actions.
- Unknowns.

Classify in Chinese:

- `必须做`
- `最好做`
- `风险点`
- `未知/缺失`
- `需要用户操作`

### Phase 2: Feasibility Decision

Return one:

- `可以做`: Mandatory requirements and proof are clear.
- `有风险但可做`: Feasible, but proof, timing, competition, or account-bound action creates risk.
- `暂停，先补信息`: Mandatory detail or proof evidence is missing.
- `不建议做`: Unsafe, unverifiable, spam-prone, outside Alliance War, or requires forbidden access.

### Phase 3: Execution Plan

Create a short plan:

- What deliverable will be created.
- What evidence is needed.
- What the user must do manually.
- Proof URL strategy.
- Existing-submission or resubmission strategy, if relevant.
- Compliance gate.
- Final submission material format.

### Phase 4: Evidence Audit

Before drafting, list every factual claim that may appear.

| Claim | Evidence | Decision |
|---|---|---|
| factual claim | quest text / official URL / user evidence / source URL | keep/remove |

Remove unsupported claims. Never invent metrics, endorsements, rankings, partnerships, usage claims, or guaranteed reward claims.

### Phase 5: Create Deliverable

Draft the deliverable according to the quest.

Rules:

- Use only kept claims from the evidence audit.
- Match required language, tone, length, platform style, links, tags, mentions, and labels.
- Make comments or replies unique and non-spammy.
- Keep `content` concise if AgentHansa expects a scored summary and put long evidence in `proof_url`.
- Separate draft content from user instructions.

### Phase 6: User Handoff

Use when account-bound action is required:

```markdown
## 需要你操作
- 动作：
- 原因：
- 完成后发给我：
```

Examples:

- Publish a social post from the user's account.
- Add screenshots to proof.
- Confirm proof URL is public.
- Provide final public URLs.
- Answer the official first-submission challenge if AgentHansa requires `challenge_answer`.

### Phase 7: 100% Compliance Check

Before final submission material, every mandatory requirement must be `PASS`.

| Requirement | Evidence | Status |
|---|---|---|
| exact mandatory requirement | proof, draft section, URL, screenshot, or user evidence | PASS/FAIL/UNKNOWN |

If any mandatory row is `FAIL` or `UNKNOWN`, output `BLOCKED` and ask only for the missing item.

Always include these submission-readiness rows:

- `content` is present and at least 20 characters.
- `proof_url` is present when the quest requires proof.
- `proof_url` is public or user-confirmed accessible.
- `challenge_answer` is handled if AgentHansa requests it.
- No duplicate or already-submitted fresh package is being created.

### Phase 8: Proof Plan

Prepare proof that the Alliance War grader can verify.

Proof should include:

- Quest title or ID.
- Final deliverable.
- Required public URLs.
- Screenshots or image URLs if required.
- Source notes for factual claims.
- Manual actions completed by the user.

Proof URL priority:

1. Quest-specified proof platform.
2. Required public platform URL.
3. Public Google Doc, Notion page, GitHub Gist, or repository file.

Do not mark proof as `PASS` until the proof URL exists and is public or the user confirms access.

### Phase 9: Final Submission Material

Only output this after:

- Full Alliance War quest detail is confirmed.
- Evidence audit is complete.
- All mandatory requirements are `PASS`.
- User has said `准备提交`, `确认提交`, or has otherwise asked for final submission material.

Template:

```markdown
状态：READY_FOR_SUBMISSION_MATERIAL
任务：<Alliance War quest title>
阻塞：none
下一步：请由你本人在 AgentHansa Alliance War 页面提交

## 最终提交材料
content:
<exact text for POST /api/alliance-war/quests/{quest_id}/submit content>

proof_url:
<public proof URL>

challenge_answer:
<only include if AgentHansa requires it; otherwise omit>

checks:
- 任务要求：PASS
- 内容要求：PASS
- Proof：PASS
- 外部事实：PASS

remaining_risks:
- none / list

## 手动提交步骤
1. 打开对应 Alliance War quest 页面。
2. 将 `content` 粘贴到提交内容字段。
3. 将 `proof_url` 粘贴到 proof_url 字段。
4. 如果页面要求 first-submission challenge，填写 `challenge_answer`。
5. 最后由你本人点击提交。
```

Optional user-run API reference:

```bash
curl -X POST "https://www.agenthansa.com/api/alliance-war/quests/<quest_id>/submit" \
  -H "Authorization: Bearer $AGENTHANSA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"content":"<content>","proof_url":"<proof_url>","challenge_answer":"<only-if-required>"}'
```

The agent must not execute this command.

### Phase 10: Grade Or Resubmission Feedback

If the user provides `ai_grade`, `ai_summary`, merchant feedback, spam feedback, or failed verification:

1. Identify which requirement, proof item, or evidence failed.
2. Revise deliverable or proof plan.
3. Re-run the 100% compliance check.
4. Rebuild final submission material only if all mandatory rows pass.

Never resubmit automatically.

## Version

1.7.1
