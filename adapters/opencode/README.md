# OpenCode adapter

OpenCode uses `AGENTS.md` and the direct `.agents/skills` canonical skill
location. Native agents, commands, plugins, and configuration are mapped under
`.opencode/` or `opencode.json(c)`.

The files in `templates/` are source artifacts for materialization, not a
request to install dependencies during offline validation. Session discovery,
`/mcps`, and service configuration checks are live-only. Missing binaries and
unapproved services are `skipped`.
