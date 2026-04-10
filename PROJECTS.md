# Проекты Workspace

> Навигационный дашборд по всем проектам этого workspace. Заполни под свои проекты.
>
> **Связанные документы:** [CLAUDE.md](./CLAUDE.md) (правила для AI) · [SERVICES.md](./SERVICES.md) (запуск сервисов)

---

## Как использовать этот файл

`PROJECTS.md` — точка входа в workspace для тебя и для AI. Здесь быстро видно:
- Какие проекты активны, в каком статусе
- Где лежат правила, задачи, ключевые файлы каждого проекта
- Как запустить/проверить каждый подпроект

Добавляй проект как отдельный раздел ниже. Старайся держать файл ≤ 200 строк — длинные списки мешают AI быстро его читать.

---

## Шаблон записи проекта

Скопируй блок ниже и заполни под новый проект:

```markdown
### <Название проекта>
**Что:** <одна строка — что это>
**Статус:** 🟢 Активный / 🟡 Пауза / 🔴 Запуск / ⚪ Архив
**Git:** <URL репозитория, если есть>

| Что искать | Где |
|------------|-----|
| Правила проекта | [<project>/CLAUDE.md](./<project>/CLAUDE.md) |
| Задачи | [<project>/tasks.md](./<project>/tasks.md) |
| README | [<project>/README.md](./<project>/README.md) |

**Ближайший шаг:** <что нужно сделать следующим>
```

---

## Активные проекты

> Ниже — два **примерных** проекта со скелетами папок. Они показывают типичные паттерны: клиентскую работу и личную базу знаний. Можешь переименовать, удалить или оставить как образцы.

### example-agency (пример клиентского проекта)
**Что:** Скелет агентства / фриланс-практики / консалтинга с CRM, встречами, стратегией и контентом.
**Статус:** 📘 Пример (не рабочий)

| Что искать | Где |
|------------|-----|
| Правила проекта | [example-agency/CLAUDE.md](./example-agency/CLAUDE.md) |
| Обзор структуры | [example-agency/README.md](./example-agency/README.md) |
| Активные сделки | [example-agency/CRM/Active/](./example-agency/CRM/Active/) |
| Задачи команды | [example-agency/Docs/Tasks/tasks.md](./example-agency/Docs/Tasks/tasks.md) |

**Паттерны:** `/prospect-research` → `CRM/Prospects/` → первая встреча → `CRM/Active/<deal>/` → `/call-analysis` → `CRM/Archive/`

---

### example-knowledge-base (пример личной базы знаний)
**Что:** Скелет second brain: цели, проекты, идеи, LLM Wiki, бренд-материалы, обучение.
**Статус:** 📘 Пример (не рабочий)

| Что искать | Где |
|------------|-----|
| Правила проекта | [example-knowledge-base/CLAUDE.md](./example-knowledge-base/CLAUDE.md) |
| Обзор структуры | [example-knowledge-base/README.md](./example-knowledge-base/README.md) |
| LLM Wiki | [example-knowledge-base/Wiki/index.md](./example-knowledge-base/Wiki/index.md) |
| Цели | [example-knowledge-base/Goals/](./example-knowledge-base/Goals/) |
| Проекты | [example-knowledge-base/Projects/Active/](./example-knowledge-base/Projects/Active/) |

**Паттерны:** идея → `Ideas/` → `Projects/Active/<slug>/`; источник → `Wiki/` ingest → cross-references → `index.md` обновлён

---

## Архив / справочники

> (Пусто.)

---

## Служебное

### INBOX
**Что:** Входящие файлы для сортировки → раскидать по проектам.
**Команда:** скажи "разбери INBOX" (см. [.claude/skills/inbox-root-processing.md](./.claude/skills/inbox-root-processing.md))

### scripts/
**Что:** Утилиты workspace: git-хуки, health checks, экспорты.

---

## Где что искать (быстрая навигация)

| Нужно | Где |
|-------|-----|
| **Правила для AI** | [CLAUDE.md](./CLAUDE.md) |
| **Сервисы / инфра** | [SERVICES.md](./SERVICES.md) |
| **Скиллы Claude Code** | [.claude/skills/](./.claude/skills/) |
| **Команды Claude Code** | [.claude/commands/](./.claude/commands/) |
| **Этот дашборд** | [PROJECTS.md](./PROJECTS.md) (ты тут) |
