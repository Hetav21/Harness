# Codex adapter

Codex reads the shared `AGENTS.md` entrypoint and discovers canonical skills
directly from `.agents/skills`. Project-safe agent, MCP, and settings mappings
belong in `.codex/config.toml`.

Adapter version 2 uses `[mcp_servers.<name>]` for selected MCP servers and
`[agents.<name>]` for selected roles. Each role entry supplies `description` and
`config_file`; write the corresponding role layer to `.codex/<role-name>.toml`
with the portable contract in `developer_instructions`. Resolve `config_file`
relative to `.codex/config.toml`, so its value is `<role-name>.toml`.

Render only selected sections. Resolve `{{...}}` placeholders from verified
inputs and escape inserted values as TOML strings; Codex does not perform this
template substitution. Keep MCP entries disabled until explicitly enabled.
Record adapter version 2 in the workspace manifest when materializing this
adapter. Skills in `.agents/skills` need no `[project]` configuration section.

Validate both rendered files against the official
[configuration schema](https://developers.openai.com/codex/config-schema.json)
and check that role file references resolve before attempting live discovery.

Offline CLI checks are limited to version, help, and `doctor`. Session diagnostics
(`/debug-config`, `/skills`, `/mcp verbose`, and `/hooks`) are live-only and
require an available session. Do not treat TOML parsing as client acceptance.
