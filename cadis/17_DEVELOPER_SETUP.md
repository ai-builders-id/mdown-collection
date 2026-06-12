# Developer Setup

## 1. Status

{{PROJECT_NAME}} now has a desktop MVP runtime:

- `{{PROJECT_SLUG}}d` local daemon over a Unix socket.
- `{{PROJECT_SLUG}}` CLI client.
- `{{PROJECT_SLUG}} status`, `{{PROJECT_SLUG}} doctor`, `{{PROJECT_SLUG}} models`, and `{{PROJECT_SLUG}} chat`.
- JSONL event logs under `~/.{{PROJECT_SLUG}}/logs`.
- Redaction before event persistence.
- Store-level atomic JSON metadata helpers under `~/.{{PROJECT_SLUG}}/state`.
- Ollama optional model adapter with local fallback.
- OpenAI optional model adapter using env-only API keys.
- Official Codex CLI optional adapter using `codex exec`.
- In-memory worker registry and `worker.tail` replay for route-time worker
  delegation logs.
- Tauri `{{PROJECT_SLUG}}-hud` desktop prototype under `apps/{{PROJECT_SLUG}}-hud`.
- HUD-local voice doctor preflight for mic, `whisper-cli`, Whisper model, Node
  helper, and audio player checks.
- `{{PROJECT_SLUG}}-avatar` renderer-neutral {{AVATAR_NAME}} avatar state crate.

Native mutating tools, risky tool execution after approval, real worker
command/test execution, worker cleanup, Telegram, production daemon-owned voice
provider execution, and full HUD parity are still planned work. The current
baseline already includes live event fan-out, safe-read tools including
`git.diff`, worker registry/tail replay, worker worktree setup, and
daemon-visible voice status/preflight.

The durable state helper baseline exists in `{{PROJECT_SLUG}}-store`; daemon startup
recovery now covers sessions, agents, AgentSessions, workers, and pending
approval state.

## 2. Requirements

- Rust stable with `rustfmt` and `clippy`.
- Git.
- Node.js 20 or newer with Corepack for the Tauri HUD frontend.
- pnpm 10.x; the HUD package pins the exact version through `packageManager`.
- Linux desktop for the primary runtime/HUD target.
- macOS for source-validation baseline work only.
- Windows for portable-crate validation only until runtime adapters exist.
- Tauri Linux development packages for HUD native checks: WebKitGTK 4.1,
  Ayatana AppIndicator, librsvg, and patchelf.
- Optional Ollama for real local model responses.
- Optional `{{PROJECT_NAME}}_OPENAI_API_KEY` or `OPENAI_API_KEY` for OpenAI responses.
- Optional official Codex CLI for ChatGPT Plus/Pro-backed Codex responses.

Cloud provider credentials are not required unless `[model].provider = "openai"`.
ChatGPT-plan auth is handled by the official Codex CLI, not by {{PROJECT_NAME}}.

See `docs/28_PLATFORM_BASELINE.md` for the platform support matrix.

## 3. Clone

```bash
git clone https://github.com/Growth-Circle/{{PROJECT_SLUG}}.git
cd {{PROJECT_SLUG}}
```

## 4. Build And Validate

```bash
cargo fmt --all --check
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-targets --all-features
```

Focused native avatar validation:

```bash
cargo test -p {{PROJECT_SLUG}}-avatar
```

HUD frontend validation:

```bash
cd apps/{{PROJECT_SLUG}}-hud
corepack enable
pnpm install
pnpm lint
pnpm typecheck
pnpm test
pnpm build
cargo check --manifest-path src-tauri/Cargo.toml --locked
```

## 4.1 Platform Baseline Checks

The project-standard macOS source baseline runs:

```bash
cargo fmt --all --check
cargo clippy --workspace --all-targets --all-features -- -D warnings
cargo test -p {{PROJECT_SLUG}}-protocol -p {{PROJECT_SLUG}}-policy -p {{PROJECT_SLUG}}-store -p {{PROJECT_SLUG}}-models -p {{PROJECT_SLUG}}-avatar -p {{PROJECT_SLUG}}-core --all-targets --all-features
```

