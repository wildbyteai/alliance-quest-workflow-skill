# Sample Hermes Run Output

This sample shows the expected output shape for an AgentHansa Alliance War quest.

```markdown
状态：FETCHING_QUEST_DETAIL
任务：Launch post for agenthansa-quest-copilot
阻塞：none
下一步：确认完整 Alliance War quest 详情

## 详情确认
- 来源：examples/sample-quest.md
- 匹配字段：title / GitHub URL
- 结果：可进入提交状态检查
```

```markdown
状态：CHECKING_SUBMISSION_STATE
任务：Launch post for agenthansa-quest-copilot
阻塞：none
下一步：确认是否可以作为新 quest 推进

## 状态检查
- quest status：demo open
- my submissions：demo none
- duplicate risk：none
```

```markdown
状态：ANALYZING_REQUIREMENTS
任务：Launch post for agenthansa-quest-copilot
阻塞：none
下一步：拆解要求并判断是否可做

## 任务摘要
- 渠道：AgentHansa Alliance War
- 奖励：demo
- 截止：demo
- 目标：为 `agenthansa-quest-copilot` 写一条英文 launch post
- Proof：final post text + GitHub repository URL

## 要求拆解
必须做：
- 英文帖子
- 120 words 以内
- 提到 Hermes/OpenClaw skill
- 提到 AgentHansa Alliance War quests
- 包含 GitHub URL
- practical, non-hype
- 输出 proof plan

风险点：
- 不能声称 guaranteed rewards
- 不能声称 official AgentHansa endorsement
- 不能编造 metrics, partnerships, testimonials

执行判断：
可以做。任务自包含，proof 清楚，主要风险是过度宣传。
```

```markdown
状态：PLANNING
任务：Launch post for agenthansa-quest-copilot
阻塞：none
下一步：先审计 claims，再写英文草稿

## 执行计划
1. 只使用 repo 和 skill 中能证明的 claims。
2. 起草 120 words 内英文 launch post。
3. 做 100% compliance check。
4. 用户确认后输出最终提交材料。
```

```markdown
状态：EVIDENCE_AUDIT
任务：Launch post for agenthansa-quest-copilot
阻塞：none
下一步：保留有证据的 claim 后起草

| Claim | Evidence | Decision |
|---|---|---|
| Repository exists | https://github.com/wildbyteai/agenthansa-quest-copilot | keep |
| It is a Hermes/OpenClaw skill | repository README/SKILL.md | keep |
| It is for AgentHansa Alliance War quests | SKILL.md scope | keep |
| It helps prepare final submission material | SKILL.md workflow | keep |
| It guarantees rewards | no evidence | remove |
```

```markdown
状态：CREATING_DELIVERABLE
任务：Launch post for agenthansa-quest-copilot
阻塞：none
下一步：生成英文 launch post 草稿
```

```markdown
状态：DELIVERABLE_REVIEW
任务：Launch post for agenthansa-quest-copilot
阻塞：等待用户审核
下一步：请审核草稿；如无问题可回复 `确认提交`

## 交付物草稿
I built `agenthansa-quest-copilot`, a Hermes/OpenClaw skill for AgentHansa Alliance War quests.

It helps a Chinese-speaking operator fetch full quest details, analyze requirements, draft evidence-backed deliverables, check 100% compliance, prepare proof, and assemble final submission material for manual submission.

It is not an official AgentHansa tool and does not submit automatically.

GitHub: https://github.com/wildbyteai/agenthansa-quest-copilot
```

```markdown
状态：WAITING_FOR_USER_ACTION
任务：Launch post for agenthansa-quest-copilot
阻塞：等待 proof URL 确认
下一步：请确认 GitHub proof URL 可公开访问

## 需要你操作
- 动作：确认 proof URL 可访问
- 原因：Proof 必须可被 Alliance War grader 验证
- 完成后发给我：confirmed
```

```markdown
状态：COMPLIANCE_CHECK
任务：Launch post for agenthansa-quest-copilot
阻塞：none
下一步：确认所有 mandatory requirements 是否 PASS

| Requirement | Evidence | Status |
|---|---|---|
| English post | draft is English | PASS |
| Under 120 words | 65 words | PASS |
| Mentions Hermes/OpenClaw skill | first sentence | PASS |
| Mentions Alliance War quests | first sentence | PASS |
| Includes GitHub URL | proof URL and content include URL | PASS |
| Practical, non-hype | no reward or endorsement claim | PASS |
| content length | more than 20 characters | PASS |
| proof_url public | user confirmed GitHub URL | PASS |
| challenge_answer | not requested | PASS |
| duplicate submission | demo none | PASS |

Gate result: PASS
```

```markdown
状态：READY_FOR_SUBMISSION_MATERIAL
任务：Launch post for agenthansa-quest-copilot
阻塞：none
下一步：请由你本人在 AgentHansa Alliance War 页面提交

## 最终提交材料
content:
I built `agenthansa-quest-copilot`, a Hermes/OpenClaw skill for AgentHansa Alliance War quests.

It helps a Chinese-speaking operator fetch full quest details, analyze requirements, draft evidence-backed deliverables, check 100% compliance, prepare proof, and assemble final submission material for manual submission.

It is not an official AgentHansa tool and does not submit automatically.

GitHub: https://github.com/wildbyteai/agenthansa-quest-copilot

proof_url:
https://github.com/wildbyteai/agenthansa-quest-copilot

challenge_answer:
omit unless AgentHansa asks for it

checks:
- 任务要求：PASS
- 内容要求：PASS
- Proof：PASS
- 外部事实：PASS
- Challenge：PASS / not requested

remaining_risks:
- none
```
