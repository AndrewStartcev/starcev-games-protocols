# 13 — Performance & Build Size

## 1. Принцип

Сначала измеряем, потом оптимизируем. Но performant design закладывается заранее.

Для Web производительность включает одновременно:

- download size;
- time to first interaction;
- CPU frame time;
- GPU frame time;
- memory/VRAM;
- stutters/loading spikes;
- mobile thermals.

## 2. Targets

Внутренние цели, не требования площадок:

- gameplay target: 60 FPS на типичном desktop;
- минимум приемлемого: стабильные 30 FPS на слабом целевом mobile для casual;
- без систематических frame spikes >50–100 ms в обычном loop;
- Pika Game Ready: стремиться ≤10 сек на стабильном соединении, поскольку это их рекомендация;
- Yandex uncompressed archive: MUST ≤100 МБ.

Цели корректируются жанром и данными.

## 3. Build size

Основные рычаги:

- удалять unused runtime assets;
- не класть source files в импортируемую часть;
- большие 2D backgrounds — lossy WebP, если quality проходит review;
- не хранить лишние 4K textures;
- mono audio где достаточно;
- разумный bitrate music;
- custom optimized export template рассматривать только после измерения, а не в каждом MVP.

Godot Web/WASM хорошо сжимается gzip/Brotli. На собственном сервере compression обязателен.

## 4. Texture memory

Lossless/lossy disk compression не означает низкую VRAM: 2D texture обычно декодируется в память.

Следовательно:

- crop transparent whitespace;
- alpha только когда нужна;
- 2048/4096 использовать осознанно;
- 3D VRAM compression/Basis применять после visual/perf test;
- UI/pixel art — не портить VRAM compression артефактами.

## 5. Scene/node cost

Не создавать тысячи Nodes, если данные/рисование можно представить проще.

Для массовых однотипных объектов рассмотреть:

- pooling;
- custom drawing;
- MultiMesh для 3D/подходящих 2D случаев;
- data structures вместо Node на каждую чисто логическую сущность.

Решение принимается по profiler.

## 6. `_process()` hygiene

- выключать processing для неактивных объектов;
- не искать nodes по строковому path каждый frame;
- не выделять большие arrays/dictionaries в hot loop без причины;
- кэшировать ссылки;
- timers/signals вместо постоянного polling, когда событие дискретное;
- избегать тяжёлых JSON/save operations в frame loop.

## 7. 2D

- минимизировать огромные overdraw layers/transparency;
- particles ограничивать по count/lifetime;
- blur/full-screen shader effects тестировать на mobile;
- CanvasItem перерисовывать только при необходимости, если используется custom `_draw()`;
- для pixel art соблюдать правильный filtering/integer scale.

## 8. 3D Web profile

Для будущих low-poly 3D:

- Compatibility renderer;
- простые materials;
- минимум shader variants;
- bake/static lighting где подходит;
- ограниченное число realtime lights/shadows;
- LOD/visibility ranges/occlusion — по сцене;
- texture budget;
- low-poly meshes;
- particles умеренно;
- разрешение 3D можно снижать отдельно от crisp UI при GPU bottleneck.

## 9. Loading/stutter

Не подгружать тяжёлый ресурс впервые в момент, когда игрок ожидает мгновенную реакцию.

Для predictable transitions:

- preload critical small resources;
- заранее instantiate/warm common scenes при необходимости;
- большие load operations ставить между gameplay segments;
- progress UI только если пауза действительно заметна.

## 10. Browser profiling

Проверить:

- Godot profiler;
- браузер Performance;
- Network tab;
- Memory, если есть рост;
- CPU throttling/mobile;
- transferred vs resource size;
- console WebGL errors.

Оптимизация считается успешной только после повторного измерения.

## 11. Quality tiers

Если игра тяжёлая, предусмотреть `LOW/HIGH` или автоматические web/mobile settings, но не создавать настройку ради настройки.

Можно отключать/уменьшать:

- particles;
- shadows;
- post effects;
- 3D render scale;
- decorative animations.

Gameplay/readability не страдают.