The project-standard Windows baseline is limited to portable crates:

```bash
cargo check -p {{PROJECT_SLUG}}-protocol -p {{PROJECT_SLUG}}-policy -p {{PROJECT_SLUG}}-store -p {{PROJECT_SLUG}}-models -p {{PROJECT_SLUG}}-avatar --all-targets --all-features
cargo clippy -p {{PROJECT_SLUG}}-protocol -p {{PROJECT_SLUG}}-policy -p {{PROJECT_SLUG}}-store -p {{PROJECT_SLUG}}-models -p {{PROJECT_SLUG}}-avatar --all-targets --all-features -- -D warnings
cargo test -p {{PROJECT_SLUG}}-protocol -p {{PROJECT_SLUG}}-policy -p {{PROJECT_SLUG}}-models -p {{PROJECT_SLUG}}-avatar --all-targets --all-features
```

## 4.2 Secret Scan

Before pushing public branches, run a redacted secret scan against both Git
history and the current working tree:

```bash
gitleaks detect --source . --redact=100
gitleaks detect --no-git --source . --redact=100
```

The first command scans committed history. The second command scans local files
that are present in the checkout, including ignored build output, so findings in
`target/`, `node_modules/`, or other ignored artifact directories must be
triaged separately from tracked repository leaks.

## 5. Daemon Commands

```bash
cargo run -p {{PROJECT_SLUG}}-daemon -- --version
cargo run -p {{PROJECT_SLUG}}-daemon -- --check
cargo run -p {{PROJECT_SLUG}}-daemon --
```

Use a temporary socket for isolated testing:

```bash
cargo run -p {{PROJECT_SLUG}}-daemon -- --socket /tmp/{{PROJECT_SLUG}}-test.sock --dev-echo
```

## 6. CLI Commands

In another terminal:

```bash
cargo run -p {{PROJECT_SLUG}}-cli -- --socket /tmp/{{PROJECT_SLUG}}-test.sock status
cargo run -p {{PROJECT_SLUG}}-cli -- --socket /tmp/{{PROJECT_SLUG}}-test.sock doctor
cargo run -p {{PROJECT_SLUG}}-cli -- --socket /tmp/{{PROJECT_SLUG}}-test.sock models
cargo run -p {{PROJECT_SLUG}}-cli -- --socket /tmp/{{PROJECT_SLUG}}-test.sock chat "hello"
```

JSON frame output:

```bash
cargo run -p {{PROJECT_SLUG}}-cli -- --socket /tmp/{{PROJECT_SLUG}}-test.sock --json chat "hello"
```

## 7. Run Desktop HUD

Start `{{PROJECT_SLUG}}d` first:

```bash
cargo run -p {{PROJECT_SLUG}}-daemon -- --socket /tmp/{{PROJECT_SLUG}}-hud.sock --dev-echo
```

In another terminal:

```bash
cd apps/{{PROJECT_SLUG}}-hud
corepack enable
pnpm install
{{PROJECT_NAME}}_HUD_SOCKET=/tmp/{{PROJECT_SLUG}}-hud.sock pnpm tauri:dev
```

For a browser-only shell preview:

```bash
VITE_{{PROJECT_NAME}}_SOCKET_PATH=/tmp/{{PROJECT_SLUG}}-hud.sock pnpm dev
```

The Tauri HUD is the main desktop client. It connects to `{{PROJECT_SLUG}}d` over the Unix
socket and does not own durable runtime state or credentials. Browser preview
cannot invoke Tauri commands, so it is only useful for renderer work.

## 8. Voice Doctor

