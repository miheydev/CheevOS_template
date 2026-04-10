---
description: Независимая оценка качества результата через агента-критика с чистым контекстом (quality gate). Вызов — /evaluate
---

# Evaluate — Quality Gate

Агент, который делал работу, НЕ может объективно её оценить — confirmation bias. Свежий агент с нулевым контекстом о процессе работы видит только "что просили" и "что получилось".

---

## Триггеры

- `/evaluate`
- `/evaluate path/to/file.md`
- "оцени результат", "quality gate", "проверь что получилось"

---

## Процесс

### Шаг 1. Собери входные данные

Из `$ARGUMENTS` и контекста беседы извлеки две вещи:

1. **ORIGINAL_REQUEST** — исходный запрос пользователя. Что именно просили сделать.
2. **RESULT_CONTENT** — результат работы. Прочитай все файлы, которые были созданы/изменены.

Если не хватает информации — спроси пользователя:
- "Какой был исходный запрос?"
- "Какие файлы нужно оценить?"

### Шаг 2. Запусти агента-критика

Запусти **1 субагента** с параметрами:
- `subagent_type: "general-purpose"`
- `model: "sonnet"`

Критически важно: субагент получает ТОЛЬКО запрос + результат. Никакой истории рассуждений, промежуточных шагов, контекста о том КАК делалась работа.

Промпт субагенту (на английском — лучше качество оценки):

```
You are an independent quality reviewer performing a blind review. You have NEVER seen this work before. You know NOTHING about how it was created, what tools were used, or what challenges were encountered.

Your job: evaluate the RESULT against the ORIGINAL REQUEST. Nothing else.

## ORIGINAL REQUEST

{ORIGINAL_REQUEST}

## RESULT TO EVALUATE

{RESULT_CONTENT}

---

## Instructions

Evaluate this result against 5 criteria. For each criterion:
- Score (1-10)
- Evidence: specific quotes/examples from the result that justify your score
- Gap: what is missing or wrong (if score < 8)

### CRITERIA

**1. COMPLETENESS (1-10)**
Does the result address ALL parts of the original request? List each requirement from the request and check if it's covered. Flag anything skipped or partially done.

**2. CORRECTNESS (1-10)**
Is the result technically/factually accurate? For code: bugs, logical errors, security issues? For text: claims supported, no contradictions? For analysis: methodology sound?

**3. QUALITY (1-10)**
Is the execution high quality? For code: clean, idiomatic, well-structured? For text: clear, well-organized, no fluff? For design: thoughtful, consistent?

**4. ALIGNMENT (1-10)**
Does the result match the INTENT behind the request, not just the literal words? Would the person who made this request be satisfied? Does it solve their actual problem?

**5. USABILITY (1-10)**
Can the requester immediately use this? Is it complete and self-contained? Or does it need significant additional work, context, or explanation?

---

## OUTPUT FORMAT (follow strictly)

### Scores

| Criterion | Score | One-line verdict |
|-----------|-------|-----------------|
| Completeness | X/10 | ... |
| Correctness | X/10 | ... |
| Quality | X/10 | ... |
| Alignment | X/10 | ... |
| Usability | X/10 | ... |
| **OVERALL** | **X/10** | **[verdict]** |

Overall = average of 5 scores, rounded to nearest integer.

### Detailed Analysis

#### Completeness (X/10)
[evidence and gaps — with specific quotes from the result]

#### Correctness (X/10)
[evidence and gaps]

#### Quality (X/10)
[evidence and gaps]

#### Alignment (X/10)
[evidence and gaps]

#### Usability (X/10)
[evidence and gaps]

### What's Good
- [specific strength 1 with evidence]
- [specific strength 2 with evidence]

### What's Wrong
- [specific issue 1]: [what's wrong] → [how to fix]
- [specific issue 2]: [what's wrong] → [how to fix]

### Top-3 Actions (priority order)
1. [most critical action] — WHY and HOW specifically
2. [second action] — WHY and HOW specifically
3. [third action] — WHY and HOW specifically

Be brutally honest. Do not soften criticism. The purpose is to catch what the original creator missed. If the result is genuinely excellent, say so — but still find at least one thing that could be better.
```

### Шаг 3. Представь результат

Получи ответ субагента и оформи для пользователя.

**Светофор по средней оценке:**
- **GREEN** (8-10) — готово, высокое качество
- **YELLOW** (5-7) — требует доработки, но основа крепкая
- **RED** (1-4) — значительная переработка

**Формат вывода в чат:**

```
## Quality Gate: [название объекта]

**Вердикт: [GREEN/YELLOW/RED] [X/10]**

[1-2 предложения — суть]

| Критерий | Оценка | Вердикт |
|----------|--------|---------|
| Полнота | X/10 | ... |
| Корректность | X/10 | ... |
| Качество | X/10 | ... |
| Соответствие | X/10 | ... |
| Готовность | X/10 | ... |

### Что исправить (топ-3)
1. ...
2. ...
3. ...

Полный отчёт → [путь к файлу]
```

### Шаг 4. Сохрани полный отчёт

Полный отчёт субагента (переведённый на русский) сохрани в файл:
- Имя: `Quality-Gate-[Topic]-YYYY-MM-DD.md` (тема коротко, латиницей)
- Куда: в текущую рабочую директорию, или рядом с оцениваемыми файлами

### Шаг 5. Если RED — план исправлений

Если средняя оценка < 5:
- Вытащи все пункты "how to fix" в конкретный план действий
- Предложи: "Хочешь чтобы я исправил? Начну с пункта 1."

---

## Правила

1. **Один агент, чистый контекст.** Критик получает ТОЛЬКО запрос + результат. Никакой истории.
2. **Промпт на английском, отчёт на русском.** Субагент работает на английском, финальная презентация — на русском.
3. **Конкретность.** Каждая оценка — с цитатами из результата. Никаких generic "could be better".
4. **Write-first.** Полный отчёт всегда в файл.
5. **Не защищай свою работу.** Если ты (оркестратор) делал оцениваемую работу — НЕ спорь с критиком. Представь результат как есть.
6. **Sonnet для критика.** Используй model: "sonnet" — быстрее, дешевле, outsider perspective не требует самой сильной модели.

---

$ARGUMENTS
