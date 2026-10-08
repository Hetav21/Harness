# Upgrade a Generic Agent Workspace

This playbook applies a released template update to an existing workspace
without silently taking ownership of files. It is a plan-first, validation-first
procedure. The agent must preserve unmanaged files and local modifications, must
not invent project facts or credentials, and must never inspect cached
credentials. Authentication remains human-controlled; when a required authenticated operation cannot be performed by the user, authentication blocks the affected check and the upgrade does not claim that check passed.

The manifest is the ownership record. A path is managed only when the manifest
identifies its owner, update policy, and content hash or link target. Unmanaged files are not adopted, overwritten, or removed by this playbook.

## Source retrieval

The generated workspace `AGENTS.md` directs update requests to the retained
`.agent-template/manifest.yaml`. Use its `template.repository` to retrieve a
target tagged release into a temporary directory outside managed output paths;
the original starter checkout is not required. If no target was specified,
inspect release tags and include the proposed target in the approval plan.
Never silently substitute the repository's default branch for a release.

Verify the target tag, full commit ID, and matching `VERSION`, then use that
checkout's upgrade playbook, schema, and migrations. Verify that the installed
release tag still resolves to `template.commit` when recorded; a mismatch
blocks the upgrade. For a legacy manifest containing only an ambiguous
repository shorthand, obtain a verified clone URL from the user or retained
evidence rather than guessing a host. Missing source access or authentication
blocks retrieval and must be reported without changing the workspace.

## Manifest read

Read `.agent-template/manifest.yaml` and validate its schema before reading or
applying migrations. Record the installed release, selected clients, selected
modules, output ownership, policies, hashes/link targets, warnings, and applied
migrations. A missing, malformed, or ambiguous manifest stops the upgrade and
produces a rollback report without changing files. Preserve any pre-existing
partial-run marker and its recorded backup location.

The target schema accepts legacy version 1 and current version 2 manifests.
Version 2 requires `template.commit`; version 1 does not prove the original
source commit. Do not invent that historical value. The migration records the
verified target commit only after the approved upgrade succeeds.

## Release range resolution

Read the target distribution `VERSION` and resolve the ordered release range
from the manifest release to that target. Require a contiguous chain of named
migrations; reject downgrades, unknown releases, duplicate migrations, and a
target already recorded as applied unless the run is an explicitly resumed
interrupted run. Do not infer a version from file contents. Explain the range in
the proposed plan.

## Migration filtering

Select only migrations in the resolved range, then filter each migration by the
clients and modules recorded as installed (and by any explicitly selected
client/module scope supplied by the user). A migration may describe shared core
changes, but client-specific steps apply only to selected clients and module
steps apply only to selected modules. Report skipped migrations and filtered
steps as warnings, not as applied migrations. Unsupported clients or modules
are conflicts, not permission to generate or adopt their files.

## Current hash and link checks

Before planning writes, recompute every managed file's content hash and every
managed symlink's resolved target. Compare them with the manifest, distinguish a
missing path from a changed path, and verify that relative links still resolve.
Never inspect secret values while doing these checks. A broken link, missing
managed output, or hash mismatch is recorded for classification; it is not
silently repaired.

## Local-change classification

Classify every affected path as clean managed, locally modified managed,
missing managed, unmanaged, or already-updated. Apply the manifest policy as
follows:

* **replace-if-unmodified**: clean managed files may receive the new generated
  content; modified, missing, or unmanaged paths become conflicts and are not
  overwritten.
* **replace-link-if-unmodified**: update a clean managed symlink only when its
  recorded target is unchanged and the new relative target resolves; otherwise
  report a link conflict and leave it alone.
* **merge**: propose a reviewed semantic merge using the source module and the
  user's local content. Preserve unknown sections and local facts; ambiguous or
  conflicting hunks require explicit user resolution before apply. A merge is
  not permission to overwrite a modified file.
* **user-owned**: never replace, merge, adopt, or remove the path. Report the
  available update as a human action.
* **remove-if-unmodified**: remove only an existing clean managed file whose
  recorded hash still matches. Modified, missing, and unmanaged obsolete files
  are preserved and reported.

An unmanaged file is never adopted or silently overwritten, regardless of its
name or similarity to generated content. Authentication or unavailable client
diagnostics are classified as skipped when optional and blocked when required;
neither is passed.

## Plan gate

Present a complete plan before changing anything. Include each selected
client/module, migration, path, current classification, policy, proposed
action, source, expected hash/link target, conflicts, warnings, backups, and
offline/live validation. The plan must explicitly describe clean replacement,
semantic merge, symlink-target update, obsolete-file removal, preserved
conflicts, and filtered selections. Obtain explicit user approval immediately
before apply. Approval is not implied by the manifest, prior bootstrap, or a
previous plan. If the working tree or manifest changes after approval, stop and
re-plan.

## Apply

After approval, create a pre-upgrade copy of every path that may change and a
durable run record containing the plan, manifest version, and completed step.
Apply only approved clean replacements, approved link updates, reviewed
semantic merges, and clean obsolete-file removals. Write shared/core outputs
before adapter outputs, and record each successful operation. Do not write
unmanaged, user-owned, dirty, or unresolved-conflict paths. On any write error,
stop immediately; do not continue a partial upgrade without first recording it.

If an earlier run left a partial or interrupted record, verify the pre-upgrade
copy and current classifications, report what was applied, and require an
explicit resume or rollback decision. Never treat a partial run as successful.

## Validate

Run offline validation first: migration continuity, manifest/schema syntax,
source portability, generated paths, content hashes, link resolution, policy
ownership, semantic-merge preservation, and selected client/module adapter
checks. Run live client diagnostics only when requested and when the human has
already authenticated in their own session; never perform login, MFA, token
refresh, role selection, or cached-credential inspection. Record every check as
`passed`, `failed`, `skipped`, or `blocked`, with command/evidence and result.
Skipped checks are not passed. Any required failure or authentication-blocked
check prevents completion and triggers rollback handling.

## Manifest update

Only after validation, and only after all required validation passes, update the manifest atomically with
schema version 2, the verified source clone URL, target release and full commit,
current hashes/link targets, warnings, selected
clients/modules, and applied migration IDs. Retain conflict, skipped, and
blocked warnings; do not convert them to passes. If validation fails, leave the
old release, hashes, warnings, and migration history authoritative and do not
claim the migration was applied. Remove the run record only after the manifest
update is durable and the final validation report is recorded.

## Rollback report

For a failed, conflicted, interrupted, or partially applied run, report the
exact paths changed, preserved, conflicted, and skipped, the validation results,
the run-record and pre-upgrade-copy locations, and whether restoration was
completed. Restore the pre-upgrade copy for all changed paths when rollback is
approved or required by a failed atomic step, then re-check hashes/links. Never
restore over an unmanaged or newly modified path without explicit resolution;
escalate that path as a conflict. The final report must state remaining human
actions and must say plainly that an incomplete upgrade is not successful.
