# {{UI_REFERENCE}} UI Adaptation Guide

## 1. Purpose

{{PROJECT_NAME}} will adapt the {{UI_REFERENCE}} HUD UI direction as the canonical desktop HUD experience.

The target is 100% product and interaction parity, not necessarily 100% source-code reuse. {{UI_REFERENCE}} is a Tauri + React implementation tied to {{LEGACY_UI}}. {{PROJECT_NAME}} is a daemon-first Rust runtime. The UI must therefore be adapted through {{PROJECT_NAME}} protocol, config, state, and policy boundaries.

## 2. Source References

Set this locally when auditing or porting:

```bash
export {{UI_REFERENCE}}_HUD_SOURCE=/path/to/{{UI_REFERENCE_SLUG}}-hud
export {{UI_REFERENCE}}_SPEC_SOURCE=/path/to/{{UI_REFERENCE_SLUG}}-specs
```

Important source files:

```text
${{UI_REFERENCE}}_HUD_SOURCE/src/App.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/lib/store.ts
${{UI_REFERENCE}}_HUD_SOURCE/src/lib/gateway.ts
${{UI_REFERENCE}}_HUD_SOURCE/src/lib/agents-roster.ts
${{UI_REFERENCE}}_HUD_SOURCE/src/styles/themes.ts
${{UI_REFERENCE}}_HUD_SOURCE/src/styles/globals.css
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/orbital/OrbitalHUD.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/orbital/RamaOrb.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/orbital/AgentWidget.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/orbital/WorkerTree.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/chat/ChatPanel.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/approvals/ApprovalCard.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/settings/ConfigDialog.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/settings/AgentRenameDialog.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/settings/VoiceConfig.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/settings/ModelsConfig.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/settings/ThemePicker.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/WindowChrome.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src/ui/wizard/Wizard.tsx
${{UI_REFERENCE}}_HUD_SOURCE/src-tauri/src/lib.rs
${{UI_REFERENCE}}_HUD_SOURCE/src-tauri/tauri.conf.json

${{UI_REFERENCE}}_SPEC_SOURCE/{{UI_REFERENCE}} Desktop HUD.html
${{UI_REFERENCE}}_SPEC_SOURCE/shared-ui.jsx
${{UI_REFERENCE}}_SPEC_SOURCE/variation-orbital.jsx
${{UI_REFERENCE}}_SPEC_SOURCE/backend-spec.jsx
${{UI_REFERENCE}}_SPEC_SOURCE/agents-data.jsx
${{UI_REFERENCE}}_SPEC_SOURCE/uploads/pasted-1777098520186-0.png
```

Before publishing {{PROJECT_NAME}} publicly, rewrite this section to point to a public design package, screenshot set, or committed UI reference files.

## 3. Adaptation Definition

100% adaptation means {{PROJECT_NAME}} preserves:

- orbital HUD composition
- central {{PROJECT_NAME}} orb behavior
- agent satellite cards
- worker tree under each agent
- status bar
- bottom chat and voice command panel
- pending approval stack
- unified config window
- agent rename flow
- voice picker and voice test flow
- model picker per agent
- theme picker and opacity control
- frameless transparent desktop window behavior
- first-run wizard intent
- gateway event semantics, translated to {{PROJECT_NAME}} protocol

100% adaptation does not mean:

- keep {{LEGACY_UI}} paths
- keep {{LEGACY_UI}} gateway topic names
- keep browser localStorage as source of truth
- make Node.js a {{PROJECT_NAME}} core dependency
- force Tauri if the final {{PROJECT_NAME}} HUD uses Dioxus

## 4. UI Surfaces

### HUD Shell

The main shell must compose:

- custom window chrome
- status bar
- orbital canvas
- approval stack overlay
- chat panel
- config dialog
- agent rename dialog

{{UI_REFERENCE}} reference composition:

```text
WindowChrome
StatusBar
OrbitalHUD
ApprovalStack
ChatPanel
ConfigDialog
AgentRenameDialog
```

{{PROJECT_NAME}} equivalent:

```text
{{PROJECT_NAME}}WindowChrome
{{PROJECT_NAME}}StatusBar
{{PROJECT_NAME}}OrbitalHud
{{PROJECT_NAME}}ApprovalStack
{{PROJECT_NAME}}ChatPanel
{{PROJECT_NAME}}ConfigDialog
{{PROJECT_NAME}}AgentRenameDialog
```

### Orbital HUD

The orbital HUD must use a 16:9 logical coordinate system. {{UI_REFERENCE}} uses 1920x1080 and places the central orb around the visual center, with 12 perimeter slots for agents.

{{PROJECT_NAME}} should keep:

- central orb
- two faint orbital rings
- dashed spokes from orb to agents
- non-overlapping perimeter agent slots
- model, context, mode, and voice readouts around orb
- state-driven orb animation

### Status Bar

The status bar must show:

- product/system name using current main agent display name
- daemon/gateway connection state
- selected main model
- active/waiting/idle counts
- optional system stats later

### Chat and Voice Panel

The bottom panel must show:

- streaming chat log
- user, assistant, and system messages
- voice status
- quick action chips
- mic button
- voice settings button
- model settings button
- textarea command composer

{{PROJECT_NAME}} must replace {{LEGACY_UI}} wording with {{PROJECT_NAME}} wording.

### Approval Stack

Approval cards must remain synchronized with daemon approval state. A click must send a response to `{{PROJECT_SLUG}}d`; the card is removed only after `approval.resolved` arrives.

### Config Dialog

Config dialog tabs:

- Voice
- Models
- Appearance
- Window

This unified dialog replaces separate one-off panels.

### Agent Rename Dialog

Agent rename opens from right-click/context action on:

