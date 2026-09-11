# 08 — Яндекс Игры

Актуальность проверена: `2026-09-11`.

Официальная документация: https://yandex.ru/dev/games/doc/ru/

## 1. Release requirements, которые считаем blocker

- SDK Яндекс Игр встроен и корректно инициализирован.
- `LoadingAPI.ready()` вызывается, когда игра действительно готова к взаимодействию и собственного loader уже нет.
- Gameplay marking (`GameplayAPI.start/stop`) соответствует фактическому gameplay.
- Игра корректно обрабатывает platform pause/resume.
- Resize/orientation не обрезают значимые элементы.
- Нет browser scrollbar/swipe-to-refresh вокруг игры.
- Архив: содержимое до сжатия ≤ 100 МБ.
- В корне один `index.html`.
- Имена файлов/папок без пробелов и кириллицы.
- Основной контент >10 минут или обеспечена достаточная реиграбельность согласно требованиям.

Внутренний стандарт Starcev Games выше минимального: проектировать на 30+ минут суммарного игрового процесса/реиграбельность, чтобы одна codebase легче проходила Пикабу.

## 2. Boot order

Рекомендуемый смысловой порядок:

```text
load Yandex SDK
→ YaGames.init()
→ subscribe lifecycle events
→ init game services
→ load/prepare first interactive screen
→ apply save
→ hide own loader
→ LoadingAPI.ready()
→ player starts gameplay
→ GameplayAPI.start()
```

`LoadingAPI.ready()` нельзя отправлять просто после `YaGames.init()`.

## 3. Gameplay lifecycle

`GameplayAPI.start()` — только когда пользователь реально играет.

`GameplayAPI.stop()` — при:

- pause menu;
- level/result transition без gameplay;
- game over;
- advertisement;
- скрытии вкладки/platform pause.

Яндекс также предоставляет `game_api_pause` / `game_api_resume`; они могут приходить при рекламе, покупках, tab switch/minimize. Обрабатывать через общий pause coordinator.

## 4. Реклама

Использовать только SDK Яндекса.

Fullscreen:

- natural break;
- перед показом gameplay/audio paused;
- `onClose(wasShown)` не трактовать автоматически как «точно был показ» без параметра;
- `onError` не ломает продолжение игры.

Rewarded:

- пользователь явно выбрал просмотр;
- награда только по соответствующему successful callback;
- закрытие/ошибка → нет награды;
- UI должен переживать недоступность формата.

Платформа сама управляет частью частоты fullscreen, но AdsService всё равно предотвращает спам и двойные запросы.

## 5. Player data

Яндекс Player Data:

- `setData` — до 200 КБ на игрока;
- `setData/getData` имеют rate limits (100 запросов за 5 минут согласно текущей документации);
- `setStats/getStats/incrementStats` — отдельные числовые данные и свои лимиты.

Следствие: batching/debounce обязателен. Не сохранять облако на каждый клик.

`flush: true` использовать в действительно критичных checkpoint, а не постоянно.

## 6. iOS/local storage

Документация Яндекса отдельно предупреждает о риске потери localStorage на новых iOS при собственном домене. Поэтому опубликованный прогресс нельзя строить только на browser-local persistence.

## 7. Авторизация

Игра не требует сторонней авторизации.

Яндекс auth dialog — только после осознанного действия игрока и объяснения преимущества.

Не показывать login wall на первом кадре.

## 8. Локализация

Источник языка: `ysdk.environment.i18n.lang`.

Минимум — заявленные в карточке локализации. Внутренняя архитектура строк должна позволять добавлять языки без изменения gameplay code.

## 9. Device info

Для адаптации можно использовать `ysdk.deviceInfo`, но layout не должен полностью зависеть от одного device flag. Реальный viewport остаётся источником размеров.

## 10. Лидерборды

Platform leaderboard требует авторизацию для ряда операций. Поэтому leaderboard не должен блокировать базовый gameplay.

Если leaderboard входит в MVP:

- проверить method availability;
- graceful fallback для anonymous;
- не спамить score calls.

## 11. In-app purchases

Не включать «на всякий случай».

Если появляются покупки:

- consumable начисляется до consume;
- необработанные покупки проверяются после запуска;
- для серьёзной экономики использовать signed/server-side verification;
- purchase flow интегрирован с pause/audio.

## 12. Yandex release checklist

См. `checklists/YANDEX.md`.

Перед каждой отправкой на модерацию перечитать официальный requirements changelog/страницу требований: правила меняются.
