---
name: Commands
description: 'Use the agentics commands for Git, planning, pull requests, releases, formatting, content generation, and issue context. Commands are case-insensitive.'
argument-hint: '<C|CP|P|CB|FF|G|GP|IMP|WN|PRD|CPR|SLC|YT>'
user-invocable: true
---

# Commands

Use this file as the default command interface for agentics. The commands below keep the shortcut style for fast workflows, while leaving room for additional repository commands that do not fit the Git shortcut pattern.

## Command Routing

When the user sends one of these commands, normalize it case-insensitively and follow the matching workflow.

### Git And Branches

- `C`: inspect, validate, stage, and commit the intended current changes.
- `CP`: perform the `C` workflow, then push the current non-`main` branch to its upstream remote. Refuse before staging or committing if the current branch is `main`.
- `P`: push the current non-`main` branch to its upstream remote without committing or modifying files. Refuse if the current branch is `main`.
- `CB {name}`: create a new git branch from the current `HEAD` without committing, pushing, or discarding uncommitted changes.
- `SLC`: summarize the latest changes on the current branch relative to its upstream or base branch without modifying the worktree.

### Formatting And Content

- `FF {paths}`: format the given files, or all files with uncommitted changes when no paths are given, without staging, committing, or pushing.
- `G`: generate the requested content or artifact without committing or pushing.

### Planning And Implementation

- `GP`: create or refine an incremental implementation plan under `.agent/plans/`.
- `IMP`: implement the requested phase from a plan and run its verification step.
- `WN`: inspect the current plan and report what should be implemented next without modifying the worktree.

### Pull Requests And Releases

- `PRD`: generate a consistent pull request description without modifying the worktree.
- `CPR [base branch or title]`: create or update a pull request through the `gh` CLI.

### Issue Context

- `YT`: pass YouTrack context to `PRD`; use `YTC {link}` when the PR closes an issue or `YTP {link}` when it addresses only part of one.

## General Procedure

1. Inspect the current branch and worktree before changing anything.
2. Review staged and unstaged diffs and preserve unrelated user changes.
3. Run the narrowest useful validation before committing.
4. Stage only the intended files.
5. Use a concise conventional commit message.
6. Report the commit, push, or pull request result clearly.

## Safety Rules

- Do not commit, push, or modify files for `G`, `WN`, `PRD`, or `SLC`.
- Do not commit or push for `FF` unless explicitly requested.
- Do not create a duplicate pull request for `CPR`.
- Never push the `main` branch. For `CP`, refuse before staging or committing when on `main`; for `P`, refuse before pushing. Use a feature branch and a pull request instead.
- Never force-push, reset hard, discard user changes, or rebase as part of a command.
- If the worktree is clean, do not create an empty commit.
- Preserve unrelated staged and unstaged changes.

## Extensible Commands

Commands may be added here when they represent a repeatable agent workflow. Keep each command explicit about whether it may edit files, commit, push, create remote state, or only report information.