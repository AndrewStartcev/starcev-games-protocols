# Pre-Release Checklist

## Build

- [ ] Release candidate привязан к Git commit SHA.
- [ ] Версия игры зафиксирована.
- [ ] Версия Godot зафиксирована.
- [ ] Нет debug/test UI.
- [ ] Нет секретов/ключей в клиентском коде.
- [ ] Нет неиспользуемых тяжёлых runtime assets.

## Gameplay

- [ ] Cold start → menu → gameplay → result → replay работает.
- [ ] Нет Blocker/Critical bugs.
- [ ] Tutorial/onboarding соответствует текущей механике.
- [ ] 30+ минут gameplay/replayability проверены или обоснована другая модель для конкретной площадки.

## UI / input

- [ ] Desktop mouse проверен.
- [ ] Mobile touch проверен.
- [ ] Resize matrix пройдена.
- [ ] Ничего важного не обрезается.
- [ ] Browser gestures не ломают drag/swipe.
- [ ] Dynamic text не выпадает из panels.

## Save

- [ ] Fresh save.
- [ ] Reload.
- [ ] Existing save.
- [ ] Cloud unavailable.
- [ ] Migration предыдущей schema, если была.
- [ ] Critical rewards/purchases идемпотентны.

## Ads

- [ ] Fullscreen success/error/no-fill.
- [ ] Rewarded success/fail/close/no-fill.
- [ ] Double click не создаёт double request/reward.
- [ ] Gameplay/audio/input корректно pause/resume.
- [ ] Реклама не блокирует core loop при недоступности.

## Audio / lifecycle

- [ ] First interaction разблокирует audio при необходимости.
- [ ] Mute сохраняется.
- [ ] Tab hide → sound off.
- [ ] Resume восстанавливает правильное состояние.
- [ ] Реклама поверх manual pause не запускает gameplay после закрытия.

## Web

- [ ] Проверен реальный Web export.
- [ ] DevTools Console без наших красных ошибок.
- [ ] Network errors обработаны.
- [ ] WASM/MIME/compression корректны для self-hosted build.
- [ ] Load time измерен.

## Visual

- [ ] Screenshot реальной игры сопоставлен с approved art direction.
- [ ] Нет растянутых/обрезанных production assets.
- [ ] Promo соответствует реальному продукту.

## Release docs

- [ ] `QA_REPORT.md` заполнен.
- [ ] Platform checklist пройден.
- [ ] `RELEASE_REPORT.md` заполнен.
- [ ] Назначены reviews D+1 / D+3 / D+7 / D+30.
