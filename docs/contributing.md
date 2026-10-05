# Contributing

## Daily flow

```bash
git switch DEV && git pull
git switch -c feature/sales-yoy-measures
# close Desktop → open src/<Name>.pbip → make changes → Save → close Desktop
git add src/
git commit -m "feat(sales-model): add YoY revenue measures"
git push -u origin feature/sales-yoy-measures
# open PR → DEV
```

Promotion: `DEV → UAT` and `UAT → main` via PR only (no direct pushes, no cherry-picks into `main`).

## Commit messages (Conventional Commits)

`<type>(<scope>): <summary>`

| type | use for |
|---|---|
| `feat` | new measure, table, page, visual |
| `fix` | wrong calculation, broken visual, relationship fix |
| `perf` | model optimization (cardinality, data types, removed columns) |
| `refactor` | renames, display folders, no logic change |
| `style` | theme, formatting, layout only |
| `chore` | CI, config, repo housekeeping |
| `docs` | documentation |

Scope = item name in kebab-case: `sales-model`, `sales-report`.

## Pull requests

- Target `DEV` for feature work.
- One logical change per PR; keep diffs reviewable.
- Fill in the PR template checklist; CI must be green.
- Reviewer checks: DAX logic, relationships, M steps (query folding to BigQuery), naming, no hard-coded env values.

## Merge conflicts in PBIP

- **TMDL** (`tables/*.tmdl`): plain text — resolve normally, then open in Desktop to validate.
- **PBIR** (`visual.json`, `page.json`): resolve per file; if a visual is corrupted, take one side fully.
- **`expressions.tmdl`**: protected by `merge=ours` — target branch keeps its own parameters.
- Never resolve conflicts while the project is open in Desktop.

## Branch protection (GitHub → Settings → Branches)

For `main` and `UAT` (recommended for `DEV` too):
- Require a pull request before merging, ≥ 1 approval
- Require status checks: `Repo hygiene & JSON validation`, `Best Practice Analyzer (semantic models)`
- Require conversation resolution
- Block force pushes and deletions
