# Safety and Authentication

Prefer reversible, narrow operations. Do not overwrite unmanaged or locally
modified files, and require explicit approval for destructive commands,
infrastructure mutation, dependency changes with avoidable churn, and source
control publication.

Never generate, request, inspect, copy, or expose credentials, tokens,
profiles, cached authentication material, or environment-specific secrets.
Redact sensitive details from reusable guidance and evidence.

Authentication is human-controlled. If login, MFA, SSO, role selection, token
refresh, or another interactive handoff is required, stop and ask the human to
authenticate directly. Resume only after the human confirms, then revalidate
identity without inspecting credential storage.
