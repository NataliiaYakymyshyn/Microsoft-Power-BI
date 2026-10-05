# POWER-BI

Power BI projects stored as **PBIP** (Power BI Project) with **TMDL** semantic models and **PBIR** reports, versioned in Git and validated by CI.

## Repository structure

```
POWER-BI/
├── .github/
│   ├── workflows/pbip-ci.yml      # PR checks: hygiene, JSON validation, BPA
│   ├── pull_request_template.md
│   └── CODEOWNERS
├── .vscode/                       # TMDL extension + editor settings (VS Code / Cursor)
├── config/
│   ├── environments/              # dev / uat / prod parameter values
│   └── bpa-rules.json             # (optional) custom BPA rules; default = Microsoft rules
├── docs/
│   ├── CONTRIBUTING.md            # branching, commits, PR flow
│   └── naming-conventions.md
├── src/                           # ← Fabric Git integration folder
│   ├── Sales.pbip                 # opens report + model in Desktop
│   ├── Sales.SemanticModel/       # shared model (TMDL)
│   ├── Sales.Report/              # report bound to Sales.SemanticModel (byPath)
│   └── Sales Executive.Report/    # extra thin report on the same model
├── .gitignore  .gitattributes  .editorconfig
├── CLAUDE.md                      # rules for AI assistants
└── README.md
```

### Item layout (what Power BI Desktop generates)

```
Sales.SemanticModel/
├── definition/
│   ├── database.tmdl  model.tmdl  relationships.tmdl
│   ├── expressions.tmdl           # Power Query parameters (env-specific)
│   ├── tables/*.tmdl              # one file per table: columns, measures, partitions (M)
│   ├── roles/*.tmdl               # RLS
│   └── cultures/*.tmdl
├── .pbi/                          # localSettings.json + cache.abf → git-ignored
├── definition.pbism
└── .platform
Sales.Report/
├── definition/                    # PBIR: pages/<page>/visuals/<visual>/visual.json
├── StaticResources/               # themes, images
├── definition.pbir                # "byPath": "../Sales.SemanticModel"
└── .platform
```

## Getting started

1. **Power BI Desktop → Options → Preview features**: enable
   *Power BI Project (.pbip) save option*, *Store semantic model using TMDL format*, *Store reports using enhanced metadata format (PBIR)*.
2. Clone and configure merge driver (once per clone):
   ```bash
   git clone https://github.com/NataliiaYakymyshyn/POWER-BI.git
   cd POWER-BI
   git config merge.ours.driver true   # keeps env parameters on DEV→UAT→main merges
   git config core.autocrlf false      # .gitattributes controls line endings
   ```
3. Create / migrate a project: open the `.pbix` → **File → Save as → Power BI project (*.pbip)** → save into `src/` as `<Name>.pbip`. Do **not** commit the `.pbix`.
4. Open the repo in Cursor / VS Code and install recommended extensions (TMDL syntax highlighting).

## Working rules

| Rule | Why |
|---|---|
| One semantic model → many thin reports (`byPath`) | Single source of truth for measures |
| Close Desktop before `git pull` / switching branches | Desktop holds files in memory and overwrites them on save |
| Edit TMDL in the editor only for bulk/text changes, then re-open in Desktop to validate | Desktop is the schema validator |
| Never commit `.pbi/cache.abf`, `localSettings.json`, `.pbix` | Data, user settings, binaries |
| Env-specific values only in Power Query parameters (`expressions.tmdl`) + `config/environments` | Same code on DEV / UAT / PROD |
| Small PRs: one feature per branch | TMDL/PBIR diffs stay reviewable |

## Branching & deployment

```
feature/* ──PR──▶ DEV ──PR──▶ UAT ──PR──▶ main
                   │           │           │
            [DEV] workspace  [UAT] ws   PROD workspace
```

- `main` — production, protected (PR + review + green CI).
- `UAT` — user acceptance, protected.
- `DEV` — integration branch, default target for feature PRs.
- `feature/<short-name>`, `fix/<short-name>` — short-lived work branches from `DEV`.

Workspace sync: either **Fabric Git integration** (workspace connected to branch, Git folder = `/src`) or GitHub Actions deployment using a Service Principal — pick one per workspace, not both.

See [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md) and [docs/naming-conventions.md](docs/naming-conventions.md).
