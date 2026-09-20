---
name: architecture-map
description: Use when an agent must investigate ownership, boundaries, data flow, runtime wiring, or debugging entry points.
---

# {{COMPONENT_NAME}} Architecture Map

> **STATUS: <map-status>**
>
> **Summary:** <status-summary>

The generated map must begin with this status. Use `DRAFT — NOT DEFINITIVE` whenever any material runtime or deployment claim is UNKNOWN, stale, conflicting, unvalidated, or inaccessible; <status-summary> must name the reason or reasons. Use a definitive status only after every material runtime and deployment claim is directly validated for the asserted scope. Listing unknowns later does not make a map definitive.

## Scope

Map only {{COMPONENT_NAME}} and the explicitly asserted {{ASSERTED_SCOPE}}. Record facts as scoped claims; do not turn this template into a project inventory.

## When to Use

Use when ownership, repository boundaries, local wiring, deployed wiring, request/data flow, contracts, or debugging entry points need evidence-backed clarification.

## When Not to Use

Do not use for implementation instructions, live incident operations, or claims outside {{ASSERTED_SCOPE}}. Do not treat local files as proof of deployed behavior.

## Requirements

Bootstrap resolves every uppercase-braced token below from project-owned evidence before activation; unresolved tokens remain blocked.

Template identity: `{{ARCHITECTURE_SKILL_NAME}}`; component identity: `{{COMPONENT_NAME}}`. Activation-time configuration required before use: `{{VALIDATION_OWNER}}`; missing activation configuration blocks mapping. Missing runtime evidence remains UNKNOWN; continue mapping.

- **Scope:** {{ASSERTED_SCOPE}}, {{OWNERSHIP_SCOPE}}, {{FLOW_SCOPE}}.
- **Evidence:** <local-evidence-record-id>, <runtime-evidence-record-id>, <deployment-evidence-record-id>, <owner-validation-record-id>.
- **Freshness:** <last-verified-date> and {{EVIDENCE_MAX_AGE}}.
- **Authority:** <source-path-or-owner>, <provenance-type>, <claim-owner>, and <owner-validation>.
- **Validation and status:** <map-status>, <status-summary>, and <confidence>.

## Mapping Workflow

1. Confirm scope and component; reject work outside them.
2. Gather inputs and classify observations as code-backed, runtime-observed, documentation-backed, human-provided, convention, or unknown.
3. Separate local wiring from runtime/deployment evidence; never promote local evidence to runtime proof.
4. Trace evidenced request/data flows; record contracts and shared dependencies with boundaries and scopes.
5. Record missing, stale, conflicting, inaccessible, or scope-limited evidence as UNKNOWN, with caveats and evidence gaps; continue as `DRAFT — NOT DEFINITIVE`.
6. Derive debugging entry points only from registered evidence, including logs, traces, tests, or checks.
7. Set prominent status: use `DRAFT — NOT DEFINITIVE` for unresolved material runtime/deployment claims; use definitive status only after direct validation.
8. Confirm validation ownership for exact scope/provenance, then perform and report required checks.

## Stop Conditions

Stop only when scope, authorization, safe access, required activation configuration, or validation ownership cannot be established. Missing, stale, conflicting, or inaccessible architectural evidence normally does not stop mapping: mark affected claims UNKNOWN, record caveats and evidence gaps, and continue as `DRAFT — NOT DEFINITIVE`. Stop when requested work is outside scope or access would be unsafe or sensitive, including secrets, credentials, personal data, production mutation, or other protected resources. Never infer deployment, reachability, environment, cloud provider, ownership, or runtime behavior from filenames, imports, local configuration, naming, or convention; request safe owner-provided evidence instead.

## Evidence Standard

Every claim must include source path or owner, last-verified date, confidence, and asserted scope. Label provenance exactly as: code-backed, runtime-observed, documentation-backed, human-provided, convention, or unknown. Confidence is HIGH, MEDIUM, LOW, or UNKNOWN; UNKNOWN is mandatory for unresolved runtime evidence or claims, missing evidence, and conflicting or stale evidence. Missing required activation configuration remains a hard block. Owner validation must confirm scope and provenance; human confirmation never becomes code-backed. Never infer deployment from filenames, imports, or local configuration. Do not assume conventional cloud topology, services, regions, URLs, or ownership.

