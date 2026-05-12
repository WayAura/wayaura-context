# Known Issues

## Active issues

- Real install/start smoke on the target Raspberry Pi / device has not
  yet been performed. Only static smoke (`bash -n`, `ast.parse`,
  `json.load`) has been run inside `wayaura-core`. Owner / device
  validation is pending.
- Reliable autostart and audio fallback (Phase 1 in `wayaura-core`)
  are implemented in core and statically validated, but not yet
  validated on the target Raspberry Pi. Implementation covers a
  udev-settle wait in `system/aura.service`, an audio-startup wait
  with retry in `start.sh`, a profile-enum export via
  `AURA_AUDIO_PROFILE`, a `PA_ALSA_PLUGHW=1` export, and a playback
  open-probe with HDMI / bcm2835 fallback recorded as
  `AURA_PLAYBACK_PROBE` in `scripts/audio_detect.sh`. Static
  validation in sandbox: `bash -n` on both shell entry points
  passes; `systemd-analyze verify system/aura.service` emits only
  the expected `__WORKDIR__` install-template warning;
  `scripts/audio_detect.sh start` on a host without USB produces a
  safe degraded `runtime/audio.env` with
  `AURA_PLAYBACK_PROBE=unknown`. Remaining: on-device cold-boot
  cases (no USB, stable USB mic, MicA / AB13X playback-failure),
  confirmation that `PA_ALSA_PLUGHW=1` is in effect for the
  running service, and RMS / `MIC_DEBUG` capture if needed. Until
  those pass, the symptom is mitigated but not closed.
- Audio tuning in `wayaura-core` (e.g. `system/asound.conf`, audio
  detect thresholds, any device-specific overrides) requires
  owner-and-device-specific decisions and remains a red zone.
- Single-USB audio card scenario: with exactly one USB audio card
  carrying both microphone and headphones / AUX, Aura starts but
  neither hears nor is heard. Adding a second USB audio adapter (with
  anything plugged) restores both directions; swapping which physical
  attachment goes to which card still works. The
  `scripts/audio_detect.sh` priority 0 single-USB combo path selects
  `hw:N,0` for capture and `plughw:N,0` for playback on the same
  card, and `install.sh` removes `/etc/asound.conf` for the 1-USB
  scenario, so no `dmix`/`dsnoop` profile is in place. Primary
  hypothesis: this is likely a device / ALSA full-duplex limitation
  of the single USB card rather than an Aura routing-selection bug,
  but this requires on-device proof (simultaneous `arecord` + `aplay`
  on the same card) before any code or config change. See
  [`NEXT_WORK.md`](NEXT_WORK.md) for the bounded next step.
- Earlier-recorded `PA_ALSA_PLUGHW=1` documentation drift in
  `wayaura-core` (docs mentioned it as exported by `start.sh` while
  the code did not) has been addressed by Phase 1: `start.sh` now
  exports `PA_ALSA_PLUGHW=1`. On-device confirmation that this is
  actually in effect for the running service is still part of the
  pending Phase 1 validation above.
- Documentation drift between `wayaura-context` and `wayaura-core`
  remains a risk if the two repositories are edited independently
  without a consistency pass.

## Ongoing caution points

- Do not treat old session memory as reliable unless reflected in
  current docs.
- Do not move runtime files into this context repository.
- Do not duplicate the full continuity layer into `wayaura-core`.
- Do not open red zones (audio, wake-word, secrets, device
  identifiers, local paths) without explicit Owner approval.
