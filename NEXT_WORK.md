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

## Bounded next step: on-device Phase 1 validation

Phase 1 of `wayaura-core` (reliable autostart + audio fallback) is
implemented in core and statically validated in sandbox. It is not
yet confirmed on the target device. The next bounded move is
on-device validation, not more code first.

Commits on `wayaura-core` `main` covering Phase 1 (range
`0ef6cab..3be4fbc`):

- `a967b64` — `aura.service` waits for udev settle before
  `start.sh` (`systemd-udev-settle.service` plus
  `ExecStartPre=udevadm settle --timeout=10`).
- `76c53db` — `scripts/audio_detect.sh` probes selected playback by
  a short `aplay` open-probe, records `AURA_PLAYBACK_PROBE`, and
  falls back to bcm2835 / HDMI on failure.
- `f7e70ec` — `start.sh` waits up to
  `AURA_AUDIO_STARTUP_WAIT_SEC` (default 8s) and retries audio
  detection if the first pass lands on an HDMI fallback while USB
  is still arriving; exports the chosen profile via
  `AURA_AUDIO_PROFILE` (`usb_combo`,
  `dual_mic_mix_usb_playback`, `usb_capture_hdmi_playback`, plus
  degraded variants) and also exports `PA_ALSA_PLUGHW=1`.
- `3be4fbc` — docs (`README.md`, `docs/AUDIO.md`, `CONFIG.md`,
  `.env.config.example`) updated for autostart audio wait, profile
  enum, and playback probe.

Static validation already performed in `wayaura-core` sandbox:

- `bash -n start.sh` and `bash -n scripts/audio_detect.sh` pass.
- `systemd-analyze verify system/aura.service` emits only the
  expected `__WORKDIR__` install-template placeholder warning.
- `bash scripts/audio_detect.sh start` on a host without USB
  produces a safe empty / degraded `runtime/audio.env` with
  `AURA_PLAYBACK_PROBE=unknown`.

What still needs to happen on the actual Raspberry Pi, in this
order, before Phase 1 is described as solved:

1. Cold boot without USB attached. Expected: the service starts
   into a predictable degraded / fallback profile with no manual
   workaround. Capture the chosen `AURA_AUDIO_PROFILE` and any
   reason logged by `start.sh` / `scripts/audio_detect.sh`.
2. Cold boot with a stable USB mic adapter attached. Expected: a
   working `AURA_AUDIO_PROFILE` (e.g. `usb_combo` or
   `dual_mic_mix_usb_playback` / `usb_capture_hdmi_playback` as
   appropriate), and logs showing the chosen profile and the
   reason it was selected after the udev-settle wait.
3. Reproduce the problematic MicA / AB13X playback-failure case.
   Expected: `AURA_PLAYBACK_PROBE=fail` is recorded and playback
   falls back to HDMI where appropriate, without manual editing of
   `runtime/audio.env`.
4. Confirm `PA_ALSA_PLUGHW=1` is in effect for the running
   service, and capture RMS / `MIC_DEBUG` output if mic level is
   in doubt.

Until these on-device checks are in, do not extend autostart or
audio-fallback code further in `wayaura-core` and do not claim the
single-USB or HDMI-fallback symptom is resolved. The previously
recorded single-USB full-duplex proof step still stands as a
separate, narrower investigation; see the section below.

`POSTCHANGECHECKLIST.md` does not exist in `wayaura-core` yet, so
on-device results should be recorded against
`TESTSCENARIOS.md` and `SECURITY.md` until a dedicated checklist
file is introduced under a separate task.

## Earlier bounded step: single-USB audio proof

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
