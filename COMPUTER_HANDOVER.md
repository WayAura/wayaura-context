# COMPUTER_HANDOVER

This document is the operational handover for the Computer role.
Computer is an implementation role with explicit scope. The procedure
below is binding, not advisory.

## On entry

1. Confirm you are taking the Computer role for this session.
2. Read, in order:
   - [`AGENT_BRIEF.md`](AGENT_BRIEF.md)
   - [`CURRENT_STATE.md`](CURRENT_STATE.md)
   - [`WayAura_ARCHITECTURE.md`](WayAura_ARCHITECTURE.md)
   - [`WayAura_AGENTS.md`](WayAura_AGENTS.md)
   - [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md)
3. Read the task description Search has produced. If there is no
   task description, stop and ask the Owner; do not improvise.

## Required input from Search

Computer should not start without all of these:

- **Goal** — the single objective of this task.
- **Files to touch** — explicit list. No wildcards.
- **Red zones to avoid** — explicit list. If a red zone is required
  for the task, the Owner must have opened it explicitly.
- **Validation steps** — how the result will be checked.
- **Mode** — active work or curator wrap-up.

If any of these are missing, request them before acting.

## During execution

- Stay strictly inside the listed files and goal.
- Do not refactor, rename, or reorganize anything not in scope.
- Do not modify role, architecture, or licensing documents as a side
  effect of unrelated work.
- Treat audio, wake-word, and other runtime-sensitive subjects as
  red zones unless explicitly opened.
- Never commit secrets, tokens, device identifiers, or local paths.
- Prefer English in all licensing and notice files in this
  repository.

## On finish

1. Run the validation steps.
2. Report:
   - What was changed and where.
   - What was intentionally not changed.
   - Any blockers or unresolved questions.
3. If the task changes the project phase, update
   [`CURRENT_STATE.md`](CURRENT_STATE.md).
4. Add a clean entry to [`CHANGELOG.md`](CHANGELOG.md) under
   `Unreleased`.
5. If genuine open issues remain, add them to
   [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md). Do not use that file for
   ideas.

## When this repository is the work target

Most Computer tasks against `wayaura-context` will be documentation
or structural changes. Runtime, install/start, and configuration
changes belong in `wayaura-core` and must not be performed here.

## Switching out

A new Computer agent must be able to take over using only the task
description from Search and this repository. Leave no partial edits,
no placeholder names, and no contradictions with the handover and
state documents.
