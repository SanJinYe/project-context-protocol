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
- creating or repairing the shared contract files
- aligning project documentation with intended scope
- preparing a minimal local environment
- setting the project up for future `init-task` and `update-repo` usage

It does **not** imply immediate implementation, repo publishing, or broad tooling setup.

## When to use

Use this skill when:
- starting a greenfield project
- adopting an existing codebase into this workflow
- repairing a partially managed project
- creating requirements, design, phase, and progress records
- preparing a project for consistent long-running work

## Shared Contract

Create or repair these files when missing or stale:

- `docs/requirements.md`: product requirements and scope
- `docs/system_design.md`: architecture and implementation design
- `NEXT_PHASE_TASK.md`: long-term stepwise implementation archive
- `README.md`: public-facing project summary
- `requirements.txt`: direct Python dependencies only
- `.gitignore`: local/runtime/publish boundary
- `CURRENT_PROGRESS.md`: single current progress snapshot
- `RECORD.md`: full implementation / experiment record

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

### `CURRENT_PROGRESS.md`
The single current-state snapshot.
It should contain:
- current implemented state
- latest validation status
- immediate next step

### `RECORD.md`
The full implementation and experiment log.
It should contain:
- important sessions
- failed attempts
- fixes
- decisions and rationale

### `NEXT_PHASE_TASK.md`
The long-horizon implementation archive.
It should contain:
- major phase breakdown
- milestone sequencing
- deferred but concrete future work

It should not duplicate the current progress snapshot.

## Workflow

1. Inspect the current directory first.
   - Detect whether `.git/` already exists.
   - Detect whether source files already exist.
   - Detect whether the shared-contract files already exist.
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
   - `NEXT_PHASE_TASK.md`
   - `README.md`
   - `requirements.txt`
   - `.gitignore`
   - `CURRENT_PROGRESS.md`
   - `RECORD.md`

5. Build the initial document roles correctly.
   - `CURRENT_PROGRESS.md` is the only current progress snapshot.
   - `RECORD.md` is the full session / experiment record.
   - `NEXT_PHASE_TASK.md` is the long-term implementation archive and should not duplicate current progress.

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
   - Recommend `init-task` as the next step for real implementation work.

## Decision Rules

- Prefer patching over replacement when a project already exists.
- If the repo already has meaningful structure, adopt it rather than fighting it.
- Keep the initial scaffold minimal and maintainable.
- Do not invent extra docs beyond the shared contract unless the user asked for them.
- Treat this as project-contract setup, not task execution.

## Output Expectations

- `mode`
- `repo mode`
- `files created or updated`
- `dependency and environment status`
- `next step`
