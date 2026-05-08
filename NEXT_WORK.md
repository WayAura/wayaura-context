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

## Bounded next step: single-USB audio proof

The current open audio incident (single USB card carrying both
microphone and headphones / AUX — Aura starts but neither hears nor
is heard; adding a second USB adapter restores both directions) is
recorded in [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md). The primary
hypothesis is that this is likely a device / ALSA full-duplex
limitation of the single USB card rather than an Aura
routing-selection bug. This needs on-device proof before any code or
config change.

The next bounded step is, on the actual target device with only the
problematic single USB card attached:

1. Identify the USB card index (for example via `arecord -l` and
   `aplay -l`).
2. Run a simultaneous capture + playback test on that one card —
   for example, in two terminals on the device, something like
   `arecord -D hw:N,0 -f S16_LE -r 16000 -c 1 /tmp/cap.wav` in one
   and `aplay -D plughw:N,0 /tmp/cap.wav` (or any safe known WAV)
   in the other, run at the same time. These commands are a
   proof-plan example, not a completed check.
3. Record whether full-duplex on the same card actually works
   outside of Aura, and capture any ALSA / driver errors.

Only after that proof is in:

- If full-duplex on the single card fails outside Aura too, the
  fix path is hardware (the second USB adapter requirement is
  documented as a constraint) or, if pursued, a narrow
  `dmix` + `dsnoop` `/etc/asound.conf` profile scoped to the
  single-USB case in `wayaura-core`.
- If full-duplex on the single card works outside Aura, the
  investigation moves into `wayaura-core` (`start.sh`,
  `scripts/audio_detect.sh`, `runtime/audio.env` merge order, and
  any `PA_ALSA_*` assumptions).

Until this proof exists, do not change audio routing code or
`asound.conf` shape in `wayaura-core`. Audio remains a red zone and
requires explicit Owner approval.

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

## Naming convention for the runtime package

When referring to the current owner-provided runtime source package
in reports, handover notes, or task framing, use the name
`aura-0.1` — for example, the `aura-0.1 archive` or the
`aura-0.1 runtime package`. Do not introduce new archive names
without an explicit Owner decision. This is a naming convention,
not an automatic claim that product logic has advanced beyond the
current runtime base.
