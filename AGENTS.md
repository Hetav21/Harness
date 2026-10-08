# Agent Workspace Template Instructions

## Purpose
Operate this distribution and bootstrap a project-specific multi-repository workspace.

## Bootstrap entrypoint
Read `playbooks/bootstrap.md`, perform its preflight and interview, present the complete materialization plan, and wait for approval before editing generated workspace files.

## Upgrade entrypoint
When the user requests an update, read `playbooks/upgrade.md`, the target release migrations, and the workspace manifest before proposing changes.

## Distribution release checklist

For changes to this source distribution, assess release impact before committing
and report the decision. This is separate from upgrading a generated workspace.

- Record notable changes in `CHANGELOG.md`; use an `Unreleased` section when a release is deferred.
- When releasing, update `VERSION`, date the changelog entry, and align current-release references in examples. Preserve historical release entries and migrations.
- Use a patch release for compatible policy corrections, a minor release for new capabilities, and explicitly assess compatibility for breaking changes.
- Add `migrations/<previous>-to-<next>.md` for every release, even if it only explains that no generated files change. Keep the migration chain contiguous and describe scope, ownership, validation, and rollback.
- Change schema and adapter versions only when their contracts change.
- Verify release metadata, example syntax/schema, migration continuity, and selected instruction entrypoints; report skipped live checks separately.
- When publication is authorized, commit the validated release and publish its matching `v<VERSION>` tag. Never move an existing release tag; verify the remote branch and tag after pushing.

## Invariants
- Treat `core/` and `catalog/` as portable source material.
- Treat `adapters/` as client-specific translation layers.
- Never invent project facts or credentials.
- Preserve unmanaged files and local modifications.
- Report skipped validation separately from passing validation.

## Engineering behavior

Read and follow [the shared engineering policy](core/instructions/engineering.md)
when working on this distribution. Use it as the source for generated
engineering instructions when that module is selected.

Adapted from [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills). These guidelines favor caution over speed; use judgment for trivial tasks.

### Think before coding

- State assumptions explicitly and ask when uncertain.
- Present multiple interpretations instead of choosing silently.
- Prefer simpler approaches and push back when warranted.
- Stop and ask when requirements are unclear.

### Simplicity first

- Write the minimum code needed to solve the request.
- Do not add unrequested features, abstractions, flexibility, or configurability.
- Do not handle impossible scenarios.
- If the implementation is larger than necessary, simplify it.

Ask: “Would a senior engineer say this is overcomplicated?” If yes, simplify.

### Surgical changes

- Touch only what the request requires.
- Do not improve, reformat, or refactor unrelated code.
- Match the existing style.
- Mention unrelated dead code instead of deleting it.
- Remove only imports, variables, or functions made unused by your changes.

Every changed line should trace directly to the user’s request.

### Goal-driven execution

- Define verifiable success criteria before implementation.
- For bug fixes and behavior changes, reproduce or test the target behavior, then make the test pass.
- For refactors, verify tests before and after.
- For multi-step work, state a brief plan with a verification check for each step.
- Continue until the stated success criteria are verified.
