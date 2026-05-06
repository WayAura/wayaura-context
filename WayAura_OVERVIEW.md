# WayAura Overview

## What WayAura is

WayAura is an in-car voice assistant project targeting a Raspberry Pi as
the primary device. The project's intent is to provide a voice-driven
assistant suited to the in-car context — hands-free, predictable, and
durable across operator and tooling changes.

## Layers

WayAura is organized into four layers:

1. **Runtime assistant layer** — voice input capture, intent handling,
   response output, and the assistant's day-to-day behavior. Lives in
   `wayaura-core`.
2. **Machine and integration layer** — device setup, services, install
   and start scripts, configuration templates. Lives in `wayaura-core`.
3. **Documentation and continuity layer** — onboarding, role definitions,
   handover logic, current state, repository strategy, changelog. Lives
   in this repository, `wayaura-context`.
4. **Governance and safety layer** — role boundaries, red zones, what
   requires Owner approval, what counts as in-scope vs out-of-scope. Lives
   in this repository as part of agent and architecture documents.

## Core principles

- **Continuity first.** No agent and no chat session is irreplaceable.
  Documents carry the project's memory.
- **Roles, not identities.** Search and Computer are operational roles
  filled by whichever compliant agent is available.
- **Explicit scope.** Computer acts only inside an explicitly granted
  scope; Search documents and structures, it does not push runtime
  changes unsupervised.
- **Red zones by default.** Audio, wake-word, and other runtime-sensitive
  subjects are red zones unless the Owner has explicitly opened them for
  a specific task.
- **Proprietary by default.** All code and documentation are the Owner's
  property; no open-source license applies.

## Repository split, in one paragraph

`wayaura-context` is the continuity and navigation system. `wayaura-core`
is the runtime and implementation system. They reference each other but
do not duplicate each other. Architecture, role rules, onboarding, and
handover live here; runtime behavior, install/start, and security-sensitive
material live in `wayaura-core`.

For more detail, see [`WayAura_ARCHITECTURE.md`](WayAura_ARCHITECTURE.md)
and [`WayAura_REPOS_STRATEGY.md`](WayAura_REPOS_STRATEGY.md).
