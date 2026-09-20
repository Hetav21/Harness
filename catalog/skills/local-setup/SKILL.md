---
name: local-setup
description: Use when an agent must prepare a component for local development from a known repository state.
---

# {{COMPONENT_NAME}} Local Setup

## Overview/Core Principle

Prepare only the project identified by the repository owner. Make setup deterministic, observable, and reversible. Preserve unknown work: never overwrite, reset, clean, stash, or silently modify pre-existing files. Unresolved variables block execution.

## When to Use

Use for a fresh or known-state local-development setup of {{COMPONENT_NAME}} at {{REPOSITORY_PATH}}.

## When Not to Use

Do not use for deployment, production changes, dependency upgrades, branch repair, credential handling, or an unknown repository state. Escalate those tasks to the owner.

## Requirements

Bootstrap resolves every uppercase-braced token below from project-owned setup evidence before activation; unresolved tokens remain blocked.

{{SETUP_REQUEST_OWNER}} must provide and resolve all of these:

- Frontmatter/title identity (activation-time configuration): `{{LOCAL_SETUP_SKILL_NAME}}` and `{{COMPONENT_NAME}}`
- Setup request owner: {{SETUP_REQUEST_OWNER}}
- Exact repository path: {{REPOSITORY_PATH}}
- Branch/worktree policy: {{BRANCH_WORKTREE_POLICY}}
- Dependency command: {{DEPENDENCY_COMMAND}}
- Allowed tracked file diffs (activation-time): {{ALLOWED_TRACKED_FILE_DIFFS}}
- Allowed local-only diffs: {{ALLOWED_LOCAL_ONLY_DIFFS}}
- Ordered setup commands: {{ORDERED_SETUP_COMMANDS}}
- Command effect manifest: {{COMMAND_EFFECT_MANIFEST}} (for each dependency/setup command, expected tracked files, untracked/generated files, lockfiles, and cache locations)
- Expected generated files: {{EXPECTED_GENERATED_FILES}}
- Verification commands and signals: {{VERIFICATION_COMMANDS_AND_SIGNALS}}
- Validation owner: {{VALIDATION_OWNER}}
- Optional related skills: {{RELATED_SKILLS}} (remove this input and the Related Skills section before activation if unused)

## Preflight

1. Confirm {{REPOSITORY_PATH}} exists and is the intended repository.
2. Record current branch, worktree location, repository state, and existing changed files.
3. Confirm {{BRANCH_WORKTREE_POLICY}} before running any setup command.
4. Check that all required inputs are resolved. If any variable remains unresolved, stop.
5. Review {{ALLOWED_TRACKED_FILE_DIFFS}}, {{ALLOWED_LOCAL_ONLY_DIFFS}}, and {{COMMAND_EFFECT_MANIFEST}}; refuse any other mutation or undeclared effect.

## Allowed Changes

Only run {{DEPENDENCY_COMMAND}} and {{ORDERED_SETUP_COMMANDS}} as specified by the project owner. Tracked files and lockfiles may change only when explicitly listed in activation-time {{ALLOWED_TRACKED_FILE_DIFFS}} and covered by {{COMMAND_EFFECT_MANIFEST}} and its approval; otherwise they are forbidden and BLOCKED. Create or modify only declared generated or local-only files. Preserve all pre-existing changes, including untracked files. Record every command, result, and changed file.

## Setup Workflow

Run the ordered setup commands exactly in {{ORDERED_SETUP_COMMANDS}}. Before each command, document its forecast from {{COMMAND_EFFECT_MANIFEST}}, including expected tracked files, untracked/generated files, lockfiles, and cache locations, and confirm any tracked or lockfile paths are listed in {{ALLOWED_TRACKED_FILE_DIFFS}}. Run a dry-run when supported; every dry-run command is itself executable, must be covered by {{COMMAND_EFFECT_MANIFEST}}, recorded with its command and result, and subject to the same approval boundary. If a dry-run's non-mutating or declared effects cannot be verified, do not run it and remain BLOCKED. When no dry-run exists, obtain explicit approval of the forecast before execution. After each command, record exit status, relevant output, and resulting file changes. Any undeclared effect, unresolved manifest, failure, unexpected mutation, or command requiring unapproved input blocks progress. Do not improvise replacement commands.

## Authentication Handoff

Never read cached credentials, credential stores, tokens, cookies, or environment secrets. Never perform login, MFA, SSO, password entry, token refresh, or role selection. If authentication expires, tell the human to authenticate through the approved project workflow, then stop. Resume only after the human confirms completion and revalidate repository identity without inspecting secrets.

## Generated/Local-Only Files

Expected generated files are {{EXPECTED_GENERATED_FILES}}. Allowed local-only diffs are {{ALLOWED_LOCAL_ONLY_DIFFS}}. Verify both lists after setup. Any unlisted generated file or diff is unexpected: stop and report it; do not delete it.

## Verification

Run {{VERIFICATION_COMMANDS_AND_SIGNALS}} in the stated order. Record each command, exit status, and expected signal. Confirm the repository is usable for the requested local-development task and that no disallowed files changed, especially tracked files and lockfiles. The validation owner is {{VALIDATION_OWNER}}.

## Rollback Boundary

Rollback may remove or revert only artifacts created by this workflow. List those artifacts first, obtain owner approval where required, and record each rollback result. Never use reset, clean, or stash. Never revert pre-existing changes or unlisted files.

## Stop Conditions

Stop for unresolved variables or manifest, unknown branch/worktree state, pre-existing changes that cannot be preserved, authentication prompts, command failure, unexpected file changes or effects, missing expected signals, or any action outside the allowed lists. Report the exact condition and wait for the owner.

## Final Report

Report repository path, branch/worktree state, every command and result, every changed file, generated files, verification signals, authentication handoff status, unresolved risks, and whether setup is ready. Do not claim readiness when verification is incomplete.

## Related Skills

Use the project’s repository-status, architecture, authentication, and verification skills named by {{RELATED_SKILLS}}. Do not substitute them for this workflow’s required inputs. Remove this entire section before activation if {{RELATED_SKILLS}} is unused; no unresolved placeholder may remain.

## Maintenance Triggers

Update this template when setup commands, dependency tooling, branch/worktree policy, generated files, verification signals, authentication policy, or ownership changes.

## Common Mistakes

- Running setup before recording branch and existing diffs.
- Assuming a clean tree or deleting unknown files.
- Reading cached credentials or attempting SSO/token refresh.
- Reordering or improvising project-owned commands.
- Treating an exit code as sufficient without checking expected signals.
- Omitting command results, changed files, or rollback artifacts from the final report.

## Self-Review and Return

{{VALIDATION_OWNER}} self-review: section coverage, variable format, and safety boundaries were checked. After activation, verify the frontmatter `name` exactly matches the containing skill folder name. Return `DONE` when complete.
