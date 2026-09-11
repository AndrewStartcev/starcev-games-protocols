# 04 — Web Runtime Protocol

## 1. Web baseline

Godot 4 Web требует WebAssembly + WebGL 2.0 и использует renderer `Compatibility`.

Для платформенных игр:

- C# не использовать;
- `Compatibility` обязателен;
- single-thread export — default;
- custom HTML shell использовать только как контролируемую интеграционную точку;
- экспортировать как `index.html`.

## 2. Почему threads OFF

Web threads используют `SharedArrayBuffer` и требуют cross-origin isolation (`COOP/COEP`). Это создаёт проблемы со сторонними интеграциями, рекламой и iframe-hosting. Godot сам рекомендует single-thread как более совместимый web-путь.

Threads включаются только после доказанного performance blocker и отдельного исследования совместимости конкретной площадки.

## 3. PWA policy

Для Яндекс/Пикабу по умолчанию PWA OFF.

Причины:

- нам не нужен offline-install lifecycle для платформенного iframe;
- service worker может кэшировать старые билды и усложнять обновления;
- cross-origin isolation вокруг PWA может конфликтовать со сторонними SDK;
- платформа сама контролирует жизненный цикл страницы.

PWA разрешается отдельному standalone build, но не смешивается автоматически с platform build.

## 4. Browser lifecycle

Нельзя рассчитывать, что `_process()` продолжит выполняться в background tab. Браузеры останавливают/душат animation frames и timers.

При `hidden`/platform pause:

- поставить gameplay на паузу;
- остановить/заглушить игровой звук;
- завершить безопасный local save;
- не считать elapsed gameplay time по кадрам в фоне.

При resume:

- не запускать gameplay автоматически, если до скрытия он был в меню/ручной паузе;
- синхронизировать wall-clock системы (idle income, cooldown) отдельно.

## 5. Persistence

`user://` в Web опирается на browser storage/IndexedDB и не является абсолютной гарантией:

- iframe/privacy settings могут ограничивать persistence;
- private/incognito может не сохранять данные;
- browser storage может быть очищен.

Следствие: local save — cache/fast path, а для опубликованного прогресса используем platform cloud/backend там, где доступно/обязательно.

## 6. Audio autoplay

Браузер может блокировать звук до user gesture.

Правило:

- игра не считается сломанной, если музыка не стартовала на boot;
- первый осознанный click/tap может активировать audio;
- кнопка mute доступна;
- нельзя пытаться бесконечно force-play звук.

## 7. Fullscreen и mouse capture

Браузеры разрешают fullscreen/cursor capture только из актуального user input event. Не вызывать автоматически при загрузке.

Для большинства casual-проектов отдельный forced fullscreen не нужен.

## 8. Canvas/page shell

Shell должен исключать конфликт браузера с игровым input:

- без внешних margins;
- без page scroll;
- без overscroll/swipe-to-refresh вокруг canvas;
- canvas заполняет доступную область;
- touch behavior не перехватывает игровое drag/swipe.

При custom shell рекомендуемые CSS-принципы:

```css
html, body { margin: 0; width: 100%; height: 100%; overflow: hidden; overscroll-behavior: none; }
canvas { display: block; touch-action: none; }
```

Конкретный shell сначала проверяется с SDK площадки.

## 9. HTTPS и MIME

Production — HTTPS.

Для собственного хостинга:

- `.wasm` отдавать как `application/wasm`;
- включить gzip/Brotli для `.wasm`, `.pck`, JS и text assets;
- корректный cache policy;
- версионировать URL/build для безопасного cache busting.

## 10. Networking

В Web не планировать низкоуровневые сокеты. Реалистичный набор: HTTP(S), WebSocket, WebRTC с браузерными ограничениями и CORS/same-origin.

Любой внешний API должен иметь:

- CORS;
- timeout;
- failure UI/fallback;
- отсутствие hard dependency для запуска, если это не core feature.

## 11. Custom JavaScript

Интеграция SDK выполняется через `JavaScriptBridge`/custom shell adapter.

Правила:

- JS API скрыт за platform adapter;
- callback references сохраняются до завершения вызова;
- избегать `JavaScriptBridge.eval()` для постоянной архитектуры, если можно вызвать явный interface;
- секреты никогда не помещаются в JS/GDScript bundle.

## 12. Browser QA

Godot Editor run не заменяет Web export test.

Проверять:

- DevTools Console;
- Network waterfall;
- transferred bytes;
- `.wasm` MIME/compression;
- resize;
- touch emulation + физический mobile;
- tab hide/show;
- reload;
- adblock path;
- slow/failed network path.
