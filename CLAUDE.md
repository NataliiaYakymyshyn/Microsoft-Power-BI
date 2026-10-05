# CLAUDE.md — rules for AI assistants in this repo

- Source of truth: PBIP in `src/` — TMDL (`*.SemanticModel/definition/**/*.tmdl`) and PBIR (`*.Report/definition/**`).
- Never create or edit `.pbi/`, `cache.abf`, `localSettings.json`, or `.pbix` files.
- TMDL: tab indentation, CRLF. Keep `lineageTag` values unchanged; new objects may omit `lineageTag` (Desktop generates it).
- New measures go to `_Measures` table with `description`, `displayFolder`, `formatString` (see `docs/naming-conventions.md`).
- Do not change `expressions.tmdl` parameter values (environment-specific) unless explicitly asked.
- Prefer query folding in M for BigQuery sources; no hard-coded project/dataset names.
- Do not hand-edit PBIR visual GUID folders; edit `visual.json` content only.
- After edits, remind the user to open the `.pbip` in Power BI Desktop to validate.
- Commits: Conventional Commits (`feat(sales-model): ...`), branch from `DEV`.
