# WayAura Official Map

This document is the canonical map of WayAura's parts and where they live.
If a document elsewhere disagrees with this map, this map is the reference.

## Layers

- **Runtime assistant layer** — voice capture, intent handling, response.
  Location: `wayaura-core`.
- **Machine / integration layer** — install, start, service templates,
  configuration templates. Location: `wayaura-core`.
- **Documentation / continuity layer** — onboarding, roles, handover,
  state, strategy. Location: `wayaura-context` (this repository).
- **Governance / safety layer** — role boundaries, red zones, Owner
  approval rules. Location: `wayaura-context`, primarily in
  [`AGENT_BRIEF.md`](AGENT_BRIEF.md), [`WayAura_AGENTS.md`](WayAura_AGENTS.md),
  and [`WayAura_ARCHITECTURE.md`](WayAura_ARCHITECTURE.md).

## Repositories

- [`wayaura-context`](https://github.com/WayAura/wayaura-context) — public.
  Documentation and continuity. This repository.
- [`wayaura-core`](https://github.com/WayAura/wayaura-core) — private.
  Runtime, install/start, configuration templates, security-sensitive
  material.

## Roles

- **Owner** — sole authority on goals, scope, permissions.
- **Search** — replaceable operational role: context, structure, planning,
  continuity.
- **Computer** — replaceable implementation role: scoped execution.

## Modes

- **Active work** — Search plans and Computer executes inside explicit
  scopes granted by the Owner.
- **Curator wrap-up** — Owner-triggered. Operators stop pushing further
  changes and instead summarize the current technical stage cleanly so
  that the next operator can continue without loss of context.

The map exists to keep these layers, repositories, roles, and modes
distinct. Any future addition must be placed explicitly into one of them.
