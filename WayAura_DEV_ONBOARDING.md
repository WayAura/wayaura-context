# WayAura Developer Onboarding

This document is the entry path for any new operator joining WayAura —
human or agent, Search or Computer.

## Entry path

1. Read [`START_HERE.md`](START_HERE.md).
2. Read [`AGENT_BRIEF.md`](AGENT_BRIEF.md) and
   [`CURRENT_STATE.md`](CURRENT_STATE.md).
3. Read [`NEXT_WORK.md`](NEXT_WORK.md) and [`REPO_MAP.md`](REPO_MAP.md).
4. Read [`WayAura_OVERVIEW.md`](WayAura_OVERVIEW.md),
   [`WayAura_OFFICIAL_MAP.md`](WayAura_OFFICIAL_MAP.md), and
   [`WayAura_ARCHITECTURE.md`](WayAura_ARCHITECTURE.md).
5. Read [`WayAura_AGENTS.md`](WayAura_AGENTS.md).
6. Read the handover for your role:
   - Search → [`SEARCH_HANDOVER.md`](SEARCH_HANDOVER.md).
   - Computer → [`COMPUTER_HANDOVER.md`](COMPUTER_HANDOVER.md).
7. Read [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md) and the most recent entries
   of [`CHANGELOG.md`](CHANGELOG.md).
8. Only then move to the runtime repository
   [`wayaura-core`](https://github.com/WayAura/wayaura-core), and only
   if your task requires it.

## Goal of onboarding

The goal is not just to learn what WayAura is. The goal is to learn
what has already been decided, what must stay stable, what is currently
in progress, and what the next practical move is — without having to
ask the Owner to reconstruct it.

## What you should be able to answer after onboarding

- What is WayAura, in one paragraph.
- Where runtime code lives, and where documentation lives.
- What Owner, Search, and Computer are responsible for.
- What the current red zones are.
- What the project's current phase is.
- Where to record a change, an issue, and a state transition.

## Current practical move

The repository split is physically realized, and `wayaura-core` now
holds an Aura 0.1 runtime working base:

- This repository, `wayaura-context`, is the continuity layer.
- `wayaura-core` is the private runtime layer with imported Aura 0.1
  runtime, install/start scripts, and operational docs.

After onboarding, the practical move depends on the Owner's instructions,
[`CURRENT_STATE.md`](CURRENT_STATE.md), and
[`NEXT_WORK.md`](NEXT_WORK.md). Do not assume work that is not stated
there.

## What to do when you finish a task

- Reflect any phase change in [`CURRENT_STATE.md`](CURRENT_STATE.md).
- Add a clean entry to [`CHANGELOG.md`](CHANGELOG.md) under
  `Unreleased`.
- Add genuine open issues to [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md);
  leave brainstorming out of it.
