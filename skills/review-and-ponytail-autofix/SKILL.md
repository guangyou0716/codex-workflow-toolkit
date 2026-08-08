---
name: review-and-ponytail-autofix
description: Review, fix, simplify, and validate staged, unstaged, and relevant untracked repository changes using exactly one correctness review and one Ponytail review.
---

# Review and Ponytail Autofix

Review the current repository's uncommitted changes, fix confirmed defects,
apply worthwhile low-risk simplifications, and run focused validation.

Each invocation must perform exactly:

1. baseline inspection;
2. one Codex correctness review;
3. fixes for confirmed findings;
4. one `$ponytail-review`;
5. up to five worthwhile simplifications;
6. final validation.

Never run either review more than once. Never invoke `$ponytail-audit`, recurse
into this skill, or continue reviewing until zero findings.

## Scope and safety

Work only inside the current repository and only on staged, unstaged, and
relevant untracked changes. Read committed code only when needed to understand
changed behavior.

Follow applicable `AGENTS.md`, nested instructions, repository documentation,
manifests, scripts, and CI configuration. Preserve intended behavior, public
contracts, architecture, security, accessibility, compatibility, and data
integrity.

Do not:

- modify unrelated code;
- commit, push, pull, merge, rebase, reset, stash, switch branches, create a PR,
  discard changes, or modify Git configuration;
- overwrite unrelated user changes;
- expose secrets;
- update dependencies, lock files, migrations, generated files, vendored code,
  snapshots, or build output unless required by the requested feature;
- weaken tests, validation, compiler/linter strictness, security checks, or
  warnings merely to obtain a pass.

Use read-only Git commands for discovery.

## Repository map

Use `.agents/repo-map.md` only as an optional navigation aid when it saves
exploration. Verify all behavior against current source.

If missing or stale, continue with targeted discovery. Do not automatically run
the `repo-map` skill or perform a full remap.

Update the map only when this workflow clearly changes mapped structure, public
symbols, entry points, dependency boundaries, major runtime flows, validation
commands, or navigation paths. Otherwise leave it unchanged. If safe
incremental maintenance is not possible, recommend a separate refresh.

## Stage 1: Baseline

1. Read applicable repository instructions.
2. Inspect `git status`, staged and unstaged diffs, and relevant untracked files.
3. Identify affected projects, dependents, tests, validation commands, and
   framework-specific risks.
4. Record the original changed-file set and pre-existing failures.
5. Run only safe, useful baseline checks.

Prefer repository-defined commands from documentation, CI, manifests, wrappers,
and scripts. Detect the actual package manager and stack. Do not invent commands
or install/upgrade dependencies unnecessarily.

For monorepositories, validate affected packages first and broaden only when
shared contracts, dependents, or repository instructions require it.

## Stage 2: One correctness review

Perform the equivalent of `/review uncommitted changes`. If slash-command use is
unsupported, review manually.

Focus only on concrete, actionable defects introduced or exposed by the current
changes, including:

- incorrect or incomplete behavior and regressions;
- plausible edge cases, invalid state, null/optional handling;
- exception, cancellation, async, concurrency, thread-affinity, or shutdown
  defects;
- resource, event, process, stream, file, socket, or transaction leaks;
- validation, authentication, authorization, injection, secret exposure, or
  insecure defaults;
- data consistency, parsing, serialization, caching, protocol/API compatibility;
- practical performance or platform regressions;
- missing tests where changed behavior has a plausible failure risk.

Apply framework-specific checks only when relevant, such as UI binding and
lifecycle, frontend state and cleanup, API validation and authorization,
database transactions, worker retries/idempotency, or CLI exit/cleanup behavior.

Do not report style preferences, formatting, optional restructuring, vague
future-proofing, speculative defensive coding, unrelated debt, or test ideas
without a concrete failure scenario.

For each retained finding record:

- severity: high, medium, or low;
- file and location;
- failure scenario and evidence;
- proposed fix.

## Stage 3: Correctness fixes

