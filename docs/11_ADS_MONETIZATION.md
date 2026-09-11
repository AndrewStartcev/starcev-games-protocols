# 11 — Ads & Monetization Protocol

Цель — увеличить доход на игрока без разрушения retention и без platform violations.

## 1. AdsService как state machine

Минимальные состояния:

```text
IDLE
CHECKING
SHOWING_FULLSCREEN
SHOWING_REWARDED
RECOVERING
```

Параллельные показы запрещены. Повторное нажатие во время request игнорируется/disable UI.

## 2. Fullscreen

Допустимые точки:

- завершение уровня/раунда;
- game over → restart;
- переход главы;
- после осознанного действия «следующий уровень»;
- возврат из gameplay в крупный transition, если не слишком часто.

Недопустимо:

- в середине активного input;
- сразу после старта без платформенного требования;
- при каждом микролевеле длиной несколько секунд;
- сразу вслед за rewarded;
- пока открыт другой modal.

Общий portable gate по умолчанию: не чаще 120 секунд между фактическими fullscreen impressions, если площадка не требует ещё строже. Площадка всё равно может отказать в показе.

## 3. Rewarded

Хорошие сценарии:

- revive/continue;
- double reward;
- bonus currency;
- extra hint;
- extra move/attempt;
- temporary boost;
- optional cosmetic unlock progress.

Reward должен быть:

- понятным до нажатия;
- достаточно ценным;
- не обязательным для нормального прохождения;
- выдан только после подтверждения платформой.

## 4. Ad availability

Перед показом проверять capability/availability.

Если реклама недоступна:

- core loop продолжается;
- rewarded CTA disabled/hidden/alternative;
- никаких бесконечных spinner;
- fullscreen transition продолжается без рекламы.

## 5. Pause/audio/input contract

Перед ad request/show:

- добавить pause reason `AD`;
- остановить gameplay marking;
- mute/pause game audio;
- заблокировать gameplay input.

После завершения:

- снять только reason `AD`;
- восстановить audio только если он играл и пользователь не включал mute;
- gameplay resume только если нет других pause reasons.

## 6. Reward idempotency

Каждый rewarded request имеет уникальный runtime id/context.

Нельзя выдать награду дважды, если callback/promises сработали повторно или UI повторно обработал результат.

Схема:

```text
request_id created
→ show
→ success reward flag
→ apply_reward_once(request_id)
→ save
```

## 7. Analytics events

Логировать минимум:

- `ad_opportunity`;
- `ad_request`;
- `ad_available`;
- `ad_started/rendered`;
- `ad_closed`;
- `ad_reward_granted`;
- `ad_failed(reason)`.

Это позволяет отличить плохую монетизацию от низкого fill/show rate.

## 8. Pika-specific

- `canShow()` непосредственно перед show;
- fullscreen cooldown минимум 120 секунд по текущей документации;
- rewarded reward только по `result.reward`;
- preloader mobile-only before `gameStarted()`;
- adblock/no-ad должен работать.

## 9. Yandex-specific

- использовать callbacks SDK;
- fullscreen frequency частично регулирует платформа;
- rewarded success определяется соответствующим callback;
- учитывать platform `game_api_pause/resume`;
- случайные клики по рекламным блокам недопустимы.

## 10. A/B и экономика

Не увеличивать частоту рекламы «на глаз».

Смотреть совместно:

- session length;
- sessions/player;
- rewarded/player;
- fullscreen/player;
- requests → impressions;
- revenue/player;
- day retention, если доступен;
- exits around ad points.

Оптимизировать `revenue per player/session`, а не просто количество запросов.

## 11. No-ad purchase

Если когда-нибудь добавляется отключение рекламы:

- rewarded обычно остаётся добровольным, если товар явно не обещает убрать вообще все рекламные форматы;
- entitlement хранится надёжно;
- fullscreen/banner gate проверяет entitlement централизованно;
- UI честно объясняет состав товара.
