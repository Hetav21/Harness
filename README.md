# Generic Agent Workspace Template

## What this is

This repository is a versioned, client-neutral starter for bootstrapping a
project-specific multi-repository agent workspace. It contains portable policy,
project-context, role, manifest, validation, and Agent Skills source material,
plus adapters for supported clients. It contains no project facts, credentials,
or assumed repository commands.

The distribution is a starter source, not the generated workspace itself. Copy
or clone a tagged release, then have an agent read the
[bootstrap playbook](playbooks/bootstrap.md), conduct the preflight and
essay-led interview, and produce a complete materialization plan. The agent
must wait for explicit approval of that final plan before editing generated
workspace files.

## Supported clients

The current release has adapters for:

- [Claude Code](adapters/claude-code/README.md)
- [OpenAI Codex](adapters/codex/README.md)
- [OpenCode](adapters/opencode/README.md)
- [Google Antigravity](adapters/antigravity/README.md)

Shared skills use the [Agent Skills specification](https://agentskills.io/specification)
and are installed canonically under `.agents/skills`. Client adapters translate
selected capabilities into native files; the presence of a client or a source
template does not select it automatically.

## Bootstrap a new workspace

Start with a tagged release by copying or cloning this repository. The agent
performs a read-only preflight, asks for a free-form project essay first, and
then asks one targeted question at a time. It records evidence, unknowns,
contradictions, ownership, and local commands without inventing facts.

Next, explicitly select clients and capabilities. The agent presents the full
plan, including every selected output and conflict, validation tier, owner, and
update policy. Only explicit approval of that complete plan permits
selected materialization. Materialization is selected-only: unselected clients,
capabilities, and source modules are not generated.

Read the [bootstrap playbook](playbooks/bootstrap.md) for the gates and output
requirements. The example ownership record is
[core/manifest/manifest.example.yaml](core/manifest/manifest.example.yaml).

## What the project essay should cover

The essay should describe, in the adopter's own words, repositories and paths,
ownership and responsibilities, dependencies, setup/run/test commands, CI/CD,
deployment, observability, authentication boundaries, desired clients, and
desired capabilities. The agent maps those claims into project context and an
evidence ledger, preserving gaps, assumptions, contradictions, and unknowns.
Do not provide credentials or ask the agent to infer facts that are not in the
essay or authoritative repository evidence.

## Select clients and capabilities

Client selection and capability selection are separate decisions. For each
selected client, choose only capabilities supported by its adapter, such as
instructions, roles, canonical skills, MCP, hooks, settings, or plugins.
Missing client binaries may still support offline file generation, but live
validation cannot be claimed for them.

The canonical skill files live in `.agents/skills`. For Claude Code, the
preferred bridge is a relative per-skill symlink under `.claude/skills` pointing
to the canonical skill. Resolve and record each link. Official Claude
documentation does not guarantee runtime discovery through such symlinks, so
filesystem resolution is insufficient: live `/skills` discovery needs to be
verified in an authenticated user session when that check is requested.

## Generated workspace state

The generated workspace retains `.agent-template/manifest.yaml` and the project
context. The manifest records the source repository clone URL, release tag,
exact source commit, selected clients and capabilities,
source modules, ownership, hashes or link targets, migrations, warnings, and
validation results. It must retain skipped and blocked checks rather than
calling them passed.

After a successful bootstrap, the starter source repository may optionally be
removed, but only with explicit approval. The generated manifest and project
context must remain outside that checkout, alongside the generated instructions.
The generated `AGENTS.md` includes a Harness updates section pointing to the
manifest. Keep these files in the project's version-controlled workspace
configuration. No separate update guide or permanent Harness clone is needed.

## Upgrade from a later release

Ask the agent to update the Harness-generated instructions. It reads the
retained manifest, retrieves a target tagged release from the recorded origin
into a temporary directory, and follows that release's
[upgrade playbook](playbooks/upgrade.md) and migrations. The agent verifies the
source identity and resolves the contiguous migration range,
classifies managed and unmanaged changes, presents a new complete plan, and
waits for explicit approval. Local modifications and unmanaged files are
preserved. See the [changelog](CHANGELOG.md) for release history.

New workspaces use manifest schema version 2 with a required source commit.
The current validator also accepts version 1 manifests so existing workspaces
can migrate without inventing missing historical provenance.

## Validation tiers

Validation is reported separately by tier and by client/capability:

1. **Offline validation** checks source portability, syntax, paths, hashes,
   ownership policies, link resolution, and static adapter diagnostics.
2. **Optional local CLI validation** uses only an already-installed client and
   its documented non-authenticating diagnostics. Missing binaries are
   skipped, not passed.
3. **Authenticated live validation** is run only when requested and when a
   human has already authenticated in their own client session. Live discovery,
   including Claude symlink discovery, must be verified rather than assumed.

Every check is `passed`, `failed`, `skipped`, or `blocked`; skipped is not
passed. Offline correctness does not imply client runtime compatibility.

## Security and authentication

Never place credentials, tokens, private keys, or secret values in this
repository, project context, examples, manifests, or generated guidance. The
agent must not inspect cached authentication material and must never handle
login, MFA, SSO, role selection, or token refresh. Authentication is human
controlled: hand off to the user and report the affected live check as skipped
or blocked. MCP examples use environment-variable names only.

## Compatibility limitations

Adapters reflect documented client contracts and are not a guarantee that every
client version discovers every generated file. Native syntax, permissions,
hooks, MCP behavior, and symlink traversal can vary by version and environment.
Official references used for this release include Claude Code [memory](https://docs.anthropic.com/en/docs/claude-code/memory),
[skills](https://docs.anthropic.com/en/docs/claude-code/skills),
[subagents](https://docs.anthropic.com/en/docs/claude-code/sub-agents), and
[hooks](https://docs.anthropic.com/en/docs/claude-code/hooks); Codex
[customization](https://developers.openai.com/codex/concepts/customization),
[`AGENTS.md`](https://developers.openai.com/codex/guides/agents-md),
[configuration](https://developers.openai.com/codex/config-reference),
[MCP](https://developers.openai.com/codex/mcp), and
[hooks](https://developers.openai.com/codex/hooks); OpenCode
[instructions](https://opencode.ai/v2/docs/instructions/),
[skills](https://opencode.ai/docs/skills/), [agents](https://opencode.ai/v2/docs/agents/),
and [configuration](https://opencode.ai/docs/config/); and Antigravity
[CLI reference](https://antigravity.google/docs/cli/reference/),
[skills](https://antigravity.google/docs/skills/),
[subagents](https://antigravity.google/docs/subagents/),
[MCP](https://antigravity.google/docs/cli/mcp/), and
[hooks](https://antigravity.google/docs/hooks/).

## Repository development

This distribution intentionally has no bundled executable test harness. Review
portable sources against `core/validation/ACCEPTANCE.md`, verify selected
adapter paths and configuration syntax, and follow the bootstrap or upgrade
playbook's evidence gates. Do not run live client sessions or authentication as
part of offline validation. Keep changes focused, preserve unmanaged files, and
report skipped validation separately from passing validation.
