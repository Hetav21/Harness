# Claude Code adapter

The adapter keeps `AGENTS.md` as the shared policy source and installs a root
`CLAUDE.md` containing the exact `@AGENTS.md` import. Shared skills live in
`.agents/skills`; preferred Claude bridges are relative per-skill links under
`.claude/skills`.

Symlink discovery is filesystem-dependent and is not guaranteed by official
Claude Code documentation. Offline validation checks link resolution only.
When an authenticated session is available, validate discovery with `/skills`.
Missing binaries and unavailable live sessions are `skipped`, never `passed`.
