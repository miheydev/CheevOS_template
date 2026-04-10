# Client-Meetings/

Транскрипты и саммари встреч с клиентами.

## Именование

```
YYYY-MM-DD_клиент_тип-встречи.md
```

Типы встреч: `discovery`, `demo`, `followup`, `kickoff`, `status`, `retro`, `closeout`

Примеры:
- `2026-03-15_acme-corp_discovery.md`
- `2026-04-02_helloworld_demo.md`

## Workflow

1. Положи сырой транскрипт сюда
2. `/call-analysis` → саммари + action items
3. Саммари скопируй в `CRM/Active/<сделка>/attachments/` и сошлись на него из `deal.md`
4. Добавь запись в `changelog.md` сделки

## Дашборд (опционально)

Если встреч становится много — заведи `Dashboard.md` с таблицей:

```markdown
| Дата | Клиент | Тип | Оценка (/10) | Ключевые action items |
|------|--------|-----|--------------|----------------------|
```
