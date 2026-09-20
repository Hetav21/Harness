# Neutral Role Contract

Every role definition must state the following before it is materialized into
any client-specific representation:

## Required fields

- **Purpose:** the outcome and responsibility of the role.
- **Allowed scope:** repositories, paths, systems, and operations it may use.
- **Denied scope:** explicit files, systems, data, operations, or permissions it must not use.
- **Inputs:** context, evidence, requests, and prerequisites it may consume.
- **Outputs:** artifacts, decisions, reports, and evidence it must produce.
- **Validation owner:** the person, role, or team responsible for accepting results.
- **Escalation conditions:** uncertainty, contradiction, missing access, unsafe change, or failed validation that requires handoff.
- **Capability requirements:** client-independent capabilities needed, plus any client-specific capability requirements that must be verified by the adapter.

Roles must not invent permissions, project facts, credentials, or tool names.
Client adapters may translate this contract into native metadata, but may not
expand the allowed scope or remove denied scope.
