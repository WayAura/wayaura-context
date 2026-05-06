# AGENT_BRIEF

## What WayAura is

WayAura is an in-car voice assistant project built around a Raspberry Pi as
the primary target device. It combines:

- a runtime assistant layer (voice input, intent handling, response output),
- a machine and integration layer (device setup, services, configuration),
- a documentation and continuity layer (this repository),
- a governance and safety layer (roles, red zones, handover rules).

The project is split across two repositories:

- [`wayaura-context`](https://github.com/WayAura/wayaura-context) — this
  repository. Documentation, continuity, onboarding, role definitions,
  state, strategy.
- [`wayaura-core`](https://github.com/WayAura/wayaura-core) — private.
  Runtime code, install/start scripts, service templates, configuration
  templates, security-sensitive material.

## The main idea

Project memory must live in documents, not in one chat session. Any agent —
Search or Computer — must be replaceable without resetting the project's
context. The repositories together make that replacement safe.

## Roles

- **Owner** — sole authority on goals, scope, permissions, and final
  decisions. Grants and revokes access for Search and Computer.
- **Search** — replaceable operational role focused on understanding,
  structuring, planning, and writing context. Search does not push
  unreviewed runtime changes.
- **Computer** — replaceable implementation role focused on scoped
  execution: making the changes that have been planned, within explicit
  boundaries.

Search and Computer are roles, not permanent identities. If a Search or
Computer instance is replaced, the new instance must be able to continue
from these documents alone.

For full role definitions, see [`WayAura_AGENTS.md`](WayAura_AGENTS.md).

## Rules

- Documents are the source of truth. If a document and a memory disagree,
  the document wins until the Owner says otherwise.
- Stay in role. Search plans and documents; Computer executes inside an
  explicit task scope. Do not silently expand scope.
- No runtime changes from this repository. Runtime changes belong in
  `wayaura-core`.
- No open-source licenses. The project is proprietary; see
  [`LICENSE`](LICENSE) and [`PROPRIETARY.md`](PROPRIETARY.md).
- All licensing and notice files in this repository are English only.

## Red zones

Red zones are areas where unscoped or speculative changes are not allowed.
For this repository, the red zones are:

- Audio pipeline, wake-word, and any other runtime-sensitive subject —
  even at the documentation level, do not invent behavior; only describe
  what is confirmed.
- Configuration, secrets, device identifiers, and local paths — never
  commit real values; refer to `wayaura-core` templates.
- Licensing and proprietary notices — do not edit without Owner approval.
- Cross-repository contracts (architecture split, role definitions,
  handover rules) — change only through a deliberate, logged update.

If you are unsure whether something is a red zone, treat it as one and
ask the Owner.
