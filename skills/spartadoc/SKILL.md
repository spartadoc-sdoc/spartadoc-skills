---
name: spartadoc
description: Drive the Spartadoc platform from an agent over its REST API — request documents from people, manage mortgage dossiers, upload/list/share documents, check completeness, and verify authenticity. Requires a Spartadoc account and an API key (`SPARTADOC_API_KEY`). Use when the user wants to collect, organize, share, or verify documents through their Spartadoc vault.
---

# Spartadoc — document vault & requests (REST API)

Spartadoc is a post-quantum "smart vault" for documents. This skill drives it
directly over its **REST API** using an API key — no CLI, no interactive login,
no extra install.

## Setup

The agent needs an **API key** from the user's Spartadoc account. Keys look like
`sdoc_…`. The user creates one in their account (see "Manage keys" below) and
provides it:

```bash
export SPARTADOC_API_KEY="sdoc_..."          # from the user's Spartadoc account
export SPARTADOC_BASE="https://spartadoc.com" # or a tenant: https://<tenant>.spartadoc.com
```

Never invent a key. Never print, log, or commit it.

## Authentication

Every request sends the key in the **`X-API-Key`** header:

```bash
curl -H "X-API-Key: $SPARTADOC_API_KEY" "$SPARTADOC_BASE/api/health"
```

## Discover the full API (source of truth)

The platform publishes an **OpenAPI 3.1** spec — always the authoritative list of
endpoints, methods, params, and schemas:

```bash
curl "$SPARTADOC_BASE/openapi.json"
```

Fetch it first and let it drive exact paths; the examples below are the common
entry points.

## Common operations

```bash
AUTH=(-H "X-API-Key: $SPARTADOC_API_KEY")

# Health / connectivity
curl "${AUTH[@]}" "$SPARTADOC_BASE/api/health"

# Dossiers / mortgage files (list, detail, completeness)
curl "${AUTH[@]}" "$SPARTADOC_BASE/api/mortgage-files"
# broker-scoped operations live under /api/broker

# Documents (list, get, upload)
curl "${AUTH[@]}" "$SPARTADOC_BASE/api/documents"

# Request documents from a person (magic-link; upload lands as PENDING)
curl "${AUTH[@]}" -X POST "$SPARTADOC_BASE/api/document-requests" \
  -H "Content-Type: application/json" \
  -d '{ "borrowerEmail": "client@example.com", "docTypes": ["NOTICE_OF_ASSESSMENT"] }'

# Shares (share a dossier/document, then revoke)
curl "${AUTH[@]}" "$SPARTADOC_BASE/api/shares"

# Inbox (incoming e-mail deposits)
curl "${AUTH[@]}" "$SPARTADOC_BASE/api/inbox"
```

> Exact fields and sub-paths: consult `openapi.json`. Request bodies vary by
> endpoint; the spec is authoritative.

## Manage keys

The user's own API keys (create/list/revoke) live under `/api/account/api-keys`
(max 5 active per account; the plaintext key is shown **once** at creation).
The very first key is created from a signed-in session (web), not from the API.

## Safety rules for the agent

- **Never** print, log, or commit `SPARTADOC_API_KEY` — it is a credential.
- Requesting documents and sharing are **outward-facing** (they e-mail real
  people). Confirm recipient, documents, and intent with the user before POSTing.
- Documents may contain personal information (Loi 25 / PIPEDA) — do not exfiltrate
  content to third parties.
- Check HTTP status codes; a 401 means the key is missing/invalid, 429 means rate
  limited (per-key hourly limit).

## The `.sdoc` format

Spartadoc encrypts documents in the open post-quantum `.sdoc` format. To work with
`.sdoc` files directly (no account needed), see the standalone
[`sdoc` skill](https://github.com/spartadoc-sdoc/sdoc/blob/main/skills/sdoc/SKILL.md).
