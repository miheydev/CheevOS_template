---
description: Independent security audit of code — secret leaks, OWASP vulnerabilities, unsafe patterns. Works with any project (git not required). Invoked as /security-review [path]
language: en
---

# Security Review — Universal Security Audit

This skill checks code for vulnerabilities and leaks. Does not require git. Works with:
- A single file (`/security-review src/api.py`)
- A folder (`/security-review src/`)
- The entire current project (`/security-review` without arguments)
- A diff (`/security-review --diff` — if git is available)

Target audience: vibe-coded code, MVPs, prototypes, scripts — places where security is typically skipped.

---

## Triggers

- `/security-review`
- `/security-review <path>`
- `/security-review --diff` (git repos only)
- "проверь на безопасность", "security audit", "нет ли утечек", "посмотри уязвимости"

---

## Process

### Step 1. Determine scope

From `$ARGUMENTS`:

| Argument | What to check |
|----------|---------------|
| empty | Entire current directory, excluding `node_modules/`, `.venv/`, `dist/`, `build/`, `.git/` |
| path to file | Only that file |
| path to folder | Everything under the folder (with the same exclusions) |
| `--diff` | `git diff HEAD` + untracked files (if git is available) |
| `--staged` | `git diff --cached` |

**Git check:** `git rev-parse --git-dir 2>/dev/null` — if absent, `--diff`/`--staged` modes are unavailable; say so and fall back to default.

**Size:** if scope contains >200 files — warn and suggest narrowing.

### Step 2. Collect project context

Before checking, quickly determine:
- **Language/stack:** by extensions (`.py`, `.ts`, `.js`, `.go`, `.rs`, `.php`, `.rb`, `.java`) and manifests (`package.json`, `requirements.txt`, `pyproject.toml`, `go.mod`, `Gemfile`, `composer.json`)
- **Application type:** web-backend / frontend / CLI / library / MCP-server / bot
- **What is already protected:** presence of `.env.example`, `.gitignore`, `SECURITY.md`, pre-commit hooks

This is needed so we don't complain about "missing rate-limiting" in a CLI utility.

### Step 3. Launch security-auditor agent

Use a `security-auditor` subagent (read-only: Read, Grep, Glob).

**If scope is large (>50 files)** — launch **2-3 agents in parallel** by domain:
1. Agent A — secrets and leaks
2. Agent B — injection and auth
3. Agent C — dependencies and configuration

**If scope is small** — one agent covers everything.

**Agent prompt (in English — more precise terminology):**

