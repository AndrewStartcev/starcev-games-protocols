# 10 — Save Protocol

## 1. Цель

Игрок не должен думать о сохранениях. Прогресс должен переживать обычный reload, временную недоступность сети и переход между сессиями.

Базовая модель: **local-first + cloud sync**.

## 2. Единая save schema

Минимальный envelope:

```json
{
  "schema_version": 1,
  "revision": 42,
  "game_version": "1.0.3",
  "updated_at_ms": 0,
  "player": {},
  "progress": {},
  "economy": {},
  "settings": {}
}
```

`schema_version` — версия формата данных, не версия игры.

`revision` увеличивается при значимом изменении snapshot.

## 3. Что хранить отдельно

Не смешивать всё в один бесконтрольный Dictionary.

Рекомендуемые секции:

- `progress`: уровни, главы, unlocks;
- `economy`: currency, owned items;
- `stats`: high score, totals;
- `settings`: mute, volume, accessibility;
- `meta`: schema/revision/version.

## 4. Local save

При значимом событии:

1. обновить in-memory state;
2. сформировать нормализованный snapshot;
3. записать local;
4. пометить cloud dirty;
5. schedule cloud sync с debounce.

Local write должен быть быстрым и не зависеть от сети.

## 5. Cloud debounce

Нельзя делать cloud request на каждый клик.

Стартовый внутренний ориентир:

- debounce 3–10 секунд после серии изменений;
- immediate/forced sync на редких critical checkpoint;
- sync при уходе в меню/result/level complete;
- best-effort sync перед page hide, но не рассчитывать, что async обязательно успеет завершиться.

Конкретное значение зависит от механики.

## 6. Значимые save events

Типовые:

- level complete;
- purchase/upgrade;
- currency batch result;
- chapter unlock;
- rewarded reward granted;
- game over/result;
- settings changed;
- session checkpoint.

Не считать particle/animation state частью persistent save.

## 7. Boot load order

```text
load local snapshot
→ init platform/player
→ request cloud snapshot
→ validate versions
→ resolve local/cloud
→ apply one canonical snapshot
→ persist canonical locally
→ gameplay ready
```

Игра может показать menu/loader, но не должна позволять необратимые действия до завершения конфликта save, если cloud state способен заменить прогресс.

## 8. Conflict strategy

Один универсальный алгоритм опасен, поэтому данные классифицируются.

### Merge-safe

Можно объединять:

- achievements: union;
- max level: max;
- high score: max;
- discovered items: union.

### Transactional/non-merge-safe

Нельзя просто суммировать:

- premium currency;
- обычную валюту после покупок/трат;
- consumables;
- inventory quantities.

Для простых client-only игр использовать canonical snapshot с revision/timestamp и понятным last-write policy. При реальных платежах/ценной экономике backend становится authoritative.

## 9. Save migration

После публикации нельзя просто менять ключи.

В коде:

```text
v1 → v2
v2 → v3
...
```

Migration:

- идемпотентна;
- не удаляет неизвестные данные без причины;
- после успешной миграции save сразу записывается в новом формате;
- тестируется на копии старого реального snapshot.

## 10. Corruption recovery

Если JSON/данные повреждены:

- не crash;
- попытаться backup/previous snapshot, если есть;
- если восстановления нет — создать default save;
- логировать причину;
- не зациклить загрузку одного повреждённого save.

Для локального сохранения полезно держать `current` + один `backup` при разумном размере.

## 11. Яндекс

Использовать Player Data для cloud state.

Текущие важные лимиты: `setData` до 200 КБ на игрока и rate limits. Поэтому snapshot компактный и batching обязателен.

`flush: true` — только critical checkpoint.

Local `user://` не считать единственным источником истины, особенно из-за browser/iOS storage поведения.

## 12. Пикабу

Cloud saves обязательны.

Рекомендуемая схема:

```text
Pikabu signed player JWT
→ Starcev backend validates HMAC
→ canonical player id
→ GET/PUT save
```

При `userAuthorized` player id может измениться. SaveService должен остановить запись под старым id, получить новую идентичность и выполнить merge/selection flow.

## 13. Rewarded и save

Награда:

1. rewarded подтвердил `reward=true`;
2. изменить state;
3. local save;
4. пометить cloud dirty;
5. UI показывает награду.

Если награда редкая/ценная, cloud push можно форсировать после local write, но не выдавать reward повторно из-за retry UI.

## 14. Тесты

Обязательно:

- fresh install/no save;
- reload после изменения;
- offline boot с local;
- cloud unavailable;
- cloud старее local;
- cloud новее local;
- corrupted local;
- migration from previous schema;
- auth/id change на Пикабу;
- rapid changes не создают request storm.
