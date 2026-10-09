# Changelog

All notable changes to this project are documented here.

## Unreleased

- Added manifest schema version 3 with source-module paths on generated outputs; legacy versions 1 and 2 remain readable, with explicit attribution required before conversion.
- Fixed the Antigravity role template to include its required name and description frontmatter.

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
