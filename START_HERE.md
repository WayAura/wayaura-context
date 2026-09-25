# START_HERE

## Aura v1 — текущая эпоха проекта (с 2026-09-23)

Если вы агент или новый участник, начните здесь:

1. [`AURA_V1_RULES.md`](AURA_V1_RULES.md) — действующие правила: одна Аура без подмен, старые файлы на Pi,
   секреты и идентификаторы (репозиторий публичный), git, когда спрашивать владельца, документация, отчёт.
2. [`HANDOFF_AURA_V1.md`](HANDOFF_AURA_V1.md) — передача дел: что где лежит (в том числе вне git), грабли, команды.
3. [`CURRENT_STATE.md`](CURRENT_STATE.md) — раздел «New development cycle: Aura v1» и краткий срез по-русски.
4. [`NEXT_WORK.md`](NEXT_WORK.md) — ближайшие шаги Aura v1.
5. Код и техническая документация — `wayaura-core`, папка `aura-v1/` (`README.md`, `FINDINGS.md`, `docs/`);
   фундамент — метка `aura-v1.0`.
6. Чекпоинты этапов — [`MIGRATION_CHECKPOINTS/`](MIGRATION_CHECKPOINTS/) (`AURA_V1_*`).

Всё, что ниже, — вход эпохи Legacy Aura / New Aura. Сохранено как история: роли Search/Computer,
«красные зоны» и порядок чтения той эпохи для Aura v1 не обязательны; при расхождении действует
`AURA_V1_RULES.md`.

---

You are entering the WayAura context repository. Read this file fully before
doing anything else.

## What this repository is

`wayaura-context` is the documentation and continuity layer of WayAura.
It is the project's memory. It is not a place to run, install, or change
runtime code.

Runtime code lives in the private repository
[`wayaura-core`](https://github.com/WayAura/wayaura-core).

## What you must do first

1. Read [`AGENT_BRIEF.md`](AGENT_BRIEF.md) — what WayAura is, the roles, the
   rules, and the red zones.
2. Read [`CURRENT_STATE.md`](CURRENT_STATE.md) — the project's current phase
   and what is in progress right now.
3. Read [`NEXT_WORK.md`](NEXT_WORK.md) — the current practical direction and
   what the next agent should do first.
4. Read [`REPO_MAP.md`](REPO_MAP.md) — which repository a given task belongs
   to.
5. Identify which role you are filling: Search, Computer, or Owner-directed
   support.
6. Read the matching handover:
   - Search → [`SEARCH_HANDOVER.md`](SEARCH_HANDOVER.md)
   - Computer → [`COMPUTER_HANDOVER.md`](COMPUTER_HANDOVER.md)
7. Read [`WayAura_ARCHITECTURE.md`](WayAura_ARCHITECTURE.md) and
   [`WayAura_AGENTS.md`](WayAura_AGENTS.md) before proposing changes.

If you need a compact index of what every document in this
repository is for, see [`DOCINDEX.md`](DOCINDEX.md). It is a
navigation aid only; this file remains the mandatory entry point.

## Hard rules

- Do not invent project memory. If a fact is not in these documents or in
  `wayaura-core`, treat it as unknown until the Owner confirms it.
- Do not duplicate runtime documentation here. Link to `wayaura-core` instead.
- Do not touch audio, wake-word, or any runtime-sensitive area as a side
  effect. Those are red zones.
- Do not commit secrets, tokens, device identifiers, or local paths.
- Do not introduce open-source licenses. The project is proprietary.

## When you are done with a unit of work

- Update [`CURRENT_STATE.md`](CURRENT_STATE.md) if the project phase changed.
- Add a clean entry to [`CHANGELOG.md`](CHANGELOG.md) under `Unreleased`.
- Add anything genuinely unresolved to [`KNOWN_ISSUES.md`](KNOWN_ISSUES.md);
  do not use it as a brainstorm dump.
