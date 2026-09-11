# Asset Manifest

Art direction/reference:  
Design resolution:  
Last approved integration screenshot:  

## Rules

- Один production asset = одна запись.
- Не генерировать массовый pack, пока первые 3–5 ключевых элементов не проверены в Godot.
- `display_rect` описывает фактический слот в игре.
- Изменяемый текст не запекается.

## Assets

### `asset_id`

- **Path:** `assets/...`
- **Purpose:**
- **Display rect:** `W×H`
- **Source size:** `W×H`
- **Aspect ratio:**
- **Format:** PNG / WebP / SVG if supported by pipeline
- **Alpha:** yes / no
- **Stretch:** none / contain / cover / 9-patch
- **9-patch margins:** L / T / R / B
- **Safe area:**
- **Baked text:** no
- **States/variants:** normal / hover / pressed / disabled
- **Reference:**
- **Status:** TODO / TEST / APPROVED / INTEGRATED
- **Notes:**

---

## Integration QA

| Asset | 16:9 | Narrow | Mobile | Visual match | Status |
|---|---|---|---|---|---|
| | | | | | |
