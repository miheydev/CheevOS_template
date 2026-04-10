# example-knowledge-base

> Скелет **личной базы знаний / second brain**. Показывает, как раскладывать цели, проекты, идеи, wiki, бренд-материалы и обучение.
>
> **Это пример.** Переименуй (`example-knowledge-base` → `my-kb` / `jeeves` / `brain`), отредактируй `CLAUDE.md`, наполняй.

## Структура

```
example-knowledge-base/
├── CLAUDE.md             # Правила проекта (для AI)
├── README.md             # Этот файл
│
├── Goals/                # Годовые, квартальные, месячные цели
├── Health/               # Тренировки, питание, показатели
├── Finance/              # Финансовые планы, инвестпортфель, траты
│
├── Projects/
│   ├── Active/           # Активные личные проекты (по 1 папке)
│   └── Archive/          # Завершённые / отложенные
│
├── Ideas/                # Сырые идеи, гипотезы, черновики
│
├── Wiki/                 # LLM Wiki — кросс-проектная база доменных знаний
│   ├── index.md          # Каталог страниц (точка входа для AI)
│   ├── current-focus.md  # Что сейчас изучаем
│   ├── sources.md        # Реестр обработанных источников
│   ├── log.md            # Append-only таймлайн операций
│   └── raw/              # Неизменяемые сырые файлы (статьи, транскрипты)
│
├── Brand/
│   └── Content/
│       ├── tov-profiles/ # ToV-профили (голос автора) для /tov-write
│       ├── style-guides/ # Style guides по каналам
│       └── Drafts/       # Черновики постов
│
├── Learning/             # Конспекты курсов, книг, видео
└── INBOX/                # Локальный inbox проекта
```

## Ключевые паттерны

### 1. Goals — иерархия целей

```
Goals/
├── 2026-year-plan.md              # Годовой план
├── 2026-Q2-plan.md                # Квартальный
├── 2026-04-goals.md               # Месячные
└── retrospectives/
    └── 2026-03-retro.md           # Месячные ретро
```

### 2. Projects/Active — личные проекты

1 папка = 1 проект. Не путать с рабочими проектами/сделками в `example-agency/`.

```
Projects/Active/<slug>/
├── <slug>.md              # Главный файл — описание, статус, следующие шаги
├── changelog.md           # Хронология
└── attachments/
```

### 3. Wiki — LLM Wiki паттерн

Паттерн из [Karpathy LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). Подробно описан в корневом [CLAUDE.md](../CLAUDE.md) → секция "LLM Wiki".

**Операции:**
- **Ingest** — пользователь даёт источник → AI создаёт/обновляет wiki-страницу + cross-references
- **Query** — вопрос по базе → AI синтезирует ответ + (опционально) сохраняет как новую страницу
- **Lint** — проверка orphans, broken refs, stale claims

**Точка входа для AI — всегда `index.md`.**

### 4. Brand/Content/tov-profiles — голос автора

ToV-профили, которые подгружает скилл `/tov-write`:

```
Brand/Content/tov-profiles/
├── personal-tg.md         # Голос для личного TG-канала
├── blog-expert.md         # Голос для экспертного блога
└── linkedin.md            # Голос для LinkedIn
```

Каждый профиль — результат анализа ~30 постов через `tone-of-voice-analyzer` (есть как плагинный скилл Anthropic).

## Куда что класть (спорные случаи)

| Материал | Куда |
|---|---|
| Статья про AI-агенты из Twitter | `Wiki/` (ingest) |
| Идея для курса | `Ideas/` (сырая) → `Projects/Active/` (когда проект запущен) |
| Ежедневная заметка по цели | обновить соответствующий `Goals/YYYY-MM-*.md` |
| Транскрипт подкаста для собственных заметок | `Learning/` |
| Черновик поста | `Brand/Content/Drafts/` |
| Входящий pdf от страховой | `INBOX/` → потом в `Finance/` или `Health/` |
