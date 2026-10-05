# Naming conventions

## Items (folders in `src/`)

| Item | Pattern | Example |
|---|---|---|
| Project file | `<Domain>.pbip` | `Sales.pbip` |
| Semantic model | `<Domain>.SemanticModel` | `Sales.SemanticModel` |
| Main report | `<Domain>.Report` | `Sales.Report` |
| Extra thin report | `<Domain> <Audience>.Report` | `Sales Executive.Report` |

Names must match the item name in the Fabric / Power BI workspace.

## Semantic model

| Object | Convention | Example |
|---|---|---|
| Fact table | `Fact <Name>` or `<Name>` (plural) | `Fact Sales` |
| Dimension table | `Dim <Name>` or `<Name>` (singular) | `Dim Customer` |
| Measure table | `_Measures` | `_Measures` |
| Measure | Business name, Title Case, units in name | `Revenue`, `Revenue YoY %` |
| Hidden helper measure | prefix `_` | `_Revenue Base` |
| Key columns | `<Entity> Key` (surrogate, hidden) | `Customer Key` |
| Calculation group | `CG <Purpose>` | `CG Time Intelligence` |
| Power Query parameter | `PascalCase` | `BQ_Project`, `BQ_Dataset` |
| Role (RLS) | `RLS <Scope>` | `RLS Region` |

Every visible measure: `description` + `displayFolder` + `formatString`.

## Report (PBIR)

- Page display names: business-friendly; hidden tooltip/drillthrough pages prefixed `TT ` / `DT `.
- Rename visual folders only via Desktop — never hand-edit GUID folder names.
