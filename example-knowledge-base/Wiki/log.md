# Wiki Operations Log

> **Append-only.** Не редактируй прошлые записи.
>
> Каждая операция с wiki (ingest, major edit, lint) оставляет здесь запись.

## Формат записи

```markdown
## YYYY-MM-DD HH:MM — <operation>
- **Action:** ingest / update / lint / merge / delete
- **Source:** <если применимо>
- **Affected pages:** [page1.md](./page1.md), [page2.md](./page2.md)
- **Notes:** короткий контекст что и почему
```

---

## 2026-01-01 00:00 — wiki initialized

- **Action:** init
- **Notes:** Wiki создан как часть CheevOS-Template. Пока пусто.
