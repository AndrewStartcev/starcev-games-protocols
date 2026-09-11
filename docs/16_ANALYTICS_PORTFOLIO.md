# 16 — Analytics & Portfolio Economics

## 1. Зачем

Мы строим не одну игру, а портфель. Главная единица решения — не «понравилась игра», а соотношение результата к производственным затратам.

## 2. Минимальные метрики на игру

```text
players
sessions
sessions_per_player
avg_session_time
playtime_per_player
load_time
ad_requests
ad_impressions
ad_show_rate
rewarded_impressions
fullscreen_impressions
revenue
revenue_per_player
revenue_per_1000_players
development_days
maintenance_hours
```

Если платформа даёт device split — фиксировать desktop/mobile отдельно.

## 3. Производственные KPI

```text
time_to_first_playable
time_to_vertical_slice
time_to_release
asset_rework_cycles
platform_integration_hours
QA_fix_hours
```

Цель протоколов — эти значения снижать от игры к игре.

## 4. Формулы

```text
sessions/player = sessions / unique_players
revenue/player = revenue / unique_players
ad show rate = impressions / requests
revenue/1000 players = revenue / players * 1000
revenue per dev day = revenue_for_period / development_days
```

На маленькой выборке не делать сильных выводов по eCPM и жанру.

## 5. Review windows

- D+1: технические проблемы, load, obvious monetization failure;
- D+3: первый поведенческий сигнал;
- D+7: первичный коммерческий benchmark;
- D+30: решение по долгосрочному месту игры в портфеле.

## 6. Решения

### SCALE

Игра показывает сильный `revenue/player`, session/playtime или органический рост. Делать content/update/genre sibling.

### IMPROVE

Есть конкретный измеримый bottleneck: load, show rate, short session, onboarding, poor rewarded uptake.

### LEAVE

Игра приносит небольшую прибыль, но ROI на доработку низкий. Оставить стабильной.

### KILL

Требует поддержки/исправлений больше потенциальной ценности или нарушает платформу. Не тратить sunk-cost часы.

## 7. Жанровый benchmark

После 5–10 игр группировать:

- genre;
- class A/B/C;
- development days;
- revenue/player;
- playtime;
- device mix;
- ad mix;
- 30d revenue.

Выбирать следующие игры не только по абсолютному доходу, но по `expected revenue / production day` и потенциалу масштабирования.

## 8. Ads diagnosis

Если requests много, impressions мало — проблема может быть fill/platform availability, а не gameplay.

Если impressions/player мало и session длинная — возможно мало ad opportunities.

Если impressions высоки, session/return падают — возможно переагрессивная монетизация.

Rewarded и fullscreen анализировать отдельно.

## 9. Портфельная дисциплина

Не сравнивать игры с разным возрастом публикации по lifetime revenue без нормализации.

Хранить weekly/monthly snapshot, чтобы видеть:

- новые игры;
- declining games;
- evergreen games;
- platform differences.

## 10. Финансовая связь

Игровой production оценивается как самостоятельный денежный поток. Не считать потенциальный доход деньгами до фактической статистики/выплаты. Прогнозы строить conservative/base/upside и регулярно заменять гипотезы фактом.
