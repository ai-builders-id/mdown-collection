# {{AVATAR_NAME}} Avatar Engine

## 1. Purpose

This document defines the {{PROJECT_NAME}}-native {{AVATAR_NAME}} avatar engine direction. The
current {{AVATAR_NAME}} Arc avatar is a useful Tauri/React/Three.js prototype, but the
long-term goal is a native, daemon-driven avatar surface that can run inside the
{{PROJECT_NAME}} HUD without making browser WebGL or JavaScript animation the permanent
engine boundary.

The avatar engine is a HUD rendering capability only. It must not own agent
state, model state, policy, approvals, voice routing, tool execution, or memory.
Those remain owned by `{{PROJECT_SLUG}}d` and are projected to the avatar through protocol
events and daemon-owned UI preferences.

## 2. Native Engine Goal

{{PROJECT_NAME}} should support {{AVATAR_NAME}} as an expressive center avatar with:

- local-first rendering and animation
- daemon-derived state, not renderer-owned behavior
- deterministic state transitions for tests and accessibility
- optional GPU acceleration without blocking the default orb path
- no dependency on camera, microphone, or face tracking for the default avatar
- graceful fallback to the existing {{PROJECT_NAME}} orb or static {{AVATAR_NAME}} texture

The native engine should eventually replace the Three.js {{AVATAR_NAME}} Arc implementation
for production builds, while keeping the prototype available as a migration
reference until parity is proven.

## 3. Current Prototype

The current implementation lives under `apps/{{PROJECT_SLUG}}-hud`:

- Tauri + React hosts the production-oriented HUD shell.
- `@react-three/fiber`, `@react-three/drei`, and `three` render the optional
  {{AVATAR_NAME}} Arc WebGL scene.
- `arc-avatar-transparent.png` provides the portrait texture.
- The scene adds particles, reticles, shader glow, blink/gaze, and mouth pulse.
- `hud.avatar_style = "{{AVATAR_SLUG}}_arc"` is persisted through daemon-owned UI
  preferences.

This prototype is acceptable for exploration because it is isolated to the HUD,
lazy-loaded, and does not move WebGL dependencies into `{{PROJECT_SLUG}}d`. It is not the
native engine target because the animation loop, shader code, and scene graph
live in the web renderer rather than in a {{PROJECT_NAME}}-owned Rust rendering layer.

## 4. Renderer Direction

The Rust foundation lives in `crates/{{PROJECT_SLUG}}-avatar`. That crate owns the
renderer-independent state machine and exposes serializable frame data, gesture
state, privacy config, and a dependency-free direct-wgpu contract. It must stay
free of `wgpu`, Bevy, Tauri, camera, microphone, and model-provider
dependencies. Renderer adapters are separate implementation layers.

### Rust/wgpu Renderer

Use a focused Rust renderer built on `wgpu` as the preferred native path.

Strengths:

- Small engine surface tailored to one avatar, HUD compositing, and state-driven
  animation.
- Direct control over frame budget, texture upload, shader uniforms, and
  fallback behavior.
- Natural fit for Rust-native HUD experiments and future shared renderer tests.
- Avoids a broad game-engine dependency for an avatar that is mostly 2.5D
  portrait, particles, rings, pose interpolation, and overlays.
- Keeps renderer state serializable enough for deterministic visual fixtures.

Costs:

- {{PROJECT_NAME}} must implement its own small scene model, asset loader, animation graph,
  and hit/gesture mapping.
- More renderer plumbing is needed than the current Three.js prototype.
- HUD integration differs between Tauri WebView and Rust-native windows.

Recommended use:

- Production native {{AVATAR_NAME}} avatar engine.
- Shader and animation parity with the Three.js prototype.
- Future Rust HUD or native surface embedded beside the Tauri shell.

Contract:

- `AvatarFrame` is the renderer-neutral state boundary.
- `WgpuAvatarUniforms` is the first direct `wgpu` dynamic payload.
- `WgpuRendererContract` declares target, texture format, alpha mode, uniform
  version, and first primitive families: portrait plane, hologram material,
  reticle rings, particles, face overlay, and body gesture rig.
- With the `wgpu-renderer` feature, `crates/{{PROJECT_SLUG}}-avatar` also exposes an
  adapter-ready `wgpu_renderer` spike. It turns `AvatarFrame` data into a
  deterministic `WgpuAvatarRenderPlan` for portrait, reticle, particle, face,
  and body-rig primitives without linking the `wgpu` crate. A concrete GPU
  adapter can translate that plan into pipelines and draw calls later.
- The public boundary stays data-only and renderer-neutral until HUD integration
  chooses a concrete GPU surface; this avoids pulling heavy GPU dependencies
  into the state engine or daemon.

