# Bootstrap a Generic Agent Workspace

This playbook turns the distribution into a project-specific workspace without
guessing project facts, silently taking ownership of files, or authenticating
on a user's behalf. The agent records evidence and stops at every gate below
when required input is missing.

The agent must never invent facts.
The agent must never overwrite unmanaged files.
Get explicit user approval immediately before file changes.

## Preflight

**Agent actions**

1. Identify the workspace root and inspect only repository metadata, directory
   names, and ordinary project files needed for the bootstrap. Detect which
   supported clients are currently present or configured (Claude Code, Codex,
   OpenCode, and Antigravity), but do not assume the current client is the
   only target. A detected client is evidence of availability, not consent to
   install its adapter.
2. Inventory existing instruction, skill, agent, MCP, hook, and settings files,
   including native paths and `.agents/skills`. Record whether each is managed
   by an existing `.agent-template/manifest.yaml`, clearly unmanaged, or
   modified locally. Preserve dirty and unmanaged files; never overwrite
   unmanaged files and never adopt them silently.
3. Check for a starter source checkout, existing project context, and manifest.
   Do not inspect cached credentials, token stores, private keys, or secret
   values. Authentication remains under human control.

**Required output:** a preflight inventory with detected clients, candidate
   files, dirty/unmanaged classifications, and a list of unavailable checks.

**Stop condition:** stop before the interview if the root or ownership state is
ambiguous, a destructive conflict cannot be represented safely, or a required
   human authentication handoff is being requested.

## Essay intake

**Agent actions**

1. Ask for a free-form project essay first. Do not begin with a checklist or
   assume repository names, owners, commands, deployment targets, or roles.
2. Extract claims from the essay, retain the user's wording, and map claims to
   repositories, ownership, dependencies, local commands, CI/CD, deployment,
   observability, authentication, desired clients, and desired capabilities.
3. After the essay, ask one targeted follow-up question at a time. Each question
   must close the highest-impact gap and wait for its answer before asking the
   next question.

**Required output:** a draft project context containing claims, evidence links
   or references, gaps, assumptions, contradictions, and unknowns.

**Stop condition:** stop asking questions and return the incomplete draft when
   the user declines to provide more information; do not fill gaps with facts
   invented by the agent.

## Evidence review

**Agent actions**

Review every material claim and classify it as **verified**, **assumed**,
**unknown**, or **contradictory**. For verified claims, record the evidence
   source, its authority, and its freshness (date or explicit unknown). Prefer
   authoritative repository/configuration evidence over prose, and prefer
   recent evidence when authority is equal. Mark stale evidence rather than
   silently treating it as current. Keep contradictory sources visible and ask
   one targeted question at a time to resolve them.

**Required output:** an evidence ledger with classification, source, authority,
   freshness, and consequence for materialization; a resolved list of facts the
   plan may use.

**Stop condition:** stop before selection if a required fact remains unknown or
   contradictory and would change ownership, permissions, credentials, or an
   output path.

## Client selection

**Agent actions**

Present all detected and requested supported clients separately. Ask the user
   to explicitly select the clients to materialize; detected clients are not
   automatically selected. Explain that missing clients may be selected for
   offline file generation but their live validation cannot be claimed.

**Required output:** an explicit selected-client list and a rejected,
   unavailable, or deferred list with reasons.

**Stop condition:** stop if no client is selected or if a requested client has
   no adapter contract in this release.

## Capability selection

**Agent actions**

For each selected client, present only capabilities supported by its adapter,
   such as instructions, roles, canonical skills, MCP, hooks, settings, and
   plugins. Ask the user to select capabilities independently; do not enable a
   capability merely because a source template exists. Keep MCP examples inert
   and use environment-variable names, never credentials.

**Required output:** selected client/capability pairs, required source modules,
   and live/offline validation requirements.

**Stop condition:** stop if a selected capability is unsupported, requires an
   unknown permission, or requires authentication that the user has not
   provided through their own client session.

## Materialization plan

**Agent actions**

Build the complete plan before changing any generated workspace file. Include
   every selected output and every conflict, including files that will remain
   untouched. Use this exact table schema:

| Path | Action | Source module | Owning adapter | Existing state | Update policy | Validation |
|---|---|---|---|---|---|---|
| `.agents/skills/<skill>` | install canonical skill | `catalog/skills/<skill>/SKILL.md` | shared core | absent/managed/unmanaged | replace-if-unmodified | file and content-hash check |
| `<native path>` | generate selected adapter output | selected core module/template | selected adapter | absent/managed/unmanaged/modified | manifest policy or user-owned | offline, then live if available |

