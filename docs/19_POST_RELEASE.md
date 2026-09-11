# 19 — Post-Release Protocol

После публикации игра становится продуктом, а не закрытым проектом.

## 1. D+1

Проверить:

- запускается ли public build;
- ошибки/отзывы;
- load time;
- ad requests vs impressions;
- save incidents;
- crashes/hard locks;
- desktop/mobile split.

Исправлять только явные critical/blocker и очень дешёвые major issues.

## 2. D+3

Смотреть:

- sessions/player;
- avg session;
- playtime/player;
- rewarded usage;
- fullscreen usage;
- exits/feedback;
- первые revenue/player.

Сформулировать максимум 1–3 гипотезы улучшения.

## 3. D+7

Заполнить `METRICS_REVIEW.md`.

Решить:

- есть ли смысл в content update;
- нужно ли менять onboarding;
- где теряются ad impressions;
- есть ли device-specific проблема;
- стоит ли делать sibling/variation жанра.

## 4. D+30

Классифицировать: `SCALE / IMPROVE / LEAVE / KILL`.

Оценить также maintenance burden.

## 5. Updates

Update должен иметь гипотезу:

```text
Problem → Change → Expected metric → Review date
```

Не делать большой update просто «чтобы игра обновлялась».

## 6. One variable rule

Если возможно, не менять одновременно onboarding, ad frequency, economy и level difficulty: потом невозможно понять, что сработало.

## 7. Feedback

Отзывы игроков классифицировать:

- bug;
- confusion/UX;
- difficulty;
- performance;
- content request;
- monetization complaint;
- subjective taste.

Один громкий комментарий не равен статистическому сигналу, но repeated UX confusion — серьёзный сигнал.

## 8. Protocol update

Если post-release выявил общую ошибку:

1. исправить конкретную игру;
2. подтвердить решение;
3. обновить protocols;
4. добавить запись в `research/DECISIONS.md`;
5. следующая игра получает fix автоматически.
