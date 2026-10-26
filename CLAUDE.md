# Team Lead Workspace

This project is the team lead's personal automation workspace. Every skill in `.claude/skills/` reads this file for context. Fill in the TODOs.

## Language
- Write all user-facing output (reports, questions, Confluence pages, Jira comments) in **Hebrew**.
- Keep code, Jira keys, branch names, commands and technical terms in English.

## Team
- Team lead: TODO (name, Jira username)
- Administrator: TODO
- Commander: TODO
- Product: TODO
- Soldiers: TODO (name — Jira username — Git username — main systems)

## Systems
| System | Jira project key | Repo path / URL | CI job | Test env URL | Main users |
|---|---|---|---|---|---|
| TODO | TODO | TODO | TODO | TODO | TODO |
| TODO | TODO | TODO | TODO | TODO | TODO |
| TODO | TODO | TODO | TODO | TODO | TODO |
| TODO | TODO | TODO | TODO | TODO | TODO |
| TODO | TODO | TODO | TODO | TODO | TODO |

## Tools
- Jira: TODO (URL) — accessed via the `atlassian` MCP server
- Confluence: TODO (URL, team space key) — accessed via the `atlassian` MCP server
- Git: TODO (GitLab / Bitbucket, URL). CLI: TODO (`glab` / `git` only)
- CI: TODO (Jenkins / GitLab CI, URL). Token in env var `CI_TOKEN`
- ServiceIT: TODO
- Desktop apps without an API (Jabber, desktop Outlook, internal Windows apps): use the `windows-mcp` server to read their windows (Snapshot/Screenshot). Clicking, typing or sending anything in them requires the user's approval.

## Conventions
- Jira hierarchy: Epic → Story/Task → Sub-task. Required fields: TODO
- Branch naming: `<JIRA-KEY>-short-description` (TODO: adjust)
- Merge requests: target branch TODO (e.g. `develop`), at least one human reviewer
- Definition of Done: TODO

## Hard rules (all skills)
- Never write to Jira, Confluence, Git remotes or CI without showing the user exactly what will be written and getting approval.
- Never push to `main`/`master`/`develop` directly. Never force-push.
- Never run anything against production environments.
- Never put credentials in files; read them from environment variables.
