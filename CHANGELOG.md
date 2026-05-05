# Changelog

## 1.8.0

- Reduced the repository to a single formal responsibility: AgentHansa Alliance War quest workflow.
- Removed redundant scope lists and broad platform language from the core skill.
- Reframed the workflow as 获取 -> 分析 -> 方案 -> 提交材料.
- Simplified README and spec files to avoid overlapping instructions.
- Kept submission as final `SubmitAnswer` material preparation; the user performs final submission.

## 1.7.2

- Completed end-to-end simulated examples for a new Alliance War quest.
- Added missing `EVIDENCE_AUDIT`, `CREATING_DELIVERABLE`, `DELIVERABLE_REVIEW`, and `WAITING_FOR_USER_ACTION` steps to examples.
- Clarified that deliverable drafting must enter `DELIVERABLE_REVIEW` before final submission material.
- Fixed purpose wording to match the five-step workflow.

## 1.7.1

- Added Alliance War submission-state checks before treating a quest as fresh.
- Added read-only references for `GET /api/alliance-war/quests/{quest_id}/submissions`.
- Documented the official `SubmitAnswer` payload: required `content`, optional `proof_url`, and optional first-submission `challenge_answer`.
- Added `CHECKING_SUBMISSION_STATE` and `WAITING_FOR_USER_ACTION` states for step-by-step quest progression.
- Added duplicate-submission protection and clearer resubmission routing.
- Strengthened final material checks for content length, proof URL readiness, challenge handling, and existing submission risk.

## 1.7.0

- Narrowed the skill scope to AgentHansa Alliance War quests only.
- Replaced broad AgentHansa task handling with a focused Alliance War quest workflow.
- Added official Alliance War endpoint references for read-only lookup and user-run submission material.
- Clarified that confirmation phrases assemble final submission material only; the agent never submits, verifies, or mutates external platforms.
- Removed community, collective bounty, forum, red packet, prediction, wallet, referral, and merchant workflows from scope.
- Rewrote `SKILL.md` and `/spec` to remove placeholders and conflicting generalized behavior.

## 1.6.0

- Introduced Evidence-First Writing Rule before deliverable creation
- Constrained Phase 4 to evidence-approved claims only

## 1.5.0

- Repositioned the skill as a preparation copilot, not a submission executor.
- Added an absolute no-auto-submit rule that overrides all workflow instructions, examples, trigger phrases, user shorthand, tool availability, demo convenience, and model assumptions.
- Changed `确认提交`, `提交`, `approved, submit`, and resubmission phrases to mean: generate or refresh the final manual submission package only.
- Removed the automatic submit workflow from the core skill behavior.
- Forbid `SUBMITTING` and `SUBMITTED` states.
- Added `READY_FOR_MANUAL_SUBMISSION` and `MANUAL_SUBMISSION_GUIDE` states.
- Added an absolute 100% compliance rule: no manual submission package unless every mandatory requirement is PASS.
- Added a strict requirement/evidence/status compliance table.
- Changed any `FAIL` or `UNKNOWN` mandatory requirement into a `BLOCKED` state.
- Updated README to document manual-only submission and 100% mandatory compliance.
- Updated Telegram example so `确认提交` refreshes manual submission guidance instead of submitting.

(remaining history unchanged)
