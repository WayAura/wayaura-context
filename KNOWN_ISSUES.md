# Known Issues

## Active issues

- Real install/start smoke on the target Raspberry Pi / device has not
  yet been performed. Only static smoke (`bash -n`, `ast.parse`,
  `json.load`) has been run inside `wayaura-core`. Owner / device
  validation is pending.
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
- Minor documentation / comment drift in `wayaura-core`: references
  to `PA_ALSA_PLUGHW=1` being exported by `start.sh` exist in
  comments / docs, but `start.sh` does not actually export it. Not
  related to the single-USB symptom above and not changed in this
  diagnostic pass; recorded here so it is not lost.
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
