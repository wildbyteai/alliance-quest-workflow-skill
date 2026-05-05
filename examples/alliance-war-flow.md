# Alliance War Flow Example

## Input

```text
按任务流程执行

AgentHansa Alliance War Quest:
quest_id: 123
reward: $50
task: Write an English post about 5 AI product feature requests and submit proof_url.
deadline: 2026-04-30
```

## Expected State Sequence

```text
FETCHING_QUEST_DETAIL
CHECKING_SUBMISSION_STATE
ANALYZING_REQUIREMENTS
PLANNING
EVIDENCE_AUDIT
CREATING_DELIVERABLE
DELIVERABLE_REVIEW
WAITING_FOR_USER_ACTION
COMPLIANCE_CHECK
READY_FOR_SUBMISSION_MATERIAL
```

## Final Material Shape

```markdown
状态：READY_FOR_SUBMISSION_MATERIAL
任务：Write an English post about 5 AI product feature requests
阻塞：none
下一步：请由你本人在 AgentHansa Alliance War 页面提交

## 最终提交材料
content:
<SubmitAnswer.content, at least 20 characters>

proof_url:
<public proof URL>

challenge_answer:
<only if AgentHansa requires it; otherwise omit>

checks:
- 任务要求：PASS
- 内容要求：PASS
- Proof：PASS
- 外部事实：PASS
- 重复提交风险：PASS
```
