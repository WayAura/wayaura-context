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

## What is pending owner / device validation

These remain open and live in `wayaura-core`'s scope, not this
repository:

- Real install/start smoke on the target Raspberry Pi / device — only
  static smoke has been run so far.
- Audio tuning values (`asound.conf`, audio detect thresholds, any
  device-specific overrides). These are red-zone, owner-and-device
  decisions.

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
