# Configuration Reference

## 1. Default Location

The desktop MVP reads user config from:

```text
~/.{{PROJECT_SLUG}}/config.toml
```

`{{PROJECT_NAME}}_HOME` can move that directory. {{PROJECT_NAME}} creates this local state layout on
first run:

```text
~/.{{PROJECT_SLUG}}/
|-- config.toml
|-- profiles/
|   `-- default/
|       |-- profile.toml
|       |-- .gitignore
|       |-- agents/
|       |   `-- <agent>/
|       |       |-- AGENT.toml
|       |       |-- PERSONA.md
|       |       |-- INSTRUCTIONS.md
|       |       |-- USER.md
|       |       |-- MEMORY.md
|       |       |-- TOOLS.md
|       |       |-- POLICY.toml
|       |       |-- SKILL_POLICY.toml
|       |       |-- skills/
|       |       |-- memory/
|       |       |-- prompts/
|       |       `-- sessions/
|       |-- memory/
|       |-- skills/
|       |-- workspaces/
|       |   |-- registry.toml
|       |   |-- aliases.toml
|       |   `-- grants.jsonl
|       |-- workers/
|       |-- sessions/
|       |-- artifacts/
|       |   `-- workers/
|       |-- checkpoints/
|       |-- sandboxes/
|       |-- eventlog/
|       |-- channels/
|       |-- secrets/
|       |-- logs/
|       |-- locks/
|       `-- run/
|-- logs/
|   |-- <session-id>.jsonl
|   `-- daemon.jsonl
|-- state/
|   |-- sessions/
|   |   `-- <session-id>.json
|   |-- agents/
|   |   `-- <agent-id>.json
|   |-- agent-sessions/
|   |   `-- <agent-session-id>.json
|   |-- workers/
|   |   `-- <worker-id>.json
|   `-- approvals/
|       `-- <approval-id>.json
|-- worktrees/
|-- run/
|-- tokens/
|-- sessions/
|-- workers/
`-- approvals/
```

`logs/` is append-only audit history. `state/` is the durable restart metadata
baseline for runtime recovery. The top-level `sessions/`, `workers/`, and
`approvals/` directories are reserved compatibility directories; new JSON
metadata belongs under `state/`.

Profile-aware helpers now also initialize `~/.{{PROJECT_SLUG}}/profiles/default/`. The
profile tree is the {{PROJECT_NAME}}-standard home for profile-local agents, memory,
workspace registry metadata, worker records, session records, artifacts,
checkpoints, event logs, channels, and secrets. The store crate keeps creating
the existing top-level paths so current daemon/core/CLI code continues to work
while profile-aware runtime pieces are added.

When `{{PROJECT_SLUG}}d` starts, daemon-known agents get non-destructive agent-home
templates under `profiles/<profile>/agents/<agent>/`. Existing files are not
overwritten.

## 2. Desktop MVP Config

The current implementation supports these keys:

```toml
{{PROJECT_SLUG}}_home = "~/.{{PROJECT_SLUG}}"
log_level = "info"

# Optional. If unset, {{PROJECT_NAME}} uses $XDG_RUNTIME_DIR/{{PROJECT_SLUG}}/{{PROJECT_SLUG}}d.sock when
# available, otherwise ~/.{{PROJECT_SLUG}}/run/{{PROJECT_SLUG}}d.sock.
# socket_path = "~/.{{PROJECT_SLUG}}/run/{{PROJECT_SLUG}}d.sock"

[profile]
# Default profile initialized under ~/.{{PROJECT_SLUG}}/profiles/<profile>/.
default_profile = "default"

[model]
# auto tries Ollama first and falls back to the local credential-free provider.
# Supported values: "auto", "codex-cli", "openai", "ollama", "echo".
# Per-agent selections may use "provider/model", for example "ollama/llama3.2"
# or "openai/gpt-5.2"; unsupported providers are rejected by {{PROJECT_SLUG}}d.
provider = "auto"
ollama_model = "llama3.2"
ollama_endpoint = "http://127.0.0.1:11434"
openai_model = "gpt-5.2"
openai_base_url = "https://api.openai.com/v1"

[hud]
theme = "arc"
# Supported today: "orb", "{{AVATAR_SLUG}}_arc".
# Future native value after renderer integration: "{{AVATAR_SLUG}}_native".
avatar_style = "orb"
background_opacity = 90
always_on_top = false

[voice]
enabled = false
# Supported visible providers: "edge", "elevenlabs", "openai", "system".
# "stub" is reserved for deterministic tests.
# For ElevenLabs, set {{PROJECT_NAME}}_ELEVENLABS_API_KEY or
# ~/.{{PROJECT_SLUG}}/secrets/elevenlabs_api_key locally; never commit provider secrets.
provider = "edge"
voice_id = "id-ID-GadisNeural"
stt_language = "auto"
rate = 0
pitch = 0
volume = 0
auto_speak = false
max_spoken_chars = 800

[agent_spawn]
# Applies to client-requested agent.spawn and explicit orchestrator /worker actions.
max_depth = 2
max_children_per_parent = 4
max_total_agents = 32

[agent_runtime]
# Applies to daemon-owned per-route AgentSession records.
default_timeout_sec = 900
max_steps_per_session = 8

[orchestrator]
# Enables explicit /worker, /spawn, /route, and /delegate message actions.
worker_delegation_enabled = true
default_worker_role = "Worker"
```

