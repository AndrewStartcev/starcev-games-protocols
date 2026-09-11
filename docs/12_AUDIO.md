# 12 — Audio Protocol

## 1. Базовые шины

Минимум:

```text
Master
├── Music
└── SFX
```

Для сложной игры MAY: UI, Voice, Ambience.

## 2. Settings

Игроку доступны минимум:

- Music on/off;
- SFX on/off;

Можно добавить sliders, если это оправдано.

Настройки сохраняются отдельно от прогресса или в секции `settings` save schema.

## 3. Web autoplay

Браузер может блокировать audio до user gesture.

Правило: first user interaction unlocks/starts desired audio. Boot не должен зависеть от успешного autoplay.

## 4. Background/ad mute

На tab hidden/platform pause/ad/auth/payment:

- gameplay audio останавливается/глушится;
- при resume восстанавливается предыдущее состояние;
- не включать музыку, если пользователь до этого её выключил.

Использовать reason-based audio suspension либо общий pause coordinator.

## 5. Godot Web limitation

В современных Godot Web exports по умолчанию используется Web Audio sample playback. Часть AudioEffects/reverb/doppler не поддерживается в этом режиме.

Следствие: критичный саунд-дизайн не должен зависеть от runtime AudioEffects, которые могут отсутствовать на Web. Нужный эффект лучше подготовить в исходном аудио либо отдельно проверить режим Stream и latency.

## 6. Форматы и вес

- музыка/длинные loops: compressed format (обычно OGG) с разумным bitrate;
- короткие SFX: выбирать между compressed/PCM исходя из веса и latency;
- не хранить стерео там, где mono достаточно;
- trim silence;
- loop points проверить в реальном web build.

## 7. Polyphony

Не создавать новый AudioStreamPlayer бесконтрольно на каждый клик.

Использовать:

- ограниченный SFX pool;
- polyphony, если доступна/подходит;
- rate limit для очень частых одинаковых звуков.

## 8. UX

Звук должен подтверждать действие, а не мешать:

- click;
- success;
- error;
- reward;
- progression;
- danger/time warning.

Не играть громкий звук на каждом incremental tick при десятках событий в секунду.

## 9. QA

Проверить:

- first launch before click;
- first click unlock;
- mute persists after reload;
- ad pause/resume;
- tab switch;
- rapid SFX;
- Safari/mobile;
- музыка не начинает играть после resume, если до hide была выключена/paused вручную.
