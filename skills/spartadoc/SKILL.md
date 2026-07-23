---
name: spartadoc
description: Drive the Spartadoc platform from an agent — request documents from people, manage mortgage dossiers, upload/classify/share documents, check completeness, and verify authenticity. Requires a Spartadoc account and an API key (`SPARTADOC_API_KEY`). Use when the user wants to collect, organize, share, or verify documents through their Spartadoc vault.
---

# Spartadoc — document vault & requests

Spartadoc is a post-quantum "smart vault" for documents (built for mortgage
brokers, extensible to any document-heavy workflow). This skill drives it from an
agent via the `spartadoc` CLI, authenticated by an API key — no interactive login.

## Setup

```bash
# 1. Install the CLI (command: `spartadoc`)
spartadoc --version || npm install -g @spartadoc/cli

# 2. Authenticate non-interactively with an API key
export SPARTADOC_API_KEY="sk_...   # obtained from the user's Spartadoc account"
```

The agent must obtain the API key from the user (their Spartadoc account →
API keys). Never invent it. Every command below reads `SPARTADOC_API_KEY`.

> Add `--json` to any command for machine-readable output.

## What you can do

### Dossiers (a person's file / set of documents)
```bash
spartadoc dossier list                 # list dossiers
spartadoc dossier get <id>             # details of one dossier
spartadoc dossier completeness <id>    # which expected documents are missing
spartadoc dossier dashboard            # overview / progress
spartadoc dossier create               # create a dossier
spartadoc dossier archive <id> | restore <id>
```

### Documents
```bash
spartadoc doc list <dossierId>         # documents in a dossier
spartadoc doc upload <file>            # upload (auto-classified by the AI)
spartadoc doc get <id> | download <id>
spartadoc doc analyze <file>           # classify/inspect without storing
```

### Request documents from someone
Ask a borrower/client to send specific documents (they get a magic link; the
upload lands as PENDING for approval). Use the request flow of the dossier /
notification commands — run `spartadoc dossier --help` and
`spartadoc notification --help` for the exact subcommand and flags.

### Share (with control after sharing)
```bash
spartadoc dossier share                # share a dossier/document
spartadoc dossier revoke <dossierId> <shareId>   # revoke access later
```

### Verify authenticity
```bash
spartadoc verify <file.sdoc>           # verify a Spartadoc-issued document
```

## Discovering exact flags

Command groups and flags evolve. When unsure, ask the CLI itself:
```bash
spartadoc --help
spartadoc <group> --help          # e.g. spartadoc dossier --help
```

## Safety rules for the agent

- **Never** print, log, or commit `SPARTADOC_API_KEY`. Treat it as a credential.
- Document requests and shares are **outward-facing** (they e-mail real people).
  Confirm recipient, documents, and intent with the user before sending.
- Uploaded documents may contain personal information — do not exfiltrate content
  to third parties.
- Prefer `--json` and check exit codes; a non-zero exit means the operation failed.

## The `.sdoc` format

Spartadoc encrypts documents in the open post-quantum `.sdoc` format. To work with
`.sdoc` files directly (no account needed), see the standalone
[`sdoc` skill](https://github.com/spartadoc-sdoc/sdoc/blob/main/skills/sdoc/SKILL.md).
