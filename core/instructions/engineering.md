# Engineering

Keep changes focused on the requested scope and match the conventions of the
nearest repository. Prefer clear, maintainable implementations over clever
shortcuts, and preserve contract compatibility unless a breaking change is
explicitly approved.

Inspect the relevant existing behavior before editing. Preserve unrelated
files, local modifications, generated state, and user-owned content. Keep
dependencies and lockfile changes minimal and explain any necessary churn.

Use evidence rather than assumptions for project facts, paths, commands, and
ownership. Separate reusable policy from project-specific context and client
integration instructions.

## Readability and simplicity

Make readability and reviewer understanding the highest code-quality priority
while preserving correctness. Keep the diff as small as the required change
allows; do not compress code or obscure intent merely to reduce the line count.
Avoid unrelated cleanup, reformatting, and refactoring.

Before coding and whenever complexity grows, step back and ask whether it is
necessary. Prefer the simplest implementation that meets the requirements.
Introduce abstractions, configuration, dependencies, or special cases only
when a concrete requirement justifies them. Explain unavoidable complexity
so the reviewer can assess the tradeoff.

## Proper fixes and workarounds

Aim for zero workarounds. Investigate the cause before implementing a fix, and
check whether the proposed approach addresses it or merely bypasses it. Repeat
that check during implementation rather than leaving it to human review.

When a workaround appears necessary, explain what is blocked, why it is
blocked, and what a proper fix would require before implementing the workaround.
Ask the user whether to pursue the proper fix or accept the workaround when
that choice changes scope or requires a tradeoff. Continue independent work
while awaiting that decision.

If the user has requested completion without prompts, pursue the proper fix
within the authorized scope. If a workaround is unavoidable within that scope,
keep it minimal and disclose it at completion. Do not silently expand scope or
claim a blocked task is complete. In the final report, identify any workaround
that remains, its reason, limitations, and what would allow its removal.

## Comments

Use clear code and names instead of narrating every operation. Add comments
only where they help a reviewer understand non-obvious intent, constraints,
or decisions. Comment any introduced workaround at the relevant code, explaining
why it is necessary and what would allow a proper fix to replace it.

## Repository conventions

Follow the repository's existing conventions strictly. Inspect its governing
instructions, nearby code, formatter and linter configuration, dependencies,
and test practices before editing. Match its style and organization.

When writing code, find and consult the repository's language or framework
style guide; search the web for the relevant official or authoritative guide
if none is provided locally. Use external guidance to fill gaps, not to override
repository conventions or justify unrelated restyling. If guidance is
unavailable or conflicts with repository instructions, state the limitation
or conflict rather than silently imposing a new convention.

Do not introduce a test framework, test directory, or committed test files into
a repository with no existing tests unless the user explicitly requests it.
Use existing tooling, temporary checks, or manual validation instead, and keep
temporary validation artifacts out of the final diff. Where tests exist, follow
their established patterns and add only tests warranted by the change.

## End-to-end validation

Actively seek a practical end-to-end check of the affected behavior. Exercise
the complete relevant user flow when feasible; passing unit tests alone does
not establish that the integrated behavior works.

If end-to-end testing is unavailable or disproportionate to the change, explain
why and choose the strongest practical alternative, such as existing unit or
integration tests, smoke checks, or build commands. Do not create unnecessary
committed files or new test infrastructure merely to perform validation.

Report what actually ran and its results separately from skipped or blocked
checks. Explain any decision to skip end-to-end testing, what the alternatives
leave unverified, and how the user can complete the remaining validation if
needed. Communicate during implementation when user input is needed, or at
completion when the user has requested uninterrupted work.
