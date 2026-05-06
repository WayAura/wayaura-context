# START_HERE

You are entering the WayAura context repository. Read this file fully before
doing anything else.

## What this repository is

`wayaura-context` is the documentation and continuity layer of WayAura.
It is the project's memory. It is not a place to run, install, or change
runtime code.

Runtime code lives in the private repository
[`wayaura-core`](https://github.com/WayAura/wayaura-core).

## What you must do first

1. Read [`AGENT_BRIEF.md`](AGENT_BRIEF.md) — what WayAura is, the roles, the
   rules, and the red zones.
2. Read [`CURRENT_STATE.md`](CURRENT_STATE.md) — the project's current phase
   and what is in progress right now.
3. Read [`NEXT_WORK.md`](NEXT_WORK.md) — the current practical direction and
   what the next agent should do first.
4. Read [`REPO_MAP.md`](REPO_MAP.md) — which repository a given task belongs
   to.
5. Identify which role you are filling: Search, Computer, or Owner-directed
   support.
6. Read the matching handover:
   - Search → [`SEARCH_HANDOVER.md`](SEARCH_HANDOVER.md)
   - Computer → [`COMPUTER_HANDOVER.md`](COMPUTER_HANDOVER.md)
7. Read [`WayAura_ARCHITECTURE.md`](WayAura_ARCHITECTURE.md) and
   [`WayAura_AGENTS.md`](WayAura_AGENTS.md) before proposing changes.

## Hard rules

- Do not invent project memory. If a fact is not in these documents or in
  `wayaura-core`, treat it as unknown until the Owner confirms it.
- Do not duplicate runtime documentation here. Link to `wayaura-core` instead.
- Do not touch audio, wake-word, or any runtime-sensitive area as a side
  effect. Those are red zones.
- Do not commit secrets, tokens, device identifiers, or local paths.
- Do not introduce open-source licenses. The project is proprietary.

## When you are done with a unit of work

- Update [`CURRENT_STATE.md`](CURRENT_STATE.md) if the project phase changed.
- Add a clean entry to [`CHANGELOG.md`](CHANGELOG.md) under `Unreleased`.
- Add anything genuinely unresolved to [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md);
  do not use it as a brainstorm dump.
