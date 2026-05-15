# Known Issues

## Active issues

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
