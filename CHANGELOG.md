# CHANGELOG

All notable changes to `wayaura-context` are recorded here in
human-readable form. The most recent changes go on top under
`Unreleased`. When a release is cut, the Unreleased section is renamed
to that release and a new Unreleased section is started above it.

## Unreleased

### Added
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
- Documentation file names normalized. Earlier intermediate names
  such as `*_public.md` and any mixed forms are not used here.
- Cross-references between documents updated to the normalized file
  names.

### Notes
- The repository split between `wayaura-context` (this repository)
  and `wayaura-core` is now physically realized, not a future
  intention.
- Runtime code, install/start scripts, and configuration templates
  are out of scope here and live in `wayaura-core`.
