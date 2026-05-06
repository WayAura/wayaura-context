# CURRENT_STATE

## Phase

WayAura is in the **normalized repo-creation phase**.

The earlier transition pack — a single curated set of documents preparing
for repository split — has now been physically realized as two repositories:

- `wayaura-context` (this repository) — public-facing documentation and
  continuity layer.
- `wayaura-core` — private runtime, install/start, configuration templates,
  and operational code.

This repository, `wayaura-context`, is now the **context source of truth**.
Where this repository and any older standalone document disagree, this
repository wins.

## What is true right now

- The repository split has been performed. It is not a future intention.
- Documentation file names in this repository are normalized; mixed or
  intermediate names from earlier packs (for example `*_public.md` or
  `contextCURRENTSTATE.md`) are no longer used here.
- All licensing for this repository is proprietary, English-only, and lives
  in [`LICENSE`](LICENSE) and [`PROPRIETARY.md`](PROPRIETARY.md). No
  open-source license applies.
- Runtime code is **not** in this repository and will not be added here.
- Search and Computer are defined as replaceable roles, not identities.

## What is in progress

- Establishing `wayaura-context` as the stable continuity layer with
  README, handovers, architecture, agents, and strategy fully aligned.
- Maintaining cross-references to `wayaura-core` without duplicating its
  runtime documentation.
- Keeping [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md) and
  [`CHANGELOG.md`](CHANGELOG.md) honest as the source of recent change
  history, without turning them into brainstorming dumps.

## What is explicitly out of scope here

- Running, installing, or configuring the runtime assistant.
- Storing secrets, real configuration values, device identifiers, or
  local file paths.
- Any audio, wake-word, or runtime-sensitive change.

For runtime status, see `wayaura-core`. This repository should never
attempt to mirror runtime state in detail.

## Next practical move

Continue using this repository as the entry point for any new agent.
New work that touches behavior or configuration must move to `wayaura-core`
under Owner approval; new work that touches structure, role definitions,
onboarding, or continuity stays here.
