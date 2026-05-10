# AGENTS.md

## Cursor Cloud specific instructions

### Repository overview

This is a **configuration-only / knowledge-base repository** — it contains no application source code, no dependencies, and no build steps. The repo provides Cursor IDE rules (`.cursor/rules/`) and skill files (`.cursor/skills/`) that teach an AI agent how to interact with the **ProBox One** ERP system via its REST API (v4).

### Required tools

- `curl` and `jq` are the primary tools used to interact with the ProBox One API. Both are pre-installed in the Cloud Agent VM.

### Authentication

- Authenticated API calls require a **Bearer token** passed via `Authorization: Bearer <token>`.
- The token should be stored locally in `token.txt` at the repo root (gitignored). Alternatively, set `PROBOX_BEARER_TOKEN` as an environment secret.
- **Never** commit tokens to the repository.

### API connectivity

- The target tenant is `https://hapl.s4.probox.one`.
- **`OPTIONS /v4`** works without authentication and returns the full module discovery (59 modules). Use this to verify network connectivity.
- All data-reading endpoints (e.g. `GET /v4/contact/contacts`) require a valid Bearer token (returns HTTP 401 without one).

### No lint / test / build

There is no application code, so there are no lint checks, automated tests, or build commands. The development workflow consists of editing `.cursor/rules/` and `.cursor/skills/` Markdown files and testing them by making REST API calls via `curl`.

### Skill navigation

See `.cursor/skills/probox-hapl-s4-rest/SKILL.md` for the skill index. Read the relevant skill before writing API calls for a specific domain (B2B purchase, warehouse inbound, accounting payment, etc.).
