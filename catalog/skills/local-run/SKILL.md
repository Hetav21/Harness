---
name: local-run
description: Use when an agent must start or stop a prepared local component and confirm its health signals.
---

# {{COMPONENT_NAME}} Local Run

## Overview/Core Principle

Run only a prepared local component with documented, deterministic commands. Do not hide setup, alter configuration, handle credentials, infer endpoint safety, or activate with unresolved required variables. Activation resolves inputs and readiness; runtime execution uses only the verified values and commands.

## When to Use

Use this skill to start or stop {{COMPONENT_NAME}} after readiness is verified and to collect bounded runtime evidence.

## When Not to Use

Do not install dependencies, change branches, set up a project, modify configuration, recover credentials, refresh tokens, select an endpoint, or silently retry unsafe actions.

## Requirements

Bootstrap resolves every uppercase-braced token below from project-owned setup evidence before activation; unresolved tokens remain blocked.

Resolve these activation-time variables before use:

- Frontmatter/title identity (activation-time configuration): `{{LOCAL_RUN_SKILL_NAME}}` and `{{COMPONENT_NAME}}`
- `{{EXACT_WORKING_DIRECTORY}}`, `{{ENVIRONMENT_SOURCE}}` (source or mechanism only; never values), and `{{RUN_COMMAND}}`
- `{{EXPECTED_URL_OR_HEALTH_SIGNAL}}`, `{{EXPECTED_RUNTIME_ARTIFACTS}}`, and `{{LOG_EVIDENCE_BOUND}}`
- `{{STOP_COMMAND}}`, `{{VALIDATION_OWNER}}`, and `{{AUTHENTICATION_OWNER}}`
- `{{READINESS_EVIDENCE}}` and `{{READINESS_EVIDENCE_MAX_AGE}}`
- Optional: `{{RELATED_SETUP_SKILL}}`, `{{RELATED_AUTH_SKILL}}`, `{{RELATED_CONFIG_SKILL}}`, and `{{RELATED_SKILL_PATHS}}`

## Readiness Checks

Confirm the exact working directory exists, the documented environment source is available without printing secrets, dependencies are already prepared, and checked-in configuration targets an explicitly verified local endpoint. `{{READINESS_EVIDENCE}}` must record the approved check or source, observed result, timestamp, and owner for every dependency-prepared and endpoint-safety claim. Evidence older than `{{READINESS_EVIDENCE_MAX_AGE}}` is stale and blocks execution until refreshed by an approved check; missing or unresolved freshness policy also blocks execution.

## Environment and Working Directory

Use exactly `{{EXACT_WORKING_DIRECTORY}}`. Source `{{ENVIRONMENT_SOURCE}}` without exposing values. Do not edit configuration or infer endpoint safety.

## Run Workflow

1. Complete activation and Readiness Checks.
2. Run exactly `{{RUN_COMMAND}}` from the exact working directory.
3. Record the process/PID or project-equivalent handle as `<process-handle>`.
4. Apply the Expected Runtime Artifacts checks.

## Authentication Handoff

Never read, refresh, create, copy, or expose tokens or credentials. If authentication is required or expired, stop and hand off directly to `{{AUTHENTICATION_OWNER}}`. Use `{{RELATED_AUTH_SKILL}}` only when configured, its path exists, and its frontmatter name matches; otherwise do not route. Report `BLOCKED` and identify the owner handoff without secret values.

## Health and Log Evidence

Process existence alone is not health. Use only the documented URL or health signal. Collect no more than `{{LOG_EVIDENCE_BOUND}}` of relevant startup, health, and failure logs, and redact sensitive values.

## Expected Runtime Artifacts

Confirm exactly `{{EXPECTED_RUNTIME_ARTIFACTS}}`, such as a documented handle, URL response, or health record. Do not invent artifacts or accept substitutes.

## Stop Workflow

Run exactly `{{STOP_COMMAND}}` from `{{EXACT_WORKING_DIRECTORY}}` or using the recorded handle. Verify the process or handle stopped and the documented health signal is no longer active. Report `BLOCKED` if either state cannot be verified.

## Blocked/Failure Reporting

Report `BLOCKED` when readiness, authentication, endpoint safety, runtime health, or stop verification fails. Include the failed check, bounded evidence, any exact verified handoff skill, and the next owner action. Never silently invoke setup or retry unsafe actions.

## Verification

The validation owner is `{{VALIDATION_OWNER}}`; verify readiness, health, expected artifacts, bounded evidence, and stop state. Before activation, every required variable must be resolved and optional entries must be fully verified or removed with every instruction referring to them. The activated skill must contain no literal unresolved template variable.

## Related Skills

Optional setup, authentication, and configuration skills are non-blocking until a readiness handoff is needed. Route only to a configured skill whose path exists and whose frontmatter `name` matches its corresponding variable. For handoffs, verify path and frontmatter name. If none is configured and verified, report `BLOCKED` with the required human or owner action; do not route.

## Maintenance Triggers

Update this template when commands, directory, environment source, endpoint, signal, artifacts, stop command, evidence bound, ownership, or freshness policy changes.

## Common Mistakes

Do not expose environment values or secrets, infer endpoint safety, treat process existence as health, invent runtime artifacts, accept substitutes, invoke hidden setup, or use unverified handoffs.

## Self-Review and Return

Before activation, confirm readiness evidence covers each dependency and endpoint claim with check/source, result, timestamp, and owner; stale or missing freshness policy blocks activation. Confirm documented commands and signals are used, no secret handling is present, and optional entries are removed with their instructions when not verified. After activation, verify the frontmatter `name` exactly matches the containing skill folder name. Return `DONE`.
