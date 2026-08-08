---
name: setup-upstream-skills
description: Install this toolkit's upstream companion Codex plugin and skills from their original GitHub repositories. Use when setting up the toolkit on a new PC, or when the user asks to install Ponytail, UI/UX Pro Max, or Emil Kowalski's skills.
---

# Set up upstream companion sources

Keep the local workflow skills from this plugin. Install the companion sources from upstream; do not copy their files into this repository.

## 1. Install Ponytail as a plugin

Run these Codex CLI commands:

```text
codex plugin marketplace add DietrichGebert/ponytail
codex plugin add ponytail@ponytail
```

Source: https://github.com/dietrichgebert/ponytail

## 2. Install UI/UX Pro Max

Use the installed Codex skill-installer helper with the source path that contains the ready-to-install skill:

```text
python <CODEX_HOME>/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo nextlevelbuilder/ui-ux-pro-max-skill --path .claude/skills/ui-ux-pro-max
```

Source: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill

## 3. Install Emil Kowalski's skills

Use the same helper once for all skills:

```text
python <CODEX_HOME>/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo emilkowalski/skills --path skills/emil-design-eng skills/animate skills/review-animations skills/improve-animations skills/find-animation-opportunities skills/animation-vocabulary skills/apple-design skills/pick-ui-library skills/prototype
```

Source: https://github.com/emilkowalski/skills

Resolve `<CODEX_HOME>` using the current machine's Codex installation. After installation, use the newly installed upstream skill names directly; do not duplicate them under this plugin.
