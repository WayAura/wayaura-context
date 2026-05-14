# CURRENT_STATE

## Phase

WayAura is in the **operational base phase**.

The repository split is in place, and `wayaura-core` now holds an
imported Aura 0.1 runtime working base — assistant code, runtime data,
install/start scripts, service templates, and operational documentation.
This repository, `wayaura-context`, is the continuity and onboarding
entry point that new agents read before touching `wayaura-core`.

Where this repository and any older standalone document disagree, this
repository wins for continuity, role, and structure topics; runtime
truth lives in `wayaura-core`.

## What is true right now

- The repository split is physically realized. It is not a future
  intention.
- `wayaura-core` contains a real Aura 0.1 runtime base imported from
  the owner archive: `assistant.py`, `aura_core.py`,
  `phrase_library.py`, `requirements.txt`, runtime JSON data files,
  `scripts/audio_detect.sh`, and `docs/AUDIO.md` /
  `docs/AUDIO_TUNING.md`.
- Top-level operational files in `wayaura-core` (`install.sh`,
  `start.sh`, `.env.example`, `.env.config.example`) have been
  refreshed from the archive. `asound.conf` and `aura.service` live
  under `system/` because `install.sh` expects `SCRIPT_DIR/system/`.
- Static smoke checks have passed in `wayaura-core` (shell `bash -n`,
  Python `ast.parse`, JSON `json.load`).
- Documentation file names in this repository are normalized; mixed or
  intermediate names from earlier packs (for example `*_public.md` or
  `contextCURRENTSTATE.md`) are no longer used here.
- All licensing for this repository is proprietary, English-only, and
  lives in [`LICENSE`](LICENSE) and [`PROPRIETARY.md`](PROPRIETARY.md).
  No open-source license applies.
- Runtime code is **not** in this repository and will not be added
  here.
- Search and Computer are defined as replaceable roles, not
  identities.
- Naming convention: the current owner-provided runtime source
  package is referred to as the `aura-0.1` archive (equivalently
  the `aura-0.1` runtime package). Reports, handover notes, and
  task framing should use this exact name when pointing at that
  package. No new archive names are introduced without an
  explicit Owner decision. This is a naming convention only and
  does not by itself claim a new product version beyond the
  current runtime base.

## What is in progress

- Holding `wayaura-context` stable as the continuity entry point while
  `wayaura-core` evolves on its engineering cadence.
- Maintaining cross-references to `wayaura-core` without duplicating
  its runtime documentation.
- Keeping [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md) and
  [`CHANGELOG.md`](CHANGELOG.md) honest as the source of recent change
  history, without turning them into brainstorming dumps.
- Bridging concrete next-step direction through
  [`NEXT_WORK.md`](NEXT_WORK.md) — short, current, and replaced as the
  practical front of work moves.

## What is true right now (runtime stabilization completed)

The runtime stabilization pass for Aura 0.1 has been completed and
validated on the owner's device. The following are now true:

- Stop-phrase interrupt during TTS playback is restored. Stop-words
  expanded; audible stop-ack added (direct `_play_wav_file` call,
  bypasses queue). Commits `0efe9ce`, `90a2152` in `wayaura-core`.
- `audio_detect.sh` exports `AURA_PLAYBACK_PROFILE` on every start;
  `assistant.py` logs playback probe status at startup. The adaptive
  routing principle is documented in `docs/AUDIO.md` in `wayaura-core`.
- Single USB combo routing policy: default for one USB audio card
  (AB13X type) is now `usb_in_hdmi_out` (USB capture + HDMI playback)
  rather than USB for both. USB playback opt-in via
  `AURA_FORCE_USB_PLAYBACK=1` in `.env.config`. Commit `5b2134c`.
- Stage progress documented in `RELEASE_NOTES.md` in `wayaura-core`
  (commit `070473e`), including known limitations and roadmap.

## What is true right now (Phase 2 — bounded self-healing layer)

Phase 2 has been implemented and pushed to `wayaura-core` main.
The following are now true:

- `health.py` (`AuraHealthMonitor`) is live in `wayaura-core`. State
  machine: HEALTHY → DEGRADED → RECOVERING → FAILED_HARD.
- Startup audio validation window (default 15s, `AURA_SELF_HEAL_STARTUP_WINDOW_SEC`).
  After `_detect_playback()`, the health monitor validates audio env vars
  and attempts recovery (max 2 attempts by default) before entering DEGRADED.
- Runtime audio recovery: on playback exception in `_play_wav_file`, the
  health monitor is invoked. Budget is shared between startup and runtime
  (single counter, no reset).
- AB13X / single USB class-based policy: if `AURA_PLAYBACK_PROFILE` is
  `usb_in_hdmi_out`, recovery reroutes to `builtin_fallback` (class-based
  string check, no hardcoded device names or paths).
- Log prefixes `[HEAL]`, `[HEALTH]`, `[RECOVERY]` on all health events.
- `AURA_SELF_HEALING=0` disables the entire module (no-op mode).
- Basic systemd restart policy confirmed present in `system/aura.service`:
  `Restart=on-failure`, `RestartSec=5`, `StartLimitBurst=3`,
  `StartLimitIntervalSec=60`.
- `docs/SELF_HEALING.md` added to `wayaura-core` with full documentation.
- Commits: `416bb5e` (implementation), `248a97e` (RELEASE_NOTES fixup).

## What is true right now (Phase 2A.1 — usb_combo risky path)

Phase 2A.1 pushed to `wayaura-core` main (commit `70165a7`):

- `_is_risky_path()`: triggers when `AURA_AUDIO_PROFILE=usb_combo` +
  USB hw-path prefix + `IS_FALLBACK!=1` — class-based, no device names.
- `_find_hdmi_device()`: parses `aplay -l`, returns HDMI `hw:N,M` for
  `AURA_PLAYBACK_PRIMARY` rewrite during recovery.
- `startup_audio_check()` now detects cold-start mute path and enters
  recovery even when ALSA probe is formally OK.
- Stop-listener confirmed already optimal (50 ms polling, flag before ack);
  `[STOP] stop propagated in <N>ms` diagnostic log added.
- No new env flags. Red zones untouched.

## What is pending

- Audio tuning values (`asound.conf`, audio detect thresholds). These
  are red-zone, owner-and-device decisions.
- Known limitation: starting with AB13X already plugged and
  `AURA_FORCE_USB_PLAYBACK=1` may still produce a mute Aura (explicit
  opt-in). Documented in `wayaura-core` `RELEASE_NOTES.md` and
  `docs/AUDIO.md` as `known issue / not blocking`. Reliable workaround:
  start without USB, plug after greeting.
- Autostart improvements, pause/resume — roadmap items in
  `wayaura-core` `RELEASE_NOTES.md`, not yet scoped as tasks.
- Phase 2B (deferred from Phase 2): `Type=notify` systemd watchdog,
  `WatchdogSec`, `sd_notify` heartbeat from `assistant.py` main loop.

## What is explicitly out of scope here

- Running, installing, or configuring the runtime assistant.
- Storing secrets, real configuration values, device identifiers, or
  local file paths.
- Any audio, wake-word, or runtime-sensitive change.

For runtime status, see `wayaura-core`. This repository should never
attempt to mirror runtime state in detail.

## Next practical move

A new agent entering through this repository should read
[`NEXT_WORK.md`](NEXT_WORK.md) for the current practical direction and
[`REPO_MAP.md`](REPO_MAP.md) for navigation between the two
repositories. Work that touches behavior, install/start, or
configuration moves to `wayaura-core` under Owner approval; work that
touches structure, role definitions, onboarding, or continuity stays
here.
