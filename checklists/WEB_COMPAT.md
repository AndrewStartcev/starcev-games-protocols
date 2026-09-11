# Web Compatibility Checklist

## Export

- [ ] Godot Compatibility renderer.
- [ ] Single-thread Web export.
- [ ] PWA OFF для platform build, если отдельно не обосновано.
- [ ] GDExtension OFF, если не требуется.
- [ ] `index.html` entry point.

## Browser shell

- [ ] `html/body` без margins/scroll.
- [ ] Canvas занимает доступную область.
- [ ] `overscroll-behavior` не мешает игре.
- [ ] `touch-action` не конфликтует с gameplay.
- [ ] Fullscreen/cursor capture не вызывается вне user gesture.

## Audio

- [ ] Игра не зависает из-за blocked autoplay.
- [ ] Первый user gesture корректно активирует звук.
- [ ] Background audio mute работает.

## Lifecycle

- [ ] `visibilitychange`/platform pause обрабатываются.
- [ ] Background tab не используется как надёжный realtime timer.
- [ ] Wall-clock механики пересчитываются по времени, если нужны.

## Storage

- [ ] Local save проверен после reload.
- [ ] Private/incognito/storage-cleared сценарий не разрушает архитектуру.
- [ ] Published important progress имеет cloud path там, где требуется.

## Network

- [ ] HTTPS.
- [ ] `.wasm` корректный MIME.
- [ ] Compression включена на self-hosted build.
- [ ] API имеет CORS/timeout/fallback.
- [ ] Offline/transient error не создаёт бесконечный loader.

## DevTools

- [ ] Console clean от наших errors.
- [ ] Network waterfall проверен.
- [ ] Transfer size записан.
- [ ] Performance profile сделан при проблемах.
