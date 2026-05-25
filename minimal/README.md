# {{PROJECT_NAME}} Documentation

Minimal, domain-neutral documentation template — usable for any software project (web app, library, CLI, service, mobile, etc.). For the full template (including AI-agent / HUD / avatar docs), see the parent folder.

See `TEMPLATE_GUIDE.md` for the placeholder reference.

## Tokens to fill in

| Token | Example |
| --- | --- |
| `{{PROJECT_NAME}}` | `Acme` |
| `{{PROJECT_SLUG}}` | `acme` |
| `{{ORG}}` | `acme-labs` |
| `{{ORG_NAME}}` | `Acme Labs, Inc.` |
| `{{AUTHOR}}` | `Jane Doe` |
| `{{REPO_URL}}` | `https://github.com/acme-labs/acme` |

Quick replace (PowerShell):
```powershell
Get-ChildItem -Recurse -Filter *.md | ForEach-Object {
  (Get-Content $_.FullName -Raw) `
    -replace '\{\{PROJECT_NAME\}\}','Acme' `
    -replace '\{\{PROJECT_SLUG\}\}','acme' |
  Set-Content $_.FullName -Encoding utf8
}
```

## Reading order

1. `00_PROJECT_CHARTER.md` — vision, scope, success criteria
2. `BLUEPRINT.md` — system shape
3. `01_PRD.md` → `02_BRD.md` → `03_FRD.md` → `04_TRD.md`
4. `05_ARCHITECTURE.md`
5. `06_IMPLEMENTATION_PLAN.md`
6. `07_MASTER_CHECKLIST.md`
7. `11_DECISIONS.md` (ADR log)
8. `standards/00_STANDARD_INDEX.md`

## Files

**Planning:** charter, blueprint, PRD, BRD, FRD, TRD, architecture, implementation plan, master checklist, roadmap, risk register, decisions, test strategy, glossary, requirements traceability, known limitations.

**Standards (`standards/`):** contribution, code, architecture, security, testing, documentation, release, governance, performance, observability, license/dependency, CI/CD, release notes, PR review workflow.

All files use `{{PLACEHOLDER}}` tokens — fill them once and you're done.