An example file is available at `config/{{PROJECT_SLUG}}.example.toml`.

Voice preferences are interpreted by `{{PROJECT_SLUG}}d`. The speech policy respects
`enabled`, `auto_speak`, and `max_spoken_chars` before dispatching text to a
provider. The daemon blocks code, diffs, terminal logs, and long raw tool or
test output from speech. HUD/Tauri remains the local microphone and playback
bridge where platform APIs require it.

## 3. Profile Home Layout

The store crate provides `{{PROJECT_NAME}}Home` and `ProfileHome` helpers for daemon-first
profile state:

```text
~/.{{PROJECT_SLUG}}/profiles/<profile>/
|-- profile.toml
|-- .gitignore
|-- agents/
|-- memory/
|-- skills/
|-- workspaces/
|   |-- registry.toml
|   |-- aliases.toml
|   `-- grants.jsonl
|-- workers/
|-- sessions/
|-- artifacts/
|   `-- workers/
|-- checkpoints/
|-- sandboxes/
|-- eventlog/
|-- channels/
|-- secrets/
|-- logs/
|-- locks/
`-- run/
```

`profile.toml` is initialized from a safe template. `.gitignore` excludes
secrets, channel state, sessions, workers, checkpoints, sandboxes, logs, locks,
runtime files, private keys, tokens, and SQLite sidecar files. Template
initialization is non-destructive: existing profile files are not overwritten.

The profile home is not an execution workspace and is not a sandbox. Project
roots must be registered separately in the workspace registry before
profile-aware tool/runtime code grants file, shell, git, or worker access.

## 3.1 Agent Home Layout

The store crate provides an `AgentHome` helper. The daemon initializes homes for
agents it knows about:

```text
~/.{{PROJECT_SLUG}}/profiles/<profile>/agents/<agent>/
|-- AGENT.toml
|-- PERSONA.md
|-- INSTRUCTIONS.md
|-- USER.md
|-- MEMORY.md
|-- TOOLS.md
|-- POLICY.toml
|-- SKILL_POLICY.toml
|-- skills/
|-- memory/
|   |-- daily/
|   |-- decisions.md
|   `-- delegation.md
|-- prompts/
|-- sessions/
`-- README.md
```

`AGENT.toml` records stable identity and conventional file names:

```toml
[agent]
id = "main"
display_name = "{{PROJECT_NAME}}"
role = "Orchestrator"
model = "auto"
specialist_id = "orchestrator"
specialist_label = "Orchestrator"
persona = "Coordinate the {{PROJECT_NAME}} agent cluster, track agent work, route tasks, and summarize cross-agent progress."

[files]
persona = "PERSONA.md"
instructions = "INSTRUCTIONS.md"
user = "USER.md"
memory = "MEMORY.md"
tools = "TOOLS.md"
policy = "POLICY.toml"
skill_policy = "SKILL_POLICY.toml"
```

`POLICY.toml` has a machine-readable structure that doctor checks parse as
typed TOML:

```toml
[policy]
version = 1
default_workspace_access = ["read"]
allow_network = false
allow_secret_access = false
allow_system_change = false
approval_required = true

