# scripts/

Утилиты workspace. Пусто по умолчанию — добавляй сюда скрипты под свои нужды.

Типичные примеры того, что сюда кладут:

- `install-git-hooks.sh` — установка pre-commit хуков (security checks, lint)
- `security-check.sh` — проверка, что в диффе нет секретов / токенов / ключей
- `backup.sh` — бэкап workspace в архив
- `wiki_check.py` — линтер для LLM Wiki (orphans, stale, broken refs)
- `update-<project>.sh` — обновление данных конкретного подпроекта

## Правила

- Скрипты должны быть самодостаточны (не требовать чего-то, что не документировано)
- Если скрипт требует `.env` — клади `.env.example` рядом
- Комментируй `#!/usr/bin/env bash` или аналог в первой строке
- Ставь `chmod +x`
