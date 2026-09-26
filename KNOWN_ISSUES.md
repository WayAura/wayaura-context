# Known Issues

## Active issues

### Aura v1 (from 2026-09-23)

- **Half-duplex.** While Aura v1 thinks or speaks the microphone is closed (echo from the TV must not
  reach the VAD), so she cannot be interrupted by voice — only by a press (s interrupts and listens) or x.
  Lifted later by echo cancellation or the phone microphone.
- **No wake word yet.** Push-to-talk is the default; hands-free cabin use needs the wake word stage
  (`aura-v1/docs/ACTIVATION.md`). In the stand room there are outside sounds up to −10…−25 dBFS, which is
  why "always listening" is debugging only.
- **Stand microphone hears ordinary speech only up close (hardware).** The USB adapter AB13X with its
  microphone applies its own voice processing: quiet speech loses its consonants (1.2–8 kHz), ordinary
  speech from 1–1.5 m arrives ≈ 20–25 dB below the reliable level. Software gain, the VAD threshold and
  the capture control do not help. Kept by Owner decision; the phone will be the main microphone in the
  car. Details: `aura-v1/FINDINGS.md`.
- **Internet search is slow.** Search answers add the Yandex search time (1–8 s). Weather for the home
  city is prefetched on a press and cached (≈ 1–1.6 s after the phrase); weather for other cities and
  general search still wait for the search.
- **Model 260528 is slower than 250923** with the same agent (≈ 0.25–0.4 s later); kept by Owner decision
  (it is the agent's model). Built-in `web_search` of the Yandex examples does not work on it; own
  search tool used instead.
- **Agent instructions (Owner's area).** The agent's instructions tell it to always start with a fixed
  greeting; every wake-up opens a new session, so the greeting comes before answers. Replacement text is
  prepared (HANDOFF); changes to the agent are made by the Owner in AI Studio only.
- **Cost per hour not verified** against the official tariff; traffic is small with push-to-talk
  (≈ 0.25–0.5 MB per question).
- **Key hygiene.** The Aura v1 API key passed through a chat and should be rotated (Owner, planned).
  A plaintext key file sits in the Pi home directory; left untouched by Owner decision.
- **Stand hotspot link drops** for seconds now and then (SSH timeouts during long work); the panel
  reconnects by itself. Not the laptop VPN (traffic to the Pi goes directly over Wi-Fi).
- **Old commits of `wayaura-core` mention the stand's city** in a test question and a test time zone
  (removed 2026-09-25, history not rewritten; the repository is private).
- Resolved: stand Pi drop-outs (Wi-Fi power save, off since 2026-09-25); voice "not working" on the first
  live test (interface feedback, fixed by push-to-talk); Legacy returning on panel exit (removed); leaving
  the panel stopped Aura (fixed 2026-09-25); a re-plugged microphone left Aura deaf (fixed 2026-09-25);
  weather ≈ 4–5 s (≈ 1–1.6 s since 2026-09-25); HDMI output missing at boot left every reply stuck in
  "speaking" (fixed 2026-09-26: the output is waited for and a dead output is detected).
- **The agent sometimes writes a tool call as text** (`{"get_weather": …}` spoken/printed instead of a real
  call): 5 of 6 in one laptop test run on 2026-09-26, 0 of 8 on the Pi. Not understood yet; watch it.

### Legacy Aura 0.1

- **AB13X USB Audio: start with adapter already plugged** (`known issue
  / not blocking`). Since Audio Policy v2, the default profile for single
  USB + HDMI is `usb_mic_hdmi_out` (capture=USB, playback=HDMI), which
  avoids the mute issue. Self-healing classifies `single_usb_combo` as
  risky and reroutes to `builtin_fallback` on recovery. `AURA_FORCE_USB_PLAYBACK=1`
  is the only path to USB playback and explicitly opted-in. On the current device setup, starting Aura with
  the AB13X USB combo adapter already connected and
  `AURA_FORCE_USB_PLAYBACK=1` set may result in a mute Aura despite
  successful probe. This is a hardware-specific characteristic of this
  adapter, not a code regression. The reliable workaround is: start
  without the USB adapter, then plug it in after Aura has greeted.
  In that mode (start → then plug) the system is stable: capture via
  USB, playback via HDMI, stop-phrases and stop-ack work correctly.
  The default routing since commit `5b2134c` in `wayaura-core` is
  `usb_in_hdmi_out` (USB capture + HDMI playback), which avoids the
  mute issue for most scenarios. `AURA_FORCE_USB_PLAYBACK=1` is an
  explicit opt-in. The Phase 2 self-healing layer will attempt
  reroute to `builtin_fallback` on startup failure, but hardware-level
  mute recovery is not guaranteed. Full detail in `wayaura-core`
  `RELEASE_NOTES.md` and `docs/AUDIO.md`.
- **Audio tuning** (`asound.conf`, detect thresholds, device-specific
  overrides) remains a red zone requiring owner-and-device decisions.
- **Phase 2B deferred:** `Type=notify` systemd watchdog, `WatchdogSec`,
  `sd_notify` heartbeat. Not implemented. Documented in
  `wayaura-core` `system/aura.service`, `docs/SELF_HEALING.md`,
  and `RELEASE_NOTES.md`. Owner approval required before opening.
- **Documentation drift risk** between `wayaura-context` and
  `wayaura-core` if the two repositories are edited independently
  without a consistency pass.
- **Duplicate Aura runtime start** — **mitigated, pending Pi
  validation**. `wayaura-core` commit `e896582` adds a single-instance
  guard so a second concurrent start (systemd or manual) is refused
  with an explicit lock log. Acceptance scenarios A1–A6 in
  `wayaura-core` `TESTSCENARIOS.md` still need owner confirmation on
  the target Pi before this can be closed.
- **`aplay` runtime failure blind spot** — **mitigated, pending Pi
  validation**. `wayaura-core` commit `7c41a88` (Phase 2A.2) surfaces
  non-zero `aplay` runtime exits to the bounded self-healing layer
  and coalesces repeated symptoms in-process; budget unchanged, no
  new env flags. T8 plus T1–T7 regression in `TESTSCENARIOS.md`
  remain pending owner confirmation on the target Pi.

## Closed issues (this session)

- Stop-phrase interrupt during TTS broken — **closed** (commits
  `0efe9ce`, `90a2152` in `wayaura-core`).
- Audible stop-ack absent — **closed** (direct `_play_wav_file` call
  in `_stop_listener`, commit `90a2152`).
- Single-USB full-duplex mute (AB13X playback silent) — **mitigated**
  by `usb_in_hdmi_out` default routing (commit `5b2134c`). Not fully
  closed at hardware level; see active issue above.
- Phase 1 autostart/audio-fallback static validation — **on-device
  validated** by owner this session. Wake, commands, stop, ack all
  confirmed working on the target Raspberry Pi.
- Self-healing / recovery absent — **closed** (Phase 2, commits
  `416bb5e`, `248a97e`). AuraHealthMonitor state machine live;
  startup + runtime recovery bounded by budget; docs/SELF_HEALING.md
  added.

## Ongoing caution points

- Do not treat old session memory as reliable unless reflected in
  current docs.
- Do not move runtime files into this context repository.
- Do not duplicate the full continuity layer into `wayaura-core`.
- Do not open red zones (audio, wake-word, secrets, device
  identifiers, local paths) without explicit Owner approval.