[sandbox]
default = "workspace"
denied_paths = ["~/.ssh", "~/.aws", "~/.gnupg", "~/.config/gcloud", "/etc", "/dev", "/proc", "/sys"]

[limits]
max_agent_toml_bytes = 65536
max_policy_toml_bytes = 65536
max_text_file_bytes = 131072
max_memory_file_bytes = 1048576
```

This file is policy metadata for the agent home. Runtime authorization remains
daemon and policy-engine owned; Track D owns enforcement across mutating tool
execution.

`workspace doctor` currently includes profile/agent-home diagnostics for
missing, invalid TOML, and oversized agent files. Dedicated `profile doctor` and
`agent doctor` commands remain future work.

## 3.2 Worker Artifact Paths

Worker output artifacts are profile-scoped, not project-scoped:

```text
~/.{{PROJECT_SLUG}}/profiles/<profile>/artifacts/workers/<worker-id>/
|-- patch.diff
|-- test-report.json
|-- summary.md
|-- changed-files.json
`-- memory-candidates.jsonl
```

The store crate exposes path helpers for this layout. The current runtime can
include these planned locations in worker events; producing the files is still
part of the future worker runtime.

## 4. Workspace Registry

Profile workspace metadata lives at:

```text
~/.{{PROJECT_SLUG}}/profiles/<profile>/workspaces/registry.toml
```

The store crate loads and writes this TOML shape:

```toml
[[workspace]]
id = "example-project"
kind = "project"
root = "~/Project/example-project"
vcs = "git"
owner = "{{AGENT_SLUG}}"
trusted = true
worktree_root = ".{{PROJECT_SLUG}}/worktrees"
artifact_root = ".{{PROJECT_SLUG}}/artifacts"
checkpoint_policy = "enabled"

[[workspace.alias]]
workspace_id = "example-project"
aliases = ["example", "demo"]
```

Supported `kind` values are `project`, `documents`, `sandbox`, and `worktree`.
Supported `vcs` values are `git` and `none`. Supported `checkpoint_policy`
values are `enabled` and `disabled`. Helper APIs expand `~` in workspace roots
after loading and write registry updates atomically with redaction applied to
serialized TOML.

Workspace grants are persisted as redacted JSONL. Grant creation appends one
record; revocation rewrites the active grant set so stale grants do not revive
after daemon restart:

```text
~/.{{PROJECT_SLUG}}/profiles/<profile>/workspaces/grants.jsonl
```

Each grant records `grant_id`, `profile_id`, optional `agent_id`,
`workspace_id`, `root`, `access`, `created_at`, optional `expires_at`, `source`,
and optional redacted `reason`. Supported access values are `read`, `write`,
`exec`, and `admin`. Supported source values are `route`, `user`, `policy`, and
`worker_spawn`.

These store helpers only persist metadata. Enforcement remains daemon/runtime
owned: file, shell, git, and worker tools must resolve an active grant before
using a project root.

## 4.1 Project `.{{PROJECT_SLUG}}/workspace.toml`

Project-local metadata is optional but recommended for registered project
workspaces:

```text
<project>/.{{PROJECT_SLUG}}/workspace.toml
```

The store crate loads and writes this TOML shape:

```toml
workspace_id = "example-project"
kind = "project"
vcs = "git"
worktree_root = ".{{PROJECT_SLUG}}/worktrees"
artifact_root = ".{{PROJECT_SLUG}}/artifacts"
media_root = ".{{PROJECT_SLUG}}/media"
```

`workspace doctor` reads this file when present. It warns when the file is
missing, errors when `workspace_id` does not match the profile registry entry,
and warns when project-local roots are absolute instead of project-relative.
The file is metadata only; it does not create a workspace grant.

## 4.2 Project Worker Worktree Metadata

Worker worktree metadata is stored below the project worktree root:

```text
<project>/.{{PROJECT_SLUG}}/worktrees/
|-- <worker-id>/
`-- .metadata/
    `-- <worker-id>.toml
```

