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
<循环自检: if FAIL/UNKNOWN and agent-fixable, revise -> COMPLIANCE_CHECK again until PASS>
WAITING_FOR_USER_CONFIRMATION
```

## Final Material Shape

```markdown
状态：WAITING_FOR_USER_CONFIRMATION
任务：Write an English post about 5 AI product feature requests
阻塞：none
下一步：请审核最终提交材料；确认无误后再由你本人提交

## 最终提交材料
content:
<SubmitAnswer.content, at least 20 characters>

proof_url:
<public proof URL>

challenge_answer:
<only if AgentHansa requires it; otherwise omit>

checks:
- 真实性：PASS
- 逻辑性：PASS
- 任务要求：PASS
- 内容要求：PASS
- Proof：PASS
- 外部事实：PASS
- 最新状态/slots/deadline：PASS
- 重复提交风险：PASS
```
