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

## New development cycle: Aura v1 (from 2026-09-23)

The active development cycle is now **Aura v1**, built on a saved Yandex
AI Studio voice agent (Realtime API). Legacy Aura 0.1 and New Aura (the
sections below) are the project's history and experience: they are kept
as they are, may be reused where they solved a problem well, and are not
binding rules or architecture for Aura v1. The Owner/Search/Computer
governance and red zones below are likewise reference only for this cycle.

**Project rule — one Aura, no substitutes (Owner decision, 2026-09-23).** Only Aura v1 runs on
the Pi, and only with the Owner's agent in Yandex AI Studio. No automatic switching to other
assistants, models or versions; if Aura v1 has a problem, it is fixed, not replaced. Legacy Aura is
to be stopped and disabled on the Pi (files and unit kept as they are); a manual emergency command
(`aura restore`) is the only way to start it again.

What is true for Aura v1 right now:

- Code lives in `wayaura-core` under `aura-v1/` on branch
  `feature/aura-v1-m0-realtime-roundtrip`; not merged into `main`.
- Milestone 0 facts about the Realtime API are recorded in
  `aura-v1/FINDINGS.md` in `wayaura-core` (agent by ID, 16/24/44.1 kHz,
  no 48 kHz, server VAD stable, manual mode unreliable,
  `input_audio_buffer.clear` unsupported, cancel only before audio).
- Foundation on the stand Raspberry Pi: a voice service with replaceable
  audio source/sink (today USB mic + HDMI TV), half-duplex echo guard,
  idle sleep, reconnects, and a WebSocket + JSON control channel meant for
  the laptop panel now and the phone app later.
- Laptop control panel (`aura` command): checks, start/stop, live event
  feed, text questions, stopping a reply. Aura v1 runs only during a panel
  session as a transient unit; Legacy Aura is stopped for the session and
  returns automatically when v1 stops (verified on hardware).
- No autostart for Aura v1 yet (Owner decision: after the live test and the
  activation decision).
- Legacy Aura on the Pi is stopped and disabled (2026-09-25): it did not start
  after a reboot; its files and unit are unchanged.
- The Owner's agent runs on `speech-realtime-260528` (the model set for the
  agent in AI Studio) — one value in code, no fallback.
- Stand network fixed: the Pi drop-outs were Wi-Fi power save on the Pi; it is
  off now and stays off after reboot.
- Aura v1 connects only to the Owner's agent: an unknown agent ID stops it with
  a clear error (verified), no fallback model; the panel shows that the Owner's
  agent answers (model, voice, fingerprint of the agent instructions, never the ID).
- The laptop panel recognises the Pi by its SSH host key, finds it at a new
  address, reports a lost link in plain words and reconnects by itself.
- Unpushed New Aura work found on the Pi was saved to GitHub from a laptop
  backup: `feature/continuous-realtime-runner` and
  `backup/pi-state-20260923`.
- Verified on the Pi hardware (2026-09-25): the Owner's agent answers (model
  260528), an unknown agent ID stops Aura v1 with a clear error, the panel finds
  the Pi after a hotspot address change and reconnects after link loss, leaving
  the panel stops only Aura v1, full synthetic loop (voice, text, stop reply).
- Live voice test by the Owner on the stand: pending.
- Checkpoint: [`MIGRATION_CHECKPOINTS/AURA_V1_002_ONE_AURA.md`](MIGRATION_CHECKPOINTS/AURA_V1_002_ONE_AURA.md).
- Checkpoint: [`MIGRATION_CHECKPOINTS/AURA_V1_001_FOUNDATION.md`](MIGRATION_CHECKPOINTS/AURA_V1_001_FOUNDATION.md).

Target picture (not implemented): the Pi is Aura's brain and voice in a
protected enclosure in the car, powered from the car battery, sound through
the car speakers; an own OBD2 adapter connects over Bluetooth to both the
Pi and the phone; the phone app is the main remote and the primary
microphone (cabin microphones are the fallback); Aura voices vehicle data
on command. SSH and the laptop are a development channel only.

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

## What is true right now (Audio Policy v2 + Autostart — 2026-05-15)

Audio Policy v2 pushed to `wayaura-core` main (commit `bc8a3a7`):

- `AURA_AUDIO_PROFILE` is now derived from `AURA_PLAYBACK_PROFILE` in
  `start.sh` via explicit `case` mapping. New first-class value:
  `usb_mic_hdmi_out` (single USB + HDMI split — previously classified
  incorrectly as `usb_combo` or `usb_capture_hdmi_playback`).
- `health.py._is_risky_path()` now checks both `AURA_AUDIO_PROFILE` and
  `AURA_PLAYBACK_PROFILE`. `usb_mic_hdmi_out` / `usb_in_hdmi_out` are
  NOT risky. `single_usb_combo` / `mics_only_usb_playback` ARE risky.
- `docs/AUTOSTART.md` added with enable/troubleshoot guide.
- `TESTSCENARIOS.md` added (T1–T6 acceptance checklist).
- audio_detect.sh Priority 0 logic unchanged (already correct).
- No new env flags. No hardcoded device names.

## What is true right now (Autostart single-instance guard — 2026-05-19)

Pushed to `wayaura-core` main (commit `e896582`):