Each worker metadata file records the worker ID, workspace ID, planned or actual
worktree path, branch name, optional base ref, lifecycle state, and profile
artifact root. `workspace doctor` reports invalid TOML, stale/missing worktree
paths, and stale/missing artifact roots. These checks are diagnostics only and
do not run `git worktree` commands.

## 5. Native Avatar Config Contract

`crates/{{PROJECT_SLUG}}-avatar` defines the Rust config contract for the future native
{{AVATAR_NAME}} renderer. These keys are documented now for privacy review and are not yet
read by the desktop MVP config loader:

```toml
[avatar]
renderer = "wgpu_native" # "headless", "wgpu_native", "bevy_scene"
renderer_fallback = "orb" # "orb", "static_{{AVATAR_SLUG}}_texture"
reduced_motion = false
max_delta_ms = 250

[avatar.face_tracking]
mode = "off" # "off", "permission_required", "local_only"
permission_required = true
camera_indicator_required = true
one_click_disable_required = true
min_confidence_percent = 35

[avatar.privacy]
local_only_face_tracking = true
persist_raw_face_frames = false
persist_face_landmarks = false
allow_remote_face_tracking = false
allow_face_identity = false
```

Rules:

- Face tracking is off by default.
- The current `wgpu-renderer` crate feature is a native adapter spike that builds
  render plans from `AvatarFrame`; the desktop MVP config loader does not yet
  instantiate a GPU surface from these keys.
- Native {{AVATAR_NAME}} renderer failure falls back to the {{PROJECT_NAME}} orb by default and must
  not block HUD launch.
- Enabling face tracking requires explicit permission, a visible camera-active
  indicator, and a one-click disable action.
- Face tracking data must stay local and must not be persisted by default.
- Identity recognition, matching, embeddings, or biometric templates require a
  separate security and privacy decision before any config key can enable them.

## 6. Agent Spawn Limits

`[agent_spawn]` configures the daemon baseline for request-driven
`agent.spawn` and explicit orchestrator worker-spawn actions:

- `max_depth`: maximum child depth below a root agent. A child of `main` is depth 1.
- `max_children_per_parent`: maximum direct children under one parent.
- `max_total_agents`: maximum registered agents, including built-in agents.

The desktop MVP defaults are conservative: depth 2, 4 children per parent, and
32 total agents. Implicit model-driven recursive spawning remains reserved for a
later runtime track.

## 7. Orchestrator Routing

`[orchestrator]` configures daemon-owned message routing behavior. This is not a
HUD setting.

- `worker_delegation_enabled`: enables explicit `/worker`, `/spawn`, `/route`, and `/delegate` message actions handled by `{{PROJECT_SLUG}}d`.
- `default_worker_role`: role used by `/worker <task>` when no `Role: task` prefix is supplied.

Direct `@agent` mention targeting remains enabled independently of this flag.
Explicit worker spawn actions still use the `[agent_spawn]` depth, child, and
total-agent limits.

## 8. Model Provider Behavior

- `auto`: tries Ollama at `ollama_endpoint`, then falls back to the local echo provider.
- `codex-cli`: delegates to the installed official Codex CLI with `codex exec`.
  Authenticate the CLI separately with `codex login` for ChatGPT Plus/Pro access.
  {{PROJECT_NAME}} does not read, copy, or persist `~/.codex/auth.json`.
- `openai`: sends chat requests to the OpenAI Chat Completions API. It requires
  `{{PROJECT_NAME}}_OPENAI_API_KEY` or `OPENAI_API_KEY` in the daemon environment. OpenAI
  responses stream through server-sent events when the provider supports them.
- `ollama`: requires a running Ollama server and returns an error event if unavailable.
  Ollama responses stream through native `/api/generate` NDJSON chunks.
- `echo`: uses the credential-free local fallback.

Per-agent model selections set through `agent.model.set` are routed by `{{PROJECT_SLUG}}d`
when they include a supported provider prefix:

```text
auto/llama3.2
ollama/llama3.2
openai/gpt-5.2
codex-cli/gpt-5.4
echo/{{PROJECT_SLUG}}-local-fallback
```

