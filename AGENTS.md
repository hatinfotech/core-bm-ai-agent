# AGENTS.md

## Cursor Cloud specific instructions

### Repository overview

This is a **configuration-only / knowledge-base repository** — it contains no application source code, no dependencies, and no build steps. The repo provides Cursor IDE rules (`.cursor/rules/`) and skill files (`.cursor/skills/`) that teach an AI agent how to interact with the **ProBox One** ERP system via its REST API (v4).

### Required tools

- `curl` and `jq` are the primary tools used to interact with the ProBox One API. Both are pre-installed in the Cloud Agent VM.

### Authentication

- Authenticated API calls require a **Bearer token** passed via `Authorization: Bearer <token>`.
- The `PROBOX_BEARER_TOKEN` environment secret is the preferred source. Use it in curl: `-H "Authorization: Bearer $PROBOX_BEARER_TOKEN"`.
- Fallback: `token.txt` at the repo root (gitignored).
- **Never** commit tokens to the repository.

### API connectivity

- The target tenant is `https://hapl.s4.probox.one`.
- **`OPTIONS /v4`** works without authentication and returns the full module discovery (59 modules). Use this to verify network connectivity.
- All data-reading endpoints (e.g. `GET /v4/contact/contacts`) require a valid Bearer token (returns HTTP 401 without one).

### Gotchas

- The `filter_Name` parameter on contacts is an exact-match filter on the `Name` field, not a fuzzy search. Contact codes (e.g. `DAYRANANHTHU`) may not match by name — use `filter_Code` for code-based lookups instead.
- Do **not** use `filter_search` unless you first confirm it is supported via `OPTIONS` on that resource — some tenants error on unsupported filter columns.
- The `select` parameter can cause JOIN errors on some tenants; avoid it unless verified via OPTIONS docs.

### No lint / test / build

There is no application code, so there are no lint checks, automated tests, or build commands. The development workflow consists of editing `.cursor/rules/` and `.cursor/skills/` Markdown files and testing them by making REST API calls via `curl`.

### Skill navigation

See `.cursor/skills/probox-hapl-s4-rest/SKILL.md` for the skill index. Read the relevant skill before writing API calls for a specific domain (B2B purchase, warehouse inbound, accounting payment, etc.).
