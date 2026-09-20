---
name: deployment-logs
description: Use when an agent must diagnose approved deployed applications through bounded, read-only inspection.
---

# Deployment Resource and Log Diagnosis

## Overview/Core Principle
Diagnose only an approved deployed application, using bounded, read-only evidence. Scope, identity, commands, and limits are inputs—not assumptions. Missing, stale, or conflicting inputs block activation and use.

## When to Use
Use for approved diagnosis of resource health, events, and workload logs within the trusted target scope.

## When Not to Use
Do not use for ambiguous targets, unbounded log collection, secret exposure, authentication, remediation, restart, rollout, scale, deletion, or any mutation. Escalate changes to {{AUTH_OWNER}} or the designated operator.

## Requirements

Bootstrap resolves every uppercase-braced token below from approved project inputs before activation; unresolved tokens remain blocked.
Activation-time configuration: {{DEPLOYMENT_LOGS_SKILL_NAME}}. Require {{ALLOWED_SERVERS}}, {{ALLOWED_CLUSTERS}}, {{ALLOWED_CONTEXTS}}, {{ALLOWED_NAMESPACES}}, {{ALLOWED_APPS}}, and {{ALLOWED_WORKLOADS}}; trusted target registry {{TRUSTED_TARGET_REGISTRY}} with owner {{TARGET_OWNER}}, last-reviewed timestamp {{TARGET_LAST_REVIEWED}}, max age {{TARGET_MAX_AGE}}, and verification method {{TARGET_VERIFICATION_METHOD}}; exact identity command {{IDENTITY_COMMAND}} and expected value {{EXPECTED_IDENTITY}}; exact context command {{CONTEXT_COMMAND}} and expected value {{EXPECTED_CONTEXT}}; approved templates {{HEALTH_COMMAND_TEMPLATE}}, {{EVENT_COMMAND_TEMPLATE}}, and {{LOG_COMMAND_TEMPLATE}}; limits {{TIME_WINDOW}}, {{LINE_LIMIT}}, {{BYTE_LIMIT}}, and {{DURATION_LIMIT}}; redaction policy {{REDACTION_POLICY}}; auth owner {{AUTH_OWNER}}; validation owner {{VALIDATION_OWNER}}. {{RELATED_SKILLS}} is optional; if unused, remove its entire Related Skills block before activation so no unresolved placeholder remains.

## Trusted Target Scope
Use only registry entries reviewed within {{TARGET_MAX_AGE}} and matching every configured allowlist. Verify the exact requested target (<requested-target>) before every command. Record registry ownership, review time, and verification method. Scope cannot be inferred from a name, default context, or prior run.

## Identity and Context Preflight
Run {{IDENTITY_COMMAND}} and compare its output (<identity-output>) exactly with {{EXPECTED_IDENTITY}}. Run {{CONTEXT_COMMAND}} and compare its output (<context-output>) exactly with {{EXPECTED_CONTEXT}}. Confirm <requested-target> against {{TRUSTED_TARGET_REGISTRY}} and every allowlist. Stop on mismatch, unavailable output, expired review, or conflicting scope. If target, identity, context, or scope preflight fails, execute zero health, event, or log commands. Preserve only partial, non-secret preflight evidence.

## Read-Only Command Boundary
Commands must be classified read-only, use an approved exact template, and include the verified target and bounds. Forbid sync, apply, restart, rollout, scale, delete, patch, exec, port-forward, remediation, and every other mutation. Never use unbounded `follow`.

## Diagnosis Workflow
1. Validate all required inputs and freshness.
2. Perform identity, context, and target preflight.
3. Run bounded health and event checks.
4. Retrieve bounded logs only when approved and necessary.
5. Redact, record, and report evidence and gaps.

## Resource Health and Events
Use only {{HEALTH_COMMAND_TEMPLATE}} and {{EVENT_COMMAND_TEMPLATE}}. Respect {{DURATION_LIMIT}}, {{LINE_LIMIT}}, and {{BYTE_LIMIT}}. Record <timestamp>, exact target, command, <result>, truncation, and missing evidence. Do not broaden selectors or namespaces.

## Bounded Log Retrieval
Use only {{LOG_COMMAND_TEMPLATE}} with {{TIME_WINDOW}}, {{LINE_LIMIT}}, {{BYTE_LIMIT}}, and {{DURATION_LIMIT}}. Prefer the smallest useful time window and workload. Mark <log-output> truncated when any limit is reached; do not retry with larger or unbounded limits without new approval.

## Redaction
Apply {{REDACTION_POLICY}} before storing or reporting evidence. Never expose credentials, tokens, cookies, private keys, secret values, or sensitive payloads. Stop and escalate if redaction cannot be reliable.

## Authentication Handoff
{{AUTH_OWNER}} performs login, MFA, SSO, role selection, and token refresh. The agent never inspects credentials, credential stores, or authentication caches. Stop and request handoff when authentication is required.

## Stop Conditions
Stop on missing, stale, or conflicting scope; identity/context mismatch; unapproved command; limit failure; suspected secret exposure; authentication need; target ambiguity; or any request for mutation, restart, scale, exec, or port-forward. A target, identity, context, or scope preflight failure means zero health/event/log commands and requires a `BLOCKED` report.

## BLOCKED Result Contract
Return `BLOCKED` with: <blocking-reason>; failed preflight field; <requested-target>; mutation or authentication request, or `none`; available evidence limited to non-secret preflight evidence; required human or owner action; and the explicit count and list of operational commands executed. For target, identity, context, or scope preflight failure, the operational command count is `0` and the list is `none`; health, event, and log commands were not executed. Do not imply diagnosis from partial evidence.

## Verification
{{VALIDATION_OWNER}} validates this template and confirms every variable is resolved, every command is approved read-only, target scope is exact, limits are enforced, and redaction works. Unresolved variables block activation and use.

## Final Report
Report <timestamp>, verified identity/context, exact target, explicit operational command count and list, bounded and redacted evidence, truncation status, limits used, gaps, and stop conditions. If blocked, use the `BLOCKED` Result Contract and state that zero health/event/log commands were executed after target, identity, context, or scope preflight failure. Distinguish observed facts from hypotheses; do not recommend or perform remediation.

## Related Skills
Use {{RELATED_SKILLS}} only when separately approved and compatible with this scope. If unused, remove this entire block before activation. Related skills do not override these boundaries.

## Maintenance Triggers
Re-review when registry ownership, targets, contexts, commands, limits, redaction, authentication ownership, or platform behavior changes; when {{TARGET_MAX_AGE}} expires; or after any incident or policy change.

## Common Mistakes
Do not trust defaults, stale registry entries, copied commands, broad selectors, unlimited output, `follow`, unredacted logs, hidden credentials, or inferred authorization. Do not turn diagnosis into remediation.

## Self-Review and Return
Before returning, verify all required sections exist, activation variables use only uppercase braced placeholders, runtime request/output examples use descriptive angle-bracket field names, scope and commands are explicit, limits and redaction are stated, forbidden actions are listed, and unresolved variables block use. After activation, verify the frontmatter `name` exactly matches the containing skill folder name. Return `DONE`.
