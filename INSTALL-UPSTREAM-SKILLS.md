# Install upstream companion sources

This repository keeps its workflow skills locally. The companion sources stay in their original GitHub repositories and are installed separately.

## Source links

- [Ponytail](https://github.com/dietrichgebert/ponytail)
- [UI/UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
- [Emil Kowalski's skills](https://github.com/emilkowalski/skills)

## Install Ponytail

Open [Ponytail](https://github.com/dietrichgebert/ponytail), then run:

```text
codex plugin marketplace add DietrichGebert/ponytail
codex plugin add ponytail@ponytail
```

## Install UI/UX Pro Max

Open [UI/UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill), then run the Codex skill installer:

```text
python <CODEX_HOME>/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo nextlevelbuilder/ui-ux-pro-max-skill --path .claude/skills/ui-ux-pro-max
```

## Install Emil Kowalski's skills

Open [Emil Kowalski's skills](https://github.com/emilkowalski/skills), then install its skills together:

```text
python <CODEX_HOME>/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo emilkowalski/skills --path skills/emil-design-eng skills/animate skills/review-animations skills/improve-animations skills/find-animation-opportunities skills/animation-vocabulary skills/apple-design skills/pick-ui-library skills/prototype
```

Replace `<CODEX_HOME>` with the Codex installation directory on the current computer. After installation, use the upstream skill names directly. Do not copy their files into this repository.

## Codex setup shortcut

After installing this repository, invoke `codex-workflow-toolkit:setup-upstream-skills` to follow the same instructions.
