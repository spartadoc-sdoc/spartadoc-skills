# spartadoc-skills

Agent skills for driving the **Spartadoc** platform — a post-quantum smart vault
for documents. Give an AI agent (Claude Code, or any skill-aware agent) the
ability to request, organize, share, and verify documents through Spartadoc.

Inspired by the "skills as folders" pattern (e.g. [vercel-labs/skills](https://github.com/vercel-labs/skills)):
each skill is a folder with a `SKILL.md` an agent can load.

## Skills

| Skill | What it does | Needs |
|---|---|---|
| [`spartadoc`](./skills/spartadoc/SKILL.md) | Request documents from people, manage dossiers, upload/classify/share/verify | Spartadoc account + `SPARTADOC_API_KEY` |

For working with the open `.sdoc` encrypted file format **without an account**, see
the standalone [sdoc skill](https://github.com/spartadoc-sdoc/sdoc/blob/main/skills/sdoc/SKILL.md).

## Prerequisites

Just an account and a key — the skill talks to the Spartadoc **REST API**
directly (no CLI, no install):

1. A **Spartadoc account** ([spartadoc.com](https://spartadoc.com)).
2. An **API key** from that account (created under *Account → API keys*).

```bash
export SPARTADOC_API_KEY="sdoc_..."            # from your Spartadoc account
export SPARTADOC_BASE="https://spartadoc.com"  # or a tenant subdomain
```

Requests authenticate with the `X-API-Key` header. The full endpoint list is in
the platform's OpenAPI spec: `$SPARTADOC_BASE/openapi.json`.

## Using a skill

**Claude Code**: copy a skill folder into `.claude/skills/` (or your skills
directory) and it becomes available. Other agents: point your loader at the
`SKILL.md`.

## License

[Apache-2.0](./LICENSE) — © 2026 Spartadoc Inc.
