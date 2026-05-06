# wayaura-context

This repository is the **documentation and continuity layer** of the WayAura
project. It is not a runtime code repository.

The runtime assistant code, install and start scripts, service templates, and
operational configuration live in the private repository
[`wayaura-core`](https://github.com/WayAura/wayaura-core).

## Purpose

`wayaura-context` exists so that:

- A new Search agent can rebuild project context without starting from zero.
- A new Computer agent can find scope, rules, and red zones in one place.
- The Owner can rely on documents — not on a single chat session — as the
  project's memory.
- Architecture, role definitions, and decisions survive across sessions,
  accounts, and tools.

## What lives here

- Project overview and official map.
- Architecture and agent-role definitions.
- Onboarding and startup context.
- Search and Computer handover logic.
- Current state, known issues, and changelog.
- Repository strategy and continuity-first guidance.

## What does not live here

- Runtime assistant code.
- Install / start / service scripts.
- Real configuration values or secrets.
- Audio pipeline, wake-word, or any runtime-sensitive module.

Those belong to `wayaura-core` and must not be duplicated here.

## Reading order for a new agent

A new Search or Computer agent should read these files in this order:

1. [`START_HERE.md`](START_HERE.md)
2. [`AGENT_BRIEF.md`](AGENT_BRIEF.md)
3. [`CURRENT_STATE.md`](CURRENT_STATE.md)
4. [`WayAura_OVERVIEW.md`](WayAura_OVERVIEW.md)
5. [`WayAura_OFFICIAL_MAP.md`](WayAura_OFFICIAL_MAP.md)
6. [`WayAura_ARCHITECTURE.md`](WayAura_ARCHITECTURE.md)
7. [`WayAura_AGENTS.md`](WayAura_AGENTS.md)
8. [`WayAura_DEV_ONBOARDING.md`](WayAura_DEV_ONBOARDING.md)
9. [`SEARCH_HANDOVER.md`](SEARCH_HANDOVER.md) — if acting as Search.
10. [`COMPUTER_HANDOVER.md`](COMPUTER_HANDOVER.md) — if acting as Computer.
11. [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md)
12. [`CHANGELOG.md`](CHANGELOG.md)
13. [`WayAura_REPOS_STRATEGY.md`](WayAura_REPOS_STRATEGY.md)

## Roles in one line each

- **Owner** — defines goals, grants permissions, takes final decisions.
- **Search** — replaceable operational role: context, structure, planning,
  continuity.
- **Computer** — replaceable implementation role: scoped execution within
  explicit boundaries and red zones.

Search and Computer are roles, not identities. Any compliant agent can fill
them; the documents in this repository are what makes that switch safe.

## Licensing

This repository is proprietary and confidential. See [`LICENSE`](LICENSE) and
[`PROPRIETARY.md`](PROPRIETARY.md). No open-source license applies.
