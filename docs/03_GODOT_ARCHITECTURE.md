# 03 — Godot Architecture

База: официальные Godot best practices — scene organization, loose coupling, project organization, Resources, typed GDScript.

## 1. Структура проекта

Рекомендуемая отправная точка:

```text
project.godot
addons/
assets/
  audio/
  fonts/
  ui/
  backgrounds/
  gameplay/
  promo/
content/
scenes/
  main/
  menu/
  gameplay/
  ui/
scripts/
  gameplay/
  ui/
services/
  platform/
  save/
  ads/
  audio/
  analytics/
localization/
docs/.gdignore
source_assets/.gdignore
```

Файлы/папки — `snake_case`. Node names — `PascalCase`. Это снижает проблемы case sensitivity после экспорта.

`docs/` и тяжёлые исходники дизайна не должны импортироваться Godot — использовать `.gdignore` или хранить вне Godot import scope.

## 2. Entry point

Игра имеет явный `Main`.

Типовая структура:

```text
Main
├── World / GameRoot
├── UI
└── RuntimeSystems (если нужны локальные системные nodes)
```

`Main` координирует крупные переходы. Он не должен содержать всю gameplay-логику.

## 3. Сцены как автономные компоненты

Сцена должна по возможности:

- иметь одну понятную ответственность;
- не знать абсолютные пути к внешним siblings;
- не искать глобально случайные nodes;
- выдавать события через signals;
- получать внешние зависимости от родителя/контекста;
- нормально инстанцироваться повторно.

Предпочитать loose coupling. Sibling-коммуникацию медиирует общий родитель/контроллер.

## 4. Signals vs direct calls

Использовать signal, когда дочерний компонент сообщает: «событие произошло».

Использовать direct method call, когда родитель явно запускает поведение дочернего компонента.

Не строить глобальную шину событий для всего подряд: это ухудшает трассировку причин.

## 5. Autoload policy

Autoload допустим, если система одновременно:

- нужна в разных сценах;
- живёт дольше отдельных сцен;
- имеет ясный API;
- может существовать изолированно.

Типовые кандидаты:

- `PlatformService`;
- `SaveService`;
- `AudioService`;
- `AnalyticsService` (опционально);
- общий `GameState` только при реальной необходимости.

Не делать autoload для каждого manager-класса.

## 6. Resources и data-driven content

Gameplay data по возможности хранить в Resources/структурированных данных, а не копировать код.

Подходит для:

- level configs;
- item definitions;
- upgrade tables;
- chapters;
- enemy stats;
- card definitions;
- localization metadata.

Node = поведение/участие в SceneTree. Resource = данные.

## 7. Typed GDScript

Для production кода рекомендуется static typing:

```gdscript
var score: int = 0
func add_score(value: int) -> void:
    score += value
```

Преимущества: раннее обнаружение ошибок, autocomplete, читаемость, часть оптимизированных opcodes.

В быстром prototype допускается более динамичный стиль, но перед architecture freeze критические сервисы и контракты типизируются.

## 8. Ошибки и async

Каждый внешний async вызов должен иметь:

- success path;
- failure path;
- timeout/fallback, если внешний сервис способен зависнуть;
- защиту от повторного вызова;
- лог с понятным prefix.

SDK failure не должен превращаться в soft lock игры.

## 9. State machines

Для сложных жизненных циклов использовать явное состояние, особенно:

- `BOOTING`;
- `MENU`;
- `PLAYING`;
- `PAUSED`;
- `AD`;
- `AUTH_DIALOG`;
- `RESULT`;
- `ERROR_RECOVERABLE`.

Не полагаться на десяток независимых boolean (`is_paused`, `ad_open`, `menu_open`, ...), если комбинации становятся неоднозначными.

## 10. Pause model

Причины паузы могут вкладываться: пользователь открыл pause menu, затем началась реклама, затем вкладка скрылась.

Поэтому рекомендуется reason-based pause lock/set:

```text
pause_reasons = { USER_MENU, AD, PAGE_HIDDEN, AUTH }
```

Gameplay возобновляется только когда набор причин пуст. Это предотвращает ошибку «закрылась реклама → игра запустилась под открытым меню».

## 11. Логи

Production logging должен быть компактным и категоризированным:

```text
[BOOT]
[PLATFORM]
[SAVE]
[ADS]
[AUDIO]
[GAME]
```

Секреты, signed tokens, purchase tokens и приватные данные не логировать.

## 12. Запрещённые анти-паттерны

- один `game.gd` на тысячи строк;
- SDK calls внутри gameplay button scripts;
- hardcoded абсолютные paths между независимыми сценами;
- save JSON формируется в десяти местах;
- копирование уровней вместе с логикой вместо data-driven;
- UI layout вручную через магические координаты без Containers/anchors там, где UI должен адаптироваться;
- бесконтрольные Autoload managers.
