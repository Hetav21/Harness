---
name: workspace-router
description: Use when an agent must identify the repository or capability that owns a workspace-level request.
---

# {{PROJECT_NAME}} Workspace Router

## Overview / Core Principle

Route from verified evidence, never from names or intuition. Route before diagnosis, editing, or execution. This skill contains operating rules, not project facts: the authoritative project-context worksheet and its tables are the single source of truth.

## When to Use

Use when a task must be mapped to the correct runnable repository, component, contract, owner, or narrower project skill.

## When Not to Use

Do not use for diagnosis, implementation, execution, or project facts that are not verified in the authoritative context. Do not route from repository or component names alone.

## Requirements

Bootstrap resolves every uppercase-braced token below from the authoritative project context before activation; unresolved tokens remain blocked.

Activation must supply and verify:

- Frontmatter/title identity (activation-time configuration): `{{WORKSPACE_ROUTER_SKILL_NAME}}` and `{{PROJECT_NAME}}`
- Authoritative project-context path: `{{PROJECT_CONTEXT_PATH}}`
- Freshness policy: `{{FRESHNESS_POLICY}}`
- Authority policy: `{{AUTHORITY_POLICY}}`
- Conflict policy and escalation owner: `{{CONFLICT_POLICY}}` / `{{CONTEXT_CONFLICT_OWNER}}`
- Validation owner: `{{VALIDATION_OWNER}}`

Unresolved activation configuration is `BLOCKED/UNKNOWN` and prevents use. Runtime input is not activation configuration.

## Runtime Input and Outputs

Required runtime input is the task: `<task>`.

Repository, component, contract, and owner are routing outputs when absent from `<task>`, not prerequisites. Resolve them from the authoritative context worksheet and its tables.

Use this concise route-result format:

```text
ROUTE: <runnable-repository> / <component>
CONTRACT: <contract-or-UNKNOWN>
OWNER: <owner-or-UNKNOWN>
DEPENDENCIES: <dependencies-or-UNKNOWN>
ENTRY_POINT: <entry-point-or-UNKNOWN>
EVIDENCE: <verified-context-path-and-table-or-UNKNOWN>
SKILL: <verified-narrower-skill-or-UNKNOWN>
STATUS: <READY|BLOCKED|UNKNOWN>
```

`BLOCKED/UNKNOWN` means routing cannot safely proceed. Explain the missing, stale, conflicting, or unverifiable evidence in one brief line. Never guess.

## Ownership Map

Read authoritative `Product and Ownership`, `Repository Inventory`, and `Runnable and Context-Only Code` in `{{PROJECT_CONTEXT_PATH}}`; PROJECT-CONTEXT remains the source and must not be copied here. Verify cited paths/evidence, freshness, and authority. Outputs: `OWNER`, `COMPONENT`.

## Shared Dependencies

Read authoritative `Shared Dependencies` in `{{PROJECT_CONTEXT_PATH}}`; PROJECT-CONTEXT remains the source and must not be copied here. Verify cited paths/evidence, freshness, and authority. Output `DEPENDENCIES` or `UNKNOWN`.

## Routing Table

Read authoritative `Skill Routing Plan` in `{{PROJECT_CONTEXT_PATH}}`; PROJECT-CONTEXT remains the source and must not be copied here. Verify cited paths/evidence, freshness, and authority. Output `ROUTE`, `CONTRACT`, and `STATUS` when supported.

## Practical Entry Points

Read authoritative `Runnable and Context-Only Code` and `Local Setup and Run Commands` in `{{PROJECT_CONTEXT_PATH}}`; PROJECT-CONTEXT remains the source and must not be copied here. Verify cited paths/evidence, freshness, and authority. Output `ENTRY_POINT` or `UNKNOWN`.

## Routing Workflow

1. Read `<task>` and identify the requested behavior and possible contract boundary without deciding ownership from names.
2. Open `{{PROJECT_CONTEXT_PATH}}`. Verify that it exists and identifies authoritative ownership, dependency, routing, repository, and skill evidence.
3. Apply `{{FRESHNESS_POLICY}}`, `{{AUTHORITY_POLICY}}`, and `{{CONFLICT_POLICY}}` to the cited evidence. Verify every referenced path and the evidence supporting the route. Escalate conflicts to `{{CONTEXT_CONFLICT_OWNER}}`.
4. Use the worksheet tables to produce repository, component, contract, owner, `DEPENDENCIES`, and `ENTRY_POINT` outputs. Mark absent or conflicting results `UNKNOWN` or `BLOCKED`; do not diagnose before routing.
5. Distinguish runnable from context-only repositories; context-only repositories may provide evidence but are never execution targets.
6. Verify the selected narrower skill exists and is applicable; load it only after routing is `READY`.
7. Return the route-result format above. Keep evidence references concise.

## Stop Conditions

Stop with `STATUS: BLOCKED` or `STATUS: UNKNOWN` when the context path is missing, a cited path does not exist, evidence is stale or lacks authority, policy is unresolved, authorized evidence conflicts, the result is context-only, the contract remains unclear, or the narrower skill cannot be verified. Ask for evidence or follow the configured conflict policy; never invent paths, owners, contracts, branches, endpoints, or skills.

## Verification

Verify the context path, cited evidence paths, freshness, authority, and conflict policy before `READY`. Verify the repository is runnable, contract, owner, dependencies, and entry point are evidenced, and the narrower skill applies.

## Related Skills

Only the verified narrower skill selected by the authoritative context is related to this route. Do not invent or list project skills here; load the selected skill only after routing is `READY`.

## Maintenance Triggers

Update this template only when routing behavior or activation configuration changes. Keep project facts in the authoritative project context; never re-add ownership, dependency, or routing tables here. Recheck the template when placeholder names, policies, or the required route-result contract change.

## Common Mistakes

- Treating repository, component, contract, or owner as required runtime input.
- Routing from names, intuition, stale evidence, or unverifiable paths.
- Treating context-only repositories as execution targets, diagnosing before routing, or loading skills before verification.
- Guessing missing owners, contracts, branches, endpoints, paths, or skills.

## Self-Review and Return

Before return, confirm the frontmatter `name` matches the containing skill folder, the description remains trigger-only, activation placeholders are resolved, evidence is verified, runnable/context-only status is explicit, and narrower-skill verification follows routing. Return only the concise route-result format, with a brief reason for `BLOCKED` or `UNKNOWN`.
