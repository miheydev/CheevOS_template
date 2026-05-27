---
description: "Safety rules for Telegram Client API (Telethon). Load before any work with the Telegram MCP."
language: en
---

# Telegram Client API — Safety Rules

The `telegram` MCP server is connected to a personal Telegram account via the Client API (Telethon).

## FORBIDDEN (hard limits)

- **DO NOT** send mass messages — only individual targeted messages
- **DO NOT** send more than 30 messages per hour (rate limit is hardcoded)
- **DO NOT** add people to groups or channels without an explicit user request
- **DO NOT** scrape members from other groups/channels
- **DO NOT** message strangers (cold outreach / spam)
- **DO NOT** send automated replies without user confirmation
- **DO NOT** forward messages between chats without an explicit request
- **DO NOT** delete other people's messages
- **DO NOT** change account settings (name, photo, bio)

## ALLOWED

- Read incoming messages and conversations
- Send messages when explicitly requested by the user
- Search for messages in chats
- Check unread messages

## Session Security

- The session file and `TELEGRAM_SESSION_STRING` are equivalent to an account password
- **NEVER** output the session string in chat
- **NEVER** commit `.env` containing the session string
- `.env` is added to `.gitignore`
