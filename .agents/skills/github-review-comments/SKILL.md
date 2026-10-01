---
name: github-review-comments
description: 'Fix open GitHub pull request code review comments with the gh CLI. Use when reviewing unresolved PR threads, addressing Copilot review comments, or fixing and resolving review feedback one comment at a time.'
argument-hint: '[pull request number or current branch]'
user-invocable: true
---

# GitHub Review Comments

Fix unresolved pull request code review threads one at a time with `gh`. Keep the developer in control of commits, pushes, and review-thread resolution.

When `rtk` is installed, prefix terminal commands with `rtk`. If it is not installed, run the commands without the prefix.

## Process

### 1. Confirm GitHub and repository state

- Run `gh auth status`. Stop if GitHub CLI is unavailable or unauthenticated.
- Determine the pull request from the argument, or use the current branch:
  ```sh
  gh pr view --json number,url,headRefName,baseRefName
  ```
- Run `git status --short --branch` and inspect staged and unstaged changes.
- Preserve unrelated user changes. Do not reset, stash, overwrite, or stage them.

### 2. List every unresolved review thread

Use the GitHub GraphQL API through `gh` so resolved and unresolved review threads are distinguished:

```sh
gh api graphql \
  -f query='query($owner:String!, $repo:String!, $number:Int!) {
    repository(owner:$owner, name:$repo) {
      pullRequest(number:$number) {
        reviewThreads(first:100) {
          nodes {
            id
            isResolved
            isOutdated
            path
            line
            startLine
            comments(first:20) {
              nodes {
                author { login }
                body
                createdAt
                url
              }
            }
          }
        }
      }
    }
  }' \
  -F owner=<owner> -F repo=<repo> -F number=<pull-request-number>
```

- List all threads where `isResolved` is `false`.
- Include the thread ID, file, line, outdated status, comment URL, author, and complete comment text.
- Treat an outdated thread as unresolved until it is intentionally verified and resolved.
- Keep the list current after every fix.

### 3. Select exactly one comment

- Choose the next comment by severity and dependency, preferring a concrete correctness or safety issue.
- If several comments describe the same root cause, explain the grouping and keep the edit focused. Do not begin a second unrelated fix in the same step.
- Read the target file, nearby implementation, and relevant tests before editing.
- State a local hypothesis about the cause and the focused check that can disconfirm it.

### 4. Fix and validate locally

- Make the smallest repository-consistent change that addresses the selected comment.
- Preserve unrelated changes and public interfaces unless the comment requires otherwise.
- Run the narrowest relevant validation command for the affected stack.
- Run `git diff --check` and inspect the focused diff.
- Do not commit, push, or resolve the thread yet.

### 5. Request developer review

Report:

- The selected review comment and its GitHub URL.
- Files changed and the proposed behavior.
- Validation commands and results.
- Any assumptions or remaining risk.

Stop and wait for explicit developer approval. Do not run `git commit`, `git push`, or `resolveReviewThread` before approval.

### 6. Publish the approved fix

After approval:

- Stage only the files belonging to the selected fix with explicit paths. Never use `git add -A`, `git add .`, or broad globs.
- Use a concise conventional commit message.
- Run `git commit` and `git push` to the current branch.
- Verify the commit and clean worktree with `git status --short --branch` and `git log -1 --oneline`.

### 7. Resolve the fixed thread

Resolve only after the approved fix is pushed:

```sh
gh api graphql \
  -f query='mutation($threadId:ID!) {
    resolveReviewThread(input:{threadId:$threadId}) {
      thread { id isResolved }
    }
  }' \
  -F threadId=<review-thread-id>
```

- Confirm the response reports `isResolved: true`.
- If the fix was not pushed or validation failed, do not resolve the thread.
- Re-list unresolved threads and report the next one. Continue only when the developer asks to proceed.

## Decision Rules

- Never resolve a review thread merely because a local edit exists. The fix must be validated, explicitly reviewed by the developer, committed, and pushed.
- Never resolve unrelated, rejected, or still-open feedback.
- Never stage or commit unrelated worktree changes.
- Never use `git reset --hard`, destructive checkout commands, force-push, merge, close, or delete operations.
- If a comment is ambiguous, show the relevant code and ask for clarification instead of guessing.
- If GitHub returns an API error, report the exact command and error and leave the thread unresolved.
- If no unresolved threads remain, report that the PR review is clear.
