---
description: SEO-исследование — ключевые слова, SERP-анализ, content gap (3 параллельных агента)
---

# SEO Research

Ты проводишь SEO-исследование для подготовки к написанию статьи. Результат — SEO-бриф, по которому можно написать статью, оптимизированную для поисковых систем.

---

## ПРОЦЕСС

### Шаг 1: Определи тему

Прочитай $ARGUMENTS. Сформулируй:
- **Основной ключевик** (1-3 слова) — то, что пользователь вводит в Яндекс/Google
- **Тематическое ядро** — 3-5 связанных запросов
- **Язык контента** (русский по умолчанию)

### Шаг 2: Параллельное исследование (3 агента)

Запусти **3 Task-агента параллельно** (subagent_type: "general-purpose"):

**Агент 1 — Keyword Research:**
```
You are an SEO keyword researcher specializing in Russian-language search (Yandex + Google).

TOPIC: [тема]
PRIMARY KEYWORD: [основной ключевик]

Your task:
1. Use WebSearch to find keyword data:
   - Search: "[основной ключевик] wordstat" or "[основной ключевик] частотность запросов"
   - Search: "[тема] LSI ключевые слова" or related queries
   - Search: "[основной ключевик]" and note Google's "People also ask" and related searches
2. Compile a keyword map:
   - Primary keyword + estimated search volume
   - 5-10 secondary keywords (long-tail variations)
   - 5-10 LSI keywords (semantically related terms)
   - Question-based keywords ("как...", "что такое...", "почему...")
3. Return a structured keyword brief in Russian.

Format:
## Основной ключевик
[ключевик] — [примерная частотность]

## Вторичные ключевики
- [ключевик] — [примерная частотность]
...

## LSI / Связанные
- [термин]
...

## Вопросы (People Also Ask)
- [вопрос]
...
```

**Агент 2 — SERP Analysis + эталонная статья:**
```
You are a SERP analyst. Analyze the top search results for a given keyword and identify the BEST reference article.

PRIMARY KEYWORD: [основной ключевик]
LANGUAGE: Russian

Your task:
1. Use WebSearch to search for "[основной ключевик]"
2. Analyze the TOP 10 results:
   - Title (H1) of each article
   - Meta description (if visible)
   - Apparent structure (H2 headings if discernible from snippets)
   - Type of content (how-to, listicle, review, guide, comparison)
   - Estimated length (short/medium/long)
   - Site authority / domain (known brands vs niche sites)
3. Identify patterns:
   - What format dominates? (lists, guides, reviews)
   - What's the average depth?
   - What angles are most common?
4. **CRITICAL: Identify the REFERENCE ARTICLE (эталон).**
   The best reference article is one that OUTRANKS higher-authority domains.
   Example: if a niche site (low domain rating) ranks above Tinkoff Journal (high DR),
   that niche article is likely excellent — use it as the reference to improve upon.
   If no such anomaly exists, pick the top-ranking article with the most depth.
5. For the reference article, extract:
   - Full structure (all H2/H3 headings)
   - Key topics covered
   - Approximate word count
   - What makes it rank well

Format:
## ТОП-10 результатов
| # | Заголовок | Сайт | Авторитетность | Тип |
|---|----------|------|---------------|-----|
| 1 | ... | ... | высокая/средняя/низкая | ... |
...

## Эталонная статья
- **Название:** ...
- **Почему эталон:** [обоснование по DR / позиции]
- **Структура:** [H2/H3 заголовки]
- **Ключевые темы:** ...
- **Объём:** ~X слов

## Паттерны
- Доминирующий формат: ...
- Типичная структура: ...
- Средняя глубина: ...
```

**Агент 3 — Content Gap Analysis:**
```
You are a content strategist specializing in finding content gaps.

TOPIC: [тема]
PRIMARY KEYWORD: [основной ключевик]

Your task:
1. Use WebSearch to find and read 3 top-ranking articles on this topic
2. For each article, note:
   - What topics/subtopics it covers well
   - What it misses or covers superficially
   - Its unique angle (if any)
3. Identify the CONTENT GAP — what's NOT covered by existing content:
   - Missing subtopics
   - Outdated information
   - Lack of practical examples
   - Missing expert opinions or data
   - Audience pain points not addressed
4. Recommend a DIFFERENTIATION STRATEGY — how our article can be better.

Format:
## Анализ существующего контента
### Статья 1: [название]
- Покрывает: ...
- Упускает: ...

### Статья 2: ...
...

## Content Gap (что не покрыто)
- ...

## Стратегия дифференциации
- ...
```

### Шаг 3: Синтез → SEO-бриф

Собери результаты трёх агентов и создай единый **SEO-бриф**:

```markdown
# SEO-бриф: [тема]

**Дата:** [дата]
**Основной ключевик:** [ключевик] ([частотность])

---

## Ключевые слова

### Ядро
| Ключевик | Тип | Частотность |
|---------|-----|-------------|
| ... | primary | ... |
| ... | secondary | ... |

### LSI / Связанные
- [термин], [термин], [термин]

### Вопросы для раскрытия
- [вопрос]?
- [вопрос]?

---

## Рекомендуемая структура статьи

**H1:** [рекомендуемый заголовок с ключевиком]

**Meta description:** [150-160 знаков]

### Структура:
- H2: [раздел 1] — [что раскрыть]
- H2: [раздел 2] — [что раскрыть]
  - H3: [подраздел]
- H2: [раздел 3] — [что раскрыть]
- H2: FAQ / Частые вопросы

**Рекомендуемая длина:** [X-Y слов]

---

## Эталонная статья

- **Название:** [название]
- **URL:** [если найден]
- **Почему эталон:** [обоснование]
- **Структура:** [H2/H3]
- **Стратегия:** Взять за основу, добавить свежие данные + закрыть content gap

---

## Content Gap / Наше преимущество

[Что упускают конкуренты и как мы можем сделать лучше]

---

## Конкурентный ландшафт

| Конкурент | Сильные стороны | Слабые стороны |
|-----------|----------------|----------------|
| ... | ... | ... |
```

### Шаг 4: Сохранить

Сохрани бриф в файл рядом с будущей статьёй:
- Типовой путь: `<project>/Docs/Content/SEO-Briefs/seo-brief-YYYY-MM-DD-[slug].md`
- По умолчанию: в текущую директорию

---

## ВАЖНЫЕ ПРАВИЛА

1. **Все 3 агента запускаются параллельно** — не последовательно.
2. **WebSearch обязателен** — не выдумывай частотности, ищи реальные данные.
3. **Язык ключевиков = язык контента.** Для русскоязычной статьи — русские ключевики.
4. **Бриф — не статья.** Это план для написания, а не готовый текст.
5. **Сохраняй в файл** — бриф будет использоваться на следующем шаге пайплайна.

---

## TODO: Будущие доработки

- [ ] **MCP-интеграция с Wordstat** — подключить Yandex Wordstat API через MCP для точных данных по частотности (сейчас — оценка через WebSearch)
- [ ] **Автоматический парсинг DR** — интеграция с Ahrefs/Serpstat API для точного domain rating конкурентов
- [ ] **Реестр опубликованных статей** — файл со списком всех наших статей для автоматической перелинковки
- [ ] **Генерация изображений** — шаг генерации через Gemini API (как у Ильи Мартына)
- [ ] **HTML-вёрстка + SEO-блоки** — автоматическая вёрстка статьи с мета-тегами, Schema.org, Open Graph
- [ ] **Автопубликация** — публикация из терминала (WordPress API, Telegram Bot API)

---

Тема для SEO-исследования:

$ARGUMENTS
