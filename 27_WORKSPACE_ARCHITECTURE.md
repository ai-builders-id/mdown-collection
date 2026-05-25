# Workspace Architecture

Status: Baseline accepted and partially implemented. {{PROJECT_NAME}} now initializes
profile homes, creates daemon-known agent homes from templates, persists the
workspace registry and active grants under the default profile, exposes
`workspace list/register/grant/revoke/doctor` through the protocol and CLI, and
keeps tool execution behind daemon-resolved workspace grants. Real worker
worktree creation/cleanup, checkpoint rollback, workspace-local skill
enforcement, artifact production, and denied-path enforcement for mutating tools
remain future work.

## 1. Purpose

{{PROJECT_NAME}} separates durable identity, runtime state, project files, and isolated
coding execution. This prevents agents from drifting into the wrong directory,
keeps secrets and sessions out of project repositories, and lets many coding
workers operate on the same project without editing the same checkout.

The required concepts are:

| Concept | Purpose | Example |
| --- | --- | --- |
| Profile home | One {{PROJECT_NAME}} identity/environment with config, channels, agents, memory, sessions, skills, workers, logs, and artifacts | `~/.{{PROJECT_SLUG}}/profiles/default/` |
| Agent home | One persistent agent's persona, instructions, memory, skills, and policy | `~/.{{PROJECT_SLUG}}/profiles/default/agents/{{AGENT_SLUG}}/` |
| Project workspace | A real user project root that tools may access only after a grant | `~/Project/chatbot-ai-saas/` |
| Worker worktree | An isolated git checkout for one coding worker/task | `~/Project/chatbot-ai-saas/.{{PROJECT_SLUG}}/worktrees/w-auth-01/` |

Rule: agent home is not project cwd, and profile home is not a sandbox.

## 2. Implemented Now

The current desktop MVP implements only part of this architecture:

- `{{PROJECT_NAME}}_HOME` defaults to `~/.{{PROJECT_SLUG}}`.
- `~/.{{PROJECT_SLUG}}/config.toml` can be loaded.
- JSONL event logs are written with redaction boundaries.
- Store-level atomic JSON metadata helpers exist under `~/.{{PROJECT_SLUG}}/state`.
- Safe-read tool baselines canonicalize paths and reject outside-workspace reads.
- Worker protocol/event types and HUD worker display concepts exist.

Implemented baseline:

- `~/.{{PROJECT_SLUG}}/profiles/<profile>/` profile homes.
- Persistent daemon-known agent homes under each profile with `AGENT.toml`,
  persona/instruction/user/memory/tool guidance files, typed `POLICY.toml`
  metadata, `SKILL_POLICY.toml`, and agent-local memory/skill/prompt folders.
- Profile-local workspace registry and grant files.
- Protocol/CLI workspace commands: `list`, `register`, `grant`, `revoke`, and
  `doctor`.
- Tool calls require a registered workspace and matching active grant before
  safe-read execution or approval flow.
- `workspace doctor` includes profile/agent-home diagnostics for missing,
  corrupt, and oversized agent files.
- Worker events include planned worktree/artifact metadata.
- Store helpers now resolve project worker worktree paths and metadata files
  under `<project>/.{{PROJECT_SLUG}}/worktrees/`, resolve worker artifact paths under
  `profiles/<profile>/artifacts/workers/`, and let `workspace doctor` report
  stale worker worktree metadata or missing artifact roots.
- Worker execution setup now creates a git worktree for session-bound project
  workspaces, moves project-local worker worktree metadata through `ready`,
  `review_pending`, and `cleanup_pending`, and writes profile-scoped worker
  artifacts for review. Worker completion runs a bounded daemon-owned
  validation command inside the worker worktree and records the redacted command
  report in worker artifacts.
- `worker.cleanup` records cleanup intent for terminal {{PROJECT_NAME}}-owned worker
  worktrees without deleting files and rejects unknown, missing, or non-owned
  paths.

Still future:

- Worker worktree file removal.
- Configurable worker command/test execution and durable worker runtime logs.
- Checkpoint and rollback manager.
- Dedicated profile and agent doctor commands.
- Workspace-local skills and project `.{{PROJECT_SLUG}}/` metadata enforcement.

