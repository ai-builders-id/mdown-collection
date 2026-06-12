# Template Guide

This documentation set is a generic template. The original product-specific
names have been replaced with `{{PLACEHOLDER}}` tokens. Fill in the tokens
below with values that match your project before publishing.

## How To Use

1. Decide on the values for each placeholder (see table below).
2. Search-and-replace each `{{TOKEN}}` across the repo with the chosen value.
3. Rename any files that still embed placeholders (none ship that way by
   default, but check before committing).
4. Delete sections that do not apply to your project (e.g. the avatar engine
   doc if your project has no avatar).

## Placeholder Reference

| Token | One-line description | Example value |
| --- | --- | --- |
| `{{PROJECT_NAME}}` | Human-readable project name, used in prose and headings (capitalized form). | `Acme` |
| `{{PROJECT_SLUG}}` | Lowercase identifier used in CLI names, crate names, daemon names, config paths, and dotfile directories. | `acme` |
| `{{ORG}}` | GitHub org or owner that hosts the repo. | `acme-labs` |
| `{{ORG_NAME}}` | Human-readable organization name used in attribution and license blocks. | `Acme Labs, Inc.` |
| `{{AUTHOR}}` | Primary author or maintainer name where a single attribution is needed. | `Jane Doe` |
| `{{REPO_URL}}` | Full URL of the canonical source repository. | `https://github.com/acme-labs/acme` |
| `{{AVATAR_NAME}}` | Display name of the avatar / mascot character used by the HUD. | `Aria` |
| `{{AVATAR_SLUG}}` | Lowercase slug form of the avatar name, used in folder and crate names. | `aria` |
| `{{UI_REFERENCE}}` | Display name of the UI reference product whose HUD design is being adapted. | `Orbital` |
| `{{UI_REFERENCE_SLUG}}` | Lowercase slug form of the UI reference product, used in crate/feature names. | `orbital` |
| `{{LEGACY_UI}}` | Name of any predecessor / legacy UI that must not be linked or bundled. | `LegacyApp` |
| `{{LEGACY_UI_SLUG}}` | Lowercase slug form of the legacy UI. | `legacyapp` |
| `{{AGENT_SLUG}}` | Sample agent identifier shown in config snippets and example folder layouts. | `default-agent` |

## Renamed Files

The original product-specific filenames have been renamed to generic ones:

| Original | Renamed |
| --- | --- |
| `26_WULAN_AVATAR_ENGINE.md` | `26_AVATAR_ENGINE.md` |
| `20_RAMACLAW_UI_ADAPTATION.md` | `20_UI_ADAPTATION.md` |

All cross-references in `README.md`, `standards/00_STANDARD_INDEX.md`, and
sibling docs have been updated to the new names.