Plain provider names such as `echo` or `ollama` select that provider with the
configured model. Plain model names select the configured default provider with
that model where the provider supports model overrides. Message events include
the effective provider and model used for the response.

Per-agent specialist selections set through `agent.specialist.set` are stored by
`{{PROJECT_SLUG}}d` and included in future daemon-built prompts for that agent. The HUD
offers curated specialist choices such as Engineering, Research, Marketing,
Product, Design, Data, Automation, Security, Operations, Finance, Writing, and
Humanizer, plus a Custom persona. The daemon also applies a built-in Humanizer
skill before model responses so normal chat stays natural, concise, and in the
user's language. The main {{PROJECT_NAME}} prompt includes a compact runtime summary of
known agents, recent agent sessions, and workers so orchestration can account
for what the rest of the cluster is doing.

The `models.list` protocol response exposes conservative readiness metadata for
clients and uses the configured `ollama_model` and `openai_model` as
provider/model IDs. `readiness = "fallback"` and `fallback = true` identify
entries such as `echo` or fallback-capable `auto` behavior so clients can
distinguish a real provider from the local fallback. `openai` is reported as
`ready` only when an OpenAI API key is present in the daemon environment.
Providers that need a daemon, login, API key, or local service are otherwise
reported as `requires_configuration` until {{PROJECT_NAME}} has active provider probing.
Ollama and OpenAI entries advertise the `streaming` capability; Codex CLI
stream events currently follow the shared callback contract but are limited by
the official CLI process output granularity.

Provider failures are surfaced through the normal `ErrorPayload` fields on
`session.failed` events. Clients should key off the stable `code` and
`retryable` fields instead of parsing the display message. Current provider
codes include `model_auth_missing`, `model_auth_failed`,
`provider_client_error`, `provider_unavailable`, `provider_rate_limited`,
`provider_http_error`, `model_not_found`, `model_request_rejected`,
`provider_response_invalid`, `provider_response_empty`, and `codex_cli_*`.
Messages are redacted for client display and must not include provider keys.

The OpenAI API key is not a config key. Do not put API keys, bearer tokens, or
auth headers in `~/.{{PROJECT_SLUG}}/config.toml`, examples, or logs.

## 9. Environment Variables

```text
{{PROJECT_NAME}}_HOME
{{PROJECT_NAME}}_LOG_LEVEL
{{PROJECT_NAME}}_MODEL_PROVIDER
{{PROJECT_NAME}}_SOCKET
{{PROJECT_NAME}}_HUD_SOCKET
VITE_{{PROJECT_NAME}}_SOCKET_PATH
OPENAI_API_KEY
{{PROJECT_NAME}}_OPENAI_API_KEY
CODEX_API_KEY
{{PROJECT_NAME}}_CODEX_BIN
{{PROJECT_NAME}}_CODEX_MODEL
{{PROJECT_NAME}}_CODEX_EXTRA_ARGS
{{PROJECT_NAME}}_WHISPER_CLI
WHISPER_CLI
{{PROJECT_NAME}}_WHISPER_MODEL
WHISPER_MODEL
{{PROJECT_NAME}}_WHISPER_LANGUAGE
WHISPER_LANGUAGE
{{PROJECT_NAME}}_HUD_NODE
XDG_RUNTIME_DIR
TELEGRAM_BOT_TOKEN
```

The desktop MVP reads `{{PROJECT_NAME}}_HOME`, `{{PROJECT_NAME}}_LOG_LEVEL`, `{{PROJECT_NAME}}_MODEL_PROVIDER`,
`{{PROJECT_NAME}}_OPENAI_API_KEY`, `OPENAI_API_KEY`, `{{PROJECT_NAME}}_CODEX_BIN`,
`{{PROJECT_NAME}}_CODEX_MODEL`, and `{{PROJECT_NAME}}_CODEX_EXTRA_ARGS`.

Socket discovery uses `{{PROJECT_NAME}}_HUD_SOCKET` for the Tauri HUD, `{{PROJECT_NAME}}_SOCKET` as a
shared client override, and `XDG_RUNTIME_DIR` for the default daemon socket
under `$XDG_RUNTIME_DIR/{{PROJECT_SLUG}}/{{PROJECT_SLUG}}d.sock`. `VITE_{{PROJECT_NAME}}_SOCKET_PATH` is a
development-only renderer seed for the browser preview; daemon state remains
authoritative.

