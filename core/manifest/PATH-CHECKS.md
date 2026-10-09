# Workspace path checks

Schema validation checks manifest structure; it does not establish destination
uniqueness or filesystem containment. Bootstrap and upgrade must also perform
these checks using the actual destination filesystem before accessing managed
file contents or changing workspace files.

1. Resolve the approved workspace root. Include all output destinations,
   `project-context`, `.agent-template/manifest.yaml`, and any planned backup or
   run-record paths. Check the manifest location before reading it, then check
   its recorded paths and the complete proposed plan.
2. Require workspace-relative paths using forward slashes. Reject absolute or
   drive-qualified paths, backslashes, control characters, `..` components,
   and paths that normalize to the workspace root. Normalize `.` components
   and repeated separators for comparison; never silently rewrite a manifest
   to resolve an ownership conflict.
3. Resolve existing parent directories, including symlinks, and verify that
   each destination stays inside the resolved workspace root. For an ordinary
   file, also check the final component if it is a symlink. For a managed link,
   check the link's location separately from its target; updating the link must
   not write through it. Validate recorded, existing, and proposed link targets
   relative to the link's parent. Apply the same syntax rules to targets,
   except that `..` is allowed here for canonical skill bridges; targets must
   resolve inside the workspace. Check a planned target before writes and
   verify it exists before creating the link.
4. Require one output record and one planned operation per destination. Reject
   duplicate destinations even when owners or policies differ. Compare
   normalized paths and resolved parent locations, accounting for aliases and
   case equivalence on the destination filesystem. A planned update may match
   its existing manifest record; two records or two planned operations may not
   claim the same destination. Link targets are not additional destinations:
   a canonical skill and its bridge are distinct outputs. Also reject plans
   that require a file or managed link to be another output's parent directory.
5. Stop before writes when a path escapes, ownership overlaps, or resolution
   cannot be verified. Report the conflicting paths and correct the plan;
   never choose an owner silently. Repeat the checks immediately before apply
   and if filesystem state changes during materialization or rollback.

Use the agent's existing filesystem tools. Record the checks and their results
with validation evidence; passing the JSON schema alone is insufficient.
