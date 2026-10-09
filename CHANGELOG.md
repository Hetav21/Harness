# Changelog

All notable changes to this project are documented here.

## 0.3.0 - 2026-10-09

- Added manifest schema version 3 with mandatory source-to-output mapping (`source-modules`) on generated outputs; retained backward compatibility for reading legacy version 1 and version 2 manifests.
- Corrected OpenAI Codex MCP settings and role registration/configuration layers to conform to the native schema, advancing the Codex adapter contract to version 2.
- Required shared filesystem path checks before bootstrap and upgrade writes to reject duplicate destinations, path traversal (`..`), and symlink escapes, including manifest and project-context paths.
- Preserved explicit denied scope and capability requirements across all role templates (Claude Code, Codex, OpenCode, and Antigravity) in accordance with the shared role contract.
- Added required YAML frontmatter (`name` and `description`) to the Google Antigravity role template.
- Allowed verified workspace routes to proceed with `SKILL: none` when a repository is known but lacks an applicable specialized skill, and aligned repository table references with the context worksheet.
- Added a contiguous upgrade migration (`0.2.0-to-0.3.0.md`) covering schema v3 attribution, path checks, Codex adapter v2, and rollback procedures.

## 0.2.0 - 2026-10-09

- Added a manifest-based update handoff to generated `AGENTS.md` so the starter checkout can be removed without losing upgrade discovery.
- Added manifest schema version 2 with a required source commit and verified clone-URL guidance; the validator retains support for version 1 manifests.
- Added source retrieval and identity checks to the upgrade flow, plus a migration preserving existing workspace instructions and ownership.
- Required verification of retained provenance and instruction paths before starter removal; no separate update guide is generated.

## 0.1.1 - 2026-10-09

- Strengthened shared engineering guidance for readability, minimal diffs, necessary complexity, proper fixes, and explained workarounds.
- Added guidance for purposeful comments, repository conventions, language/framework style guides, and practical end-to-end validation with explicit skip reporting.
- Clarified that selected portable instructions are embedded in generated workspace guidance so they survive removal of the starter checkout.
- Added a distribution release checklist and a migration for existing engineering instructions; schema and adapter contracts are unchanged.

## 0.1.0 - 2026-09-14

- Introduced a portable, client-neutral core for agent workspaces.
- Added adapters for Claude Code, Codex, OpenCode, and Antigravity.
- Added catalog skills, bootstrap and upgrade playbooks, manifest ownership policies, and agent-facing validation guidance.
