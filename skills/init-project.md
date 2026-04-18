---
name: init-project
description: Initialize a new or existing project into a stable working contract.
layer: project-contract
scope: bootstrap / adopt / repair
---

# Init Project

Initialize a new or existing project into a stable, long-running working contract.

This skill is used to bootstrap project structure, document roles, and local working conventions before implementation work begins in earnest.

This v1 is Python-first for environment setup. It assumes `requirements.txt` and `.venv` unless the user explicitly asks for a different runtime.

## Purpose

`init-project` establishes the project-level contract.

It is responsible for:
- classifying the current project situation
- creating or repairing the shared protocol files
- creating `AGENTS.md` as the project-level agent behavior contract
- separating engineer-facing docs from agent-facing state
- aligning project documentation with intended scope
- preparing a minimal local environment
- setting the project up for future `init-session` and `update-repo` usage

It does **not** imply immediate implementation, repo publishing, or broad tooling setup.

## When to use

Use this skill when:
- starting a greenfield project
- adopting an existing codebase into this workflow
- repairing a partially managed project
- creating requirements, design, phase, and progress records
- preparing a project for consistent long-running work

## Shared Contract

Create or repair these files when missing or stale.

Engineer-facing docs:
- `docs/requirements.md`: product requirements and scope
- `docs/system_design.md`: architecture and implementation design

Agent-facing local protocol:
- `AGENTS.md`: stable project-level agent behavior contract; keep only rules that directly affect agent behavior
- `IMPLEMENTATION_PLAN.md`: long-lived implementation plan; update only when requirements change
- `RECORD.md`: append-only implementation / experiment / rollback log
- `CURRENT_PROGRESS.md`: single current-state snapshot; update at the end of every session, after `RECORD.md`

Repo surface and setup:
- `README.md`: public-facing project summary
- `requirements.txt`: direct Python dependencies only
- `.gitignore`: local/runtime/publish boundary

## Project Modes

- `greenfield`: empty or nearly empty directory
- `adopt-existing`: existing codebase without this contract
- `repair`: contract exists partially and needs cleanup
- `already-managed`: contract already exists and only needs minor alignment

## Repo Modes

- `bootstrap-only`: default; do not initialize or publish a repo
- `private-repo`: initialize git only when the user explicitly wants version control now
- `existing-repo`: reuse the current repo and avoid destructive changes

## Project States

- `project_uninitialized`
- `project_bootstrapped`
- `design_locked`
- `implementation_active`
- `mvp_ready`
- `publish_ready`
- `published`

`init-project` should move the project to at least `project_bootstrapped`, and usually to `design_locked`.

## File Role Rules

These file roles should stay distinct:

### `AGENTS.md`
The stable project-level agent behavior contract.
It should contain:
- project-specific workflow rules that remain stable across sessions
- preferred commands or checks that directly affect agent behavior
- hard boundaries or safety rules that are specific to this project

It should not duplicate requirements, design, current progress, or roadmap content.
Remove stale or misleading rules instead of preserving them.

### `CURRENT_PROGRESS.md`
The single current-state snapshot.
It should contain:
- current implemented state
- latest validation status
- the current step in `IMPLEMENTATION_PLAN.md`
- immediate next step

### `RECORD.md`
The full implementation, experiment, and rollback log.
It should contain:
- important sessions
- failed attempts
- rollbacks and corrections
- fixes
- decisions and rationale

It is append-only: do not rewrite old entries.

### `IMPLEMENTATION_PLAN.md`
The long-horizon implementation archive.
It should contain:
- major phase breakdown
- milestone sequencing
- deferred but concrete future work

Update it only when requirements or the long-term plan changed materially.
It should not track normal step-by-step progress.

## Write Contract

Whenever both `RECORD.md` and `CURRENT_PROGRESS.md` are updated in the same session:
- write `RECORD.md` first
- write `CURRENT_PROGRESS.md` after that
- make `CURRENT_PROGRESS.md` `Immediate Next Step` match the latest `RECORD.md` entry conclusion / next step

## Workflow

1. Inspect the current directory first.
   - Detect whether `.git/` already exists.
   - Detect whether source files already exist.
   - Detect whether the shared protocol files already exist.
   - Classify the project mode before writing anything.

2. Clarify only the minimum blocking unknowns.
   - If the user already wants Python, `requirements.txt`, and `.venv`, do not ask again.
   - If the runtime or package manager materially changes the scaffold and cannot be inferred, ask one short blocking question.

3. Respect existing work.
   - Never overwrite user code blindly.
   - If a managed file already exists, read it and patch it in place.
   - For existing repos, prefer `adopt-existing` or `repair` over rebuilding.

4. Create or repair the managed files in this order.
   - `docs/requirements.md`
   - `docs/system_design.md`
   - `IMPLEMENTATION_PLAN.md`
   - `AGENTS.md`
   - `README.md`
   - `requirements.txt`
   - `.gitignore`
   - `RECORD.md`
   - `CURRENT_PROGRESS.md`

5. Build the initial document roles correctly.
   - `AGENTS.md` is the stable project-level agent behavior contract.
   - `IMPLEMENTATION_PLAN.md` is the long-term implementation archive and should not duplicate current progress.
   - `RECORD.md` is the append-only session / experiment / rollback record.
   - `CURRENT_PROGRESS.md` is the only current progress snapshot.
   - When both state files are touched, write `RECORD.md` before `CURRENT_PROGRESS.md`.

6. Set up the Python environment.
   - Create `.venv`.
   - Install only the direct dependencies listed in `requirements.txt`.
   - Do not replace `requirements.txt` with `pip freeze`.

7. Handle repo initialization conservatively.
   - Default to `bootstrap-only`.
   - Do not run `git init` unless the user explicitly wants a repo now.
   - Do not publish or push here; that belongs to `update-repo`.

8. End with a working handoff.
   - Report the detected mode and repo mode.
   - Report which files were created or repaired.
   - Report the dependency / `.venv` status.
   - Recommend `init-session` as the next step for real implementation work.

## Decision Rules

- Prefer patching over replacement when a project already exists.
- If the repo already has meaningful structure, adopt it rather than fighting it.
- Keep the initial scaffold minimal and maintainable.
- Do not invent extra docs beyond the shared contract unless the user asked for them.
- If legacy `AGENT.md` exists, merge its valid project-specific rules into `AGENTS.md` and remove stale or misleading content.
- If legacy `NEXT_PHASE_TASK.md` exists, migrate it to `IMPLEMENTATION_PLAN.md`.
- Default `AGENTS.md` to local-only and keep it out of the publish surface.
- Treat this as project-contract setup, not task execution.

## Output Expectations

- `mode`
- `repo mode`
- `files created or updated`
- `dependency and environment status`
- `next step`
