# {{PROJECT_NAME}} Documentation Template

A reusable documentation skeleton for software projects — PRD, BRD, FRD, TRD, architecture, roadmap, risk register, decision log, plus 22 engineering standards. All product-specific names have been replaced with `{{PLACEHOLDER}}` tokens so you can adopt the skeleton in any project.

See `TEMPLATE_GUIDE.md` for the full placeholder reference.

---

## How To Use This Template

### 1. Copy the docs into your project

```bash
# clone or copy the folder into your repo
cp -r mdown-collection/ your-project/docs/
```

### 2. Fill in the placeholders

Pick values for each token (see table below), then do a project-wide search-and-replace.

**Minimum tokens** every project must replace:

| Token | What to put | Example |
| --- | --- | --- |
| `{{PROJECT_NAME}}` | Human-readable name | `Acme` |
| `{{PROJECT_SLUG}}` | Lowercase CLI / package name | `acme` |
| `{{ORG}}` | GitHub org / owner | `acme-labs` |
| `{{ORG_NAME}}` | Legal / human org name | `Acme Labs, Inc.` |
| `{{AUTHOR}}` | Primary maintainer | `Jane Doe` |
| `{{REPO_URL}}` | Canonical repo URL | `https://github.com/acme-labs/acme` |

**Optional tokens** — only needed if your project has these concepts:

| Token | When you need it |
| --- | --- |
| `{{AVATAR_NAME}}` / `{{AVATAR_SLUG}}` | Your project ships a mascot / avatar character |
| `{{UI_REFERENCE}}` / `{{UI_REFERENCE_SLUG}}` | You are adapting another product's UI as reference |
| `{{LEGACY_UI}}` / `{{LEGACY_UI_SLUG}}` | You have a predecessor UI to call out / phase out |
| `{{AGENT_SLUG}}` | Your project has agent / plugin identifiers |

If a token does not apply, just delete the doc(s) that mention it.

### 3. Quick search-and-replace recipe

PowerShell:
```powershell
Get-ChildItem -Recurse -Filter *.md | ForEach-Object {
  (Get-Content $_.FullName -Raw) `
    -replace '\{\{PROJECT_NAME\}\}','Acme' `
    -replace '\{\{PROJECT_SLUG\}\}','acme' |
  Set-Content $_.FullName -Encoding utf8
}
```

Bash / ripgrep:
```bash
rg -l '\{\{PROJECT_NAME\}\}' | xargs sed -i 's/{{PROJECT_NAME}}/Acme/g'
```

### 4. Delete what you don't need

This template was extracted from a desktop-AI-agent project, so some docs assume that domain. Use the **Applicability** column below to decide what to keep.

---

## Document Index

### Always relevant (core planning)

| File | Purpose | Applicability |
| --- | --- | --- |
| `00_PROJECT_CHARTER.md` | Vision, scope, success criteria | Any project |
| `BLUEPRINT.md` | High-level shape of the system | Any project |
| `01_PRD.md` | Product requirements | Any project |
| `02_BRD.md` | Business requirements | Any project |
| `03_FRD.md` | Functional requirements | Any project |
| `04_TRD.md` | Technical requirements | Any project |
| `05_ARCHITECTURE.md` | System architecture | Any project |
| `06_IMPLEMENTATION_PLAN.md` | Build plan & phases | Any project |
| `07_MASTER_CHECKLIST.md` | Release readiness checklist | Any project |
| `08_ROADMAP.md` | Roadmap | Any project |
| `10_RISK_REGISTER.md` | Risk tracking | Any project |
| `11_DECISIONS.md` | Decision log (ADR-style) | Any project |
| `12_TEST_STRATEGY.md` | Test strategy | Any project |
| `13_GLOSSARY.md` | Domain glossary | Any project |
| `19_REQUIREMENTS_TRACEABILITY.md` | Traceability matrix | Any project |

### Common but optional

| File | Purpose | Applicability |
| --- | --- | --- |
| `09_OPEN_SOURCE_STANDARD.md` | OSS posture | Open-source projects |
| `14_SECURITY_THREAT_MODEL.md` | Threat model | Anything handling user data / network |
| `16_CONFIG_REFERENCE.md` | Config file reference | Apps with config files |
| `17_DEVELOPER_SETUP.md` | Dev environment setup | Any project with contributors |
| `18_INSTALLATION.md` | End-user install guide | Distributable software |
| `24_CONTRIBUTOR_SKILLS.md` | Skills matrix for contributors | Larger teams / OSS |
| `28_PLATFORM_BASELINE.md` | OS / runtime baseline | Cross-platform projects |
| `KNOWN_LIMITATIONS.md` | Known limitations | Any project |

### Domain-specific (delete if not applicable)

These were written for a **desktop AI-agent + HUD + avatar** product. Keep if your project has the same shape; otherwise delete.

| File | Domain assumed |
| --- | --- |
| `15_PROTOCOL_DRAFT.md` | Custom IPC / daemon protocol |
| `20_UI_ADAPTATION.md` | Adapting another product's UI |
| `21_UI_FEATURE_PARITY_CHECKLIST.md` | UI parity with a reference |
| `22_UI_STATE_PROTOCOL_CONTRACT.md` | UI ↔ daemon state contract |
| `23_UI_DESIGN_SYSTEM.md` | UI design system |
| `25_MEMORY_CONCEPT.md` | Agent memory model |
| `26_AVATAR_ENGINE.md` | Avatar rendering engine |
| `27_WORKSPACE_ARCHITECTURE.md` | Multi-workspace agent runtime |
| `29_PROTOCOL_FREEZE.md` | Protocol stability policy |
| `30_STABLE_CLI.md` | CLI stability contract |
| `31_STORAGE_FORMAT.md` | On-disk storage format |

### Engineering Standards (`standards/`)

22 reusable standards covering contribution, code, architecture, security, testing, docs, release, governance, CI/CD, observability, performance, licensing, and PR review. See `standards/README.md`. Most are universal; a few (`10_AGENT_STANDARD`, `15_UI_HUD_STANDARD`, `16_VOICE_STANDARD`) are domain-specific — delete those if irrelevant.

---

## Recommended Reading Order

1. `00_PROJECT_CHARTER.md`
2. `BLUEPRINT.md`
3. `01_PRD.md` → `02_BRD.md` → `03_FRD.md` → `04_TRD.md`
4. `05_ARCHITECTURE.md`
5. `06_IMPLEMENTATION_PLAN.md`
6. `07_MASTER_CHECKLIST.md`
7. `11_DECISIONS.md`
8. `standards/00_STANDARD_INDEX.md`

---

## License

The template content carries no license claim — adopt the docs into your own project under whatever license you choose.
