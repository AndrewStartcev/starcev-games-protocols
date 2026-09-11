# 18 — Security & Backend Protocol

## 1. Threat model

Всё, что отправлено браузеру, считается доступным игроку:

- GDScript/WASM нельзя считать секретным хранилищем;
- JS variables доступны/исследуемы;
- local save можно менять;
- network requests можно повторять/подменять на клиентской стороне.

Следствие: секреты и authoritative money logic — только backend.

## 2. Никогда в клиенте

- platform secret key;
- database password;
- private API key;
- HMAC signing secret;
- admin token;
- backend service credentials.

## 3. Пикабу signed player

Рекомендуемый cloud save auth:

```text
client: sdk.player.getSignedData()
→ HTTPS request
→ backend verifies HS256/HMAC with game secret
→ trusts verified player id
→ issues short session / handles request
```

Нельзя доверять `playerId` из отдельного JSON body без проверки подписи.

## 4. Secret rotation

Secret хранится в environment/secret store backend.

При ротации:

- обновить backend одновременно;
- старые подписи могут перестать валидироваться;
- не коммитить secret в Git.

## 5. Save API minimum

Для простого backend:

```text
POST /session or signed auth per request
GET /save
PUT /save
```

Требования:

- HTTPS;
- body size limit;
- schema validation;
- rate limit;
- request timeout;
- per-player authorization;
- server logs без секретов;
- CORS только нужным origins, где возможно.

## 6. Revision / optimistic concurrency

Чтобы два клиента не затирали состояние молча:

```text
save.revision = N
PUT expected_revision=N
server stores N+1
```

Conflict → клиент получает canonical snapshot и применяет merge policy.

Для первого простого MVP можно упростить, но риск должен быть зафиксирован.

## 7. Purchases

Если есть реальные покупки:

- verify signed purchase/server signature;
- purchase processing идемпотентно по `purchase_id/token`;
- повтор запроса не начисляет товар повторно;
- consumable помечается обработанным только после успешного начисления;
- audit log хранит минимум purchase id, player id, product, status, timestamps.

## 8. Anti-cheat

Для casual ad-monetized игр не строить тяжёлый anti-cheat без экономического смысла.

Защищать то, что реально имеет стоимость:

- leaderboard, если cheat портит продукт;
- purchases;
- premium currency;
- referral/reward claims;
- серверную экономику.

Обычный local score в single-player не требует банковской безопасности.

## 9. Privacy/minimization

Хранить только данные, нужные игре. Не собирать email/имя/прочие персональные данные «на будущее».

Platform player id обычно достаточно для saves.

## 10. Backend failure

Backend outage не должен стирать local progress.

Поведение:

- local continue;
- cloud state = degraded/offline;
- retry с backoff;
- сообщить пользователю только если риск прогресса/функции значим;
- не спамить запросами каждый frame.
