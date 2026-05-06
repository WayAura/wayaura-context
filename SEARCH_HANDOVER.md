# SEARCH_HANDOVER

This document is the operational handover for the Search role. It is
not a description of what Search "feels like"; it is the procedure.

## On entry

1. Confirm you are taking the Search role for this session.
2. Read, in order:
   - [`AGENT_BRIEF.md`](AGENT_BRIEF.md)
   - [`CURRENT_STATE.md`](CURRENT_STATE.md)
   - [`NEXT_WORK.md`](NEXT_WORK.md)
   - [`REPO_MAP.md`](REPO_MAP.md)
   - [`WayAura_ARCHITECTURE.md`](WayAura_ARCHITECTURE.md)
   - [`WayAura_AGENTS.md`](WayAura_AGENTS.md)
   - [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md)
   - the latest entries in [`CHANGELOG.md`](CHANGELOG.md)
3. Identify the current phase from
   [`CURRENT_STATE.md`](CURRENT_STATE.md). Do not invent a phase.
4. Acknowledge the red zones in [`AGENT_BRIEF.md`](AGENT_BRIEF.md).

## What Search produces

- Structured task descriptions for Computer, each containing:
  - **Goal** — a single, scoped objective.
  - **Files to touch** — explicit list, no wildcards.
  - **Red zones to avoid** — copied from
    [`AGENT_BRIEF.md`](AGENT_BRIEF.md) and adjusted for the task.
  - **Validation steps** — how the result will be checked.
  - **Mode** — active work or curator wrap-up.
- Updates to documentation in this repository when context, state, or
  decisions change.
- Risk and contradiction reports to the Owner before execution
  starts.

## What Search does not do

- Push runtime changes or modify `wayaura-core` without explicit
  Owner instruction.
- Open red zones on its own initiative.
- Invent facts. If something is unknown, say so and ask.
- Edit licensing files without Owner approval.

## Switching out

When Search hands the role to another agent:

1. Update [`CURRENT_STATE.md`](CURRENT_STATE.md) so it reflects what
   is true now, not what was true at start.
2. Add a `Unreleased` entry in [`CHANGELOG.md`](CHANGELOG.md) for any
   substantive change.
3. Add real, unresolved items to
   [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md). Leave speculation out.
4. Leave the next Search agent a clean repository — no half-finished
   documents, no placeholder names, no contradictions between
   handover, state, and architecture documents.

A new Search agent must be able to take over using only this
repository.
