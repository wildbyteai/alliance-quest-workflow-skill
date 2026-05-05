# Telegram Chat Example

This example shows the intended mobile workflow for an AgentHansa Alliance War quest.

## User

```text
按任务流程执行

AgentHansa Alliance War Quest:
Quest ID: 123
Reward: $50
Task: Find 5 AI product feature requests, write a short English post explaining why they matter, and submit proof_url with the published post and screenshots.
Deadline: 2026-04-30
```

## Hermes

```markdown
状态：FETCHING_QUEST_DETAIL
任务：Find 5 AI product feature requests
阻塞：none
下一步：确认完整 Alliance War quest 详情

## 详情确认
- 来源：用户提供的 quest detail
- 匹配字段：Quest ID / reward / deadline
- 结果：可进入需求分析
```

## Hermes

```markdown
状态：CHECKING_SUBMISSION_STATE
任务：Find 5 AI product feature requests
阻塞：none
下一步：确认这不是已有提交的重复材料

## 状态检查
- quest status：open / 用户提供或只读查询确认
- my submissions：未发现当前 quest 的已提交记录
- duplicate risk：低
```

## Hermes

```markdown
状态：ANALYZING_REQUIREMENTS
任务：Find 5 AI product feature requests
阻塞：none
下一步：拆解要求并判断是否可做

## 要求拆解
必须做：
- 找到 5 个与 AI 产品需求相关的 feature requests
- 写一条英文短帖
- 说明每个 request 为什么重要
- 提供已发布帖子 URL
- 提供截图证据
- 最终提交 `content` 和 `proof_url`

风险点：
- 外部平台动作必须由用户本人完成
- proof_url 必须公开可访问
- 内容不能像 spam

未知/缺失：
- 5 个 request 链接尚未提供
- published post URL 尚未提供

执行判断：
有风险但可做。任务清楚，但 proof 依赖用户手动完成外部动作。
```

## Hermes

```markdown
状态：PLANNING
任务：Find 5 AI product feature requests
阻塞：等待用户提供 request 链接和 proof
下一步：用户完成外部动作后发回链接、截图和 published post URL

## 需要你操作
- 动作：找到 5 个 request，完成任务要求的外部动作，发布帖子，保存截图
- 原因：这些动作涉及你的外部账号，agent 不能代替执行
- 完成后发给我：5 个 request 链接、截图、published post URL
```

## User

```text
Here are the 5 request links, screenshots, and the published post URL:
[links...]
```

## Hermes

```markdown
状态：COMPLIANCE_CHECK
任务：Find 5 AI product feature requests
阻塞：none
下一步：确认所有 mandatory requirements 是否 PASS

| Requirement | Evidence | Status |
|---|---|---|
| 5 AI product requests | 用户提供 5 个 request links | PASS |
| English post | published post URL | PASS |
| Explains why they matter | post content covers 5 reasons | PASS |
| Screenshots | 用户提供截图 | PASS |
| proof_url public | published URL accessible / user confirmed | PASS |
| content length | content is longer than 20 characters | PASS |
| challenge_answer | not requested by platform / user will fill manually if requested | PASS |
| duplicate submission | no existing fresh submission found | PASS |

Gate result: PASS
```

## User

```text
确认提交
```

## Hermes

```markdown
状态：READY_FOR_SUBMISSION_MATERIAL
任务：Find 5 AI product feature requests
阻塞：none
下一步：请由你本人在 AgentHansa Alliance War 页面提交

## 最终提交材料
content:
I reviewed five AI product feature requests and explained why each matters for builders. The published post and supporting screenshots are included in the proof URL.

proof_url:
<published proof URL>

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

## 手动提交步骤
1. 打开对应 Alliance War quest 页面。
2. 粘贴 `content`。
3. 粘贴 `proof_url`。
4. 如果 AgentHansa 要求 first-submission challenge，由你本人填写 challenge answer。
5. 由你本人点击提交。
```

The agent does not call the submit endpoint.