Daemon-owned voice preferences read `[voice].provider`, `[voice].voice_id`,
`[voice].stt_language`, and `[voice].max_spoken_chars` for status, doctor, and
preflight reporting. The current green slice supports `edge`, `elevenlabs`,
`openai`, and `system` provider labels at the protocol/config layer. HUD/Tauri
still performs local capture and playback. `voice_id` is provider-specific:
{{PROJECT_NAME}} ships provider defaults for first-run setup, and users may replace the
value with their own Edge or ElevenLabs voice ID without changing code.

HUD voice input reads `{{PROJECT_NAME}}_WHISPER_CLI`, `WHISPER_CLI`,
`{{PROJECT_NAME}}_WHISPER_MODEL`, `WHISPER_MODEL`, `{{PROJECT_NAME}}_WHISPER_LANGUAGE`, and
`WHISPER_LANGUAGE`. `{{PROJECT_NAME}}_HUD_NODE` can point the Tauri side at a specific
Node.js binary for local voice helper execution.

ElevenLabs TTS reads `{{PROJECT_NAME}}_ELEVENLABS_API_KEY` first, then
`ELEVENLABS_API_KEY`, then `~/.{{PROJECT_SLUG}}/secrets/elevenlabs_api_key`. Optional
overrides are `{{PROJECT_NAME}}_ELEVENLABS_BASE_URL`, `{{PROJECT_NAME}}_ELEVENLABS_MODEL`, and
`{{PROJECT_NAME}}_ELEVENLABS_OUTPUT_FORMAT`.

`CODEX_API_KEY` is consumed by the official Codex CLI when that CLI is
configured for API-key auth; {{PROJECT_NAME}} does not read it directly. `TELEGRAM_BOT_TOKEN`
is reserved for the planned Telegram adapter and must stay empty in examples
until that adapter exists.

## 10. Secret Rules

- Do not store raw API keys in committed files.
- Do not commit `~/.codex/auth.json`; treat it as a password-equivalent file.
- Prefer `~/.{{PROJECT_SLUG}}/config.toml` for local runtime config and keep provider keys in environment variables or a future OS keychain integration.
- Do not write resolved secrets to logs.
- Event logs pass through {{PROJECT_NAME}} redaction before JSONL persistence.
- Workspace registry writes and grant JSONL appends also pass through {{PROJECT_NAME}}
  redaction before persistence.
- Redact values from keys containing `api_key`, `token`, `secret`, or `authorization`.
- Do not commit `.env`, generated auth JSON, HAR traces, crash dumps, logs, local sockets, or Tauri bundle output.

## 11. Durable State Files

The store crate provides atomic JSON helpers for these exact durable files:

```text
~/.{{PROJECT_SLUG}}/state/sessions/<session-id>.json
~/.{{PROJECT_SLUG}}/state/agents/<agent-id>.json
~/.{{PROJECT_SLUG}}/state/agent-sessions/<agent-session-id>.json
~/.{{PROJECT_SLUG}}/state/workers/<worker-id>.json
~/.{{PROJECT_SLUG}}/state/approvals/<approval-id>.json
```

Path components are redaction-safe: only ASCII letters, digits, `-`, and `_`
are preserved, and every other character is replaced with `_`. Empty IDs become
`unnamed`.

State writes use a private temporary file in the same directory:

```text
~/.{{PROJECT_SLUG}}/state/<kind>/.<safe-id>.json.tmp.<pid>.<counter>.<nanos>
```

The store writes redacted pretty JSON, syncs the temporary file, renames it over
the target `.json`, and syncs the parent directory. Recovery only reads final
`.json` files. Partial temp files are ignored, and corrupt final JSON files are
skipped with diagnostics instead of becoming trusted runtime state.

The daemon persists per-route `AgentSession` records in
`state/agent-sessions/` on lifecycle transitions. On restart, valid records are
replayed through `events.snapshot` as `agent.session.*` events. Corrupt final
AgentSession JSON is skipped and surfaced as a redacted `daemon.error`
diagnostic; partial `.tmp` files are ignored.
