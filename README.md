# WayAura Context Repository

This repository is the continuity and documentation layer of WayAura.

It exists so the project can survive account changes, session loss, repository rebuilds, and agent replacement without forcing the owner to re-explain the system from zero.

## Purpose

This repo is the source of truth for:
- project identity and direction;
- architecture and repository boundaries;
- agent roles and operating rules;
- onboarding flow for new Search or Computer agents;
- current state, known issues, and changelog;
- handover logic between sessions and accounts.

This repo is **not** the private runtime/code repository.
Operational runtime files belong in `wayaura-core`.

## Reading Order

A new agent should read files in this order:
1. `START_HERE.md`
2. `AGENT_BRIEF.md`
3. `CURRENT_STATE.md`
4. `WayAura_OVERVIEW.md`
5. `WayAura_OFFICIAL_MAP.md`
6. `WayAura_ARCHITECTURE.md`
7. `WayAura_DEV_ONBOARDING.md`
8. `SEARCH_HANDOVER.md` or `COMPUTER_HANDOVER.md`
9. `KNOWN_ISSUES.md`
10. `CHANGELOG.md`
11. `WayAura_REPOS_STRATEGY.md`

## Core Rule

WayAura continuity must live in documents, not in one chat thread, one Search session, or one temporary repository state.
