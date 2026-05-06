# NEXT_WORK

This file is the short operational bridge between
[`CURRENT_STATE.md`](CURRENT_STATE.md) and the next agent's first
practical move. It is not a roadmap. It is replaced as the front of
work moves.

## Current practical direction

`wayaura-core` now has a real Aura 0.1 runtime working base. The
present goal is to validate that base on the actual target device
without touching anything outside the granted scope, and to keep
`wayaura-context` aligned as the entry point.

## What is missing in `wayaura-core`

- Real install/start smoke on the target Raspberry Pi / device. So
  far only static smoke has passed (`bash -n`, Python `ast.parse`,
  JSON `json.load`).
- Confirmed, device-specific audio tuning values
  (`system/asound.conf`, audio detect thresholds). These are red-zone
  decisions and require Owner / device input.

## What the next agent should do first

1. Enter through this repository. Read
   [`START_HERE.md`](START_HERE.md), [`AGENT_BRIEF.md`](AGENT_BRIEF.md),
   and [`CURRENT_STATE.md`](CURRENT_STATE.md).
2. Use [`REPO_MAP.md`](REPO_MAP.md) to decide which repository the
   task belongs to.
3. If the task is documentation, role, onboarding, or continuity —
   stay here.
4. If the task is runtime, install/start, services, or configuration —
   move to `wayaura-core` and follow its own docs there.
5. If the task is real on-device install/start smoke or audio tuning,
   confirm scope with the Owner first; treat audio as red zone by
   default.

## What not to touch without Owner approval

- Audio pipeline, wake-word, and any other runtime-sensitive
  subject.
- Real configuration values, secrets, device identifiers, local
  paths.
- Licensing and proprietary notices.
- The cross-repository contract: architecture split, role
  definitions, handover rules.
- The Aura 0.1 runtime files in `wayaura-core` beyond the scope of
  the granted task.
