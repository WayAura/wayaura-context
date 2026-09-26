# NEXT_WORK

This file is the short operational bridge between
[`CURRENT_STATE.md`](CURRENT_STATE.md) and the next agent's first
practical move. It is not a roadmap. It is replaced as the front of
work moves.

## Current practical direction — Aura v1 (from 2026-09-23)

Rules: [`AURA_V1_RULES.md`](AURA_V1_RULES.md). Code: `wayaura-core` `aura-v1/`, `main`, foundation tag
`aura-v1.0`; new work in new branches from `main`. Legacy directions below are paused, not deleted.

Next steps, in order (updated 2026-09-26):

0. **Stage 3 merge** (Owner): the revert PR `revert/wake-word-from-main` (then tag `aura-v1.0`), and the
   stage 3 branches of both repositories. **Phone link decisions** (Owner + app team): how the hotspot gets
   switched on, Android/iOS background listening, the adapter's two connections — `PHONE_LINK.md` §8–11.
   Next engineering step: Aura listening on the hotspot network with TLS and per-phone tokens, then BLE
   pairing and provisioning on the Pi.

1. **Wake word stage** (Owner's direction): "Аура" on the Pi as a second activation mode, switched on
   and off from the panel, off by default — Vosk first, openWakeWord if needed; measure false accepts
   (TV, conversation), misses and delay. Work in its own branch; merge only when reliable.
2. **Greeting** (Owner, in AI Studio): replace the agent's "always say at the very beginning …" line with
   the prepared text so the greeting is not said before answers (text in the stage report / HANDOFF).
3. **Microphone** (Owner's decision pending): the stand microphone hears ordinary speech only up close
   (hardware). Options: a far-field USB microphone without its own voice processing; or wait for the
   phone as the main microphone. Tools for tuning a new microphone are ready (`tools/loopback.py`).
4. Later (not scoped): phone app as remote and microphone over the same control protocol, OBD2 adapter
   over Bluetooth, car speakers and installation.

## Legacy Aura 0.1 — paused direction (before 2026-09-23)

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
