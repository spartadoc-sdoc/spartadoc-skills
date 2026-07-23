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

1. A **Spartadoc account** ([spartadoc.com](https://spartadoc.com)).
2. An **API key** from that account.
3. The CLI: `npm install -g @spartadoc/cli` (command: `spartadoc`).

```bash
export SPARTADOC_API_KEY="sk_..."   # from your Spartadoc account
```

## Using a skill

**Claude Code**: copy a skill folder into `.claude/skills/` (or your skills
directory) and it becomes available. Other agents: point your loader at the
`SKILL.md`.

## License

[Apache-2.0](./LICENSE) — © 2026 Spartadoc Inc.
