# 14 — QA & Testing Protocol

## 1. Severity

- **Blocker:** запуск/прогресс/публикация невозможны, потеря save, hard lock.
- **Critical:** ломается core gameplay, rewarded exploit, major platform violation.
- **Major:** заметная функциональная/визуальная проблема без полного блокера.
- **Minor:** polish/локальный дефект.

Blocker/Critical перед релизом = 0.

## 2. Test layers

### A. Editor smoke

Быстрая проверка сцены/логики.

### B. Local Web export

Обязательно. Проверяет WebAssembly/WebGL/JS/audio/input.

### C. Platform draft/debug

Обязательно для SDK/ads/save/lifecycle.

### D. Device matrix

Desktop + mobile реальные сценарии.

## 3. Core smoke suite

На каждом release candidate:

1. cold start;
2. menu;
3. start game;
4. выполнить core action;
5. win/result;
6. restart/next;
7. pause/resume;
8. mute/unmute;
9. save;
10. reload;
11. continue from save;
12. rewarded available;
13. rewarded unavailable;
14. fullscreen;
15. tab hide/show;
16. resize;
17. return to menu.

## 4. Responsive suite

Проверить минимум:

- 1280×720/768;
- 1366×768;
- 1600×900;
- 1920×1080;
- narrow desktop iframe;
- ultrawide;
- target mobile portrait/landscape;
- browser zoom 80–125% там, где применимо.

Искать:

- overlap;
- crop;
- unreadable font;
- unreachable button;
- background exposing wrong area;
- drag coordinate mismatch.

## 5. Input suite

Desktop:

- mouse click;
- drag;
- rapid click;
- click outside modal;
- optional keyboard.

Mobile:

- tap;
- drag;
- long press, если есть;
- multi-touch accidental;
- browser gesture conflict;
- orientation change, если поддерживается.

## 6. Lifecycle suite

В каждом состоянии (menu/gameplay/pause/result/ad flow) хотя бы раз:

- switch tab;
- minimize;
- return;
- убедиться, что audio/game state не запустились неверно.

## 7. Ads suite

- canShow=false;
- show success;
- show error;
- close/skip;
- rewarded success;
- rewarded failure;
- double click CTA;
- ad while game manually paused;
- reload after reward;
- adblock path (Пикабу обязательно работоспособен).

Проверить отсутствие duplicate reward.

## 8. Save suite

См. `10_SAVES.md`, включая migration/corruption/cloud conflicts.

## 9. Performance suite

Фиксировать:

- game ready time;
- first input responsiveness;
- avg/min FPS на representative gameplay;
- visible stutters;
- build sizes;
- transferred bytes на собственном hosting;
- console errors.

## 10. Console rule

Release candidate не должен иметь необработанных красных ошибок в обычном happy path.

Допустимые внешние/advertising warnings документируются в QA report, если доказано, что они не принадлежат нашей игре и не влияют на неё.

## 11. Regression

После исправления Blocker/Critical повторить не только точный bug, но core smoke вокруг затронутой системы.

После изменения:

- save → весь save suite;
- platform adapter → platform lifecycle + ads + save;
- responsive root → ключевые экраны;
- input layer → mouse + touch;
- audio service → background/ad/mute.

## 12. QA report

Перед релизом заполнить template `QA_REPORT.md` с:

- build/version/commit;
- devices/browsers;
- пройденными suites;
- известными issues;
- platform draft links/ids, если безопасно;
- итогом `PASS / PASS WITH KNOWN MINORS / FAIL`.
