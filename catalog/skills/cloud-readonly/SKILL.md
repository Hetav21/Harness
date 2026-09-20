---
name: cloud-readonly
description: Use when an agent must inspect approved cloud resources without changing cloud state.
---

# Cloud Read-Only Inspection

## Overview/Core Principle
Use this skill for inventory, status, configuration, health, logs, metrics, and dependency inspection of approved cloud resources. Inspect only the explicitly approved scope. Every command must be demonstrably read-only, scoped, bounded, and redacted; stop rather than guess when anything is unclear.

## When to Use
Use for approved cloud inspection and evidence collection without changing cloud state.

## When Not to Use
Do not use for deployment, remediation, provisioning, deletion, tagging, access or cost changes, incident mutation, or discovery outside approval. Use the relevant change-management or troubleshooting skill instead.

## Requirements

Bootstrap resolves every uppercase-braced token below from approved project inputs before activation; unresolved tokens remain blocked.
Obtain all of these before activation. Activation-time configuration: `{{CLOUD_READONLY_SKILL_NAME}}`.
- Profiles, accounts/subscriptions/projects, regions, services, and resource patterns: {{APPROVED_PROFILES}}, {{APPROVED_ACCOUNTS}}, {{APPROVED_REGIONS}}, {{APPROVED_SERVICES}}, {{APPROVED_RESOURCE_PATTERNS}}
- Scope authority source and trusted authority registry: {{SCOPE_AUTHORITY_SOURCE}}, {{TRUSTED_SCOPE_AUTHORITY_REGISTRY}}
- Verification method, maximum registry age, and authority provenance/date: {{AUTHORITY_VERIFICATION_METHOD}}, {{AUTHORITY_REGISTRY_MAX_AGE}}, {{AUTHORITY_PROVENANCE}}, {{AUTHORITY_DATE}}
- Approved read-only command families and exact identity command: {{APPROVED_READ_ONLY_COMMAND_FAMILIES}}, {{EXACT_IDENTITY_COMMAND}}
- Expected identity: {{EXPECTED_IDENTITY}}
- Time, result, page, and output limits: {{TIME_LIMIT}}, {{RESULT_LIMIT}}, {{PAGE_LIMIT}}, {{OUTPUT_LIMIT}}
- Continuation-token policy, redaction policy, authentication owner, and validation owner: {{CONTINUATION_TOKEN_POLICY}}, {{REDACTION_POLICY}}, {{AUTHENTICATION_OWNER}}, {{VALIDATION_OWNER}}
- Related skills (optional): {{RELATED_SKILLS}}. If unused, remove this input and the entire Related Skills block before activation.

Unresolved variables block activation and use.

## Allowed Scope
Accept {{SCOPE_AUTHORITY_SOURCE}} only if it appears in {{TRUSTED_SCOPE_AUTHORITY_REGISTRY}} and passes {{AUTHORITY_VERIFICATION_METHOD}}. The registry must identify approved owners/roles and its last-reviewed date; older than {{AUTHORITY_REGISTRY_MAX_AGE}} is stale and BLOCKED. Missing, stale, conflicting, or unresolved authority, freshness, provenance, date, method, or scope is BLOCKED. Record the authority provenance/date and exact approved scope. Never infer, expand, enumerate, or substitute profiles, identities, accounts, regions, services, or resource patterns.

## Identity and Authentication Preflight
The human authentication owner must authenticate. Never inspect cached credentials/files or perform login, MFA, SSO, role selection, token refresh, or credential changes. After human confirmation, run exactly {{EXACT_IDENTITY_COMMAND}} and verify an exact match to {{EXPECTED_IDENTITY}}, approved account, and region. Revalidate after interruption, context change, or auth error; stop on mismatch or uncertainty.

## Read-Only Command Classification
Limit commands to {{APPROVED_READ_ONLY_COMMAND_FAMILIES}} (for example, approved describe, get, list, inspect, read-only logs, metrics, or status operations). Before each execution classify: <tool>, <subcommand>, <arguments>, <profile/account>, <region>, <service>, <resource-target>, <expected-size>, and why it cannot mutate state. Unknown, ambiguous, shell-expanded, or broad-scope commands are denied.

## Denied Actions
Deny create, update, modify, patch, set, put, apply, deploy, run, execute, invoke, start, stop, restart, reboot, terminate, delete, destroy, detach, attach, associate, disassociate, import, export, tag, untag, grant, revoke, rotate, login, logout, configure, assume-role, and token operations. Never tag resources, print secrets, reveal credentials, bypass approval, use arbitrary profiles, or broaden scope.

## Inspection Workflow
1. Confirm inputs, registry freshness, human authentication, exact identity, and scope.
2. Classify and approve each command against all limits; execute the smallest read-only query.
3. Record <exact-command>, <identity>, <approved-scope>, <timestamp>, and <bounded-redacted-result>.

## Pagination and Output Bounds
Use the provider's read-only pagination mechanism explicitly. Count pages/results and stop at {{PAGE_LIMIT}}, {{RESULT_LIMIT}}, {{TIME_LIMIT}}, or {{OUTPUT_LIMIT}}, whichever comes first; never silently fetch all pages. Apply {{CONTINUATION_TOKEN_POLICY}}. Treat continuation tokens as secret by default: never display or store one unless explicit policy permits it; describe continuation without exposing the token.

## Redaction
Apply {{REDACTION_POLICY}} before display or storage. Redact tokens, keys, passwords, cookies, private data, sensitive identifiers, secret-bearing fields, and unrelated resources.

## Stop Conditions
Stop on identity mismatch, auth prompt/error, ambiguity, denied command, mutation risk, secret exposure, limit breach, rate-limit escalation, unexpected target, or need for human judgment. Resume authentication-dependent work only after the owner confirms and identity is revalidated.

## Verification
Verify every executed command was approved, read-only, in scope, bounded, and redacted. {{VALIDATION_OWNER}} owns validation of evidence and scope.

## Final Report
Report <approved-scope>, <exact-identity>, <exact-commands>, <timestamp>, <limits>, <pagination-or-truncation>, <redaction-status>, <bounded-findings>, <unresolved-risks>, and <validation-status>. Never include secrets.

## Related Skills
{{RELATED_SKILLS}}
If unused, remove this entire block before activation; the activated skill must contain no unresolved placeholder.

## Maintenance Triggers
Update this template when provider command behavior, identity checks, authentication policy, approved boundaries, redaction policy, limits, validation ownership, or freshness policy changes.

## Common Mistakes
Using an arbitrary profile; assuming the current account or region; accepting a partial identity match; treating list as permission to broaden scope; fetching unbounded pages; printing raw output; or continuing after an auth prompt or mismatch.

## Self-Review and Return
Confirm activation variables use the uppercase-braced form, runtime request/output examples use angle-bracket fields, all required boundaries and owners are present, denied actions are explicit, and the workflow is executable without mutation. After activation, verify the frontmatter `name` exactly matches the containing skill folder name. Return `DONE`.
