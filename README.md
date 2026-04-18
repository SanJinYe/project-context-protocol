# project-context-protocol

A lightweight protocol for keeping engineer docs, agent context, and repo surface consistent across long-running coding workflows.

## Why this exists

When working on demos, agent-assisted projects, or fast-moving codebases, the main problem is often not implementation itself, but context breakdown:

- entering a repo without a reliable current-state snapshot
- starting a new session without rehydrating the right context
- starting new projects without a stable working contract
- preparing a repo for sync or publishing without separating public surface from local working records

This repository is a small protocol for handling that problem.

It organizes project work into three related layers:

- **project contract**
- **session context**
- **repo surface**

## The Three Skills

### `init-project`
Bootstrap a new or existing project into a stable working contract.

Use it when:
- starting a greenfield project
- adopting an existing codebase
- repairing a partially managed project
- establishing requirements, design, and progress records

### `init-session`
Rehydrate the minimum useful context before starting or resuming a coding session.

Use it when:
- starting a new session in an existing project
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

- what engineers need to build
- what the agent must remember between sessions
- what should be visible in the repository

The goal is to reduce context loss across time, task switches, and publication boundaries.

## Shared Contract

The workflow revolves around a small set of shared files with different audiences and update rules.

Engineer-facing docs:
- `docs/requirements.md`
- `docs/system_design.md`

Agent-facing local protocol:
- `AGENTS.md`
- `IMPLEMENTATION_PLAN.md`
- `CURRENT_PROGRESS.md`
- `RECORD.md`

Repo surface:
- `README.md`

These files are not equal in role.

### `AGENTS.md`
The stable project-level agent behavior contract.

It should contain:
- project-specific workflow rules that directly affect agent behavior
- preferred commands or checks
- stable project boundaries

It should not repeat requirements, design, progress, or roadmap content.

### `CURRENT_PROGRESS.md`
The single current-state snapshot.

It should contain:
- current implemented state
- latest validation status
- current step in `IMPLEMENTATION_PLAN.md`
- immediate next step

### `RECORD.md`
The append-only implementation, experiment, and rollback log.

It should contain:
- important sessions
- failed attempts
- rollbacks and corrections
- fixes
- decisions and rationale

Write `RECORD.md` first, then update `CURRENT_PROGRESS.md`.
`CURRENT_PROGRESS.md` `Immediate Next Step` must match the latest `RECORD.md` entry conclusion / next step.

### `IMPLEMENTATION_PLAN.md`
The long-lived implementation archive.

It should contain:
- major phase breakdown
- milestone sequencing
- deferred but concrete future work

Update it only when requirements or the long-term plan changed materially.
It should not duplicate the current progress snapshot.

## Publish Boundary

`update-repo` keeps these files local-only by default:

- `AGENTS.md`
- `IMPLEMENTATION_PLAN.md`
- `CURRENT_PROGRESS.md`
- `RECORD.md`
- `docs/requirements.md`
- `docs/system_design.md`

## Typical Lifecycle

1. Start or adopt a project with `init-project`
2. Re-enter or switch work with `init-session`
3. Refresh the repo surface with `update-repo`

## Repository Structure

```text
.
|- skills/
|  |- init-project.md
|  |- init-session.md
|  \- update-repo.md
|- templates/
|  |- AGENTS.template.md
|  |- CURRENT_PROGRESS.template.md
|  |- IMPLEMENTATION_PLAN.template.md
|  |- RECORD.template.md
|  \- docs/
|     |- requirements.template.md
|     \- system_design.template.md
\- examples/
   \- lifecycle-example.md
```
