# WayAura Context Repository

This repository is the continuity and documentation layer of WayAura.

It exists so the project can survive account changes, session loss, repository rebuilds, and agent replacement without forcing the owner to re-explain the system from zero.

## Purpose

This repo is the source of truth for:
- project identity and direction;
- architecture and repository boundaries;
- agent roles and operating rules;
- onboarding flow for new Search or Computer agents;
- current state, known issues, and changelog;
- handover logic between sessions and accounts.

This repo is **not** the private runtime/code repository.
Operational runtime files belong in `wayaura-core`.

## Статус проекта

Aura 0.1 — голосовой ассистент для Raspberry Pi (автомобильный + домашний сценарии).

**Что реализовано:**
- Голосовое управление (Yandex Cloud STT/TTS/LLM), распознавание wake-word
- Адаптивная аудио-маршрутизация: USB-микрофон + HDMI-вывод из коробки
- Self-healing: ограниченный механизм восстановления аудио при старте
- Автозапуск через systemd (`aura.service`) с политикой restart-on-failure
- In-car сценарий: USB-переходник с микрофоном → AUX или HDMI в головное устройство

**Известные ограничения:**
- Single USB-адаптер без подключённой гарнитуры: физический вывод может молчать
  (профиль `usb_combo`). Дефолтное поведение (HDMI-вывод) от этого защищает.
- Переключение устройств через PulseAudio/GUI во время работы может ломать
  ALSA-маршрутизацию. Headless-окружение рекомендовано.

**Runtime-код:** https://github.com/WayAura/wayaura-core (приватный)

## Reading Order

A new agent should read files in this order:

1. `START_HERE.md`
2. `AGENT_BRIEF.md`
3. `CURRENT_STATE.md`
4. `NEXT_WORK.md`
5. `REPO_MAP.md`
6. `WayAura_OVERVIEW.md`
7. `WayAura_OFFICIAL_MAP.md`
8. `WayAura_ARCHITECTURE.md`
9. `WayAura_AGENTS.md`
10. `WayAura_DEV_ONBOARDING.md`
11. `SEARCH_HANDOVER.md` or `COMPUTER_HANDOVER.md`
12. `KNOWN_ISSUES.md`
13. `CHANGELOG.md`
14. `WayAura_REPOS_STRATEGY.md`

For a compact index of what each document is for, see
[`DOCINDEX.md`](DOCINDEX.md). It is a navigation aid, not a
replacement for the reading order above.

## Core Rule

WayAura continuity must live in documents, not in one chat thread, one Search session, or one temporary repository state.
