---
name: repo-map
description: Create or rebuild a compact repository navigation map at .agents/repo-map.md. Use when asked to map, index, analyze, or fully refresh a repository.
---

# Repository Map

Create `.agents/repo-map.md` as a compact navigation aid. Never modify source
files, commit, push, or expose secrets.

## Output budget

The completed map must be approximately **500-1,500 model tokens**.

- Aim for 800-1,200 tokens.
- Never exceed 1,500 tokens.
- Prefer paths, symbols, and short phrases over prose.
- Do not list every file, class, model, helper, test, or configuration item.
- Combine related components into one line.
- If the repository is large, map only central architecture and high-value
  navigation points.
- Before finishing, estimate the map size and compress it if necessary.

## Discovery

1. Read applicable `AGENTS.md`, `README.md`, contribution, architecture, build,
   test, CI, formatter, and package-manager files.
2. Inspect top-level structure, manifests, entry points, service registration,
   important public declarations, tests, persistence, and external boundaries.
3. Use targeted search and progressive discovery. Do not recursively inspect
   every implementation.
4. Exclude generated files, build output, binaries, caches, vendored code,
   dependencies, lock-file contents, large assets, secrets, and trivial helpers.
5. Determine commands and technologies only from repository evidence.

## Required content

Use only sections that add navigation value:

```markdown
# Repository Map

## Metadata
- Updated:
- Root:
- Branch / commit:
- Working tree:
- Stack:
- Repository purpose:
- Map status:

## Validation
- `<command>` â€” purpose

## Structure
- `path` â€” responsibility

## Runtime and Boundaries
1. Concise startup or primary flow.
2. Important dependency and persistence boundaries.
3. External or asynchronous boundaries.

## Navigation
### Task or subsystem
- Start: `path`
- Related: `path`, `path`

## High-Risk Areas
- `path` â€” reason

## Recent Structural Changes
- Only when supported by Git or uncommitted changes.

## Limitations
- Important unverified or intentionally omitted areas.
```

Merge or omit sections when necessary to remain within the token budget.

## Mapping rules

- Record only central projects, modules, entry points, interfaces, ViewModels,
  handlers, services, repositories, workers, schemas, routes, and integrations.
- Include signatures only when essential for locating behavior.
- Summarize no more than five important runtime or dependency relationships.
- Provide no more than eight navigation groups.
- Provide no more than six high-risk areas.
- Keep the top-level tree or structure list to roughly 12 entries.
- Do not copy implementation bodies or large configuration fragments.
- Mark uncertainty; future agents must verify live source before editing.

## Existing map

When `.agents/repo-map.md` exists, update incrementally when practical. Rebuild
only when missing, invalid, materially stale, substantially reorganized, or
explicitly requested.

Include material staged, unstaged, and relevant untracked structural changes.
Record whether they were included.

## Safety and validation

- Only write `.agents/repo-map.md`.
- Confirm referenced paths and important symbols exist.
- Do not invent validation commands.
- Use read-only Git commands only.
- Do not commit, push, pull, merge, rebase, reset, stash, switch branches, or
  modify Git configuration.

## Final response

Briefly report the action, output path, mapped branch/commit, whether
uncommitted changes were included, approximate token count, omitted/unverified
areas, and confirmation that source files and Git history were unchanged.

