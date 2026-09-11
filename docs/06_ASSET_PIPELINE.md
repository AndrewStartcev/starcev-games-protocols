# 06 — Asset Pipeline

Этот протокол исправляет повторяющуюся проблему: красивый концепт → неточные ассеты → растяжение/обрезка в Godot → визуал хуже концепта.

## 1. Правильная последовательность

```text
GAMEPLAY/UX STRUCTURE
→ REAL UI SLOTS
→ ASSET_MANIFEST
→ ART DIRECTION / CONCEPT
→ 3–5 TEST ASSETS
→ INTEGRATION SCREENSHOT
→ APPROVAL
→ FULL ASSET PACK
→ FINAL SCREENSHOT REVIEW
```

Не наоборот.

## 2. Концепт и production asset — разные вещи

Concept sheet может показывать атмосферу, композицию, свет, формы и стиль.

Он **не является**:

- sprite sheet для прямой нарезки;
- финальным экраном одной картинкой;
- доказательством, что UI соберётся;
- источником точных размеров.

## 3. ASSET_MANIFEST обязателен

Для каждого production asset:

```text
id:
path:
purpose:
display_rect:
source_size:
aspect_ratio:
format:
alpha:
stretch_mode:
nine_patch_margins:
safe_area:
baked_text:
variants/states:
notes:
```

Без manifest нельзя запускать массовую генерацию ассетов.

## 4. UI assets

По умолчанию:

- отдельные элементы;
- без встроенных изменяемых надписей;
- без цен/счёта;
- панели, которые растягиваются, проектируются как 9-patch/StyleBox или вектороподобная сборка;
- декоративные corners/borders не должны деформироваться;
- кнопки имеют state variants или строятся через StyleBox + overlays.

Запрещено брать узкую красивую панель и растягивать её до любого размера без stretch contract.

## 5. Background assets

Background должен иметь safe composition:

- главный объект не попадает под HUD;
- допустимый crop указан;
- критичные детали не прижаты к краю;
- для portrait/landscape при сильном различии лучше отдельный вариант, чем разрушительный crop.

## 6. Форматы

### 2D UI / pixel art

- Lossless PNG/WebP;
- alpha только когда нужна;
- pixel art — lossless + nearest filtering;
- не использовать VRAM compression для мелкого UI/pixel art из-за артефактов.

### Большие 2D фоны

- WebP/lossy, если визуально приемлемо;
- quality подбирать по реальному screenshot, не «максимум всегда»;
- не хранить 4K, если игра физически показывает ~1080p и detail не нужен.

### 3D textures

- VRAM compressed/Basis Universal по реальной платформенной проверке;
- разумные размеры 512/1024/2048;
- 4K только при доказанной необходимости.

## 7. VRAM awareness

Disk size изображения не равен GPU memory. Большая RGBA texture после декодирования может занимать десятки мегабайт VRAM.

Поэтому:

- избегать огромных transparent textures;
- crop пустые поля;
- не дублировать визуально идентичные assets;
- использовать atlases там, где это реально упрощает/ускоряет, а не автоматически.

## 8. Source vs runtime

Тяжёлые `.psd`, `.ai`, рабочие sheets, генерационные исходники хранить в `source_assets/` под `.gdignore` или вне импортируемой части проекта.

В `assets/` — только runtime-ready файлы.

## 9. Имена

Примеры:

```text
btn_primary_normal.webp
btn_primary_hover.webp
btn_primary_pressed.webp
panel_upgrade.9patch.webp
icon_coin.webp
bg_village.webp
fx_success_01.webp
```

Без `final_final2.png`.

## 10. Visual QA loop

После интеграции ключевого экрана обязательно сравнить:

- утверждённый reference/concept;
- screenshot из реальной игры при целевом разрешении;
- screenshot при одном узком/широком разрешении.

Проверить:

- пропорции;
- crop;
- иерархию;
- spacing;
- readability;
- consistency теней/обводок;
- не выглядят ли элементы как «растянутые»;
- совпадает ли насыщенность/контраст с концептом.

Если реальная игра заметно хуже концепта, asset stage не завершён.

## 11. Массовый concept sheet

Большой лист с ассетами допустим только как communication/art-direction board.

Для production каждый элемент затем создаётся/экспортируется отдельно по manifest. Нельзя автоматически нарезать лист и считать задачу готовой, если элементы имеют perspective, общий свет, пересечения или неточные границы.

## 12. Авторские права

Для каждого стороннего ассета/аудио фиксировать источник и лицензию в `docs/ASSET_SOURCES.md`. Не использовать неизвестные assets «из Google».
