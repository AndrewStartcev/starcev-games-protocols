# 20 — Common Failures / Анти-повтор

Этот файл — каталог ошибок, которые нельзя переносить из игры в игру.

## 1. Красивый concept ≠ реальная игра

### Симптом

Concept выглядит дорого, но после интеграции панели растянуты, объекты обрезаны, композиция развалилась.

### Причина

Ассеты создавались до реальных UI slots и без stretch contract.

### Правило

Graybox → реальные размеры → `ASSET_MANIFEST` → 3–5 test assets → screenshot review → full pack.

---

## 2. Целый экран как один PNG

### Симптом

UI нельзя нормально адаптировать, кнопки/текст не интерактивны, resize ломает композицию.

### Правило

Background/decor могут быть крупным art asset. Интерактивный UI собирается из отдельных элементов/StyleBox/9-patch + dynamic text.

---

## 3. Mobile UI просто растянули на desktop

### Симптом

Огромные кнопки, пустые зоны, слабая информационная плотность, неудобная мышь.

### Правило

Desktop и mobile — co-equal layouts. Общая дизайн-система, но допускаются разные arrangement/scale rules.

---

## 4. Hardcoded coordinates вместо responsive layout

### Симптом

На одном разрешении красиво, на другом overlap/crop.

### Правило

Anchors + Containers + minimum sizes + safe zones. Магические координаты только для контролируемой gameplay geometry.

---

## 5. SDK размазан по gameplay

### Симптом

Добавление Пикабу ломает Яндекс, local run требует platform SDK, реклама вызывается из случайных кнопок.

### Правило

Только `Platform/Ads/Save` adapters. Gameplay вызывает общий контракт.

---

## 6. Save на каждый клик

### Симптом

Request storm, rate limits, лишняя задержка.

### Правило

Local immediate + cloud dirty + debounce/batching + critical checkpoints.

---

## 7. Только localStorage/user://

### Симптом

После очистки storage/другого устройства прогресс исчезает; Пикабу не проходит требования.

### Правило

Local-first, но важный опубликованный прогресс имеет cloud/backend path согласно платформе.

---

## 8. Rewarded выдаёт награду на close/error

### Симптом

Игрок получает бонус без успешного просмотра или дважды.

### Правило

Reward только подтверждённым success/reward callback + idempotent request id + save после начисления.

---

## 9. После рекламы игра сама возобновилась в неправильном состоянии

### Симптом

Открыто pause menu, закрылась реклама — gameplay идёт под меню.

### Правило

Reason-based pause set. Закрытие рекламы снимает только `AD`, а не глобальную паузу.

---

## 10. Ad unavailable блокирует progression

### Симптом

Spinner навсегда, кнопка обязательного продолжения не работает при adblock/no-fill.

### Правило

Core loop независим от доступности рекламы. Rewarded — optional bonus.

---

## 11. Включены Web threads «потому что быстрее»

### Симптом

Проблемы SharedArrayBuffer/COOP/COEP, third-party ads/iframe integration.

### Правило

Single-thread default. Threads — только после measured performance blocker и platform test.

---

## 12. PWA/service worker случайно кэширует старую игру

### Симптом

После релиза часть игроков получает старые assets/build.

### Правило

PWA OFF для platform builds по умолчанию. Self-hosted cache versioning проверяется отдельно.

---

## 13. Final art до проверки core loop

### Симптом

Дни работы уходят на игру, которая неинтересна или требует другой UX.

### Правило

До `GO` — graybox/prototype. Финальный art после vertical slice/slot freeze.

---

## 14. Массовая генерация ассетов одним заходом

### Симптом

20 файлов сделаны в неверных пропорциях/стиле и требуют полной переделки.

### Правило

Сначала 3–5 ключевых assets → интеграция → screenshot → approve → batch.

---

## 15. Слишком большой контекст для AI

### Симптом

Каждая сессия перечитывает весь проект и протоколы, лимиты заканчиваются быстрее фактической работы.

### Правило

`PROJECT_STATE` + current task + 1–3 relevant protocols + targeted code reads. Не читать всё без причины.

---

## 16. Агент молча расширяет scope

### Симптом

Во время asset-задачи меняется код/баланс; во время bugfix делается большой refactor.

### Правило

Scope discipline. Вне задачи — фиксируем suggestion/decision и спрашиваем/переходим отдельным milestone.

---

## 17. Editor test принимается за Web QA

### Симптом

В браузере появляются autoplay, JS, WebGL, resize, storage или SDK ошибки, которых не было в Editor.

### Правило

Каждый RC проходит Local Web и platform draft/public-host test.

---

## 18. Оптимизация по запросам рекламы вместо денег/retention

### Симптом

Запросов стало больше, impressions/player или session quality не выросли, игроки уходят.

### Правило

Смотреть funnel `opportunity → request → impression → revenue` вместе с session/playtime/return metrics.

---

## Как пополнять каталог

Добавлять ошибку сюда, если она:

- уже повторилась;
- имеет существенную стоимость по времени/деньгам/качеству;
- имеет понятное профилактическое правило.

Если ошибка относится к узкой системе, также обновить основной тематический protocol.
