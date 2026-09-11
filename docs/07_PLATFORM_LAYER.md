# 07 — Platform Layer

## 1. Цель

Одна gameplay codebase должна работать:

- локально без SDK;
- в Яндекс Играх;
- в Пикабу Играх;
- потенциально на других web-площадках.

Gameplay не знает названия площадки.

## 2. Контракт

Типовой API (названия могут адаптироваться, смысл сохраняется):

```text
Platform.init()
Platform.ready()
Platform.gameplay_start()
Platform.gameplay_stop()
Platform.get_language()
Platform.get_device_type()
Platform.get_player()
Platform.request_auth()

Ads.can_show(type)
Ads.show_fullscreen(context)
Ads.show_rewarded(context)

Save.load_cloud()
Save.push_cloud(snapshot, force)

Leaderboard.submit(...)
Payments.purchase(...)
```

Optional capabilities должны проверяться через capability flags, а не предполагаться.

## 3. Adapter selection

На boot определяется окружение:

```text
local → LocalPlatformAdapter
yandex → YandexPlatformAdapter
pikabu → PikabuPlatformAdapter
```

Не делать scattering вида `if platform == "yandex"` по gameplay-файлам.

## 4. Local adapter

Local adapter обязан:

- успешно инициализироваться без network;
- имитировать ready;
- разрешать тестовые rewarded/fullscreen без реальной рекламы;
- сохранять только local;
- возвращать debug player;
- логировать вызовы.

Это критично для скорости разработки.

## 5. Capability model

Пример:

```text
supports_rewarded
supports_fullscreen
supports_cloud_save
supports_auth
supports_leaderboard
supports_payments
supports_preloader
```

UI принимает решение по capability/state, а не по имени площадки.

## 6. Lifecycle

Platform layer нормализует внешние события в события игры:

```text
platform_pause_requested(reason)
platform_resume_requested(reason)
player_changed
ad_started
ad_finished
cloud_state_changed
```

Фактический pause контролирует общий pause coordinator, чтобы не сломать вложенные состояния.

## 7. SDK init failure

Если SDK не загрузился:

- игра не должна бесконечно висеть на loader;
- показать/логировать recoverable status;
- повторить init ограниченное количество раз, если разумно;
- local gameplay path допускается только если правила конкретной площадки это позволяют;
- platform-specific release QA обязан обнаружить такую проблему заранее.

## 8. JS bridge

JS-код площадки держать централизованно:

- custom HTML shell / platform JS file;
- GDScript adapter;
- строго определённые callback names.

Не вставлять произвольные `eval()` куски в gameplay code.

## 9. Build configuration

Платформенная разница конфигурируется build/export profile или единой bootstrap-конфигурацией, а не ручным редактированием десятков файлов перед каждым экспортом.

Цель:

```text
build/local/
build/yandex/
build/pikabu/
```

При этом исходная gameplay codebase остаётся одна.

## 10. Новый SDK

Добавление новой площадки считается успешным, если:

- добавлен adapter;
- gameplay не менялся ради базовой интеграции;
- save/ads lifecycle проходят тесты;
- local adapter не сломан;
- документация платформы добавлена в protocols.