- central orb for main agent
- agent satellite card for subagents

The rename must:

- trim whitespace
- collapse repeated whitespace
- cap at 32 characters
- fall back to `{{PROJECT_NAME}}` for main or role default for subagents if blank
- persist through daemon config/state
- emit `agent.renamed` event

## 5. Feature Adaptation Map

| {{UI_REFERENCE}} feature | {{UI_REFERENCE}} source | {{PROJECT_NAME}} adaptation |
| --- | --- | --- |
| Zustand HUD store | `src/lib/store.ts` | `{{PROJECT_SLUG}}d` owns durable state; UI keeps ephemeral view model |
| Local voice prefs | `{{UI_REFERENCE_SLUG}}.voicePrefs.v2` | `~/.{{PROJECT_SLUG}}/config.toml` and `ui.preferences.updated` |
| Local agent names | `{{UI_REFERENCE_SLUG}}.agentNames.v1` | `agent.rename` request plus daemon persistence |
| Local background opacity | `{{UI_REFERENCE_SLUG}}.backgroundOpacity.v1` | `hud.appearance.background_opacity` |
| Local chat prefs | `{{UI_REFERENCE_SLUG}}.chatPreferences.v1` | `chat.preferences` in daemon config/session |
| Local agent models | `{{UI_REFERENCE_SLUG}}.agentModels.v1` | per-agent model config in daemon |
| {{LEGACY_UI}} model catalog | `models.list.response` | `models.list` response from `{{PROJECT_SLUG}}d` |
| {{LEGACY_UI}} approvals | `approval.respond` | `approval.resolve` or `approval.respond` normalized by {{PROJECT_NAME}} protocol |
| {{LEGACY_UI}} gateway | `{{UI_REFERENCE_SLUG}}-hud.v1` | `{{PROJECT_SLUG}}-hud.v1` or {{PROJECT_NAME}} protocol version |
| Tauri Edge TTS command | `edge_tts_speak` | {{PROJECT_NAME}} voice provider command or daemon voice service |

## 6. Required {{PROJECT_NAME}} Protocol Additions

Add these requests:

```text
agent.rename
agent.model.set
agent.specialist.set
models.list
ui.preferences.get
ui.preferences.set
voice.preview
voice.stop
window.preference.set
```

Add or confirm these events:

```text
agent.renamed
agent.model.changed
agent.specialist.changed
models.list.response
ui.preferences.updated
voice.preview.started
voice.preview.completed
voice.preview.failed
window.preference.updated
```

Existing events that must drive HUD:

```text
daemon.status
session.started
message.delta
message.completed
agent.spawned
agent.status.changed
agent.task.changed
worker.started
worker.log.delta
worker.completed
approval.requested
approval.resolved
orchestrator.route
```

## 7. State Ownership Rules

{{PROJECT_NAME}} must not copy {{UI_REFERENCE}}'s localStorage-first model.

Use this split:

| State | Owner | UI cache allowed |
| --- | --- | --- |
| daemon connection | UI | yes |
| active chat draft | UI | yes |
| transcript view scroll | UI | yes |
| selected config tab | UI | yes |
| theme | daemon config | yes |
| background opacity | daemon config | yes |
| voice prefs | daemon config | yes |
| agent display names | daemon config/session store | yes |
| per-agent model | daemon config | yes |
| approvals | daemon | yes, event-derived only |
| workers | daemon | yes, event-derived only |
| agent statuses | daemon | yes, event-derived only |

## 8. Visual Requirements

{{PROJECT_NAME}} HUD must preserve:

- dark transparent glass window
- OKLCH hue-based theming
- low-radius panels
- mono operational labels
- central high-fidelity orb
- subtle grid, scanlines, dashed spokes
- compact readable cards
- glow used for state only, not decoration

The UI should feel like an operational desktop HUD, not a marketing dashboard.

## 9. Technical Strategy

There are two viable implementation paths.

### Path A: Exact UI First With Tauri + React

Use if the priority is fastest visual parity.

Pros:

- can reuse most {{UI_REFERENCE}} UI code
- easiest to match CSS, layout, animations
- faster to validate screenshots

Cons:

- adds Node/React/Tauri to HUD app
- conflicts with earlier Dioxus preference
- must ensure core remains Rust daemon and UI remains a client

### Path B: Port To Dioxus With {{UI_REFERENCE}} As Reference

Use if the priority is Rust-first UI consistency.

Pros:

- matches existing {{PROJECT_NAME}} Rust-first direction
- avoids React as long-term UI dependency
- cleaner Rust workspace story

Cons:

- slower to reach exact visual parity
- all CSS/animation behavior must be reimplemented
- screenshot parity will require more work

Recommendation:

- Keep `{{PROJECT_SLUG}}d` core independent.
- Decide UI toolkit with an ADR before implementation.
- If the user wants "100% UI" fastest, use Tauri + React for HUD v0.6 while keeping daemon protocol clean.
- If the user wants "100% Rust" strongest, port to Dioxus and accept slower visual parity.

## 10. Acceptance Criteria

{{PROJECT_NAME}} UI adaptation is complete when:

- screenshot parity passes for main HUD at 1600x1000 and 1920x1080
- all six themes work live
- config dialog has Voice, Models, Appearance, Window tabs
- main and subagent rename works and persists
- voice picker lists bilingual voices and preview works
- per-agent model picker updates daemon
- background opacity changes live and persists
- approval card waits for daemon resolution before disappearing
- workers appear under their parent agent
- chat panel sends messages through `{{PROJECT_SLUG}}d`
- auto-speak follows {{PROJECT_NAME}} content routing policy
- no UI client executes tools directly
- no {{LEGACY_UI}} file path remains in {{PROJECT_NAME}} runtime docs or code
