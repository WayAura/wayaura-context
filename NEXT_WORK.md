# NEXT_WORK

This file is the short operational bridge between
[`CURRENT_STATE.md`](CURRENT_STATE.md) and the next agent's first
practical move. It is not a roadmap. It is replaced as the front of
work moves.

## Current practical direction

Audio Policy v2 is complete (`wayaura-core` commit `bc8a3a7`):
- `usb_mic_hdmi_out` is the first-class default profile for single USB + HDMI
- `_is_risky_path()` checks both AURA_AUDIO_PROFILE and AURA_PLAYBACK_PROFILE
- Autostart via systemd is configured and working on the target Pi
- Known limitations documented in `wayaura-core` `README.md` and `docs/AUDIO.md`

Two recent `wayaura-core` changes are mitigated in code but **pending
Pi validation** before they can be called fully closed:

- **Autostart single-instance guard** (`wayaura-core` commit `e896582`).
  Duplicate runtime starts (systemd + manual, or two manual) are
  refused with an explicit lock log. Acceptance scenarios A1–A6 in
  `wayaura-core` `TESTSCENARIOS.md` still need owner-device confirmation.
- **Phase 2A.2 — aplay runtime failures surfaced to self-healing**
  (`wayaura-core` commit `7c41a88`). Non-zero `aplay` exits during
  playback now reach the health monitor and route through bounded
  recovery; repeated runtime symptoms are coalesced. No new env flags,
  no retry-loop expansion. Acceptance T8 plus T1–T7 regression in
  `TESTSCENARIOS.md` are pending.

The next work directions (roadmap, not yet active tasks):

- **PulseAudio/DE integration:** resolve ALSA route conflicts when a desktop
  environment is running. Not scoped — owner decision required.
- **In-car audio tuning:** fine-tune SPEECH_RMS/SILENCE_RMS for car noise
  environment. Red zone — owner-and-device decision.
- **Phase 2B (deferred):** `Type=notify` systemd watchdog, `WatchdogSec`,
  `sd_notify` heartbeat. Documented in `wayaura-core` `system/aura.service`
  and `docs/SELF_HEALING.md`.
- **Pause/resume UX:** pause/resume for long TTS responses.

None of these are active tasks. Owner approval required before opening.

## What the next agent should do first

1. Enter through this repository. Read `START_HERE.md`, `AGENT_BRIEF.md`,
   `CURRENT_STATE.md`.
2. Use `REPO_MAP.md` to decide which repository the task belongs to.
3. If the task is documentation, role, onboarding, or continuity — stay here.
4. If the task is runtime, install/start, services, or configuration — move
   to `wayaura-core` and follow its own docs there.
5. For any audio or autostart change, confirm scope with the Owner first;
   treat audio as a red zone by default.

## Current known limitations (not blocking)

- **Single USB without headset:** playback may be silent if USB playback is
  selected. Default `usb_mic_hdmi_out` (HDMI output) prevents this.
  `AURA_FORCE_USB_PLAYBACK=1` is the explicit opt-in for USB playback.
- **PulseAudio/GUI:** switching audio devices via system GUI during Aura runtime
  can capture the ALSA route. Use headless environment for reliable operation.
- **Phase 2B deferred:** full systemd watchdog not yet implemented.
- **Duplicate-start guard:** mitigated in `wayaura-core` (`e896582`),
  not yet confirmed on the target Pi (A1–A6 pending).
- **aplay runtime blind spot:** mitigated in `wayaura-core` (`7c41a88`),
  not yet confirmed on the target Pi (T8 + T1–T7 regression pending).

## What not to touch without Owner approval

- Audio pipeline, wake-word, and any other runtime-sensitive subject.
- Real configuration values, secrets, device identifiers, local paths.
- Licensing and proprietary notices.
- The cross-repository contract: architecture split, role definitions, handover rules.
