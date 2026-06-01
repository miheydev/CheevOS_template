---
description: "Extracts transcript from a YouTube video using youtube-transcript-api."
language: en
---

# Skill: /youtube-transcript

> Извлечение транскрипта из YouTube-видео

## Триггеры

- `/youtube-transcript`
- "достань транскрипт с ютуба"
- "получи субтитры из видео"

## Зависимости

**Пакет:** `youtube-transcript-api` (Python)
**Расположение:** `/Users/igor/Library/Python/3.9/lib/python/site-packages/`
**Установка:** `pip3 install youtube-transcript-api`
**Безопасность:** Open-source, ~3K stars GitHub, только чтение субтитров через YouTube API. Не скачивает видео/аудио, не работает с куками.

## Инструкция

### Алгоритм

1. Получи YouTube URL от пользователя
2. Извлеки video ID из URL (часть после `v=`)
3. Запусти Python-скрипт для получения транскрипта
4. Верни транскрипт с таймкодами

### Код для извлечения

```python
python3 -c "
from youtube_transcript_api import YouTubeTranscriptApi

ytt_api = YouTubeTranscriptApi()
transcript = ytt_api.fetch('VIDEO_ID', languages=['ru', 'en'])
for entry in transcript:
    print(f'[{entry.start:.1f}] {entry.text}')
"
```

### Если транскрипт недоступен

Попробовать другие языки:
```python
transcript_list = ytt_api.list('VIDEO_ID')
for t in transcript_list:
    print(f'{t.language} ({t.language_code}), auto={t.is_generated}')
```

## Формат вывода

```
[0.0] Первая фраза
[3.2] Вторая фраза
...
```

## Ограничения

- Работает только с видео, у которых есть субтитры (ручные или авто-сгенерированные)
- YouTube может блокировать запросы при частом использовании
- Автосубтитры содержат ошибки распознавания — учитывать при анализе

## Связанные скиллы

- `/video-summary` — глубокий конспект из транскрипта
- `/voice` — структурирование потока мыслей

---

**URL видео:**

$ARGUMENTS
