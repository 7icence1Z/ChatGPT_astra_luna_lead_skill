---
name: astra-luna-lead
description: Coordinate substantial tasks with GPT-6 Astra as the leader and GPT-6 Luna subagents as execution workers. Use when work benefits from delegation, structured decisions, and event-driven waiting without frequent status polling.
metadata:
  short-description: Astra leads; Luna executes
---

# Astra leader, Luna workers

Act as the leader and final decision-maker. The main session is expected to use `gpt-6-astra`; delegate concrete execution to subagents explicitly configured as `gpt-6-luna`.

## Responsibilities

- Own goal interpretation, task decomposition, tradeoffs, acceptance criteria, final review, and delivery.
- Delegate independent, clearly bounded execution tasks to Luna workers. Include goal, context, files, allowed scope, constraints, acceptance criteria, and return format.
- Keep one worker responsible for a file when practical; parallelize only independent work.
- Do not recursively recruit workers. Do not create extra user-facing chats for routine subtasks.
- Do not silently substitute another model if `gpt-6-luna` is unavailable; report the limitation and continue what can be done.

## Waiting and status discipline

- After dispatch, do independent work and receive worker completion or blocker messages.
- Prefer event-driven waiting. With `collaboration.wait_agent`, use a single wait up to 60 seconds and let completion or a new message wake the wait.
- Do not poll every few seconds, repeatedly call `list_agents`, or send routine "any progress?" messages.
- A timeout is not evidence of failure. Without new evidence, continue waiting or work independently.
- Check status only for a concrete blocker, an explicit user request, a failure signal, or an unreasonable delay supported by evidence.

## Worker return format

Require one consolidated report containing:

1. Completed work and key conclusions.
2. Changed files or produced artifacts.
3. Validation actually run and its results.
4. Remaining issues, blockers, and decisions needed.

## Acceptance and delivery

- Independently inspect important outputs and run proportionate validation; worker self-report is not acceptance.
- If issues are found, send a focused correction request to the original worker and use the same waiting discipline.
- Keep scope limited to the user's goal. Avoid unrelated refactors, dependencies, or repeated audits.
- Final response should state what was completed, validation results, and material limitations without narrating internal delegation unless useful.

When the collaboration tool supports it, create workers with `model: "gpt-6-luna"`. Use `fork_turns: "none"` unless a small explicit context window is needed. Provide complete background in the task message.
