---
name: git-remote-operations
description: Use when an agent must perform approved clone, fetch, pull, or remote-access checks.
---

# Git Remote Operations

## Overview/Core Principle

Perform only the smallest explicitly authorized remote operation. Verify policy, identity, repository, destination, branch, and worktree state before changing anything. Never guess credentials, URLs, keys, or recovery actions. Unresolved activation variables or request fields block use.

## When to Use

Use for approved clone, fetch, pull, or read-only remote-access checks against allowlisted repositories and remotes.

## When Not to Use

Do not inspect SSH or credential files, choose keys, expose identities beyond approved output, reset, clean, stash, or overwrite dirty work. This skill does not perform push, force push, or remote URL edits; those require a separately defined and triggered workflow. Stop for login, MFA, SSO, password, token refresh, or key changes; instruct a human to authenticate.

## Requirements

Bootstrap resolves every uppercase-braced token below from approved project inputs before activation; unresolved tokens remain blocked.

Activation-time configuration: `{{GIT_REMOTE_SKILL_NAME}}`. Require `{{ALLOWED_REPOSITORIES}}`, `{{ALLOWED_REMOTES}}`, `{{ALLOWED_URL_PATTERNS}}`, `{{APPROVED_IDENTITY_COMMAND}}`, `{{APPROVED_REMOTE_ACCESS_COMMAND_TEMPLATE}}`, `{{DESTINATION_POLICY}}`, `{{BRANCH_POLICY}}`, `{{PERMITTED_OPERATIONS}}`, `{{EXPECTED_TRACKING_REFS}}`, `{{CLONE_AUTHORIZATION}}`, `{{FETCH_AUTHORIZATION}}`, `{{PULL_AUTHORIZATION}}`, and `{{VALIDATION_OWNER}}`. Require `<repository-url>`, `<repository-path>`, `<destination-path>`, `<target-branch>`, `<operation>`, `<remote-name>`, `<remote-ref-scope>`, and `<authentication-owner>` for each request as applicable.

## Repository and Remote Allowlist

Proceed only when the requested repository exactly matches `{{ALLOWED_REPOSITORIES}}`, the remote exactly matches `{{ALLOWED_REMOTES}}`, and its URL matches `{{ALLOWED_URL_PATTERNS}}`. Do not infer aliases, repositories, hosts, protocols, branches, or destinations. Clone destinations must satisfy `{{DESTINATION_POLICY}}`; existing paths require explicit policy approval.

## Bounded Remote Access Check

Validate `<repository-url>` and `<remote-name>` against the allowlists. Run `{{APPROVED_IDENTITY_COMMAND}}` and use exactly `{{APPROVED_REMOTE_ACCESS_COMMAND_TEMPLATE}}`, populated only with the verified runtime `<repository-url>` and `<remote-name>`, as the bounded remote-access check. The identity command alone is not an access check. Do not fetch, update refs, change the worktree, or create a directory. Capture only bounded, redacted output and confirm that access to the requested remote is permitted. This workflow does not authorize clone, fetch, or pull.

## Non-Secret Identity Preflight

Run only `{{APPROVED_IDENTITY_COMMAND}}`. Capture its bounded, redacted result; do not run alternative identity commands or inspect credential material. Confirm the result is acceptable to the requesting `<authentication-owner>`. Stop if identity is unavailable, unexpected, or requires interactive authentication.

## Worktree Preflight

For an existing repository, record safe before-state: path, current branch, HEAD, upstream, staged changes, unstaged changes, and untracked files. A dirty or ambiguous worktree blocks pull and any operation covered by `{{BRANCH_POLICY}}`. Never reset, clean, stash, overwrite, or silently switch branches. Clone creation must not replace an existing directory.

## Operation Authorization Matrix

Treat each operation as distinct and authorize it separately:

| Operation | Required authorization | Allowed effect |
|---|---|---|
| Clone directory creation | `{{CLONE_AUTHORIZATION}}` | Create only an approved destination and checkout policy branch. |
| Fetch metadata updates | `{{FETCH_AUTHORIZATION}}` | Update only approved remotes/refs; no worktree changes. |
| Pull history/worktree changes | `{{PULL_AUTHORIZATION}}` | Only approved branch, clean state, and expected tracking ref. |

The requested `<operation>` must be in `{{PERMITTED_OPERATIONS}}` and separately authorized before execution.

## Clone Workflow

Validate `<repository-url>`, `<destination-path>`, `<target-branch>`, repository policy, and clone authorization. Run the minimum clone command with no credential or key selection. Verify the resulting remote, branch, HEAD, and expected tracking refs. Do not retry with guessed URLs or options.

## Fetch Workflow

Validate the existing `<repository-path>`, `<remote-name>`, `<remote-ref-scope>`, remote allowlist, clean/dirty policy, and fetch authorization. Record state, fetch only approved refs, then verify the remote and expected tracking refs. Fetch must not alter the worktree.

## Pull Workflow

Require `<repository-path>`, `<remote-name>`, `<target-branch>`, a clean worktree, an approved branch, expected upstream, explicit pull authorization, and a recorded before-state. Pull only the approved remote/ref. Record after-state and stop on conflicts or unexpected changes; do not auto-resolve, reset, clean, stash, or overwrite.

## Authentication Handoff

On any authentication prompt or failure requiring login, MFA, SSO, password, token, role selection, credential refresh, or key change, stop immediately. Tell `<authentication-owner>` to authenticate directly, without requesting secrets. Resume only after human confirmation, then rerun the non-secret preflight.

## Failure Handling

Fail closed on policy mismatch, unresolved variable, dirty or ambiguous state, unexpected URL/ref/identity, permission error, conflict, or nonzero command. Do not broaden scope or retry destructively. Report the exact command, bounded/redacted result, error category, and before/after state; preserve local work.

## Verification

Validate `<repository-path>`, the exact `<repository-url>` and `<remote-name>`, `<target-branch>`, expected tracking refs, and `<operation>`-specific effects. Confirm no unauthorized worktree, remote, or history changes. Validation is owned by `{{VALIDATION_OWNER}}`.

## Final Report

Return `<operation>`, repository, `<repository-url>`, `<repository-path>`, `<remote-name>`, `<destination-path>` when applicable, `<target-branch>` when applicable, authorization, `<authentication-owner>`, identity-preflight status, command(s), bounded/redacted results, before/after Git state, verification, and any handoff or unresolved failure. Never include secrets or unapproved identity details.

## Related Skills

Use optional `{{RELATED_SKILLS}}` only when explicitly applicable; if unused, remove this entire block before activation. Do not substitute a skill for authorization or policy validation.

## Maintenance Triggers

Review this template when repositories, hosts, URL patterns, identity command, destination/branch rules, permitted operations, tracking refs, authentication ownership, or validation ownership changes.

## Common Mistakes

Guessing a remote or key; inspecting SSH files; treating fetch as pull; assuming clone authorization permits pull; running against a dirty worktree; retrying with broader scope; or reporting unbounded command output.

## Self-Review and Return

Before returning, confirm every activation variable and request field is resolved, allowlists and authorization were checked, no prohibited action occurred, and before/after state is recorded. After activation, verify the frontmatter `name` exactly matches the containing skill folder name. Return `DONE`.
