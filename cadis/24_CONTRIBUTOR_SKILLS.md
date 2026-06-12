# Contributor Skills

## Purpose

This document explains which skills {{PROJECT_NAME}} contributors should use and why.

{{PROJECT_NAME}} needs two types of skills:

- global installed skills for common workflows
- project-local skills for {{PROJECT_NAME}}-specific architecture and product rules

## Installed Global Skills

The following curated skills were installed into `~/.codex/skills`:

| Skill | Why {{PROJECT_NAME}} needs it |
| --- | --- |
| `cli-creator` | Build and review the `{{PROJECT_SLUG}}` CLI experience. |
| `doc` | Maintain product, technical, and contributor docs. |
| `playwright` | Validate HUD behavior and screenshot flows. |
| `screenshot` | Inspect UI output during visual parity checks. |
| `security-best-practices` | Review secure defaults for tools, shell, storage, and config. |
| `security-threat-model` | Maintain and expand the {{PROJECT_NAME}} threat model. |
| `security-ownership-map` | Assign security-sensitive ownership areas. |
| `speech` | Work on TTS and voice output behavior. |
| `transcribe` | Work on STT, wake word, and transcription workflows. |
| `gh-fix-ci` | Diagnose GitHub Actions failures after CI exists. |
| `gh-address-comments` | Address review comments on GitHub PRs. |

OpenAI docs are available as a preinstalled system skill and should be used for OpenAI provider work.

## Project-Local Skills

Project-local skills live in `skills/`.

| Skill | Use when |
| --- | --- |
| `{{PROJECT_SLUG}}-rust-core` | Implementing daemon, CLI, store, sessions, or crate boundaries. |
| `{{PROJECT_SLUG}}-protocol` | Changing requests, events, content routing, or UI protocol. |
| `{{PROJECT_SLUG}}-policy-security` | Working on approval, risk classes, sandboxing, redaction, or threat model. |
| `{{PROJECT_SLUG}}-tool-runtime` | Implementing file, shell, git, patch, worktree, or other native tools. |
| `{{PROJECT_SLUG}}-model-provider` | Adding or reviewing model providers and streaming behavior. |
| `{{PROJECT_SLUG}}-{{UI_REFERENCE_SLUG}}-ui` | Working on HUD, config window, rename, themes, voice/model settings, or visual parity. |
| `{{PROJECT_SLUG}}-voice` | Working on TTS, STT, wake word, auto-speak, and speech policy. |
| `{{PROJECT_SLUG}}-open-source` | Working on docs, release, CI, issue templates, changelog, or governance. |

## Recommended Skill Combos

| Task | Skills |
| --- | --- |
| Build `{{PROJECT_SLUG}}` CLI | `{{PROJECT_SLUG}}-rust-core`, `cli-creator` |
| Add protocol event | `{{PROJECT_SLUG}}-protocol` |
| Add shell tool | `{{PROJECT_SLUG}}-tool-runtime`, `{{PROJECT_SLUG}}-policy-security`, `security-best-practices` |
| Add OpenAI provider | `{{PROJECT_SLUG}}-model-provider`, `openai-docs` |
| Port {{UI_REFERENCE}} HUD | `{{PROJECT_SLUG}}-{{UI_REFERENCE_SLUG}}-ui`, `playwright`, `screenshot` |
| Add voice preview | `{{PROJECT_SLUG}}-voice`, `speech` |
| Add STT | `{{PROJECT_SLUG}}-voice`, `transcribe` |
| Update threat model | `{{PROJECT_SLUG}}-policy-security`, `security-threat-model` |
| Fix CI | `gh-fix-ci` |
| Prepare public release | `{{PROJECT_SLUG}}-open-source`, `doc`, `security-ownership-map` |

## Contributor Rule

For any non-trivial task, contributors should read `AGENT.md`, then the one project-local skill that matches the task. Avoid loading every skill at once.

