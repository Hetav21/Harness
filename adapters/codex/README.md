# Codex adapter

Codex reads the shared `AGENTS.md` entrypoint and discovers canonical skills
directly from `.agents/skills`. Project-safe agent, MCP, and settings mappings
belong in `.codex/config.toml`.

Offline checks are limited to version, help, and `doctor`. Session diagnostics
(`/debug-config`, `/skills`, `/mcp verbose`, and `/hooks`) are live-only and
require an available session. Do not treat TOML parsing as client acceptance.
