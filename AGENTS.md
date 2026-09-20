# Agent Workspace Template Instructions

## Purpose
Operate this distribution and bootstrap a project-specific multi-repository workspace.

## Bootstrap entrypoint
Read `playbooks/bootstrap.md`, perform its preflight and interview, present the complete materialization plan, and wait for approval before editing generated workspace files.

## Upgrade entrypoint
When the user requests an update, read `playbooks/upgrade.md`, the target release migrations, and the workspace manifest before proposing changes.

## Invariants
- Treat `core/` and `catalog/` as portable source material.
- Treat `adapters/` as client-specific translation layers.
- Never invent project facts or credentials.
- Preserve unmanaged files and local modifications.
- Report skipped validation separately from passing validation.