### Bevy Renderer

Do not make Bevy the first {{AVATAR_NAME}} engine unless {{PROJECT_NAME}} decides to build a broader
interactive 3D scene system.

Strengths:

- Mature ECS, asset pipeline, animation primitives, input handling, and `wgpu`
  backend.
- Faster path if {{PROJECT_NAME}} later needs full-body 3D rigs, skeletal animation,
  physics-like interaction, or complex camera scenes.
- Good development ergonomics for scene iteration.

Costs:

- Adds a large engine dependency for a narrow HUD avatar feature.
- ECS ownership can blur the {{PROJECT_NAME}} rule that daemon protocol is the authority
  and HUD render state is disposable.
- More startup, binary size, and dependency-audit surface than a focused
  renderer.
- Embedding Bevy cleanly inside a Tauri/WebView HUD is more complex than keeping
  it as a separate native surface.

Recommended use:

- Re-evaluate only after `wgpu` parity work proves insufficient or {{PROJECT_NAME}} accepts
  a broader native 3D UI decision record.
- Keep the `bevy-renderer` feature dependency-free until that decision exists.

## 5. Avatar State Contract

The avatar must map daemon-visible state into a compact render state:

| State | Meaning | Render cues |
| --- | --- | --- |
| `idle` | No active speech or task | slow breathing, low particle density |
| `listening` | User audio/input capture active | attentive gaze, subtle forward lean |
| `thinking` | Model or orchestrator is working | orbit motion, scanning gaze |
| `speaking` | Voice or assistant output is active | mouth pulse, emphasis gestures |
| `coding` | Worker/tool/code activity is active | focused posture, violet/blue work glow |
| `approval` | Approval or user input required | held pose, amber edge pulse |
| `error` | Failed request or blocked action | brief recoil, magenta/red accent |

The HUD may derive temporary animation intensity from message deltas, voice
amplitude, worker activity, and approval state. The daemon remains the source of
truth for the underlying event stream.

The Rust state engine currently exposes:

- `AvatarMode` for idle, listening, thinking, speaking, coding, approval, and
  error.
- `BodyGestureState` with gesture, priority, intensity, elapsed time,
  interruption, and reduced-motion metadata.
- `BodyPose`, `FacePose`, and `AvatarMaterial` as renderer-neutral pose and
  shader hints.
- `AvatarFallbackContract` and `AvatarFallbackState` so renderer failures can
  degrade to the {{PROJECT_NAME}} orb or a static {{AVATAR_NAME}} texture without blocking HUD
  launch.
- `HeadlessAvatarRenderer` for tests and non-graphical planning.
- Feature-gated `wgpu_renderer::WgpuAvatarSpikeRenderer` and
  `WgpuAvatarRenderPlan` for the next native renderer adapter slice.

## 6. Body Gesture Set

The first native {{AVATAR_NAME}} engine should support a small gesture vocabulary before
adding full skeletal complexity:

- idle breath and posture sway
- attentive lean while listening
- single nod for acknowledgement
- small head tilt for uncertainty or waiting
- gaze shift toward active agent, approval, or chat region
- hand raise or palm cue for approval-required state
- speaking emphasis pulse synchronized to voice amplitude or text cadence
- thinking scan with orbit reticle motion
- coding focus pose with reduced eye motion and tighter reticle rotation
- error recoil followed by recovery to the previous stable state

Gestures must be composable and interruptible. Safety and approval states should
win over decorative animation. Reduced-motion mode must disable large pose
changes and keep only minimal opacity or color transitions.

Gesture priorities:

| Priority | Use |
| --- | --- |
| `ambient` | idle breathing and low-priority loops |
| `activity` | listening, thinking, speaking, and coding |
| `interaction` | approval or user-input hold states |
| `safety` | error and blocked-action alerts |

## 7. Optional Face Tracking

Face tracking is optional future work, not a requirement for {{AVATAR_NAME}}.

Privacy and permission rules:

- Off by default.
- Requires explicit user opt-in before camera access.
- Uses OS/browser camera permission prompts and a visible in-HUD active-camera
  indicator.
- Processes tracking locally; no frames, landmarks, embeddings, or biometric
  templates are sent to model providers or stored by default.
- Does not write camera frames to JSONL logs, diagnostics, crash reports, or
  telemetry.
- Does not persist derived landmarks, embeddings, identity labels, or biometric
  templates by default.
- Does not send face tracking data to model providers, remote relays, telemetry,
  or logs.
- Provides a one-click disable action and clears transient tracking state when
  disabled.
