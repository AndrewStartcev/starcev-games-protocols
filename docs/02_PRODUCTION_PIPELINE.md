# 02 — Production Pipeline

## Этап 0 — Intake

Вход: идея + пустой Git + ссылка на protocols.

Создать в game repo:

- `docs/GAME_BRIEF.md`;
- `docs/GDD.md`;
- `docs/PROJECT_STATE.md`;
- `docs/DECISIONS.md`;
- `docs/ASSET_MANIFEST.md`;
- `docs/QA_REPORT.md`;
- `docs/RELEASE_REPORT.md`.

Зафиксировать engine version, orientation, platforms, project class A/B/C.

**Gate:** Production Score позволяет идти дальше.

## Этап 1 — Core loop prototype

Только gameplay:

- серые/простые формы;
- один полный игровой цикл;
- минимум экранов;
- без финального арта;
- без массового контента;
- без рекламной полировки.

Проверить:

- понятно ли управление;
- хочется ли повторить действие;
- можно ли играть мышью и touch;
- есть ли технический blocker.

**Gate:** `GO / PIVOT / KILL`.

## Этап 2 — Vertical slice

Собрать один участок игры почти в финальном качестве:

- main menu → gameplay → result/progression → replay;
- базовый save;
- базовый audio;
- responsive UI;
- один пример финального art direction.

Vertical slice нужен, чтобы проверить, что красивая концепция реально собирается в Godot без обрезки, растяжения и подмены UI одним PNG.

**Gate:** реальный screenshot сопоставим с утверждённым визуалом.

## Этап 3 — Architecture freeze

До массового контента:

- разделить gameplay/platform/save/ads/audio;
- утвердить scene tree и autoload list;
- оформить save schema;
- определить content data format;
- закрыть input abstraction;
- закрыть base responsive rules.

После freeze крупная перестройка допускается только при blocker-проблеме.

## Этап 4 — Asset production

Порядок:

1. реальная UI-компоновка;
2. `ASSET_MANIFEST`;
3. style guide/concept;
4. отдельные production assets;
5. импорт в Godot;
6. screenshot review;
7. точечные исправления.

Нельзя заказывать/генерировать весь пак до проверки первых 3–5 ключевых ассетов в игре.

## Этап 5 — Content production

После подтверждения систем:

- уровни;
- главы;
- задания;
- апгрейды;
- тексты;
- tutorial hints;
- баланс.

Контент по возможности data-driven, чтобы не копировать сцены и логику.

## Этап 6 — Platform integration

Подключить через общий platform layer:

- init/ready;
- lifecycle;
- ads;
- cloud save;
- player/auth при необходимости;
- leaderboard/payment только если входят в scope.

Локальный запуск без SDK обязан оставаться рабочим через fallback adapter.

## Этап 7 — Monetization pass

Проверить:

- rewarded placement;
- fullscreen opportunities;
- cooldown/state machine;
- UI недоступной рекламы;
- no-ad/adblock path;
- восстановление игры после рекламы.

## Этап 8 — QA

Пройти `14_QA_TESTING.md` и platform checklists.

Минимум:

- desktop Chromium;
- mobile Chromium;
- Safari/iOS, если доступен физически/через реальное устройство;
- разные aspect ratios;
- tab hide/show;
- ad unavailable;
- offline/transient network errors;
- reload/save;
- platform draft/debug.

## Этап 9 — Release candidate

Freeze новых фич. Только:

- blocker/critical fixes;
- визуальные дефекты;
- platform compliance;
- build-size/load-time fixes.

## Этап 10 — Release

Подготовить:

- icon;
- horizontal cover;
- screenshots;
- title/description/how-to;
- version;
- release report.

## Этап 11 — Post-release

Review: `D+1`, `D+3`, `D+7`, `D+30`.

Решение: `SCALE / IMPROVE / LEAVE / KILL`.

## Правило параллельной работы

Параллелить можно независимые задачи (например, музыка и level data). Нельзя параллелить работу, если один результат определяет размеры/контракт другого. Особенно запрещено параллельно «придумывать UI» и «генерировать весь UI asset pack» до фиксации слотов.
