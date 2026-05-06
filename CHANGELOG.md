# CHANGELOG

All notable changes to `wayaura-context` are recorded here in
human-readable form. The most recent changes go on top under
`Unreleased`. When a release is cut, the Unreleased section is renamed
to that release and a new Unreleased section is started above it.

## Unreleased

### Added
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
