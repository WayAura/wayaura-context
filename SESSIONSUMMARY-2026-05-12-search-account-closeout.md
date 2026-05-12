# SESSIONSUMMARY — 2026-05-12 — Search account closeout

This is a GitHub-backed closeout summary for the Search account
phase that ends on 2026-05-12. It is a continuity record, not a
new work phase and not a roadmap. It is written so that a future
agent entering through `wayaura-context` can reconstruct what was
done, what is safe, and where to look next, without inventing
state.

The next bounded move is **not** broad feature work and must not
be opened automatically. A new owner-scoped task is required
before any further runtime change.

## Scope of this closeout

- This account phase was deliberately limited in usage. It did
  not attempt to advance the runtime beyond Phase 1.
- The goal at closeout was a safe continuity point: the operating
  repos are consistent, the entry path is unambiguous, and no
  half-finished change is left on `main` in either repo.

## Operating model in force at closeout

The repository split is physically realized and remains the
operating model:

- `wayaura-context` (this repository) is the continuity and
  source-of-truth entry point. New agents must enter here first.
- `wayaura-core` is the private runtime / implementation
  repository. Runtime code, install/start, services, and
  configuration live there, not here.

The cross-repo contract and entry rules are documented in
[`START_HERE.md`](START_HERE.md), [`REPO_MAP.md`](REPO_MAP.md),
[`WayAura_REPOS_STRATEGY.md`](WayAura_REPOS_STRATEGY.md), and the
quick map in [`DOCINDEX.md`](DOCINDEX.md).

## What landed in `wayaura-core` during this phase

`wayaura-core` Phase 1 (reliable autostart + audio fallback) was
implemented and pushed to `main`. The commits, as recorded in
[`NEXT_WORK.md`](NEXT_WORK.md) and
[`CHANGELOG.md`](CHANGELOG.md), are:

- `a967b64` — `system/aura.service` waits for udev settle before
  `start.sh`.
- `76c53db` — `scripts/audio_detect.sh` performs an `aplay`
  open-probe, records `AURA_PLAYBACK_PROBE`, and falls back to
  bcm2835 / HDMI on failure.
- `f7e70ec` — `start.sh` waits for late USB arrival, retries
  detection, and exports `AURA_AUDIO_PROFILE` plus
  `PA_ALSA_PLUGHW=1`.
- `3be4fbc` — runtime docs (`README.md`, `docs/AUDIO.md`,
  `CONFIG.md`, `.env.config.example`) updated for the autostart
  audio wait, profile enum, and playback probe.

Static validation in the `wayaura-core` sandbox has passed
(`bash -n`, `systemd-analyze verify`, and a dry run of
`scripts/audio_detect.sh start` producing a safe degraded
`runtime/audio.env` on a host without USB).

On-device validation on the target Raspberry Pi is **still
pending** and has not been performed. Phase 1 must not be
described as fully solved until the on-device checks listed in
[`NEXT_WORK.md`](NEXT_WORK.md) (cold boot without USB, cold boot
with stable USB mic, MicA / AB13X playback-failure case, and
confirmation of `PA_ALSA_PLUGHW=1` in effect) are in.

## What landed in `wayaura-context` during this phase

Two continuity-only commits were made on `main` after the
`wayaura-core` Phase 1 push:

- `0fa91dd` — `docs: record phase 1 autostart audio fallback
  status`. Updated `CURRENT_STATE.md`, `NEXT_WORK.md`,
  `KNOWN_ISSUES.md`, and `CHANGELOG.md` to record that Phase 1 is
  implemented and statically validated in sandbox but pending
  on-device validation, with the concrete `wayaura-core` commit
  range and the bounded next move.
- `fe6dba7` — `docs: clarify context entry path`. Ultra-safe
  entry-layer cleanup of `README.md`, `START_HERE.md`,
  `NEXT_WORK.md`, and a new `DOCINDEX.md` so the entry path
  through this repository is unambiguous.

No runtime code, audio routing, install/start, service template,
or configuration value was changed from this repository during
this phase, by design. Those continue to live in `wayaura-core`
and are red-zone by default.

## What is safe to assume right now

- The repository split is real and is the operating model.
- The entry path through `wayaura-context` is the single
  documented way in:
  [`START_HERE.md`](START_HERE.md) →
  [`AGENT_BRIEF.md`](AGENT_BRIEF.md) →
  [`CURRENT_STATE.md`](CURRENT_STATE.md) →
  [`NEXT_WORK.md`](NEXT_WORK.md) →
  [`REPO_MAP.md`](REPO_MAP.md), with
  [`DOCINDEX.md`](DOCINDEX.md) as a compact map.
- `CURRENT_STATE.md` and `NEXT_WORK.md` reflect Phase 1 as
  implemented and statically validated but pending on-device
  validation.
- `KNOWN_ISSUES.md` continues to carry the single-USB
  full-duplex incident as an active issue with a bounded
  on-device proof step before any further audio change.

## What must not be assumed

- Phase 1 has **not** been validated on the target Raspberry Pi.
  Do not claim it has.
- The single-USB audio incident has **not** been resolved. The
  bounded proof step in [`NEXT_WORK.md`](NEXT_WORK.md) is a
  proof plan, not a completed check.
- No new product version beyond the `aura-0.1` runtime base is
  declared. The `aura-0.1` naming convention recorded in
  [`CURRENT_STATE.md`](CURRENT_STATE.md) still applies.

## What the next account / agent should do

1. Treat this closeout summary as historical context. Do not
   extend it; write a new session summary for the next phase.
2. Enter through this repository in the order documented in
   [`START_HERE.md`](START_HERE.md) and
   [`DOCINDEX.md`](DOCINDEX.md).
3. Do **not** auto-open broad feature work. The next bounded
   move documented in [`NEXT_WORK.md`](NEXT_WORK.md) is
   on-device Phase 1 validation on the target Raspberry Pi, not
   more code first. Any expansion of scope beyond that requires
   a new explicit Owner decision.
4. Audio pipeline, wake-word, install/start, services,
   licensing, real configuration values, and the cross-repo
   contract remain red zones. They require explicit Owner
   approval before any change.

## Why this summary exists

A single GitHub-backed file at the root of `wayaura-context` is
sufficient to mark the closeout of this account phase without
opening a new ACCOUNTHANDOVER file or rewriting continuity
documents. Continuity facts (phase, pending validation, known
issues, change history) already live in their canonical files
(`CURRENT_STATE.md`, `NEXT_WORK.md`, `KNOWN_ISSUES.md`,
`CHANGELOG.md`) and are not duplicated here.
