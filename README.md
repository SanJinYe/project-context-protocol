# project-context-protocol

A lightweight protocol for keeping project docs, task context, and repo surface consistent across long-running coding workflows.

## Why this exists

When working on demos, agent-assisted projects, or fast-moving codebases, the main problem is often not implementation itself, but context breakdown:

- entering a repo without a reliable current-state snapshot
- switching tasks without rehydrating the right context
- starting new projects without a stable working contract
- preparing a repo for sync or publishing without separating public surface from local working records

This repository is a small protocol for handling that problem.

It organizes project work into three related layers:

- **project contract**
- **task context**
- **repo surface**

## The Three Skills

### `init-project`
Bootstrap a new or existing project into a stable working contract.

Use it when:
- starting a greenfield project
- adopting an existing codebase
- repairing a partially managed project
- establishing requirements, design, and progress records

### `init-task`
Rehydrate the minimum useful context before starting a new coding task.

Use it when:
- switching tasks
- resuming work after interruption
- continuing an implementation phase
- mapping a new request onto the current project state

### `update-repo`
Refresh the repository surface before syncing or publishing.

Use it when:
- rewriting the README
- updating local records
- cleaning up the repo surface
- preparing a private or public push

## Core Idea

This protocol separates three things that often get mixed together:

- what the project is supposed to be
- what is currently happening
- what should be visible in the repository

The goal is to reduce context loss across time, task switches, and publication boundaries.

## Shared Contract

The workflow revolves around a small set of shared files:

- `docs/requirements.md`
- `docs/system_design.md`
- `NEXT_PHASE_TASK.md`
- `README.md`
- `CURRENT_PROGRESS.md`
- `RECORD.md`

These files are not equal in role.

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

## Typical Lifecycle

1. Start or adopt a project with `init-project`
2. Re-enter or switch work with `init-task`
3. Refresh the repo surface with `update-repo`

## Repository Structure

```text
.
├─ skills/
│  ├─ init-project.md
│  ├─ init-task.md
│  └─ update-repo.md
├─ templates/
│  ├─ CURRENT_PROGRESS.template.md
│  ├─ RECORD.template.md
│  ├─ NEXT_PHASE_TASK.template.md
│  └─ docs/
│     ├─ requirements.template.md
│     └─ system_design.template.md
└─ examples/
   └─ lifecycle-example.md
