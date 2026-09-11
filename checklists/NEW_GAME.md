# New Game Checklist

Используется в пустом репозитории новой игры.

## A. Intake

- [ ] Прочитан `README.md` protocols.
- [ ] Прочитан `docs/00_MASTER_PROTOCOL.md`.
- [ ] Прочитан `docs/17_AI_AGENT_WORKFLOW.md`.
- [ ] Зафиксирована версия protocols.
- [ ] Зафиксирована версия Godot.
- [ ] Определены целевые платформы: Local / Yandex / Pikabu.
- [ ] Определена ориентация: portrait / landscape / both.
- [ ] Определён класс: A / B / C.

## B. Документы игры

Скопировать и заполнить:

- [ ] `docs/GAME_BRIEF.md`.
- [ ] `docs/GDD.md`.
- [ ] `docs/PROJECT_STATE.md`.
- [ ] `docs/DECISIONS.md`.
- [ ] `docs/ASSET_MANIFEST.md`.
- [ ] `docs/ASSET_SOURCES.md`.
- [ ] `docs/QA_REPORT.md`.
- [ ] `docs/RELEASE_REPORT.md`.

## C. Идея

- [ ] Core loop описан одним абзацем.
- [ ] Игрок понимает первое действие за 10–30 секунд.
- [ ] Production Score рассчитан.
- [ ] Есть `GO / PROTOTYPE / KILL` решение.
- [ ] Описана причина вернуться в игру.
- [ ] Описаны 2+ возможные rewarded-сценария или зафиксировано, почему они не нужны.
- [ ] Найдены естественные fullscreen-паузы или зафиксировано, почему fullscreen не нужен.
- [ ] Есть 30+ минут gameplay/replayability как внутренний target.

## D. Prototype

- [ ] Создан минимальный Godot project.
- [ ] Renderer = Compatibility.
- [ ] Web single-thread.
- [ ] Один полный core loop работает без финального арта.
- [ ] Проверены mouse + touch semantics.
- [ ] Сделан первый Web export.
- [ ] Core loop получил `GO` до производства большого asset pack.

## E. Перед финальным артом

- [ ] Основные экраны собраны graybox.
- [ ] Responsive layout проверен.
- [ ] Реальные UI slot sizes известны.
- [ ] `ASSET_MANIFEST` заполнен.
- [ ] Art direction утверждён.
- [ ] Сначала делаются только 3–5 ключевых ассетов.

## F. Следующий шаг

Обновить `PROJECT_STATE.md`, сделать commit и переходить к текущему milestone по `02_PRODUCTION_PIPELINE.md`.
