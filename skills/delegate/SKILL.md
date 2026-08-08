---
name: delegate
description: Delegate a concrete coding or repository-editing task to GPT-5.6 Luna with max reasoning while keeping the primary session lightweight, and request that Luna edit the user's current checkout directly without creating a separate worktree. Use when the user explicitly invokes $luna-worktree-orchestrator:delegate or asks for Luna / Max to modify the current branch directly. Do not use for question-only or research-only requests.
---

# Luna Direct-Checkout Delegate

## Goal

Keep the primary session lightweight while delegating implementation to one GPT-5.6 Luna / Max task that edits the user's current checkout directly.

Fixed behavior:

- The user selects the primary model and reasoning level in the Codex UI. Recommend Sol / Medium, but never claim this skill can set it.
- Delegate to exactly one user-visible task using `gpt-5.6-luna` with `thinking: max`.
- Request the existing project checkout/local environment, not an isolated Git worktree.
- Leave every resulting file as an ordinary unstaged working-tree change.
- Do not stage, commit, push, merge, create a branch, create a PR, or launch a separate reviewer.

## Hard platform boundary

Use direct-checkout delegation only when the current host's task-creation tool explicitly supports selecting the existing checkout/local environment for a Git repository.

Inspect and follow the exact `create_thread` schema exposed by the host. Never invent a `worktree`, `isolation`, `environment`, or similar argument.

If the host forces Git tasks into isolated worktrees or exposes no supported way to target the current checkout, stop and report that direct Luna editing is unavailable in this host. Do not silently fall back to a worktree, patch transfer, another model, or primary-session implementation.

## Activation boundary

The user's explicit invocation authorizes creation of one Luna task and direct edits in the current checkout. No extra confirmation is required for normal scoped edits.

Stop before delegation if another Codex task is already editing the same checkout or if the current project cannot be identified reliably.

## Required task tools

The workflow requires:

- `list_projects`
- `create_thread`
- `list_threads` when thread-id correlation is needed
- `wait_threads`
- `read_thread`
- `send_message_to_thread` for bounded corrections

Use only arguments supported by the current host schemas.

## Workflow

### 1. Capture primary state

Before creating the Luna task, record:

- absolute checkout path
- current branch or detached-HEAD state
- current HEAD SHA
- `git status --porcelain=v2 -z` output
- existing staged, unstaged, renamed, deleted, and untracked paths

Existing local changes are allowed. They remain user-owned and must not be reset, cleaned, restored, staged, or overwritten.

### 2. Resolve direct-checkout support

Call `list_projects` and identify the project matching the current checkout.

Inspect the available `create_thread` schema and project metadata. Proceed only when a documented argument or project mode targets the same existing checkout without creating a separate worktree.

Do not continue merely because the prompt can ask Luna to avoid a worktree; the actual task environment must be verifiably the recorded checkout path.

### 3. Send a compact task packet

Include only:

- objective and acceptance criteria
- exact checkout path and current branch
- likely files or areas, when known
- constraints and non-goals
- required verification commands, when known
- instruction to inspect existing code before editing
- instruction to preserve all pre-existing local changes
- instruction not to stage, commit, push, merge, create branches, open PRs, run destructive Git commands, or modify unrelated files
- required return format

Use this return format:

```text
Return:
1. Summary of changes
2. Files changed
3. Verification run and results
4. Remaining risks or blockers
5. Final checkout path, branch, HEAD, and git status summary
```

Do not send long chat history or duplicated repository context.

### 4. Create one Luna task

Call `create_thread` with:

- model: `gpt-5.6-luna`
- thinking: `max`
- selected project
- the supported direct-checkout/local-environment setting
- compact task packet

Record the real `threadId` and `hostId`. If only a client identifier is returned, correlate the new task through `list_threads` using trustworthy project, path, time, and state metadata.

Immediately verify that the task's actual checkout path equals the recorded primary checkout path. If it differs or points under a worktree directory, stop the task when supported and report the mismatch. Do not accept or transfer its edits automatically.

### 5. Wait and inspect

Use `wait_threads`, then `read_thread`.

Inspect the actual current checkout rather than trusting the handoff alone. At minimum:

- review the actual unstaged diff
- compare changed paths with the task scope
- verify pre-existing local changes were preserved
- run the narrowest meaningful verification
- check that no files were staged and no commit, branch change, push, merge, or PR occurred

### 6. Corrections

For one concrete defect, send one concise correction to the same Luna task. Permit a second correction only for a clear remaining blocker. After two rounds, stop and report the blocker.

### 7. Final state requirements

Completion is allowed only when:

- Luna edited the recorded current checkout directly
- the branch and HEAD are unchanged unless the user explicitly requested otherwise
- all Luna changes are unstaged
- pre-existing staged and unstaged work remains intact
- no commit, push, merge, branch creation, PR, or destructive Git operation occurred
- verification passed or limitations are clearly reported

Do not run `git add`, `git commit`, `git reset`, `git clean`, `git checkout --`, `git restore`, `git stash`, or any equivalent operation on user-owned changes.

## Completion report

Report only:

- files Luna changed
- verification results
- material residual risks
- confirmation that changes are directly visible in the user's current checkout as unstaged files
- confirmation that no worktree, stage, commit, push, merge, branch, or PR was created

If direct-checkout task creation was unsupported, state that clearly and do not claim implementation occurred.

## Recommended invocation

```text
$luna-worktree-orchestrator:delegate Implement the following change: <task>.
```

