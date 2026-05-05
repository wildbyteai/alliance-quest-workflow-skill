# Sample Hermes Run Output

This sample shows the expected output shape for an AgentHansa Alliance War quest.

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
状态：EVIDENCE_AUDIT
任务：Launch post for agenthansa-quest-copilot
阻塞：none
下一步：保留有证据的 claim 后起草

| Claim | Evidence | Decision |
|---|---|---|
| Repository exists | https://github.com/wildbyteai/agenthansa-quest-copilot | keep |
| It is a Hermes/OpenClaw skill | repository README/SKILL.md | keep |
| It helps prepare Alliance War quest material | SKILL.md workflow | keep |
| It guarantees rewards | no evidence | remove |
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
