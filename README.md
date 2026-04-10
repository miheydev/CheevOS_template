# CheevOS Template

> Скелет multi-project workspace для работы с [Claude Code](https://docs.claude.com/en/docs/claude-code). Скиллы, команды и правила — сразу из коробки, содержимое проектов — твоё.

## Что внутри

```
.
├── CLAUDE.md                   # Правила для AI (безопасность, Write-First, anti-bloat, LLM Wiki, Telegram)
├── PROJECTS.md                 # Дашборд проектов (шаблон, заполнишь под себя)
├── SERVICES.md                 # Локальные/удалённые сервисы (шаблон)
├── .gitignore                  # Безопасные дефолты: .env, ключи, токены, логи, кэш
├── .claudeignore               # Что Claude Code игнорирует при авточтении
├── INBOX/                      # Корневая входящая папка для сортировки
├── scripts/                    # Утилиты workspace
│
├── .claude/
│   ├── commands/               # Слэш-команды (параллельные исследования, контент-пайплайн, advisory teams)
│   └── skills/                 # Скиллы (голос, write-post, mega-polish, video-summary, zamesin JTBD и др.)
│
├── example-agency/             # 📘 ПРИМЕР: клиентский проект (CRM, встречи, стратегия)
│   ├── CLAUDE.md
│   ├── CRM/{Active,Archive,Prospects}/
│   ├── Docs/{Strategy,Tasks,Content,Research}/
│   ├── Client-Meetings/
│   └── INBOX/
│
└── example-knowledge-base/     # 📘 ПРИМЕР: личная база знаний (Wiki, цели, проекты)
    ├── CLAUDE.md
    ├── Goals/ Health/ Finance/
    ├── Projects/{Active,Archive}/
    ├── Ideas/
    ├── Wiki/{index,current-focus,sources,log}.md + raw/
    ├── Brand/Content/{tov-profiles,style-guides,Drafts}/
    └── Learning/
```

**Два примерных проекта** (`example-agency/` и `example-knowledge-base/`) — скелеты с правилами (`CLAUDE.md`), README и пустыми папками с пояснениями. Они показывают типичные паттерны организации: клиентская работа и личный second brain. Переименуй, удали или оставь как образцы — на твой выбор.

## Быстрый старт

```bash
git clone <this-repo> my-workspace
cd my-workspace

# 1. Прочитай CLAUDE.md — подстрой правила под себя
# 2. Посмотри example-agency/ и example-knowledge-base/ — понять паттерны
# 3. Переименуй examples или удали их, создай свои подпроекты
# 4. Обнови PROJECTS.md — дашборд твоих проектов
# 5. Открой папку в Claude Code / VS Code
```

## Что в .claude/

### Команды (`.claude/commands/`)

Запускаются через `/<name>` в Claude Code:

| Команда | Что делает |
|---|---|
| `/business-advisory-team` | Обсуждение бизнес-идеи командой из 6 AI-экспертов (3 раунда) |
| `/product-dev-team` | Обсуждение продуктового/технического решения командой из 6 специалистов |
| `/expecto-patronum` | Обсуждение бизнес-идеи командой из 6 магов Хогвартса (3 раунда) |
| `/idea-quickcheck` | Быстрая проверка бизнес-идеи за 2 минуты (3 эксперта, 1 раунд) |
| `/deep-research` | Глубокое параллельное исследование темы через 5 субагентов |
| `/prospect-research` | Исследование потенциального клиента (5 параллельных агентов) |
| `/seo-research` | SEO-исследование — ключевые слова, SERP-анализ, content gap |
| `/content-ideas` | 10 идей для контента по теме — тренды, боли ЦА, неочевидные углы |
| `/content-pipeline` | End-to-end генерация контента от идеи до готового поста |
| `/project-context-setup` | Инициализация проектного контекста — 4 базовых файла |
| `/reflect` | Фиксация урока из ошибки (с маршрутизацией в CLAUDE.md / memory) |

### Скиллы (`.claude/skills/`)

On-demand инструменты, которые вызываются по ключевым словам:

- **Контент:** `write-post`, `tov-write`, `mega-polish`, `create-docx`, `create-proposal`
- **Входные данные:** `voice`, `voice-tg`, `video-summary`, `youtube-transcript`, `tg-copy-paste`
- **Встречи/звонки:** `call-analysis`, `call-quality-review`
- **Продажи/клиенты:** `prospect-brief`
- **Анализ:** `svetofor` (красное/жёлтое/зелёное), `evaluate` (quality gate через свежего агента)
- **Процессы:** `inbox-root-processing`
- **JTBD:** `zamesin/` — набор скиллов по методологии AJTBD (Замесин)

## Безопасность

Этот шаблон заточен под работу с чувствительными данными (клиенты, сделки, личные документы). Ключевые точки:

- `.gitignore` исключает `.env`, `*.key`, `*.pem`, токены, credentials, `.mcp.json`
- CLAUDE.md запрещает хранить секреты в `env`-блоке `.mcp.json` (только через `dotenv`)
- CLAUDE.md требует пинить версии npm-пакетов в `.mcp.json` — защита от supply chain атак
- Для Telegram Client API (если подключаешь) — жёсткие ограничения на массовые действия

## Лицензия

MIT (или добавь свою)
