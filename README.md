# Codex Workflow Toolkit

A lightweight public Codex plugin for repository workflows and handoffs.

## Install

Use this GitHub repository URL in Codex:

https://github.com/guangyou0716/codex-workflow-toolkit

The repository keeps the local workflow skills below. It does not copy the upstream companion repositories; the setup skill points Codex to their original GitHub URLs so they can be installed and updated independently.

## Local skills

- `codex-workflow-toolkit:delegate`
- `codex-workflow-toolkit:chat-handoff-summary`
- `codex-workflow-toolkit:repo-map`
- `codex-workflow-toolkit:repo-update`
- `codex-workflow-toolkit:review-and-ponytail-autofix`
- `codex-workflow-toolkit:setup-upstream-skills`

## Upstream companion sources

Invoke `codex-workflow-toolkit:setup-upstream-skills` after installing this plugin to install these sources from their original repositories:

- [Ponytail](https://github.com/dietrichgebert/ponytail) - Codex plugin
- [UI/UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) - UI/UX skill
- [Emil Kowalski's skills](https://github.com/emilkowalski/skills) - design and animation skills

Codex plugin manifests do not declare dependencies that silently install other plugins, so the setup skill is the explicit second step after installing this repository URL.
