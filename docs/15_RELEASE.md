# 15 — Release Protocol

## 1. Release freeze

После статуса RC:

- не добавлять новые mechanics;
- не менять architecture без blocker;
- не заменять весь visual style;
- исправлять bugs, compliance, performance и явные visual defects.

## 2. Versioning

Практичный формат:

```text
0.x.y — до первой публичной стабильной версии
1.0.0 — первый стабильный public release
1.x.y — content/features/fixes
```

В `PROJECT_STATE` фиксируется engine version и game version.

## 3. Build reproducibility

Release report должен содержать:

- Git commit SHA;
- Godot version;
- export preset/platform;
- date;
- build URL/archive name;
- known issues.

Нельзя отправлять на модерацию «какой-то локальный архив», происхождение которого неизвестно.

## 4. Promo assets

Promo создаются после финального/почти финального visual pass.

Они должны:

- показывать реальную стилистику;
- не обещать отсутствующую механику;
- быть читаемыми thumbnail-size;
- не содержать запрещённый clickbait;
- соответствовать размерам площадки.

Пикабу: square ≥1024×1024, horizontal ≥1920×1080 16:9.

## 5. Metadata

Подготовить:

- название;
- короткое описание;
- полное описание;
- how to play;
- жанр/tags;
- возрастной рейтинг;
- локализации;
- support contact.

Текст должен соответствовать реальной игре и управлению.

## 6. Yandex package

Проверить:

- ZIP uncompressed ≤100 MB;
- root `index.html`;
- no Cyrillic/spaces в paths;
- release export, не debug мусор;
- SDK integration;
- draft test;
- Yandex checklist.

## 7. Pikabu hosting

Перед отправкой:

- HTTPS URL стабилен;
- cache не отдаёт старый build;
- compression работает;
- gameStarted timing корректен;
- cloud saves production backend доступен;
- no secrets в client;
- Pikabu checklist.

## 8. Rollback

Для self-hosted build хранить минимум предыдущую рабочую версию/артефакт, чтобы быстро откатить Critical regression.

Не удалять рабочий build сразу после выкладки нового.

## 9. Post-release hooks

Сразу после публикации:

- записать release date;
- поставить review D+1/D+3/D+7/D+30;
- собрать baseline metrics;
- не менять 10 вещей одновременно до первого анализа, если нет blocker.

## 10. Release report

Использовать `templates/RELEASE_REPORT.md`.