```
You are performing a security audit of code. Scope: {SCOPE_DESCRIPTION}. Stack: {STACK}. App type: {APP_TYPE}.

This code is often "vibe-coded" — written quickly without formal review. Be paranoid but pragmatic: flag real risks, not theoretical ones. Skip bikeshedding.

## CHECK EVERY CATEGORY

### 1. Secrets & Credentials Leakage (CRITICAL)
- Hardcoded API keys, tokens, passwords, private keys (look for: sk-, ghp_, xoxb-, AKIA, -----BEGIN, Bearer, api[_-]?key, password\s*=, token\s*=)
- Connection strings with embedded passwords (postgres://user:pass@, mongodb://...)
- JWT secrets, session secrets in code
- `.env` files committed or tracked
- Secrets in logs / console.log / print()
- Secrets in client-side / frontend code (anything in public bundles)
- Secrets in error messages / stack traces returned to users
- Sample credentials that look real (not `example.com` / `xxx`)

### 2. Injection Vulnerabilities
- SQL injection: string concatenation in queries, f-strings in SQL, unparameterized queries
- Command injection: `exec`, `system`, `subprocess.run(..., shell=True)` with user input; `child_process.exec` with interpolation
- Path traversal: user input flowing to file paths without `path.resolve` / `os.path.abspath` + allowlist check
- LDAP/NoSQL injection
- Template injection (Jinja2 render_string with user input, Handlebars SafeString)
- Prompt injection risks in LLM calls (unfiltered user input in system prompts)

### 3. XSS & Output Encoding
- `innerHTML =`, `dangerouslySetInnerHTML`, `v-html` with user data
- `document.write` with input
- Unescaped template variables in server-rendered HTML
- CSP absent or `unsafe-inline`/`unsafe-eval`

### 4. Authentication & Authorization
- Missing auth checks on protected endpoints
- Broken access control: user ID from request body / query instead of session
- IDOR: direct object references without ownership check
- JWT without signature verification, `alg: none`, weak secret
- Session fixation, missing HttpOnly / Secure / SameSite cookie flags
- Password storage: plain text, MD5, SHA1, no salt (should be bcrypt/argon2/scrypt)
- Weak password reset: predictable tokens, no expiry

### 5. Crypto & Randomness
- `Math.random()` / `random.random()` for security purposes (tokens, IDs) — should be `crypto.randomBytes` / `secrets.token_*`
- MD5/SHA1 for anything security-related
- ECB mode, no IV, hardcoded IV
- Custom crypto instead of vetted libraries
- TLS disabled / cert validation skipped (`verify=False`, `rejectUnauthorized: false`, `InsecureSkipVerify: true`)

### 6. Input Validation & Deserialization
- `pickle.loads`, `yaml.load` (not safe_load), `eval`, `exec`, `Function()`, `setTimeout(string)` on untrusted input
- Missing validation on external inputs
- Trusting client-side validation only
- Type confusion / mass assignment (whole request body → ORM)

### 7. SSRF & Outbound Requests
- User-controlled URLs in `fetch` / `requests` / `http.get` without allowlist
- Webhook targets without validation (cloud metadata: 169.254.169.254, internal IPs)
- Redirect URLs from query params without allowlist (open redirect)

### 8. Logging & Error Handling
- Logging PII, tokens, passwords, full request bodies
- Stack traces returned to clients in production
- `DEBUG=True` / `development` mode checks

### 9. Dependencies & Supply Chain
- Unpinned versions: `npx <pkg>@latest`, `pip install pkg` without version, `^`/`~` in critical deps
- Known-vulnerable packages (flag and suggest `npm audit` / `pip-audit` / `cargo audit`)
- Typosquatting-prone names
- MCP servers with unpinned `npx` (RCE vector)
- Post-install scripts in untrusted packages

### 10. Configuration & Secrets Management
- `.env` not in `.gitignore`
- `.mcp.json` with secrets in `env` block (should use dotenv)
- Overly permissive CORS (`*` with credentials)
- Default credentials (admin/admin, root/root)
- Exposed admin/debug endpoints

### 11. File Upload & Storage
- No MIME/extension check, no size limit
- User-controlled filenames (path traversal via filename)
- Uploads served from same origin without `Content-Disposition: attachment`

### 12. Race Conditions & Business Logic
- TOCTOU (check-then-use without locking)
- Money/balance updates without transactions
- Rate limiting absent on expensive / abusable endpoints (OTP, password reset, LLM calls)

## OUTPUT FORMAT (strict)

For each finding:

### [SEVERITY] Title
- **File:** `path/to/file.ext:line` (exact line if possible)
- **Snippet:** 3-10 lines of relevant code
- **Why it's a problem:** 1-2 sentences, concrete attack scenario
- **Fix:** specific code change or library to use
- **Confidence:** high / medium / low (low = potential FP, needs human eyes)

Severity rubric:
- **CRITICAL** — exploitable now, leaks secrets, RCE, auth bypass
- **HIGH** — significant risk, likely exploitable under realistic conditions
- **MEDIUM** — defense-in-depth issue, exploitable with chained conditions
- **LOW** — hardening recommendation, not directly exploitable
- **INFO** — observation / best practice

At the end:

## Summary
- Total: X findings (Ccritical / Hhigh / Mmedium / Llow / Iinfo)
- Top-3 immediate actions
- Clean areas (what was checked and looked good — at least 2-3 items, so user knows what you actually looked at)

Be direct. Skip generic advice. Cite exact file:line. No findings = say "clean on the checked scope" and list what you checked.
```

