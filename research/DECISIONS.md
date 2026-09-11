# Protocol Architecture Decisions

## ADR-001 — Godot 4.7.2 baseline

**Date:** 2026-09-11  
**Status:** accepted

### Context

На дату ревизии актуальный maintenance release — Godot 4.7.2 (18 Aug 2026).

### Decision

Новые web-проекты начинают с `4.7.2-stable`, пока protocol baseline не обновлён.

### Consequence

Версия фиксируется на проект и не прыгает посреди production.

---

## ADR-002 — Single-thread Web by default

**Date:** 2026-09-11  
**Status:** accepted

### Context

Web threads требуют SharedArrayBuffer/cross-origin isolation. Godot отмечает ограничения взаимодействия с third-party websites, что особенно рискованно для рекламных SDK и iframe-площадок.

### Decision

Single-thread — default. Threads только после доказанного performance blocker и отдельной platform compatibility проверки.

---

## ADR-003 — Platform adapters instead of SDK in gameplay

**Date:** 2026-09-11  
**Status:** accepted

### Decision

Local/Yandex/Pikabu реализуются адаптерами общего контракта. Gameplay не содержит разрозненных проверок платформы.

### Consequence

Одна gameplay codebase, быстрый local fallback, меньше повторной интеграции.

---

## ADR-004 — Local-first save + cloud sync

**Date:** 2026-09-11  
**Status:** accepted

### Context

Web local persistence может быть очищена/ограничена; Пикабу требует cloud saves; Яндекс предоставляет player data.

### Decision

State фиксируется локально немедленно, cloud синхронизируется батчами/debounce. Save schema versioned.

---

## ADR-005 — Production art requires real slots first

**Date:** 2026-09-11  
**Status:** accepted

### Context

Повторяющаяся ошибка прошлых проектов: красивый concept sheet не совпадает с фактическим UI, ассеты растягиваются/обрезаются.

### Decision

Graybox → real UI slots → `ASSET_MANIFEST` → concept/art direction → 3–5 assets → screenshot approval → full pack.

---

## ADR-006 — Desktop and mobile are co-equal targets

**Date:** 2026-09-11  
**Status:** accepted

### Context

Пикабу публично указывает примерно 50% desktop / 40% mobile / 10% other.

### Decision

Нельзя проектировать desktop как случайно растянутую mobile-версию. Input abstraction и responsive layout проверяются на обеих группах.

---

## ADR-007 — Protocol repo contains process, not game-specific code

**Date:** 2026-09-11  
**Status:** accepted

### Decision

Этот репозиторий — документация/checklists/templates/research. Конкретные игры живут в отдельных Git-репозиториях.

Повторяемый runtime/template code, если будет создан позже, должен иметь явное versioning и не превращать protocols repo в скрытый monolith.

---

## ADR-008 — Context-efficient AI workflow

**Date:** 2026-09-11  
**Status:** accepted

### Decision

Новая сессия читает Master + AI workflow + current project state + только релевантные протоколы. Не перечитывает весь корпус на каждом действии.

### Consequence

Меньше токенов и меньше задержек при сохранении обязательных правил.
