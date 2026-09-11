# Pikabu Games Release Checklist

Перед отправкой перечитать: https://games.pikabu.ru/sdk/docs/requirements/tech

## Hosting

- [ ] Игра размещена на стабильном HTTPS URL.
- [ ] MIME корректны.
- [ ] gzip/Brotli включены.
- [ ] Cache/versioning не отдаёт старый build после обновления.
- [ ] Есть предыдущий рабочий build для rollback.

## SDK / boot

- [ ] `PkbSDK.init()` вызывается один раз.
- [ ] `gameStarted()` вызывается только после фактической готовности.
- [ ] Preloader, если используется, только mobile и до `gameStarted()`.
- [ ] SDK failure/no-ad не приводит к hard lock.

## Platform UX

- [ ] Desktop Chrome проверен.
- [ ] Mobile Chrome проверен.
- [ ] Яндекс Браузер проверен настолько, насколько доступен.
- [ ] Safari desktop/mobile проверен настолько, насколько доступен.
- [ ] Русский UI показывается по умолчанию.
- [ ] Desktop и mobile UI/input адаптированы.
- [ ] Tab switch ставит игру на паузу.
- [ ] Ads/auth/payment UI ставят gameplay на паузу.
- [ ] Background sound выключается.
- [ ] Игра работает с adblock.

## Content / promo

- [ ] Суммарный gameplay ≥30 минут.
- [ ] Square promo ≥1024×1024.
- [ ] Horizontal promo ≥1920×1080, 16:9.
- [ ] Promo не вводит в заблуждение.
- [ ] Нет сторонних ссылок кроме допустимой поддержки.

## Ads

- [ ] `canShow()` проверяется непосредственно перед show.
- [ ] Fullscreen не запрашивается агрессивно и соблюдает минимум 120 секунд между фактическими показами.
- [ ] Rewarded только opt-in.
- [ ] Reward выдаётся только при `reward == true`.
- [ ] Core loop работает при adblock/no-fill/error.

## Save / identity

- [ ] Cloud saves реализованы — обязательное требование публикации.
- [ ] Backend проверяет signed player data, если используется собственное хранилище.
- [ ] Secret key не находится в клиенте/Git.
- [ ] `userAuthorized`/смена player id обработаны.
- [ ] Merge/conflict strategy проверена.

## Финал

- [ ] `QA_REPORT = PASS`.
- [ ] Public URL соответствует проверенному commit/build.
- [ ] Requirements перечитаны перед публикацией.