The HUD Settings -> Voice tab has a local doctor for desktop voice dependencies.
It reports renderer mic status, MediaRecorder availability, WebAudio analyser
and PCM fallback telemetry, `whisper-cli`, the configured Whisper model, Node
helper execution for Edge TTS, and audio player availability.

Useful local environment overrides:

```bash
export {{PROJECT_NAME}}_WHISPER_CLI="$HOME/.local/bin/whisper-cli"
export {{PROJECT_NAME}}_WHISPER_MODEL="$HOME/.local/share/{{PROJECT_SLUG}}/whisper-models/ggml-base.bin"
export {{PROJECT_NAME}}_WHISPER_LANGUAGE="id"
```

This is still a HUD-local capture/playback preflight. The HUD publishes the
preflight into daemon-visible voice status, while production daemon-owned TTS
provider execution remains planned.

## 9. Optional Ollama Setup

Install and start Ollama, then pull a model:

```bash
ollama pull llama3.2
```

Create `~/.{{PROJECT_SLUG}}/config.toml`:

```toml
[model]
provider = "ollama"
ollama_model = "llama3.2"
ollama_endpoint = "http://127.0.0.1:11434"
```

Use `provider = "auto"` to fall back to the local credential-free provider when
Ollama is not running.

## 10. Optional OpenAI Setup

Keep the API key in the daemon environment:

```bash
export {{PROJECT_NAME}}_OPENAI_API_KEY="..."
```

Create or update `~/.{{PROJECT_SLUG}}/config.toml`:

```toml
[model]
provider = "openai"
openai_model = "gpt-5.2"
openai_base_url = "https://api.openai.com/v1"
```

Do not store the API key in `config.toml`.

## 11. Optional Codex CLI Setup

Install and authenticate the official Codex CLI:

```bash
npm i -g @openai/codex
codex login
```

Then create or update `~/.{{PROJECT_SLUG}}/config.toml`:

```toml
[model]
provider = "codex-cli"
```

{{PROJECT_NAME}} calls `codex exec` in read-only ephemeral mode. It does not fork Codex
CLI and does not read or persist `~/.codex/auth.json`.

## 12. Event And Orchestrator Notes

The daemon currently emits route-time session, orchestrator, message delta,
message completed, and agent status events as follow-up frames for a request.
It also supports request-driven `agent.spawn` with configured depth, child, and
total-agent limits.

`events.subscribe` streams daemon-wide events. `session.subscribe` streams the
current session snapshot, bounded replay, and live events filtered to one
session ID. `worker.tail` can replay recent in-memory daemon worker logs.
Persistent worker recovery and daemon worker execution are not implemented yet.

## 13. Durable State Notes

The store crate owns the durable metadata path contract:

```text
~/.{{PROJECT_SLUG}}/state/sessions/<session-id>.json
~/.{{PROJECT_SLUG}}/state/agents/<agent-id>.json
~/.{{PROJECT_SLUG}}/state/agent-sessions/<agent-session-id>.json
~/.{{PROJECT_SLUG}}/state/workers/<worker-id>.json
~/.{{PROJECT_SLUG}}/state/approvals/<approval-id>.json
```

Use `StateStore` for new metadata writes. It sanitizes IDs for paths, writes
redacted JSON through temp-file-plus-rename, ignores partial temp files during
recovery, and reports corrupt final JSON as diagnostics.
`{{PROJECT_SLUG}}-core` currently reloads session, agent, and AgentSession metadata on
runtime startup. Recovered AgentSession records are replayed in
`events.snapshot`; corrupt final AgentSession JSON is reported as a redacted
`daemon.error` diagnostic.

## 14. Development Rules

- Add a crate only when it has a clear responsibility.
- Keep protocol types stable and tested.
- Keep core code independent from UI frameworks.
- Keep provider integrations modular.
- Add security tests before expanding risky tool behavior.
- Do not commit real provider keys, Telegram tokens, Codex auth JSON, `.env`
  files, JSONL logs, diagnostics, Tauri bundles, or local {{PROJECT_NAME}} state.
