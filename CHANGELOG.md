# CHANGELOG

All notable changes to `wayaura-context` are recorded here in
human-readable form. The most recent changes go on top under
`Unreleased`. When a release is cut, the Unreleased section is renamed
to that release and a new Unreleased section is started above it.

## Unreleased


### Added (Phase 2A.1 — usb_combo risky path + stop telemetry — 2026-05-14)

- `health.py`: `_is_risky_path()` — class-based trigger for cold-start mute on
  single USB combo (`AURA_AUDIO_PROFILE=usb_combo` + USB hw-path prefix +
  `IS_FALLBACK!=1`). No hardcoded device names. Commit `70165a7`.
- `health.py`: `_find_hdmi_device()` — parses `aplay -l`, returns first HDMI/bcm
  `hw:N,M` or `""`. Used in recovery to rewrite `AURA_PLAYBACK_PRIMARY`.
- `health.py`: `startup_audio_check()` now enters `_recover()` when risky path is
  detected, even if standard validation passes. Budget unchanged (shared 2-attempt
  counter from Phase 2). No new env flags.
- `assistant.py`: `[STOP] stop propagated in <N>ms` diagnostic timing log. Stop
  architecture confirmed already optimal (50 ms polling, flag before ack).
- `docs/SELF_HEALING.md`: new "usb_combo risky path detection" section with trigger
  conditions, recovery sequence, and example logs.
- `RELEASE_NOTES.md`: Phase 2A.1 subsection added.

### Added (Phase 2 — bounded self-healing layer — 2026-05-14)

- `health.py` (`AuraHealthMonitor`) added to `wayaura-core`: state machine
  HEALTHY → DEGRADED → RECOVERING → FAILED_HARD. Commits `416bb5e`,
  `248a97e`.
- Startup audio validation window (15s default, `AURA_SELF_HEAL_STARTUP_WINDOW_SEC`).
- Runtime audio recovery on playback exception (budget shared with startup,
  `AURA_SELF_HEAL_MAX_ATTEMPTS=2` default).
- AB13X / single USB class-based reroute to `builtin_fallback` on recovery
  (profile string check, no hardcoded device names).
- `[HEAL]` / `[HEALTH]` / `[RECOVERY]` log prefixes on all health events.
- `AURA_SELF_HEALING=0` makes all public methods no-ops.
- Basic systemd restart policy confirmed: `Restart=on-failure`, `RestartSec=5`,
  `StartLimitBurst=3`, `StartLimitIntervalSec=60` in `system/aura.service`.
- `docs/SELF_HEALING.md` added to `wayaura-core` with state diagram, recovery
  order, env flags table, example logs, explicit boundaries, and Phase 2B note.
- `RELEASE_NOTES.md` Phase 2 section added to `wayaura-core`.
- `docs/AUDIO.md` links to `docs/SELF_HEALING.md`.
- `CURRENT_STATE.md`, `KNOWN_ISSUES.md`, `NEXT_WORK.md` updated in
  `wayaura-context` to reflect Phase 2 completion.
- Phase 2B (Type=notify watchdog) explicitly deferred and documented.

### Added (runtime stabilization + context sync — 2026-05-14)

- `CURRENT_STATE.md`, `KNOWN_ISSUES.md`, `NEXT_WORK.md` updated to
  reflect completed runtime stabilization pass in `wayaura-core`
  (commits `0efe9ce`, `90a2152`, `5b2134c`, `070473e`):
  - Stop-phrase interrupt restored; audible stop-ack added.
  - `AURA_PLAYBACK_PROFILE` exported by `audio_detect.sh`;
    playback probe logged by `assistant.py`.
  - Single USB combo routing policy: `usb_in_hdmi_out` default,
    `single_usb_combo` opt-in via `AURA_FORCE_USB_PLAYBACK=1`.
  - `RELEASE_NOTES.md` added to `wayaura-core` with stage summary,
    known limitations, and roadmap.
- `KNOWN_ISSUES.md` closed stale Phase 1 / static-validation items;
  added AB13X hot-plug limitation as `known issue / not blocking` with
  workaround; added Closed issues section for session record.
- `NEXT_WORK.md` replaced Phase 1 validation checklist with current
  practical direction (roadmap items, limitation note).
- Publishing policy: `wayaura-context` is updated in sync with each
  completed `wayaura-core` stage. Runtime files (`assistant.py`,
  scripts, configs, JSON data) do not appear here; only high-level
  factual state, issue status, and direction links.

