# Research Sources

Последняя ревизия: `2026-09-11`.

Правило: platform requirements и engine behavior подтверждаем по первоисточникам. Блоги/форумы можно использовать для идей и диагностики, но не как источник обязательных правил площадки.

## Godot

- Godot 4.7.2 maintenance release — https://godotengine.org/article/maintenance-release-godot-4-7-2/
- Stable documentation — https://docs.godotengine.org/en/stable/
- Exporting for Web — https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_web.html
- Web export options / thread support — https://docs.godotengine.org/en/stable/classes/class_editorexportplatformweb.html
- JavaScriptBridge — https://docs.godotengine.org/en/stable/classes/class_javascriptbridge.html
- Supporting multiple resolutions — https://docs.godotengine.org/en/stable/tutorials/rendering/multiple_resolutions.html
- Project organization — https://docs.godotengine.org/en/stable/tutorials/best_practices/project_organization.html
- Scene organization — https://docs.godotengine.org/en/stable/tutorials/best_practices/scene_organization.html
- Resources — https://docs.godotengine.org/en/stable/tutorials/scripting/resources.html
- GDScript static typing — https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/static_typing.html
- General optimization — https://docs.godotengine.org/en/stable/tutorials/performance/general_optimization.html
- Importing images — https://docs.godotengine.org/en/stable/tutorials/assets_pipeline/importing_images.html

## Яндекс Игры

- Developer docs — https://yandex.ru/dev/games/doc/ru/
- Game requirements — https://yandex.ru/dev/games/doc/ru/concepts/requirements
- Technical errors / lifecycle scenarios — https://yandex.ru/dev/games/doc/ru/requirements/1/14
- SDK methods / Game Ready / Gameplay — https://yandex.ru/dev/games/doc/ru/requirements/1/19
- Game events — https://yandex.ru/dev/games/doc/ru/sdk/sdk-game-events
- Ads placement — https://yandex.ru/dev/games/doc/ru/requirements/4/4
- Player data — https://yandex.ru/dev/games/doc/ru/sdk/sdk-player
- Environment — https://yandex.ru/dev/games/doc/ru/sdk/sdk-environment
- Device info — https://yandex.ru/dev/games/doc/ru/sdk/sdk-device-info
- Leaderboards — https://yandex.ru/dev/games/doc/ru/sdk/sdk-leaderboard
- Payments — https://yandex.ru/dev/games/doc/ru/sdk/sdk-purchases

## Пикабу Игры

- SDK docs — https://games.pikabu.ru/sdk/docs/
- Technical requirements — https://games.pikabu.ru/sdk/docs/requirements/tech
- Content requirements — https://games.pikabu.ru/sdk/docs/requirements/content
- SDK init — https://games.pikabu.ru/sdk/docs/sdk/init
- Player/auth — https://games.pikabu.ru/sdk/docs/sdk/player
- Ads — https://games.pikabu.ru/sdk/docs/sdk/ads
- Data validation — https://games.pikabu.ru/sdk/docs/sdk/validation
- Featuring recommendations — https://games.pikabu.ru/sdk/docs/requirements/featuring
- Add a game / developer audience — https://games.pikabu.ru/add-own-game

## Web Platform

- Page Visibility API — https://developer.mozilla.org/docs/Web/API/Page_Visibility_API
- Pointer Events — https://developer.mozilla.org/docs/Web/API/Pointer_events
- CSS `touch-action` — https://developer.mozilla.org/docs/Web/CSS/touch-action
- Autoplay guide — https://developer.mozilla.org/docs/Web/Media/Guides/Autoplay
- Web Audio API — https://developer.mozilla.org/docs/Web/API/Web_Audio_API
- WebAssembly — https://developer.mozilla.org/docs/WebAssembly
- IndexedDB — https://developer.mozilla.org/docs/Web/API/IndexedDB_API

## Cross-platform sanity references

Не являются требованиями Яндекса/Пикабу; используются для сравнения практик HTML5-порталов.

- Poki SDK / gameplay-commercial-break lifecycle — https://sdk.poki.com/
- CrazyGames SDK — https://docs.crazygames.com/sdk/html5/

## Refresh policy

Перед релизом или интеграцией нового API перечитывать соответствующий официальный раздел, если:

- последняя ревизия protocols старше ~30 дней;
- platform API неожиданно ведёт себя иначе;
- модерация прислала новое требование;
- вышла новая stable версия Godot и планируется обновление проекта.
