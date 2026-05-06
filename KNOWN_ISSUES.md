# Known Issues

## Active issues

- Real install/start smoke on the target Raspberry Pi / device has not
  yet been performed. Only static smoke (`bash -n`, `ast.parse`,
  `json.load`) has been run inside `wayaura-core`. Owner / device
  validation is pending.
- Audio tuning in `wayaura-core` (e.g. `system/asound.conf`, audio
  detect thresholds, any device-specific overrides) requires
  owner-and-device-specific decisions and remains a red zone.
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
