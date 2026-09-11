# 05 — UI/UX и responsive

## 1. Принцип

UI проектируется как игровая система, а не как картинка поверх canvas.

Один и тот же core gameplay должен быть удобен:

- desktop мышь;
- mobile touch;
- разные aspect ratios;
- browser zoom/resize в допустимых пределах платформы.

## 2. Base resolution

Выбирается под жанр и ориентацию и фиксируется в `GAME_BRIEF`.

Типичные отправные точки:

- landscape: `1600×900` или `1920×1080` design space;
- portrait: `1080×1920` design space.

Это design size, а не требование к физическому экрану.

## 3. Godot stretch

Для обычного responsive 2D/UI обычно использовать `canvas_items` либо осознанный `viewport` в зависимости от art style.

Выбор фиксируется в проекте и тестируется на крайних aspect ratios.

- UI/HUD: anchors + Containers + min sizes;
- не раскладывать responsive меню абсолютными x/y вручную;
- pixel art: viewport/integer scaling, если необходимо сохранить пиксельную сетку;
- backgrounds могут cover/crop только в заранее разрешённой safe composition.

## 4. Safe zones

Каждый критичный экран имеет:

- safe gameplay zone;
- safe UI zone;
- разрешённый декоративный crop;
- крайние aspect ratio examples.

Нельзя обрезать:

- кнопки;
- цены;
- счетчики;
- задания;
- tutorial prompts;
- интерактивные игровые объекты.

## 5. Pointer-first abstraction

Основное действие должно одинаково интерпретироваться от mouse и touch.

Правила:

- click/tap — единый semantic action;
- drag — не завязан на hover;
- hover — только feedback enhancement;
- right click не используется для обязательного gameplay;
- keyboard shortcuts — бонус на desktop, не единственный путь;
- при использовании эмуляции touch/mouse следить, чтобы одно физическое действие не обрабатывалось дважды.

## 6. Target size

Interactive targets должны быть достаточно крупными для пальца. Внутренний ориентир — не мельче ~44–48 logical px по короткой стороне для важных controls, если визуальная система позволяет.

Учитывать расстояние между соседними destructive/expensive actions.

## 7. UI states

Интерактивный элемент имеет понятные состояния:

- normal;
- hover (desktop, если применимо);
- pressed;
- disabled;
- focus, если keyboard navigation используется;
- loading/busy, если действие async.

Нельзя оставлять rewarded button «рабочим на вид», если реклама недоступна и нажатие ничего не делает. Лучше disabled/hidden/alternative behavior.

## 8. Dynamic vs baked text

По умолчанию текст — dynamic.

Baked text допустим только если одновременно:

- строка гарантированно не меняется;
- не нужна другая локализация;
- это действительно улучшает визуал;
- в `ASSET_MANIFEST` стоит `baked_text: true`;
- asset не содержит цену, число, счёт, имя игрока, награду или изменяемую величину.

Логотип может быть baked. Gameplay values — нет.

## 9. Typography

- fonts входят в build, а не зависят от внешнего CDN;
- проверять кириллицу;
- не использовать слишком тонкий вес на маленьких размерах;
- числа/валюта не должны прыгать по ширине критично для layout;
- длинные локализованные строки должны иметь wrap/ellipsis/alternative sizing policy.

## 10. Первый вход

Цель: как можно быстрее довести игрока до действия.

Рекомендуется:

- минимум обязательных splash/диалогов;
- tutorial через действие;
- одна главная CTA;
- настройки доступны, но не блокируют старт.

## 11. Feedback

Каждое значимое действие даёт feedback минимум одним способом:

- animation;
- sound;
- haptic (если платформа/браузер позволяет и реально нужен);
- number/particle response;
- state change.

Feedback не должен заслонять следующий input.

## 12. Проверка адаптива

Минимальный desktop набор:

- 1280×720/768;
- 1366×768;
- 1600×900;
- 1920×1080;
- ultrawide scenario.

Плюс narrow/tall iframe, mobile portrait/landscape согласно заявленной ориентации.

Яндекс отдельно проверяет resize по горизонтали, вертикали и диагонали, поэтому «работает только на 16:9» недостаточно, если игра заявлена шире.
