# Planning and Review

Define scope, acceptance criteria, affected files, dependencies, and explicit
out-of-scope work before implementation. Record the spec review mode
(`per-task` or `final-only`) and execution mode (`interactive` or an approved
project-defined mode).

Before editing, verify the repository, branch, worktree, governing
instructions, and existing local changes. Use an approved branch and worktree
policy; do not invent ticket identifiers or fixed naming conventions. Name a
validation owner and keep independent work in disjoint scopes.

Review acceptance criteria before implementation. For implementation work,
perform spec-compliance review before code-quality review. Resolve scope,
safety, and requirement gaps before reviewing maintainability and consistency.

Run the project's verified checks and retain command-level evidence. Report
passed, failed, skipped, blocked, and unknown results accurately. Separate
implementation from the final choice to debug, commit, push, pull request, or
release; require explicit approval for those actions.