### Step 4. Format report for chat

**Short summary (in chat):**

```
## Security Review: [scope]

**Result:** 🔴 X critical / 🟠 Y high / 🟡 Z medium / ⚪ W low

### Top-3 urgent actions
1. 🔴 [title] — `file:line`
2. 🟠 [title] — `file:line`
3. 🟡 [title] — `file:line`

### Checked and clean
- ...
- ...

Full report → [path to file]
```

If everything is clean:

```
## Security Review: [scope]

**Result:** ✅ Clean on the checked scope.

### What was checked
- [category 1]: N files
- [category 2]: N files
...

### Hardening recommendations (non-critical)
- ...
```

### Step 5. Save full report

**Where:**
- If the project has `docs/` or `security/` — save there
- Otherwise — in the root of the checked folder
- Name: `Security-Review-YYYY-MM-DD.md`

**If there are critical findings with secrets** — DO NOT write the actual secrets into the report. Use masking: `sk-ant-***` + reference to `file:line`. The report may be accidentally committed.

### Step 6. Suggest next step

- If CRITICAL → "Want me to fix it right now? I'll start with [item 1]."
- If HIGH/MEDIUM → "I can format these as issues / tasks, or fix them in order."
- If secret leak → "Secret in code — it must be **revoked and rotated**, even if you delete it from history. Help with rotation?"

---

## Rules

1. **Read-only by default.** The skill does not modify code on its own — only reports. Fixes happen in a separate step after agreement.
2. **DO NOT output secrets to chat.** Mask them: `sk-ant-api03-***abc`. Reason: chat is logged and secrets can end up in the transcript.
3. **DO NOT use `grep -h` on suspicious lines.** When collecting evidence, use `grep -l` (file names only) or `grep -n` with a narrow pattern. To show content — read only the needed lines via Read. Lesson from `feedback_no_secret_leaks_in_grep.md`: leaked 2 keys to stdout during debug.
4. **Mark FP-prone findings with Confidence.** Example: "password" in a comment — low confidence. Real hardcoded `sk-ant-...` — high.
5. **Stack-aware.** For Python CLI, do not complain about missing CSP. For React frontend, do not complain about SQL injection. Consider app type from Step 2.
6. **DO NOT duplicate `npm audit`.** If the project already uses pip-audit/npm audit in CI — mention it, but do not recount CVEs yourself.
7. **Secrets in git history.** If a secret is found — check `git log -p --all -- <file>` (if git is available) to see if it was already in history. Rotation is required even if it has since been deleted.
8. **MCP-specific.** When encountering `.mcp.json` — check: (a) pinned versions, (b) no secrets in `env`, (c) no `@latest`, (d) no unverified package authors.
9. **Prompt injection.** If the project uses an LLM API — separately check whether user input is concatenated into system prompts without sanitization.
10. **DO NOT panic over CORS `*` in dev-only config.** Verify that the config is actually used only in dev.

---

## Common skill mistakes (what to avoid)

- Complaining about missing rate-limiting in a script that is not an HTTP server
- Flagging `console.log("user:", user.id)` as a PII leak (ID ≠ PII in general)
- Treating `Math.random()` for animation seed as a security issue
- Alarming over `eval()` in `eslint.config.js` (it's a config, not user input)
- Requiring 2FA / MFA in a library that has no auth at all
- Suggesting "add a WAF" — not a security review concern, that's infrastructure
- Inventing CVE numbers — if unsure, say "known vulns — run `npm audit`"

---

## Integration with other skills

- After `/security-review` + CRITICAL findings → suggest `/evaluate` after fixes
- If a secret is found → mention the rotation runbook (if present in the project)
- For large projects → suggest adding to CI: `npm audit` / `pip-audit` / `gitleaks`

---

$ARGUMENTS
