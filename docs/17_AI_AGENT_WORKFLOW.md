# 17 — AI / Codex Workflow & Token Economy

Этот документ нужен, чтобы новая сессия не тратила тысячи токенов на повторное объяснение процесса.

## 1. Bootstrap новой игры

Пользователь даёт две ссылки:

```text
PROTOCOLS: https://github.com/AndrewStartcev/starcev-games-protocols
GAME: https://github.com/.../new-game
```

Агент обязан:

1. прочитать `README.md` protocols;
2. прочитать `docs/00_MASTER_PROTOCOL.md`;
3. прочитать этот файл;
4. прочитать `checklists/NEW_GAME.md`;
5. создать docs из templates в game repo;
6. читать остальные protocol docs только по текущему milestone.

Нельзя каждый раз перечитывать всё подряд.

## 2. PROJECT_STATE — главный handoff

`docs/PROJECT_STATE.md` конкретной игры — краткий текущий снимок.

Он содержит только:

- current milestone;
- engine/platform targets;
- что уже работает;
- что сейчас делаем;
- blocker/issues;
- последние решения;
- следующие 3–7 действий;
- последний проверенный commit/build.

Не превращать его в дневник на 3000 строк.

История долгих решений — в `DECISIONS.md`, а Git уже хранит историю кода.

## 3. Перед задачей

Агент читает:

- PROJECT_STATE;
- конкретный task/user message;
- 1–3 релевантных protocol files;
- только релевантные файлы проекта.

Не делать repository-wide анализ повторно без причины.

## 4. После задачи

Если milestone заметно продвинулся:

- обновить PROJECT_STATE;
- обновить ASSET_MANIFEST/QA/DECISIONS при необходимости;
- сделать commit;
- дать короткий report: что изменено, пути, что проверить.

## 5. Экономия токенов

Предпочитать:

- ссылки/пути вместо повторного копирования больших спецификаций;
- diff/targeted file reads;
- machine-readable manifests;
- короткий state handoff;
- переиспользуемые scripts/tools;
- один утверждённый protocol вместо повторных объяснений.

Не экономить токены ценой пропуска QA или неверной архитектуры.

## 6. Model effort

High reasoning использовать для:

- game architecture;
- незнакомой/сложной механики;
- системного бага;
- save conflicts/security;
- финального visual/architecture review;
- исследования нового SDK.

Medium достаточно для:

- обычной реализации;
- content/data entry;
- wiring UI;
- повторяемых fixes;
- docs updates;
- asset integration по готовому manifest.

Решение по модели не является частью build и может меняться.

## 7. Scope discipline

Если пользователь просит только assets — не менять code.

Если задача разработка — не переделывать approved visual без причины.

Если найдено системное улучшение вне scope:

- зафиксировать suggestion/decision;
- не устраивать большой refactor молча.

## 8. Visual tasks

Перед генерацией production graphics агент обязан получить/создать:

- real slot dimensions;
- ASSET_MANIFEST;
- approved art direction.

После 3–5 ключевых assets — интеграция + screenshot review. Только после approval делать остаток.

## 9. Research freshness

Platform protocols чувствительны ко времени. Перед релизом или крупной новой интеграцией агент проверяет official docs, если `Last reviewed` старше ~30 дней или есть признаки изменения API.

Изменения правил сначала фиксируются в protocols, затем применяются в игре.

## 10. Systemic feedback loop

После проекта ответить:

- что повторилось из старых ошибок;
- чего не хватало в protocol;
- какая ручная операция повторяется;
- что можно автоматизировать;
- что стало медленнее из-за protocol overhead.

Добавлять в общий репо только подтверждённые системные выводы.
