# Environment parameters

One file per environment with the Power Query parameter values that differ between
DEV, UAT and PROD (e.g. BigQuery project / dataset). Used by deployment scripts to
override parameters after publish — never hard-code them in reports.

| File        | Branch | Workspace          |
|-------------|--------|--------------------|
| `dev.json`  | `DEV`  | `<Project> [DEV]`  |
| `prod.json` | `main` | `<Project>`        |

Secrets (keys, client secrets) go to GitHub Secrets — not here. `*.secret.json` is git-ignored.
