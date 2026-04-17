---
name: init-task
description: Rehydrate a project before starting work on a new coding task.
layer: task-context
scope: resume / switch / re-enter
---

# Init Task

Rehydrate the minimum project context before designing or implementing a new task.

This is a read-first skill. It works best on projects initialized by `init-project`, but it can also operate on partially managed repos.

## Purpose

`init-task` restores working context before action.

It is responsible for:
- identifying the current project state
- rehydrating the minimum useful context
- mapping the new request onto implemented reality
- identifying likely impact areas
- producing a compact task-start framing before editing anything

It is a re-entry and task-switch skill, not a project bootstrap skill.

## When to use

Use this skill when:
- switching tasks
- resuming work after interruption
- continuing an implementation phase
- taking over a repo that may follow the shared contract
- reframing a new request against current project reality

## Shared Contract

Prefer these files when they exist:

- `CURRENT_PROGRESS.md`: current implemented state, latest validation, current next-step entry
- `RECORD.md`: full implementation / experiment record
- `NEXT_PHASE_TASK.md`: long-term stepwise implementation archive
- `docs/requirements.md`: product requirements and scope
- `docs/system_design.md`: architecture and interfaces
- `README.md`: public-facing summary

## Project States

- `project_uninitialized`
- `project_bootstrapped`
- `design_locked`
- `implementation_active`
- `mvp_ready`
- `publish_ready`
- `published`

## Workflow

1. Inspect the current project state.
   - Run `git status --short --branch` when inside a repo.
   - Treat unrelated local changes as user-owned.
   - Detect whether the shared-contract files exist.

2. Classify the project before reading deeply.
   - If `CURRENT_PROGRESS.md` and `RECORD.md` both exist, treat the repo as managed.
   - If the contract is missing, classify it as `project_uninitialized` or an `adopt-existing` candidate.
   - If the task is long-horizon and the contract is missing, recommend `init-project` in `adopt-existing` or `repair` mode.

3. Rehydrate lightweight state first.
   - Read `README.md`.
   - Read `CURRENT_PROGRESS.md` when present.
   - Read the role section and the most recent `2-4` entries in `RECORD.md` when present.

4. Rehydrate planning or design only when it matters.
   - Read `NEXT_PHASE_TASK.md` when the task is about roadmap, phases, sequencing, or long-term plan changes.
   - Read `docs/system_design.md` when architecture, interfaces, data flow, storage, or API shape may change.
   - Read `docs/requirements.md` when scope, behavior, or product boundaries are unclear.

5. Map the new request onto current reality.
   - State the current project state.
   - State what is already implemented that overlaps with the request.
   - Identify the likely impacted modules and files.
   - Call out any mismatch between the user request and the project state.

6. Explore code only after the project state is clear.
   - Search the smallest useful file set.
   - Read only the files needed to remove uncertainty.

7. Produce a compact task-start output before implementation.
   - `current state`
   - `task understanding`
   - `impact area`
   - `constraints and risks`
   - `next steps`

## Source Priority

When sources overlap, prefer them in this order:

1. `CURRENT_PROGRESS.md`
2. `RECORD.md`
3. `NEXT_PHASE_TASK.md`
4. `README.md`
5. `docs/system_design.md`
6. `docs/requirements.md`
7. code files
8. dependency / infra files

## Decision Rules

- Prefer recent `RECORD.md` entries over older history.
- Do not read full design docs by default.
- Do not start editing until the task has been reframed against current project state.
- Ask only the minimum blocking question.
- If the project is not initialized, either continue from README/code only or recommend `init-project`, depending on the user's goal.
- Keep re-entry lightweight; restore only enough context to work safely.

## Output Expectations

- `current state`
- `task understanding`
- `impact area`
- `constraints and risks`
- `next steps`
