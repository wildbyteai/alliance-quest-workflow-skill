# Screen Recording Plan

## Goal

Show that `agenthansa-quest-copilot` is a focused Alliance War quest preparation skill, not a general AgentHansa automation bot.

## Flow

### 0:00-0:10 Repository

Show `README.md`.

Narration:

> This is agenthansa-quest-copilot, a Hermes/OpenClaw skill for AgentHansa Alliance War quests only.

### 0:10-0:25 Scope And Safety

Show `SKILL.md` sections:

- Purpose.
- Hard Scope Boundary.
- Submission Boundary.

Narration:

> It fetches quest details, analyzes requirements, prepares proof, and assembles final submission material. It does not submit or verify for the user.

### 0:25-0:45 Quest Analysis

Show `examples/telegram-chat-example.md`.

Highlight:

- `FETCHING_QUEST_DETAIL`
- `CHECKING_SUBMISSION_STATE`
- `ANALYZING_REQUIREMENTS`
- Chinese requirement extraction

### 0:45-1:05 Compliance

Show:

- Evidence audit.
- 100% compliance table.
- `PASS/FAIL/UNKNOWN` gate.

Narration:

> Every mandatory requirement must be PASS before final material is produced.

### 1:05-1:20 Final Material

Show `READY_FOR_SUBMISSION_MATERIAL`.

Narration:

> The output is copy-ready `content`, `proof_url`, and optional challenge material. The human still performs final AgentHansa submission.

## Do Not Show

- `SUBMITTING`
- `SUBMITTED`
- Automatic API submission
- Non-Alliance War modules
