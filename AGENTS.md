# Agent Workspace Template Instructions

## Purpose
Operate this distribution and bootstrap a project-specific multi-repository workspace.

## Bootstrap entrypoint
Read `playbooks/bootstrap.md`, perform its preflight and interview, present the complete materialization plan, and wait for approval before editing generated workspace files.

## Upgrade entrypoint
When the user requests an update, read `playbooks/upgrade.md`, the target release migrations, and the workspace manifest before proposing changes.

## Invariants
- Treat `core/` and `catalog/` as portable source material.
- Treat `adapters/` as client-specific translation layers.
- Never invent project facts or credentials.
- Preserve unmanaged files and local modifications.
- Report skipped validation separately from passing validation.

## Engineering behavior

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
