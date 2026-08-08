# Luna Direct Checkout Orchestrator

A Codex plugin that delegates one coding task to GPT-5.6 Luna / Max and, when the host supports it, edits the current checkout directly as unstaged files.

## Install

Use this public GitHub repository URL in Codex:

https://github.com/guangyou0716/luna-worktree-orchestrator

## Use

Invoke the skill with:

```
$luna-worktree-orchestrator:delegate Implement <task>.
```

The plugin preserves existing local changes and does not stage, commit, push, merge, create branches, or open pull requests.