### Added (earlier)
- `CURRENT_STATE.md`, `NEXT_WORK.md`, `KNOWN_ISSUES.md` — recorded
  that `wayaura-core` Phase 1 (reliable autostart + audio fallback)
  has been implemented and statically validated in sandbox, but
  still requires on-device validation on the target Raspberry Pi
  before it can be described as solved. Concrete `wayaura-core`
  commit range `0ef6cab..3be4fbc` (`a967b64`, `76c53db`, `f7e70ec`,
  `3be4fbc`) covering `system/aura.service`, `start.sh`,
  `scripts/audio_detect.sh`, and the associated docs is captured in
  `NEXT_WORK.md`. The bounded next move is on-device validation
  (cold boot without USB, cold boot with stable USB mic,
  MicA / AB13X playback-failure case, and confirmation of
  `PA_ALSA_PLUGHW=1` in effect plus RMS / `MIC_DEBUG` if needed),
  not more code first. The earlier `PA_ALSA_PLUGHW=1`
  documentation-drift note in `KNOWN_ISSUES.md` is updated to
  reflect that `start.sh` now exports it; on-device confirmation
  remains part of pending Phase 1 validation.
- `KNOWN_ISSUES.md` — recorded the single-USB audio incident as an
  active issue: with one USB card carrying both microphone and
  headphones / AUX, Aura starts but neither direction works; adding
  a second USB adapter restores both. Primary hypothesis: likely a
  device / ALSA full-duplex limitation of the single USB card
  rather than an Aura routing-selection bug, pending on-device
  proof. Also recorded a minor `wayaura-core` doc / comment drift
  about `PA_ALSA_PLUGHW=1` not actually being exported by
  `start.sh`, separate from the incident.
- `NEXT_WORK.md` — added a bounded next-step section describing
  the on-device simultaneous capture + playback proof that must
  precede any audio code or config change, with example
  `arecord` / `aplay` commands framed as a proof plan rather than
  completed checks.
- `CURRENT_STATE.md` — added a single concise factual note in the
  pending-validation section pointing at the single-USB incident
  in `KNOWN_ISSUES.md` and the bounded next step in `NEXT_WORK.md`.
- Naming convention recorded in `CURRENT_STATE.md` and
  `NEXT_WORK.md`: the current owner-provided runtime source
  package is referred to as the `aura-0.1` archive (the
  `aura-0.1` runtime package) in reports, handover, and task
  framing. No new archive names without explicit Owner decision.
- `NEXT_WORK.md` — compact operational bridge between
  `CURRENT_STATE.md` and the next agent's first practical move.
- `REPO_MAP.md` — compact navigation between `wayaura-context` and
  `wayaura-core`, including which kinds of tasks belong in each repo.
- Initial normalized continuity repository structure for
  `wayaura-context`:
  - `README.md`, `START_HERE.md`, `AGENT_BRIEF.md`,
    `CURRENT_STATE.md`.
  - `WayAura_OVERVIEW.md`, `WayAura_OFFICIAL_MAP.md`,
    `WayAura_ARCHITECTURE.md`, `WayAura_AGENTS.md`,
    `WayAura_DEV_ONBOARDING.md`.
  - `SEARCH_HANDOVER.md`, `COMPUTER_HANDOVER.md`.
  - `KNOWN_ISSUES.md`, `CHANGELOG.md`,
    `WayAura_REPOS_STRATEGY.md`.
  - `LICENSE` and `PROPRIETARY.md` — proprietary, all-rights-reserved
    licensing for this repository, English only.

### Changed
- `CURRENT_STATE.md` updated: phase is now the **operational base
  phase**. `wayaura-core` holds an imported Aura 0.1 runtime working
  base; static smoke has passed; real on-device install/start smoke
  and audio tuning remain pending Owner / device validation.
- `KNOWN_ISSUES.md` rewritten to drop stale "split is conceptual"
  wording and instead reflect the real open items: device smoke,
  audio tuning, and cross-repo drift risk.
- `START_HERE.md`, `WayAura_DEV_ONBOARDING.md`, `SEARCH_HANDOVER.md`,
  `COMPUTER_HANDOVER.md`, and `README.md` reading orders updated to
  include `NEXT_WORK.md` and `REPO_MAP.md`, and to add
  `WayAura_AGENTS.md` to the README sequence.
- Documentation file names normalized. Earlier intermediate names
  such as `*_public.md` and any mixed forms are not used here.
- Cross-references between documents updated to the normalized file
  names.

### Notes
- The repository split between `wayaura-context` (this repository)
  and `wayaura-core` is physically realized.
- Runtime code, install/start scripts, and configuration templates
  are out of scope here and live in `wayaura-core`.
