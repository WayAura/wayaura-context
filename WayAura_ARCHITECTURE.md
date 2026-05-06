# WayAura Architecture

## Two-repository architecture

WayAura is deliberately split into two repositories with different roles
and different visibility.

### `wayaura-context` — this repository

- Public-facing documentation and continuity.
- Contains: project overview, official map, architecture (this document),
  agent role definitions, onboarding, Search and Computer handover logic,
  current state, known issues, changelog, repository strategy, licensing
  and proprietary notice.
- Contains no runtime code, no install/start scripts, no real
  configuration values, no secrets, no device identifiers, no local paths.

### `wayaura-core` — separate, private

- Private runtime and implementation repository.
- Contains: assistant code, install and start scripts, service and
  configuration templates, internal runtime utilities, anything
  security- or device-sensitive.
- Contains no general onboarding, no role definitions, and no
  cross-project strategy. Those live here in `wayaura-context`.

## Why the split exists

- **Visibility separation.** Public continuity documents must be readable
  without exposing runtime, configuration, or security material.
- **Continuity stability.** The continuity repository changes on a
  documentation cadence; the runtime repository changes on an
  engineering cadence. Splitting them keeps each clean.
- **Replaceability.** A new Search or Computer agent can be brought up
  to working context using only `wayaura-context`, without needing
  access to runtime systems first.

## How the two repositories reference each other

- `wayaura-context` references `wayaura-core` by name and by URL when
  the topic is runtime, install/start, services, or configuration.
- `wayaura-context` does not duplicate `wayaura-core`'s runtime
  documentation. High-level role and split references only.
- `wayaura-core` is expected to point back to `wayaura-context` for
  onboarding, role definitions, handover, and continuity.

## Boundaries enforced by this architecture

- A change that affects runtime behavior, install steps, or
  configuration goes to `wayaura-core`. It does not get described in
  speculative detail here.
- A change that affects role definitions, handover rules, onboarding,
  current state, repository strategy, or continuity goes here. It
  does not get embedded inside `wayaura-core`.
- Audio, wake-word, and any runtime-sensitive subject are red zones.
  This repository never opens them by default; only the Owner can
  authorize a scoped change in `wayaura-core`.

## Modes of work

- **Active mode.** Search plans, Computer executes inside an explicit
  Owner-approved scope. Scope creep is not allowed.
- **Curator wrap-up mode.** Triggered by the Owner. Operators stop
  pushing changes and instead produce a clean summary of the current
  technical stage and what the next operator must know.

This architecture is a contract between the two repositories. It is
changed only deliberately, with an entry in
[`CHANGELOG.md`](CHANGELOG.md) and Owner approval.
