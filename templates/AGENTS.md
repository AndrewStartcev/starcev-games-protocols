# AGENTS.md — <GAME>

## Source of truth

Общие правила производства:

`https://github.com/AndrewStartcev/starcev-games-protocols`

Protocol version: `<VERSION>`

Перед существенной работой прочитать в protocols:

1. `docs/00_MASTER_PROTOCOL.md`;
2. `docs/17_AI_AGENT_WORKFLOW.md`;
3. только protocol docs, относящиеся к текущему milestone.

В этом репозитории сначала читать:

1. `docs/PROJECT_STATE.md`;
2. `docs/GAME_BRIEF.md`;
3. релевантные файлы задачи.

## Scope

- Не переписывать уже работающие системы без причины.
- Не менять утверждённую механику/баланс/visual style вне scope задачи.
- Не добавлять прямые Yandex/Pikabu SDK calls в gameplay.
- Не создавать финальный asset pack без `ASSET_MANIFEST` и реальных UI slots.
- Не считать Editor run достаточным тестом Web build.

## End of task

Если задача заметно изменила проект:

- обновить `docs/PROJECT_STATE.md`;
- обновить `docs/DECISIONS.md` при новом архитектурном решении;
- обновить `docs/ASSET_MANIFEST.md` при изменении asset contract;
- выполнить релевантный smoke test;
- commit с понятным сообщением;
- в отчёте указать изменённые пути, что проверено и что осталось.
