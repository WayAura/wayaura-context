# WayAura Repos Strategy

This document explains the repository split for WayAura, why it exists,
and how the two repositories relate to each other in practice.

## The split

WayAura is split into two repositories:

### `wayaura-context` — this repository

- Public-facing.
- Documentation, continuity, and navigation.
- Project overview and official map; architecture; agent role
  definitions; onboarding; Search and Computer handover; current
  state; known issues; changelog; repository strategy; licensing.
- No runtime code. No install/start scripts. No real configuration
  values. No secrets, device identifiers, or local paths.

### `wayaura-core` — separate, private

- Private.
- Runtime and implementation.
- Assistant code, install and start scripts, service templates,
  configuration templates, internal runtime utilities, anything
  security- or device-sensitive.
- No general onboarding, no role definitions, no cross-project
  strategy. Those live in `wayaura-context`.

## Why the split exists

- **Continuity must be readable without exposing runtime.** Any
  agent or operator needs to be able to learn the project's shape
  without being granted access to runtime systems first.
- **Different change cadences.** Documentation evolves on a
  decision cadence; runtime evolves on an engineering cadence.
  A single repository would mix the two and make both noisier.
- **Replaceability.** Search and Computer are replaceable roles.
  The continuity repository is the artifact that makes that
  replacement safe; keeping it separate from runtime is what
  makes it possible to share with a new operator quickly.
- **Visibility separation.** Public continuity material and
  private runtime material have different audiences and different
  risk profiles. They should not share a publication boundary.

## State of the split

The split is now physically realized. Both repositories exist as
distinct GitHub repositories under the WayAura organization. This
strategy document is no longer describing an intention; it is
describing the current reality.

## How the two repositories must relate

- `wayaura-context` references `wayaura-core` by name and URL when
  the topic is runtime, install/start, services, or configuration.
- `wayaura-context` does not duplicate runtime documentation
  beyond high-level role and split references.
- `wayaura-core` is expected to point back to `wayaura-context`
  for onboarding, role definitions, handover, and continuity.
- Cross-repository changes (architecture, role definitions,
  licensing) require Owner approval and a logged entry in
  [`CHANGELOG.md`](CHANGELOG.md).

## What this strategy is not

- It is not an open-source plan. The project is proprietary; see
  [`LICENSE`](LICENSE) and [`PROPRIETARY.md`](PROPRIETARY.md).
- It is not a deployment plan. Deployment lives in `wayaura-core`.
- It is not a roadmap. Phase information lives in
  [`CURRENT_STATE.md`](CURRENT_STATE.md).
