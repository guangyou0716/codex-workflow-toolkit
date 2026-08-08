---
name: repo-update
description: Incrementally update .agents/repo-map.md from material repository changes without rescanning the whole repository.
---

# Repository Map Update

Incrementally update `.agents/repo-map.md`. Modify no other file. Never commit,
push, or expose secrets.

## Output budget

The updated map must remain approximately **500-1,500 model tokens**.

- Aim for 800-1,200 tokens.
- Never exceed 1,500 tokens.
- Preserve concise valid content.
- Remove stale or low-value detail when additions would exceed the budget.
- Prefer paths, symbols, and short phrases over prose.
- Before finishing, estimate the total map size and compress it if necessary.

## Preconditions

If `.agents/repo-map.md` is missing, invalid, or unusable, stop and recommend
running the `repo-map` skill. Do not create a full map.

Read its metadata, recorded branch/commit, and current token-size estimate.

## Incremental discovery

Use the smallest reliable scope:

- `git status`
- current branch and commit
- `git diff --name-status` and `--stat`
- changes since the recorded commit
- rename detection
- staged, unstaged, and relevant untracked files

Inspect only changed files, affected map sections, and narrowly required
callers, consumers, manifests, registrations, schemas, routes, or tests.

Do not broadly rescan the repository when the changed-file list is sufficient.

## Material update criteria

Update only for structural or navigational changes, such as:

- projects, packages, modules, or important files added, removed, moved, renamed
- entry points or major runtime flows changed
- public APIs, routes, handlers, services, repositories, ViewModels, workers,
  schemas, or exported symbols added or removed
- dependency injection or dependency boundaries changed
- persistence, transaction, external integration, test-project, build, lint,
  packaging, or validation commands changed

Normally ignore internal bug fixes, private helpers, formatting, comments,
styles, UI text, local refactors with unchanged structure, assertion-only test
changes, and version bumps without architectural impact.

## Update behavior

1. Update only affected sections.
2. Preserve accurate existing content.
3. Remove stale paths and symbols.
4. Correct renames and relationships.
5. Update metadata and whether uncommitted changes were included.
6. Add concise recent structural notes only when useful.
7. Compress unrelated low-value details only when needed to keep the entire map
   within 1,500 tokens.

Do not rewrite or reorder unrelated sections merely for consistency.

## Escalation

Stop and recommend a full `repo-map` rebuild when safe incremental maintenance
is not possible, including:

- unavailable or invalid recorded commit
- rewritten history with broad uncertainty
- major repository reorganization
- widespread renames
- many projects added or removed
- thousands of changed files
- broad conflicts between the map and current structure

A limited comparison is allowed when history is unavailable, but mark the map
as partially verified.

## Validation and safety

- Only modify `.agents/repo-map.md`.
- Confirm updated paths and important symbols exist unless recording removal.
- Confirm changed commands from repository evidence.
- Do not validate the entire unchanged map.
- Use read-only Git commands only.
- Do not modify source, tests, manifests, CI, configuration, or Git state.
- Do not commit, push, pull, merge, rebase, reset, stash, or switch branches.

## Final response

Briefly report whether the map changed, previous/current commit, changed files
inspected, sections updated, structural changes found, ignored non-structural
changes, uncommitted-change handling, approximate final token count, whether a
full rebuild is recommended, and confirmation that no source or Git state was
modified.

