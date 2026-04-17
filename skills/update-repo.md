---
name: update-repo
description: Refresh a repository before syncing or publishing.
layer: repo-surface
scope: cleanup / sync / publish-prep
---

# Update Repo

Refresh a repository with a stable, low-ceremony workflow before syncing or publishing.

This skill assumes the shared contract created by `init-project`, but it can also operate on older repos by first classifying what exists.

## Purpose

`update-repo` refreshes the repo-facing surface and local project records.

It is responsible for:
- inspecting the current repo state
- classifying publish intent
- refreshing the public-facing surface
- keeping internal working records aligned with implemented reality
- preparing commit / push only when requested

It does not assume public release, and it does not imply `git init`.

## When to use

Use this skill when:
- cleaning up the repo surface
- rewriting `README.md`
- updating local progress records
- preparing a private or public push
- synchronizing plan and design files with implemented reality

## Shared Contract

Handle these files explicitly:

- `README.md`: public-facing project summary
- `.gitignore`: ignore and publish boundary
- `CURRENT_PROGRESS.md`: only current progress snapshot
- `RECORD.md`: full implementation / experiment record
- `NEXT_PHASE_TASK.md`: long-term implementation archive
- `docs/requirements.md`: local requirements source of truth
- `docs/system_design.md`: local design source of truth

## Repo Modes

- `local-only`: no repo yet, or the user does not want to publish now
- `private-repo`: repo exists but should remain private for now
- `public-repo`: repo is ready for public-facing cleanup and push

## Workflow

1. Inspect the current repo state.
   - If this is a git repo, run `git status --short`, `git branch --show-current`, and inspect remotes as needed.
   - Read `README.md` and `.gitignore`.
   - Read `CURRENT_PROGRESS.md` and `RECORD.md` when present.
   - Read `NEXT_PHASE_TASK.md` only when the long-term plan may have changed.
   - Read `docs/` only when requirements or design changed materially, or when the user explicitly asks for sync.

2. Classify the repo mode before touching git.
   - Use `local-only` when the project is not ready to publish or the user wants no repo action.
   - Use `private-repo` for early-stage or internal repos.
   - Use `public-repo` only when the user actually wants a publish-ready surface.

3. Refresh the publish surface.
   - Rewrite `README.md` so it describes the shipped product directly.
   - Keep `README.md` self-contained.
   - Keep internal planning and agent-state files local-only by default:
     - `CURRENT_PROGRESS.md`
     - `RECORD.md`
     - `NEXT_PHASE_TASK.md`
     - `docs/requirements.md`
     - `docs/system_design.md`
     - `AGENTS.md`
     - `.codex/`
     - `.vscode/`
   - Do not hide tests by default; decide based on the repo's actual publish policy.
   - Add or update `LICENSE` only when the repo is intended to be public or open source.

4. Update local project records.
   - Update `CURRENT_PROGRESS.md` to reflect the latest real product state.
   - Update `RECORD.md` at the end of the session.
   - Update `NEXT_PHASE_TASK.md` only when the long-term implementation plan, major design, or concrete phase breakdown changed.
   - Update `docs/system_design.md` only when architecture or interfaces changed.
   - Update `docs/requirements.md` only when scope or expected behavior changed materially.

5. Validate the intended publish set.
   - Update `.gitignore` before removing tracked local-only files from the index.
   - Use `git rm --cached` for tracked files that should become local-only.
   - Stage only the intended files when the worktree is mixed.
   - Run `git diff --cached --check` before commit.

6. Commit and push only when the repo mode and user request call for it.
   - `local-only`: no commit or push is required.
   - `private-repo` / `public-repo`: commit and push the current branch when asked.
   - Report clearly if no remote publish step was performed.

## Decision Rules

- `update-repo` does not imply `git init`.
- `update-repo` does not force public release.
- Keep internal planning files local by default.
- Prefer KISS cleanup over repo-process ceremony.
- Do not silently stage unrelated user changes.
- Treat repo visibility and project maturity as related but separate concerns.

## Minimal Command Pattern

```text
git status --short
git branch --show-current
git diff --cached --check
git commit -m "<message>"
git push origin <branch>
