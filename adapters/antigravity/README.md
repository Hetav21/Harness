# Antigravity adapter

Antigravity uses `AGENTS.md` and directly discovers canonical skills in
`.agents/skills`. `GEMINI.md` is an optional compatibility shim and is added
only when a selected workflow needs it. Native outputs include rules, one
role directory per selected role, MCP configuration, hooks, and conditionally
selected plugins; plugin support is not guaranteed for every profile.

Offline checks use `agy --help` and optionally `agy plugin list` only when the
local profile is already initialized. `/skills`, `/agents`, `/mcp`, `/hooks`,
and authenticated `agy -p` diagnostics are live-only.
