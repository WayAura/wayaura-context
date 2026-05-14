# REPO_MAP

Compact navigation between the two WayAura repositories. Use this to
decide where a given task or document belongs before doing any work.

## Open this repository first

Always enter WayAura through `wayaura-context` (this repository). It
is the continuity and onboarding layer. A new agent must be able to
reach working context from here without first being granted runtime
access.

## Where docs live

- Project identity, layers, and map:
  [`WayAura_OVERVIEW.md`](WayAura_OVERVIEW.md),
  [`WayAura_OFFICIAL_MAP.md`](WayAura_OFFICIAL_MAP.md).
- Architecture and repository contract:
  [`WayAura_ARCHITECTURE.md`](WayAura_ARCHITECTURE.md),
  [`WayAura_REPOS_STRATEGY.md`](WayAura_REPOS_STRATEGY.md).
- Roles, rules, red zones:
  [`AGENT_BRIEF.md`](AGENT_BRIEF.md),
  [`WayAura_AGENTS.md`](WayAura_AGENTS.md).
- Onboarding and handover:
  [`WayAura_DEV_ONBOARDING.md`](WayAura_DEV_ONBOARDING.md),
  [`SEARCH_HANDOVER.md`](SEARCH_HANDOVER.md),
  [`COMPUTER_HANDOVER.md`](COMPUTER_HANDOVER.md).
- State, issues, history:
  [`CURRENT_STATE.md`](CURRENT_STATE.md),
  [`NEXT_WORK.md`](NEXT_WORK.md),
  [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md),
  [`CHANGELOG.md`](CHANGELOG.md).
- Licensing:
  [`LICENSE`](LICENSE), [`PROPRIETARY.md`](PROPRIETARY.md).

## Where runtime lives

Runtime, install/start, services, and configuration live in the
private repository
[`wayaura-core`](https://github.com/WayAura/wayaura-core). It holds
the Aura 0.1 runtime base (assistant code, runtime data, install and
start scripts, service templates, audio docs) and any
security-sensitive material. It is not mirrored here.

## Which task belongs where

Stay in `wayaura-context` if the task is:

- onboarding, role definitions, or handover;
- architecture, repository strategy, or the cross-repo contract;
- current state, known issues, changelog, next-work bridge;
- licensing or proprietary notice wording.

Move to `wayaura-core` if the task is:

- assistant runtime behavior, intent handling, voice I/O;
- install or start scripts, service templates, environment templates;
- audio pipeline, wake-word, or any device-specific tuning (red
  zone — Owner approval required);
- real configuration values, secrets, device identifiers, local
  paths.

If a task spans both repositories, document the structural side here
and link to `wayaura-core` for the runtime side. Do not duplicate
runtime documentation into this repository, and do not embed
continuity documentation into `wayaura-core`.

---

## Publishing policy

`wayaura-context` (this repository) is updated in sync with each
completed stage in `wayaura-core`. The mode is: every completed
engineering stage in `wayaura-core` triggers a matching factual update
here — not a copy of runtime files, but an update to `CURRENT_STATE.md`,
`KNOWN_ISSUES.md`, `NEXT_WORK.md`, and `CHANGELOG.md` reflecting what
changed and what the current truth is.

`wayaura-context` is public. `wayaura-core` is private. The split is
permanent and intentional. The following content must never appear in
`wayaura-context`:

- Runtime code (`assistant.py`, `aura_core.py`, `phrase_library.py`,
  scripts, service templates).
- Configuration templates (`.env.example`, `.env.config.example`) or
  any real configuration values.
- JSON phrase/data files.
- Any secrets, device identifiers, local file paths, or install-time
  artifacts.

`wayaura-context` may reference `wayaura-core` commits by hash when
recording what changed, but does not reproduce the diff or content.