Verify each finding against callers, consumers, tests, and framework behavior.

Automatically fix confirmed high- and medium-severity findings. Fix a
low-severity finding only when the change is small, localized, clearly correct,
behavior-preserving, low-risk, and directly relevant.

Do not apply a fix requiring product decisions, broad redesign, guessing,
unapproved public-API breakage, unrelated modules, or insufficient evidence.
Leave unsafe findings unchanged and report them.

After fixes, run the narrowest relevant formatting, analysis, type checks,
tests, and build commands. Repair failures introduced by this workflow when
safe. Do not run the correctness review again.

## Stage 4: One Ponytail review

Run `$ponytail-review` against current uncommitted changes only.

Look for high-confidence complexity introduced or exposed by the change:

- dead, unreachable, obsolete, or duplicate code;
- unnecessary wrappers, interfaces, factories, adapters, pass-through layers,
  configuration, dependencies, or indirection;
- duplicated validation or framework/standard-library functionality;
- temporary abstractions left after refactoring.

Apply at most five findings, prioritizing meaningful removal of dead code,
unused dependencies, redundant wrappers, duplicated logic, obsolete layers, or
custom code replaced by an established repository or standard capability.

Apply a simplification only when behavior is preserved, readability and
maintenance improve, regression risk is low, tests and public APIs remain
sound, framework conventions and dependency boundaries remain valid, and the
change can be verified.

Reject cosmetic cleanup, tiny line-count savings, broad redesign, speculative
simplification, unrelated debt, reduced readability/testability, DI bypasses,
framework violations, or removal of necessary platform abstractions and public
extension points.

Run focused validation after applying simplifications. Do not run Ponytail
again.

## Stage 5: Final validation

Run targeted checks first and broader checks only when impact or repository
instructions justify them. Applicable checks may include formatting, linting,
static analysis, type checking, compilation, packaging, unit/integration tests,
affected end-to-end tests, and repository-specific verification.

Do not claim a command passed when it was not run or failed. Separate:

- pre-existing failures;
- failures from the original changes;
- failures introduced by this workflow;
- unavailable or unverified checks.

If a temporary application, watcher, container, worker, service, emulator,
database, or test host was started, stop only the process created by this
workflow and clean up its temporary resources.

## Failure handling

Continue through all safe stages without routine confirmation.

When validation fails:

1. identify its origin;
2. repair workflow-introduced failures when safe;
3. rerun only the relevant command;
4. record unresolved failures accurately.

Do not alter infrastructure, production data, credentials, deployed migrations,
or unrelated code to force success. Do not disable tests, assertions, warnings,
or validation.

## Severity

- **High:** security/auth bypass, data loss, secret exposure, normal-path crash,
  persistent-state corruption, deadlock, serious race, broken public contract,
  irreversible migration defect.
- **Medium:** plausible incorrect result, resource leak, user-visible error
  handling gap, realistic concurrency/integration failure, invalid state,
  cancellation/shutdown defect, incorrect serialization.
- **Low:** narrow concrete edge case, localized maintainability defect, or small
  meaningful test gap.

Style preferences are not correctness findings.

## Final report

Keep the report concise:

### Scope
- affected projects/files and detected stack;
- instructions followed;
- whether the repository map was used and its status.

### Reviews and fixes
- confirmed correctness findings and fixes;
- meaningful rejected false positives when useful;
- Ponytail simplifications applied and rejected.

### Validation
- command, working directory, result;
- whether failures were pre-existing, unresolved, or not run.

### Files and map
- files already changed before the workflow;
- files changed by correctness fixes;
- files changed by Ponytail;
- map unchanged, incrementally updated, or needing separate refresh.

### Remaining risks
- unresolved findings, ambiguity, unavailable services/dependencies, unverified
  platform behavior, and tests that could not run.

### Git confirmation
Confirm that no commit, push, pull, PR, branch switch, merge, rebase, reset,
stash, or destructive Git operation occurred.

Do not expose private reasoning or claim the repository is bug-free.

