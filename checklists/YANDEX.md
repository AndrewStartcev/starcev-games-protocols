# Yandex Games Release Checklist

Перед отправкой перечитать актуальные требования: https://yandex.ru/dev/games/doc/ru/concepts/requirements

## SDK

- [ ] SDK встроен.
- [ ] SDK init соответствует текущей документации.
- [ ] `LoadingAPI.ready()` вызывается только после фактической готовности игры.
- [ ] `GameplayAPI.start()` вызывается только при реальном gameplay.
- [ ] `GameplayAPI.stop()` вызывается при логической остановке gameplay.
- [ ] `game_api_pause/resume`, если используются, не конфликтуют с общим pause state.

## Ads

- [ ] Только реклама через SDK Яндекс Игр.
- [ ] Fullscreen только в логических паузах.
- [ ] Rewarded только после явного действия игрока.
- [ ] Reward выдаётся только после successful rewarded callback.
- [ ] Возврат после рекламы не теряет прогресс.

## UX / technical

- [ ] Game complete, no placeholders.
- [ ] Управление описано.
- [ ] Resize horizontal/vertical/diagonal проверен.
- [ ] Rotate/minimize/long press/gestures/history scenarios не ломают игру.
- [ ] Нет browser scrollbar/swipe-to-refresh конфликта.
- [ ] Нет сторонней обязательной авторизации.
- [ ] Yandex auth только по осознанному действию.

## Content

- [ ] Основной контент >10 минут либо есть достаточная реиграбельность.
- [ ] Внутренний стандарт Starcev Games 30+ минут выполнен, если жанр позволяет.

## Package

- [ ] Uncompressed archive ≤100 MB.
- [ ] В корне ровно корректная точка входа `index.html`.
- [ ] Paths без кириллицы и пробелов.
- [ ] Draft build протестирован.
- [ ] Browser console проверена внутри draft.

## Save

- [ ] Player Data calls batched/debounced.
- [ ] Snapshot укладывается в лимиты платформы.
- [ ] Нет save request на каждый клик.
- [ ] Local-only progress не является единственной защитой опубликованного прогресса, если потеря значима.

## Финал

- [ ] Metadata/promos соответствуют игре.
- [ ] `QA_REPORT = PASS`.
- [ ] Требования перечитаны в день/перед отправкой при давней ревизии protocols.