- Degrades to scripted gestures when permission is denied or the camera is
  unavailable.

Acceptable tracking outputs are short-lived coarse controls such as gaze offset,
head yaw/pitch, blink confidence, and smile/open-mouth intensity. Any persistent
identity, recognition, or biometric feature requires a separate security and
privacy decision.

The state crate encodes this as `FaceTrackingConfig`, `AvatarPrivacy`, and
`AvatarEngineConfig::validate_privacy()`. Face tracking defaults to `off`; when
enabled, config must require explicit permission, a camera-active indicator, a
one-click disable action, local-only processing, and non-persistence.

## 8. Migration From Three.js {{AVATAR_NAME}} Arc

Migration should be incremental:

1. Keep `{{AVATAR_NAME}} Arc` as the optional Three.js prototype and default `{{PROJECT_NAME}} Orb` as
   the stable fallback.
2. Use `crates/{{PROJECT_SLUG}}-avatar` as the renderer-neutral state engine before adding
   a renderer crate.
3. Port the visual primitives to Rust/wgpu: portrait plane, alpha cutoff,
   hologram shader, particles, reticle rings, eye overlay, and mouth overlay.
4. Match the prototype states: idle, listening, thinking, speaking, coding,
   approval, and error.
5. Add deterministic visual fixtures for state transitions and reduced-motion
   mode.
6. Gate native {{AVATAR_NAME}} behind the existing `hud.avatar_style` preference or a new
   compatible value such as `{{AVATAR_SLUG}}_native` after protocol/config review.
7. Remove the Three.js dependency path only after native parity, screenshot
   coverage, license review, and packaging checks pass.

No migration step may move animation authority into `{{PROJECT_SLUG}}d`; the daemon emits
events and preferences, while the HUD renderer decides pixels.

## 9. Phased Implementation

### Phase A - Engine Spec and Assets

- Finalize render-state names, animation priorities, and reduced-motion rules.
- Confirm {{AVATAR_NAME}} texture and future rig/asset license provenance.
- Define snapshot expectations for current Three.js behavior.

Exit criteria:

- {{AVATAR_NAME}} engine spec is linked from decisions, plan, checklist, HUD standard, and
  HUD README.
- Asset provenance is documented before any new asset ships.

### Phase B - Native Renderer Spike

- Create a small Rust/wgpu renderer outside `{{PROJECT_SLUG}}d`. The first feature-gated
  spike now lives in `crates/{{PROJECT_SLUG}}-avatar::wgpu_renderer` as an adapter-ready
  render-plan builder.
- Load the {{AVATAR_NAME}} portrait texture. The spike requires a
  `portrait_texture_ready` readiness bit and fails into fallback state when the
  adapter reports it unavailable; real texture upload remains a HUD/native
  adapter task.
- Render alpha-cutout portrait, reticle rings, particles, and state color. The
  spike emits deterministic draw-state for those primitives but does not yet
  issue GPU draw calls.
- Support a mock render-state feed.

Exit criteria:

- Adapter-ready render plans cover idle, thinking, speaking, approval, and error
  states, including reduced-motion particle and reticle limits.
- It runs without camera or microphone permission and rejects renderer contracts
  that require camera access.

### Phase C - HUD Integration

- Embed or launch the native surface from the HUD without changing daemon
  authority.
- Connect render state to daemon events already consumed by the HUD.
- Add fallback to {{PROJECT_NAME}} Orb when native rendering fails.

Exit criteria:

- Avatar selection remains daemon-backed.
- Disconnected and fallback states are visible and recoverable.

### Phase D - Gesture and Voice Parity

- Add body gesture interpolation and mouth amplitude.
- Respect reduced-motion and low-power settings.
- Map approval, worker, model, and voice states to gesture priorities.

Exit criteria:

- {{AVATAR_NAME}} behavior communicates listening, thinking, speaking, coding, approval,
  and error without extra explanatory UI text.

### Phase E - Optional Face Tracking

- Add explicit permission UX and local-only processing.
- Add visible camera-active status and one-click disable.
- Add tests or manual checks for denied permission and camera-unavailable paths.

Exit criteria:

- Face tracking is useful but never required.
- Privacy rules are documented, testable, and enforced in UI behavior.

## 10. Non-Goals

- Do not put avatar engine logic in `{{PROJECT_SLUG}}d`.
- Do not require Bevy for v0.1.
- Do not require camera access for avatar expressiveness.
- Do not use face recognition, identity matching, or persistent biometrics.
- Do not block HUD launch on native avatar renderer failure.
- Do not remove the default {{PROJECT_NAME}} orb until {{AVATAR_NAME}} native fallback behavior is
  proven.