## Verification and Failure Reporting

When scope, authorization, safe access, required activation configuration, or validation owner cannot be established, return `BLOCKED` with reason, failed gate, safe evidence, and required owner action. When architectural evidence is missing, stale, conflicting, or inaccessible but safe scope exists, continue and return `DRAFT — NOT DEFINITIVE` with UNKNOWN claims, caveats, and evidence gaps.

## Verified Ownership

| Scope | Repository or System | Responsibility | Owner | Evidence Record | Confidence |
|---|---|---|---|---|---|
| <asserted-scope> | <repository-or-system> | <responsibility> | <claim-owner> | <evidence-record-id> | <confidence> |

Treat ownership as unknown until the owner validates the exact asserted scope.

## Local-Development Wiring

Describe only wiring proven in local code, scripts, manifests, or documented setup. Distinguish local-only behavior from runtime behavior. Record <local-entry-point>, <local-dependency>, <local-endpoint>, and <local-configuration> with evidence records; leave each unresolved item UNKNOWN.

## Runtime and Deployment Wiring

Record runtime-observed or authoritative deployment evidence separately from local evidence. Include <runtime-component>, <runtime-owner>, <runtime-endpoint>, <deployment-resource>, and <environment> only when directly verified. A filename, import, local config, or naming convention cannot establish deployment, reachability, environment, or cloud provider.

## Request/Data Flow

| Order | From | To | Data or Contract | Runtime/Local Scope | Evidence Record | Confidence |
|---|---|---|---|---|---|---|
| <flow-order> | <flow-source> | <flow-target> | <data-or-contract> | <runtime-or-local-scope> | <evidence-record-id> | <confidence> |

Do not fill gaps with a conventional flow. Mark unknown hops and conflicting paths explicitly.

## Contracts and Shared Dependencies

| Contract or Dependency | Provider | Consumer | Version or Boundary | Scope | Evidence Record | Confidence |
|---|---|---|---|---|---|
| <contract-or-dependency> | <provider> | <consumer> | <version-or-boundary> | <asserted-scope> | <evidence-record-id> | <confidence> |

## Evidence Register

| Record | Claim | Provenance | Source Path or Owner | Last Verified | Asserted Scope | Owner Validation | Confidence |
|---|---|---|---|---|---|---|---|
| <evidence-record-id> | <claim> | <provenance-type> | <source-path-or-owner> | <last-verified-date> | <asserted-scope> | <owner-validation> | <confidence> |

## Caveats and Unknowns

List stale, missing, conflicting, or scope-limited evidence. Keep <unknown-claim> UNKNOWN until the validation owner supplies or validates evidence. Do not resolve contradictions by popularity, naming, or cloud convention.

## Debugging Entry Points

List only likely entry points proven by the Evidence Register: <debugging-entry-point>, <debugging-evidence-record-id>, and <debugging-relevance>. For each, state why it is relevant, scope, source path or owner, last-verified date, and confidence. Do not invent paths, commands, endpoints, or logs.

## Related Skills

Reference only verified skills: <related-skill>. Record each skill's source path or owner, last-verified date, asserted scope, and confidence.

## Maintenance Triggers

Reverify after ownership, repository, boundary, contract, dependency, environment, deployment, entry-point, or runtime-wiring changes; after stale evidence exceeds {{EVIDENCE_MAX_AGE}}; or after an owner disputes a claim.

## Common Mistakes

- Treating local evidence as definitive runtime evidence.
- Omitting the prominent status or calling a map definitive while material runtime/deployment evidence is unresolved.
- Inferring deployment from filenames, imports, local configuration, or conventional cloud architecture.
- Omitting provenance, source path/owner, date, confidence, or asserted scope.
- Converting human-provided, convention, or stale evidence into code-backed facts.
- Filling unresolved variables or conflicting evidence with guesses.
- Listing debugging entry points or owners that were not verified.

## Self-Review and Return

Check the required frontmatter, exact trigger description, activation-time configuration, all required sections, provenance separation, confidence labels, runtime/local distinction, owner validation, unknown handling, and cloud-neutrality. After activation, verify frontmatter name exactly matches the containing skill folder. Return `DONE`. Validation owner: {{VALIDATION_OWNER}}.