Install shared content first in the plan, then adapter outputs. The canonical
   skill location is `.agents/skills`. For Claude, prefer relative per-skill
   symlinks under `.claude/skills` to those canonical skills, and record target
   resolution. Symlink traversal and discovery are filesystem/client dependent
   and are not guaranteed by official Claude documentation, so require a
   filesystem-resolution check plus live `/skills` discovery when an
   authenticated session is available.

For selected instruction modules, plan to embed their portable policy in the
generated shared `AGENTS.md`, including `core/instructions/engineering.md` when
selected. Do not copy the distribution's bootstrap or release-maintenance
instructions into project policy, or leave links that depend on retaining the
starter checkout. Record the selected source modules and output ownership.

**Required output:** the completed table, conflict decisions, selected-only
   source list, and predicted manifest entries. Existing unmanaged files must
   remain explicitly untouched; they cannot be overwritten or adopted silently.

**Stop condition:** stop and revise the plan if any selected output would
   overwrite a dirty or unmanaged file, or if the table is incomplete.

## Approval gate

Show the evidence ledger, selected clients and capabilities, complete plan
table, conflicts, warnings, and validation tiers. State exactly which files may
   change and whether the starter source may be removed later. Obtain explicit
   user approval immediately before any file changes. Approval of the essay or
   plan draft alone is not approval to edit.

**Required output:** a recorded approval referring to the final plan version.

**Stop condition:** if approval is absent, conditional, or changed, do not edit
   files; return to the affected selection or planning step.

## Materialization

**Agent actions**

After approval, re-check that the working tree and ownership inventory still
   match the plan. Materialize selected shared content first, including
   `.agents/skills`, then materialize selected adapter outputs. Resolve and
   record relative Claude skill links. Generate only selected clients and
   capabilities. Do not overwrite unmanaged or newly modified files; pause and
   request a revised plan if state changed.

**Required output:** generated paths, source-module ownership, hashes or link
   targets, and a list of files deliberately preserved.

**Stop condition:** stop on any write error, changed ownership state, broken
   required link, or unexpected output. Do not continue partially without
   recording the partial state and obtaining a revised approval.

## Validation

Run offline checks first: source portability, frontmatter/config syntax,
   output paths, content hashes, ownership policies, relative-link resolution,
   and selected adapter diagnostics that do not authenticate or start live
   sessions. Then run live checks only when the human has an authenticated
   session and has requested them. Never inspect cached credentials and never
   perform login, MFA, token refresh, or role selection for the user.

Check that generated instruction entrypoints contain the selected policy and
that their required imports and links resolve within the retained workspace,
independently of the starter checkout.

Report each check as exactly one of **passed**, **failed**, **skipped**, or
   **blocked**, with command/evidence and result. Missing client binaries or an
   unavailable live session are **skipped** when the check is optional and
   **blocked** when required; neither is **passed**. A failed check blocks the
   affected output from being declared valid.

Skipped checks are not passed.

**Required output:** a validation report by client, capability, and tier,
   including warnings and all skipped or blocked checks.

**Stop condition:** stop before the manifest if any required validation is
   failed or blocked, or if a warning changes the ownership or safety claim.

## Manifest

Write `.agent-template/manifest.yaml` only after required validation completes.
   Record the template release, selected clients and capabilities, source
   modules, output owners, update policies, content hashes/link targets,
   project-context reference, applied migrations, validation results, warnings,
   skips, and blocks. Do not claim skipped or blocked checks as passed.

**Required output:** a manifest that describes only selected materialization and
   the final project context, with warnings and skipped/blocked checks retained.

**Stop condition:** if the manifest cannot faithfully represent ownership,
   hashes, links, or validation status, do not publish it as complete.

## Completion

Report generated paths, preserved files, manifest location, validation status,
   warnings, and remaining human actions. The starter source repository may be
   removed only after successful bootstrap and only with explicit approval; the
   generated `.agent-template/manifest.yaml` and project context must remain.
   If validation was skipped or blocked, say so plainly and identify the human
   action needed. Do not report an incomplete bootstrap as successful.

**Required output:** a concise completion report and the next safe action.

**Stop condition:** end without further edits. Future changes must use the
   upgrade playbook and the manifest rather than silently re-running bootstrap.
