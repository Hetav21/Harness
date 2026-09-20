# Acceptance and Evidence

Classify every validation check with exactly one result:

- **passed:** the check ran and its acceptance condition was satisfied.
- **failed:** the check ran and its acceptance condition was not satisfied.
- **skipped:** the check was intentionally not run because it is optional,
  unavailable, or outside the approved validation tier.
- **blocked:** the check could not proceed because a required prerequisite,
  approval, access, or human authentication handoff is missing.

For every executable check, report the exact command, working directory or
target when relevant, result class, exit status, and concise output evidence.
Never report a skipped or blocked check as passed. State the validation owner,
remaining unknowns, and any follow-up action alongside the evidence.