## 3. Target Home Layout

Top-level {{PROJECT_NAME}} home:

```text
~/.{{PROJECT_SLUG}}/
|-- config.toml                    # global non-secret defaults
|-- profiles/                      # independent {{PROJECT_NAME}} environments
|-- global-cache/                  # shared cache; safe to delete
|-- plugins/                       # installed {{PROJECT_NAME}} plugins/extensions
|-- bin/                           # optional managed helper binaries
|-- logs/                          # daemon-level redacted logs
|-- run/                           # sockets, pid files, selected ports
`-- VERSION
```

Profile home:

```text
~/.{{PROJECT_SLUG}}/profiles/default/
|-- profile.toml                   # profile config and feature flags
|-- .env                           # secrets fallback; chmod 0600; never committed
|-- secrets/                       # encrypted or OS-keyring-backed secret handles
|-- channels/                      # Telegram/HUD/mobile state, no plaintext secrets
|-- agents/                        # persistent agent homes
|-- memory/                        # profile-wide memory
|-- skills/                        # profile-level skills
|-- workspaces/                    # workspace registry, aliases, grants
|-- workers/                       # worker state records and streams
|-- sessions/                      # profile session JSONL and metadata
|-- artifacts/                     # patches, test reports, generated files
|-- checkpoints/                   # shadow repos for rollback
|-- sandboxes/                     # temporary isolated roots
|-- eventlog/                      # append-only event streams
|-- cron/                          # scheduled jobs
|-- logs/                          # profile logs, redacted
`-- locks/                         # state mutation locks
```

Agent home:

```text
~/.{{PROJECT_SLUG}}/profiles/default/agents/{{AGENT_SLUG}}/
|-- AGENT.toml
|-- PERSONA.md
|-- INSTRUCTIONS.md
|-- USER.md
|-- MEMORY.md
|-- TOOLS.md                       # guidance only, not permission policy
|-- POLICY.toml                    # hard permissions and sandbox defaults
|-- SKILL_POLICY.toml
|-- skills/
|-- memory/
|   |-- daily/
|   |-- decisions.md
|   `-- delegation.md
|-- prompts/
|-- sessions/                      # symlink or index into profile sessions
`-- README.md
```

## 4. Project Metadata

Project files stay in the user's project. {{PROJECT_NAME}} metadata inside a project lives
under `.{{PROJECT_SLUG}}/` and must not contain secrets:

```text
<project>/.{{PROJECT_SLUG}}/
|-- workspace.toml
|-- skills/
|-- artifacts/
|-- worktrees/
`-- media/
```

`workspace.toml` records project-local defaults such as workspace ID, VCS type,
worktree root, artifact root, and media root. It does not grant access by
itself; the daemon must still resolve a workspace grant.

Current baseline: `{{PROJECT_SLUG}}-store` can load and write this project-local metadata,
and `workspace doctor` reports missing files, registry ID mismatch, absolute
project-local roots, and duplicate registered roots. Creation/initialization UX
remains future work.

## 5. Media Assets Convention

Project-scoped media assets generated or curated by {{PROJECT_NAME}} should use:

```text
<project>/.{{PROJECT_SLUG}}/media/
|-- input/                         # user-provided or copied references
|-- generated/                     # generated images, audio, video, sprites
|-- thumbnails/
|-- manifests/
`-- exports/
```

Rules:

- Media files that are source assets for the project may be moved into the
  project proper only through an explicit write/apply action.
- Generated media manifests must record source prompt or task ID, producing
  agent/worker ID, model/provider if known, license/source notes, and target use.
- Secrets, provider tokens, raw channel tokens, and private session transcripts
  must never be stored in project `.{{PROJECT_SLUG}}/media/`.
- Large binary media should be ignored by default unless the project explicitly
  opts into tracking it.

## 6. Workspace Registry and Grants

Project roots are registered in profile state, not guessed repeatedly:

