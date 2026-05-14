# NEXT_WORK

This file is the short operational bridge between
[`CURRENT_STATE.md`](CURRENT_STATE.md) and the next agent's first
practical move. It is not a roadmap. It is replaced as the front of
work moves.

## Current practical direction

Phase 2 — bounded self-healing layer — has been implemented in
`wayaura-core` (commits `416bb5e`, `248a97e`). A `AuraHealthMonitor`
state machine is live; startup and runtime audio recovery is bounded
by `AURA_SELF_HEAL_MAX_ATTEMPTS`; basic systemd restart policy is in
place; full documentation is in `docs/SELF_HEALING.md`.

The next work directions (roadmap, not yet active tasks):

- **Phase 2B (deferred):** `Type=notify` systemd watchdog, `WatchdogSec`,
  `sd_notify` heartbeat from `assistant.py` main loop. Explicitly
  deferred and documented in `RELEASE_NOTES.md`, `system/aura.service`,
  and `docs/SELF_HEALING.md`. Owner approval required before opening.
- **Autostart to ideal:** cold-boot always produces a correct Aura 0.1
  without race conditions or manual workarounds.
- **Pause/resume and UX polish:** pause/resume for long TTS responses,
  further adaptive improvements, cosmetic tuning.

None of these are active tasks. Owner approval required before any of
them opens as a task in `wayaura-core`.

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
5. For any audio or autostart change, confirm scope with the Owner
   first; treat audio as a red zone by default.

## Current known limitation (not blocking)

Starting Aura with the AB13X USB adapter already plugged and
`AURA_FORCE_USB_PLAYBACK=1` may produce a mute Aura. This is a
hardware characteristic of the adapter, documented as `known issue /
not blocking` in `wayaura-core` `RELEASE_NOTES.md` and `docs/AUDIO.md`.
Reliable workaround: start without USB adapter, plug after greeting.

The self-healing layer (Phase 2) will attempt recovery in this case
(reroute to `builtin_fallback`), but success depends on hardware state
at startup.

## What not to touch without Owner approval

- Audio pipeline, wake-word, and any other runtime-sensitive subject.
- Real configuration values, secrets, device identifiers, local paths.
- Licensing and proprietary notices.
- The cross-repository contract: architecture split, role definitions,
  handover rules.
- The Aura 0.1 runtime files in `wayaura-core` beyond the scope of
  the granted task.

For the `aura-0.1` runtime package naming convention, see
[`CURRENT_STATE.md`](CURRENT_STATE.md). Do not introduce new archive
names without explicit Owner approval.