- The Aura runtime now has a single-instance guard covering both the
  systemd-managed start path and manual launches. A second concurrent
  start is refused with an explicit lock log instead of producing a
  duplicate runtime.
- `docs/AUTOSTART.md` and `TESTSCENARIOS.md` extended with the
  A1–A6 acceptance scenarios covering systemd + manual start
  interactions and lock-release on clean exit.
- No new env flags. No changes to audio routing or self-healing
  budgets.
- **Status:** mitigated in code, **pending Pi validation** (A1–A6).
  Not claimed as fully closed until the owner confirms on the target
  device.

## What is true right now (Phase 2A.2 — aplay runtime failures surfaced — 2026-05-19)

Pushed to `wayaura-core` main (commit `7c41a88`):

- Non-zero `aplay` runtime exits are now surfaced to the bounded
  self-healing layer instead of being silently swallowed at the
  playback boundary. Repeated runtime symptoms within the existing
  budget window are coalesced in-process so they do not inflate
  recovery attempts.
- No new env flags. No expansion of the retry-loop. The shared
  startup + runtime budget from Phase 2 is unchanged.
- `docs/SELF_HEALING.md` and `RELEASE_NOTES.md` updated to describe
  the runtime-failure surface and coalescing behavior. `TESTSCENARIOS.md`
  T8 added for runtime aplay failure validation.
- **Status:** mitigated in code, **pending Pi validation** (T8 plus
  T1–T7 regression on the target device). Not claimed as fully closed
  until owner confirmation.

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
- Pi validation of the autostart single-instance guard (A1–A6,
  `wayaura-core` commit `e896582`) and of the aplay runtime-failure
  surface (T8 plus T1–T7 regression, `wayaura-core` commit `7c41a88`).
  Both are mitigated in code, not yet confirmed on hardware.

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

## Текущее состояние системы (на 2026-05-19)

Краткий срез: что реально работает на целевом Raspberry Pi.

**Аудио — стабильно:**
- Cold start без USB → `playback_only_degraded`, HDMI-вывод, Аура слышна
- Cold start с USB + гарнитура/AUX → Аура слышит и слышна (usb_combo или usb_mic_hdmi_out)
- AURA_AUDIO_PROFILE теперь строится из AURA_PLAYBACK_PROFILE (Audio Policy v2, commit `bc8a3a7`)
- Новый первоклассный профиль: `usb_mic_hdmi_out` (USB-capture + HDMI-playback)

**Аудио — известные ограничения:**
- Single USB без гарнитуры → может уйти в usb_combo с беззвучным USB-выводом
- PulseAudio/GUI конфликты → ALSA-маршрут перехватывается, не решается внутри Ауры

**Self-healing:**
- `_is_risky_path()` проверяет как AURA_AUDIO_PROFILE, так и AURA_PLAYBACK_PROFILE
- `usb_mic_hdmi_out` / `usb_in_hdmi_out` не считаются рискованными (HDMI-вывод безопасен)
- `usb_combo` / `single_usb_combo` — рискованные, health входит в recovery

**Автозапуск:**
- `aura.service` настроен, `systemctl enable aura.service` включает автозапуск на boot
- Restart=on-failure, StartLimitBurst=3/60s
- На реальной Pi: `Active: active (running)` после reboot
- Защита от двойного запуска (single-instance guard) добавлена в
  `wayaura-core` (commit `e896582`): повторный старт отказывается с
  явной записью о блокировке. Pending Pi validation сценариев A1–A6.

**Self-healing (Phase 2A.2):**
- Ненулевые runtime-фейлы `aplay` теперь поднимаются в health-монитор
  и проходят через bounded recovery (commit `7c41a88`). Повторные
  симптомы коалесцируются в процессе; бюджет попыток из Phase 2 не
  расширяется. Pending Pi validation T8 и регрессии T1–T7.

**Документация в wayaura-core:**
- `docs/AUTOSTART.md` — включение автозапуска, troubleshoot
- `TESTSCENARIOS.md` — acceptance checklist T1–T6
- `docs/SELF_HEALING.md` — self-healing логика, примеры логов
- `docs/AUDIO.md` — карта профилей

## Aura v1 — краткий срез (на 2026-09-23)

- Новый цикл разработки. Legacy Aura 0.1 и New Aura — история и опыт, не трогаются.
- Код: `wayaura-core`, папка `aura-v1/`, рабочая ветка `feature/aura-v1-m0-realtime-roundtrip`.
- На стендовом Pi: сервис Aura v1 (агент Yandex Realtime, USB-микрофон, HDMI на телевизор,
  полудуплекс, сон после 2 минут тишины, переподключение) и канал управления для панели/телефона.
- На ноутбуке: панель `aura` — проверки, запуск/остановка, живая лента, вопрос текстом.
  Legacy на время сеанса останавливается и возвращается сама.
- Правило «одна Аура, без подмен»: на Pi только Aura v1 с агентом владельца (модель
  speech-realtime-260528). Legacy остановлена и отключена (2026-09-25), файлы не тронуты.
- Автозапуска Aura v1 пока нет (после живого теста и решения по активации).
- Пропадания Pi на стенде — энергосбережение Wi-Fi на Pi; выключено, переживает перезагрузку.
- Живой голосовой тест владельцем — впереди.
