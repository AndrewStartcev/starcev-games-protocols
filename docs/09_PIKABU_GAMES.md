# 09 — Пикабу Игры

Актуальность проверена: `2026-09-11`.

Официальная документация: https://games.pikabu.ru/sdk/docs/

## 1. Важная специфика площадки

На странице для разработчиков Пикабу указывает аудиторию примерно:

- 50% desktop;
- 40% mobile;
- 10% другое;
- 80% старше 25 лет.

Следствие: desktop UX — first-class, а не растянутый mobile UI.

## 2. Hosting

На текущий момент build размещается на собственном сервере; в Студии указывается URL игры.

Production hosting должен иметь:

- HTTPS;
- корректные MIME;
- gzip/Brotli;
- cache/version strategy;
- стабильный публичный URL;
- CORS для собственного backend при необходимости.

## 3. Boot

Порядок:

```text
load Pikabu SDK
→ PkbSDK.init() exactly once
→ init platform adapter
→ load/prepare game
→ optional mobile preloader flow
→ first interactive screen ready
→ sdk.gameStarted()
```

`gameStarted()` вызывается только после готовности игры.

## 4. Технические требования blocker

- последние desktop/mobile Chrome, Яндекс Браузер, Safari;
- русский UI по умолчанию;
- корректный responsive layout;
- autopause при tab switch, ads, auth, payment UI;
- background audio off;
- игра работает с adblock;
- облачные сохранения обязательны для публикации;
- никакой обязательной регистрации во внешнем сервисе.

## 5. Content requirements

- минимум 30 минут суммарного gameplay;
- никаких запрещённых азартных/сексуальных/явно нарушающих авторские права материалов;
- promo соответствует реальной игре;
- обязательные promo: квадрат от 1024×1024 и горизонтальный от 1920×1080 16:9.

## 6. Ads

Пикабу предоставляет:

- Preloader;
- Fullscreen;
- Rewarded.

Перед каждым показом:

1. `isSupported`/`canShow()`;
2. pause gameplay/audio/input;
3. `await show()`;
4. обработать result;
5. восстановить предыдущее состояние.

### Preloader

- доступен только mobile;
- только до `sdk.gameStarted()`;
- может выполняться параллельно загрузке ресурсов;
- не делать его основой всей monetization strategy.

### Fullscreen

- natural breaks only;
- текущая документация указывает минимальный интервал 120 секунд между фактическими показами;
- AdsService имеет общий gate и не вызывает spam.

### Rewarded

- только явный opt-in;
- `reward == true` → выдать награду;
- `false`/adblock/skip → не выдавать;
- Пикабу предупреждает, что чрезмерный rewarded может ухудшить retention/session metrics.

## 7. Ad unavailable/adblock

Игра должна быть полностью играбельна при:

- `canShow() == false`;
- `rendered == false`;
- adblock;
- SDK ad error.

Rewarded-only bonus может стать временно недоступным, но core progression не блокируется.

## 8. Player ID и авторизация

`sdk.player.id` доступен и anonymous, но для anonymous хранится локально в браузере.

После авторизации ID может измениться, если у аккаунта уже была история этой игры на другом устройстве.

Поэтому backend/save layer обязан:

- читать актуальный `sdk.player.id` после auth;
- обрабатывать `userAuthorized`;
- не продолжать слепо писать save под старый id;
- иметь понятный merge/association flow.

## 9. Cloud saves

Поскольку Пикабу требует cloud saves, а Player API даёт идентичность, общий вариант Starcev Games:

- собственный lightweight backend;
- клиент получает `player.getSignedData()`;
- backend проверяет JWT/HMAC подпись секретом игры;
- только после проверки доверяет `player.id`;
- save хранится server-side;
- secret key никогда не попадает в клиент.

Подробно: `10_SAVES.md`, `18_SECURITY_BACKEND.md`.

## 10. Performance/featuring

Для продвижения Пикабу рекомендует:

- быстрый старт, ориентир до 10 секунд на стабильном соединении;
- стабильность без фризов, особенно mobile;
- прогрессию/цели;
- retention systems;
- качественный visual/audio;
- обновления после публикации.

## 11. Analytics

Студия даёт по устройствам:

- unique players;
- sessions;
- average session time;
- average time/player;
- load speed;
- ads: requests, impressions, show share, revenue;
- purchases.

CSV выгружать/фиксировать на D+1/3/7/30 для портфельной аналитики.

## 12. Pikabu release checklist

См. `checklists/PIKABU.md`.
