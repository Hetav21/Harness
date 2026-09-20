---
name: repository-status-audit
description: Use when an agent must check repository branch, worktree, dependency, and setup state without changing it.
---

# Repository Status Audit

## Overview / Core Principle

Produce a deterministic, read-only status report. Inspect only named repositories and checks; separate evidence from remediation advice. Never make the repository conform during an audit.

## When to Use

Use when <audit-request> requires a snapshot of branch, worktree, remotes, dependencies, local files, or setup readiness for {{ALLOWED_REPOSITORY_SCOPE}}.

## When Not to Use

Do not use for fixing, synchronizing, installing, cleaning, switching branches, or deciding whether an unapproved mutation is safe. Use the remediation or setup skill instead.

## Requirements

Bootstrap resolves every uppercase-braced token below from project-owned policy evidence before activation; unresolved tokens remain blocked.

Require frontmatter identity (activation-time configuration) `{{STATUS_AUDIT_SKILL_NAME}}`, {{ALLOWED_REPOSITORY_SCOPE}}, {{EXPECTED_BRANCH_POLICY}}, {{EXPECTED_WORKTREE_POLICY}}, {{APPROVED_CHECK_COMMANDS}}, {{APPROVED_DEPENDENCY_COMMANDS}}, {{APPROVED_IDENTITY_PREFLIGHT_COMMANDS}}, {{APPROVED_NETWORK_QUERY_POLICY}}, {{REQUIRED_LOCAL_FILES}}, {{SETUP_READINESS_CRITERIA}}, {{REPORT_DESTINATION}}, {{REPORT_FORMAT}}, {{AUTHENTICATION_OWNER}}, {{APPROVED_AUTHENTICATION_ACTION}}, and {{VALIDATION_OWNER}}. {{APPROVED_CHECK_COMMANDS}} must contain only general repository/status checks, excluding dependency or setup inspection. {{APPROVED_DEPENDENCY_COMMANDS}} must contain only explicitly approved, non-mutating dependency metadata/state checks. Define the allowed scope and expected state before inspection. Unresolved variables block use.

## Read-Only Boundary

Allowed: path existence checks, local metadata inspection, status queries, and approved non-mutating dependency/setup commands. Forbidden: checkout, switch, pull, fetch, install, edit, stash, reset, clean, delete, credential inspection, and authentication. Pull and fetch are unconditionally forbidden. For remote truth, use only an exact approved network query documented with approval owner, command, scope, and last-reviewed date; otherwise classify BLOCKED.

## Audit Workflow

1. Resolve every variable and confirm the exact allowed repository scope; stop if any is unresolved.
2. Verify each path exists and record the command, timestamp, and bounded output used as evidence.
3. Run only {{APPROVED_CHECK_COMMANDS}} for general repository/status checks and only {{APPROVED_DEPENDENCY_COMMANDS}} for dependency metadata/state checks, without changing directories’ contents or configuration.
4. Compare observations with the declared expected state.
5. Classify every check and report remediation recommendations in a separate section; do not perform them.

## Authentication Handoff

Authentication is human-controlled. Never inspect credentials or authenticate. If an approved check requires authentication, tell {{AUTHENTICATION_OWNER}} exactly which approved action is required: {{APPROVED_AUTHENTICATION_ACTION}}. Stop and preserve the partial report. Resume only after {{AUTHENTICATION_OWNER}} confirms that action is complete; then rerun only the configured approved non-secret identity/preflight checks from {{APPROVED_IDENTITY_PREFLIGHT_COMMANDS}} before continuing. If the owner, approved action, or identity/preflight checks are unresolved, classify the affected check BLOCKED and stop.

## Repository Checks

For each repository, use only {{APPROVED_CHECK_COMMANDS}} for existence, branch, worktree, and local remote/tracking metadata. These general repository/status checks must not inspect dependencies or setup. Record changed/untracked paths, remote/tracking details, expected value, command, and evidence. Pull and fetch are never allowed. If local metadata cannot establish remote truth, an approved network query is allowed only when {{APPROVED_NETWORK_QUERY_POLICY}} documents approval owner, command, scope, and last-reviewed date; otherwise classify BLOCKED.

## Dependency / Setup Checks

Inspect dependency metadata and state using only {{APPROVED_DEPENDENCY_COMMANDS}} (for example, project-approved lockfile or package-manager verification commands that are explicitly non-mutating). Check required local files and setup readiness against {{SETUP_READINESS_CRITERIA}}. Never install, generate, edit, or repair files. Distinguish missing metadata from an unavailable command.

## Result Taxonomy

Classify every check exactly once:

- **PASS** — evidence matches the expected state.
- **WARNING** — evidence is usable but incomplete, stale, unexpected, or non-blocking.
- **BLOCKED** — the check cannot be safely or deterministically completed, including unresolved variables, missing approved commands, or forbidden authentication.
- **NOT_APPLICABLE** — the check is explicitly outside this repository’s declared scope, with a reason.

Every result must include evidence and a separate remediation recommendation. Recommendations are not actions.

## Report Format

Use {{REPORT_FORMAT}} with one row per check:

| Repository | Check | Expected | Observed | Classification | Evidence | Remediation recommendation |
|---|---|---|---|---|---|---|
| <repository-path> | <check-name> | <expected-value> | <observed-value> | <classification> | <evidence> | <remediation-recommendation> |

Include the allowed scope, timestamp, commands, classification totals, blocked checks, and separate remediation. Do not report assumptions as facts.

## Stop Conditions

Stop and return BLOCKED when scope, expected-state rules, an approved command, a required path, a variable, or network-query approval record is unresolved; a check would require pull, fetch, any other mutation, an undocumented network query, credentials, or authentication; or evidence is contradictory. Preserve the partial read-only report.

## Verification

{{VALIDATION_OWNER}} validates scope, command safety, check coverage, taxonomy, evidence, and separation of recommendations from actions. No mutation or commit is verification.

## Related Skills

Optional: {{RELATED_STATUS_SKILLS}}. Use only verified related skills for remediation, setup, dependency management, or repository operations, and only after this audit is complete. If this optional input is unused, remove the Related Skills section, including its placeholder, before activation; no unresolved placeholder may remain.

## Maintenance Triggers

Update when repository scope, expected-state policy, approved commands, dependency tooling, local setup requirements, taxonomy, or related skill names change.

## Common Mistakes

- Auditing an unspecified repository or using an implicit scope.
- Treating a clean worktree as proof of correct branch or dependencies.
- Pulling or fetching to learn remote truth, even when project policy calls it safe.
- Using a network query without documented approval owner, exact command, scope, or last-reviewed date.
- Installing, editing, or authenticating during inspection.
- Mixing remediation commands into evidence collection.
- Omitting evidence, expected values, blocked reasons, or NOT_APPLICABLE justification.

## Self-Review and Return

Confirm the frontmatter; after activation, verify that its `name` exactly matches the containing skill folder; confirm variables, deterministic allowed scope, all required checks, read-only boundary, four result classes, evidence column, separate remediation section, stop conditions, and {{VALIDATION_OWNER}} ownership. Return `DONE`.