```text
~/.{{PROJECT_SLUG}}/profiles/default/workspaces/
|-- registry.toml
|-- aliases.toml
`-- grants.jsonl
```

Example:

```toml
[[workspace]]
id = "chatbot-ai-saas"
kind = "project"
root = "~/Project/chatbot-ai-saas"
vcs = "git"
owner = "{{AGENT_SLUG}}"
trusted = true
worktree_root = ".{{PROJECT_SLUG}}/worktrees"
artifact_root = ".{{PROJECT_SLUG}}/artifacts"
media_root = ".{{PROJECT_SLUG}}/media"
checkpoint_policy = "enabled"
```

Every `file.*`, `shell.*`, `git.*`, and `worker.*` operation must resolve a
workspace grant before execution:

```text
WorkspaceGrant {
  profile_id,
  agent_id,
  workspace_id,
  root,
  access: read | write | exec | admin,
  expires_at,
  source: route | user | policy | worker_spawn
}
```

If no grant exists, the tool fails closed or requests approval. Grants are
runtime policy records; project `.{{PROJECT_SLUG}}/workspace.toml` is only metadata.

## 7. Denied Paths

Path resolution must canonicalize paths, reject symlink escapes, verify the
granted root, check access mode, and then enforce denied paths.

Minimum denied paths:

```text
~/.ssh
~/.aws
~/.gnupg
~/.config/gcloud
~/.{{PROJECT_SLUG}}/profiles/*/.env
~/.{{PROJECT_SLUG}}/profiles/*/secrets
~/.{{PROJECT_SLUG}}/profiles/*/channels/*/tokens
/etc
/dev
/proc
/sys
```

Broad grants to `/`, `$HOME`, system directories, cloud credential directories,
or profile secret directories are invalid unless a future admin mode explicitly
defines a safer exception.

## 8. Worker Worktrees

Coding workers use git worktrees by default:

```text
main project checkout  -> user-owned stable branch
worker worktree        -> isolated branch per task
checkpoint shadow repo -> rollback safety net
final patch/PR         -> reviewed before merge
```

Default worker worktree path:

```text
<project>/.{{PROJECT_SLUG}}/worktrees/<worker-id>/
```

Worker state and artifacts live under the profile:

```text
~/.{{PROJECT_SLUG}}/profiles/default/workers/<worker-id>.toml
~/.{{PROJECT_SLUG}}/profiles/default/workers/<worker-id>.jsonl
~/.{{PROJECT_SLUG}}/profiles/default/artifacts/workers/<worker-id>/
```

Project-local worker worktree metadata lives beside the project worktree root:

```text
<project>/.{{PROJECT_SLUG}}/worktrees/
|-- <worker-id>/                  # planned/actual git worktree checkout
`-- .metadata/
    `-- <worker-id>.toml          # worker ID, workspace ID, path, branch, base ref, state, artifact root
```

`workspace doctor` treats these metadata files as diagnostics only. It warns
when a recorded worktree path or profile artifact root is stale/missing; it does
not create, remove, or mutate git worktrees. Worktree creation and cleanup
planning are owned by the daemon worker runtime when a session-bound worker
starts or reaches a terminal cleanup flow. Cleanup planning moves metadata to
`cleanup_pending`; actual file removal remains a later explicit executor.

Workers receive write/exec grants only for their worktree unless the user
explicitly approves broader access. The parent project checkout remains
read-only by default.

## 9. Skill Precedence

Target precedence, highest first:

1. Agent-local: `profiles/<profile>/agents/<agent>/skills/`
2. Workspace-local: `<project>/.{{PROJECT_SLUG}}/skills/`
3. Profile-local: `profiles/<profile>/skills/approved/`
4. Managed plugins: `~/.{{PROJECT_SLUG}}/plugins/*/skills/`
5. Bundled {{PROJECT_NAME}} skills

Generated skills should start as candidates and become active only after review
or explicit policy.

## 10. Future Work Phases

The workspace architecture should be implemented in this order:

| Phase | Outcome |
| --- | --- |
| W0 | Finalize terminology and protocol types |
| W1 | {{PROJECT_NAME}} home resolver and directory skeleton |
| W2 | Profile manager baseline implemented |
| W3 | Agent home manager/templates baseline implemented |
| W4 | Workspace registry and grants |
| W5 | Worker worktree manager |
| W6 | Checkpoint and rollback manager |
| W7 | Event log and session store integration |
| W8 | Channel bindings and deterministic routing |
| W9 | Doctor, migration, and templates; profile/agent file diagnostics baseline implemented |
